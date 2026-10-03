# Plan: issue #62 - health check references `settings.redis_host`

## Diagnosis

The Redis probe in `api/routes/health.py` builds its client from
`settings.redis_host` and `settings.redis_port`. `Settings`
(`core/config.py`) doesn't define either field - it carries a single
`redis_url: str = Field(default="redis://localhost:6379/0")`. The
resulting `AttributeError` is caught by the probe's bare
`except Exception`, so the health check reports Redis as down even when
it's reachable.

This matches my own posted repro exactly: with Redis confirmed reachable
(`docker compose exec redis redis-cli ping` -> `PONG`), the endpoint still
returned `HTTP 503` with `"redis": "unhealthy"`, and the terminal log
showed `redis_health_check_failed error="'Settings' object has no
attribute 'redis_host'"`. The fix is to build the client from
`settings.redis_url` instead of the two fields that don't exist.

## Scope

In scope: the Redis check block in `api/routes/health.py`, and the stale
mypy suppression for it in `pyproject.toml`.

Not in scope:
- The Postgres check in the same function. My repro also showed
  `postgres: unhealthy` (a separate `SELECT 1` / SQLAlchemy `text()`
  issue), which is a different, unrelated defect - not something I'm
  touching here.
- The Vector DB check (a placeholder, unrelated).
- Issue #61. `pyproject.toml`'s mypy override for `api.routes.health`
  currently disables three error codes - `attr-defined`,
  `call-overload`, and `index`. CONTRIBUTING.md names `attr-defined` as
  #62 specifically; the override's own comment separately flags the
  whole entry as "related #61" without itemizing which codes that
  covers. I'm reading `call-overload`/`index` as #61's by elimination -
  an inference, not something either document states outright - and
  removing `attr-defined` only, leaving the rest of the override in
  place.

## Files

- `api/routes/health.py` - the Redis check block inside `health_check()`.
- `pyproject.toml` - the `[[tool.mypy.overrides]]` entry for
  `module = "api.routes.health"`: drop `"attr-defined"` from its
  `disable_error_code` list, keep `"call-overload"` and `"index"`.
- `tests/unit/test_health.py` (new) - there's no existing unit test for
  this endpoint and no `xfail` marker to remove for this particular
  issue, so this is a new regression test rather than an unmarking.

## Approach

1. In the Redis check block, replace the `redis.Redis(host=..., port=...,
   db=0, decode_responses=True)` construction with
   `redis.Redis.from_url(settings.redis_url, decode_responses=True)`.
   `redis_url`'s default already includes the db index
   (`redis://localhost:6379/0`), so there's no separate `db=` argument to
   carry over.
2. Remove `"attr-defined"` from the `api.routes.health` mypy override's
   `disable_error_code` list in `pyproject.toml`, per CONTRIBUTING.md's
   instruction that fixing a seeded bug removes its suppression entry.
   Leave `"call-overload"` and `"index"` in place for #61.
3. Add `tests/unit/test_health.py`: mock the Redis client so `ping()`
   succeeds, call `health_check()`, and assert
   `dependencies.redis == "healthy"` with no `AttributeError` raised.

## Test plan

Re-run my week-2 repro steps against the fix, same environment (Windows
10, Git Bash, Python 3.11.4, Docker 29.1.3 / Compose v2.40.3):

1. `docker compose up -d`, `make setup`, `make run` (unchanged).
2. `docker compose exec redis redis-cli ping` -> confirm `PONG` again,
   same control check as before.
3. `curl -s -o /tmp/health.json -w "HTTP %{http_code}\n"
   http://localhost:8000/health`

Today: `HTTP 503`, body reports `"redis": "unhealthy"`, log shows
`redis_health_check_failed error="'Settings' object has no attribute
'redis_host'"`.

Expected after the fix: body reports `"redis": "healthy"`, and the log
has no `redis_health_check_failed` entry at all. The overall response
will likely still be `HTTP 503` and `"status": "unhealthy"`, because the
unrelated Postgres check is still broken in this same snapshot - that's
expected and out of scope here, not a sign the fix didn't work.

Also run `make test-unit` (new `test_health.py` passes) and `make
typecheck` (confirms no new `attr-defined` error surfaces now that the
suppression for it is gone - if one does, the suppression removal was
premature and needs to stay, or the fix is incomplete).

## Risks and unknowns

- `redis.Redis.from_url()` needs to behave the same as the explicit
  `host=`/`port=`/`db=` construction for this use case; I haven't yet
  confirmed there's no subtle option (e.g. a non-default `decode_responses`
  interaction) that the explicit form was relying on - the test plan's
  `make typecheck` and the live repro re-run are both meant to catch that.
- Removing only `"attr-defined"` from the mypy override assumes the
  other two error codes genuinely belong to #61 and not also to this
  line; if `make typecheck` still shows a fresh `attr-defined` elsewhere
  in the file after my change, I'll need to look again before concluding
  the suppression removal is safe.

## Deviations

Built essentially as planned. One small addition beyond what's written
above: I also reworded the prose comment directly above the
`api.routes.health` mypy override (previously "attr-defined -> issue
#62... and related #61") so it no longer refers to a suppression entry
that's no longer there. Leaving it unchanged would have made the
comment misleading once `attr-defined` was removed from the list below
it - not a logic change, just keeping the documentation next to the
code honest.

Everything else matched: the Redis client now builds from
`settings.redis_url` exactly as described, the mypy override keeps only
`call-overload` and `index`, and `tests/unit/test_health.py` covers both
the healthy and the genuine-outage path. Re-running my week-2 repro
steps against the change produced exactly what the test plan predicted:
`"redis": "healthy"` in the response body, `redis_health_check_passed`
in the log (no `AttributeError`, no `redis_health_check_failed`), and
the overall response still `HTTP 503` because the separate, unrelated
Postgres issue is still present in this snapshot - expected, not a sign
anything is wrong with this fix. `make test-unit` and `make typecheck`
both pass clean.

One caveat on "pass clean" for `make test-unit`: when I re-ran it on the
final commit, 30 errors showed up in `test_structural_chunker`. All 30 are
`CERTIFICATE_VERIFY_FAILED` while tiktoken downloads an encoding file - my
machine's endpoint-security tool intercepts TLS. They are network-only,
unrelated to this change, and the two new `test_health` tests pass.
