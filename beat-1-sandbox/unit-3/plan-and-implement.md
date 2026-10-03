# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

vzan2012

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5925038952

Text as posted:

> Opening a plan for this, built on the repro I posted above (commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`).
>
> What happened: `api/routes/health.py` builds its Redis client from `settings.redis_host` and `settings.redis_port`, neither of which exist on `Settings` - only `redis_url` is defined there. The resulting `AttributeError` falls into the probe's bare `except Exception`, so the check reports Redis as unhealthy even though it's actually up - exactly what the repro above shows.
>
> Fix: build the client with
> ```python
> redis.Redis.from_url(settings.redis_url, decode_responses=True)
> ```
> and add a regression test for this path since there isn't one currently. I'll also drop `attr-defined` from the `api.routes.health` mypy override in `pyproject.toml` - CONTRIBUTING.md names this exact suppression as tied to #62. The override's own comment separately flags the entry as "related #61" without saying which codes that covers; I'm reading `call-overload`/`index` as #61's by elimination and leaving those two alone. I'm leaving the Postgres check alone too - it showed unhealthy in my repro, but that's a different, unrelated `SELECT 1`/SQLAlchemy `text()` problem.
>
> Verifying with `make test-unit` and `make typecheck`, plus the same repro steps re-run against the change.

---

## Your branch

**Branch**

`fix/62-redis-health-check`

**Evidence**

**Before** - my Unit 2 repro comment on the issue (https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5850801524), commit `2f4e82f`, Windows 10 / Git Bash:

```
$ curl -s -o /tmp/health.json -w "HTTP %{http_code}\n" http://localhost:8000/health
HTTP 503
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"...

2026-09-26 18:56:41 [error] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'" ...
INFO: 127.0.0.1:57969 - "GET /health HTTP/1.1" 503 Service Unavailable

$ docker compose exec redis redis-cli ping
PONG
```

**After** - same steps (`docker compose up -d`, start the backend, hit `/health`) on commit `b30a202`, 2026-10-03:

```
$ git rev-parse --short HEAD
b30a202

$ curl -s -w "\nHTTP %{http_code}\n" http://localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"healthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-03T16:06:05.357344"}}
HTTP 503

Terminal log from that request:
2026-10-03 12:06:05 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" request_id=0946d362-7f8e-4b43-824c-aa7cd1fc6be3
2026-10-03 12:06:05 [debug    ] redis_health_check_passed      request_id=0946d362-7f8e-4b43-824c-aa7cd1fc6be3
2026-10-03 12:06:05 [debug    ] vector_db_health_check_passed  request_id=0946d362-7f8e-4b43-824c-aa7cd1fc6be3
INFO:     127.0.0.1:56629 - "GET /health HTTP/1.1" 503 Service Unavailable

$ docker compose exec redis redis-cli ping
PONG
```

Redis flips from `unhealthy` (AttributeError) to `healthy` (`redis_health_check_passed`). The overall response is still 503 because of the separate, unrelated Postgres `text('SELECT 1')` defect, which my plan said was out of scope.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (full run, 2026-10-01): 19/20 (bar 18/20, PASS) - every category matched. This was the only full run, so it is the last score and matches the `agreement:` line in `eval-run.txt`. The one disagreement was pkg-14 (gold accept, my rubric said reject); I chose to leave it rather than loosen a check to chase 20/20.

**Package analysis**

pkg-14 (zellij#5174, OSC color sequences leaking on session reattach) had gold label `accept`. My rubric said `reject`, failing only `executable-by-a-stranger`. The other five checks passed, and I think that is right: the diagnosis fits every data point in the repro (clean first attach, leak on every reattach, clean on 0.44.1, the cache control), the scope is bounded, and the test plan is the repro loop with a concrete pass condition. What tripped it was the Files line. The plan names an area ("the client attach/reattach path in `zellij-server`") but says "exact functions to be pinned in the PR after tracing". My check fails a plan that only names an outcome with no concrete file or step, and a stranger could not open a specific function from this plan alone. The gold label reads the same plan as an honestly scoped-down one: the author names the area, explains why the functions are not pinned yet, and has a debug-log method to find them. I read that as a real gap in the plan, and the gold label reads it as acceptable deferral. I left it at 19/20 rather than loosen the check to chase 20/20.

**Check rationale**

> | `executable-by-a-stranger` | The plan's approach/changes section | Someone who's never seen the issue could start making the change from what's written, without asking the author a question first. Fails if the plan only names an outcome ("make it fast again") with no concrete file or step | required |

It reads this way because the question it asks is "could someone else start?", not "is this plan polished?". The failure example is an outcome-only plan, and the bar is a concrete file or step, so a plan does not have to be long or detailed to pass, it just has to be startable. I do not remember the individual wording changes I made while iterating, so I will not claim a revision history. What the final wording shows is what it rejects: a plan that is only a goal, and a plan that needs a follow-up question before work can begin. It also matches the standard I held my own #62 plan to, which names `api/routes/health.py` and the exact `redis.Redis.from_url(settings.redis_url, decode_responses=True)` call.

**Trade-offs**

This check gives up recall on plans that honestly defer exact locations, and pkg-14 is the concrete case: gold accept, my rubric rejects. I accept that miss because a plan that cannot say where the change goes is not yet something a stranger can start from. I ran the full eval once (through Claude Code executing `run_eval.py`, not by hand) and did not re-run any package with `--only`, so I cannot say whether loosening this check would move other packages. That is why I left it unchanged.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
