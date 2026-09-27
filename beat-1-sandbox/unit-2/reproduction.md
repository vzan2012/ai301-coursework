# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

vzan2012

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5850800672

Picking this up as my Unit 2 reproduction target - the `/health` Redis probe reads `settings.redis_host`/`settings.redis_port`, but `Settings` only exposes `redis_url`, so the resulting `AttributeError` gets caught by the bare `except Exception` and reported as `redis: unhealthy` even when Redis is up. I'll set up the repo locally, reproduce the 503 against a real Redis instance, and post a repro report with my environment, steps, and observed output before touching any code.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5850801524

Environment: Windows 10 (build 19045), Git Bash (MINGW64), Python 3.11.4, Docker 29.1.3 / Compose v2.40.3, commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088` (`main`).

Steps:

1. Forked and cloned the repo, `cp .env.example .env` (defaults untouched — `LLM_PROVIDER=mock`, no API key needed for this bug)
2. `docker compose up -d` (postgres, redis, chromadb), waited for `db`/`redis` to report healthy
3. `make setup`
4. `make run`
5. `curl -s -o /tmp/health.json -w "HTTP %{http_code}\n" http://localhost:8000/health`

Expected (per issue): 503 with body reporting `"redis": "unhealthy"`, and an `AttributeError` for `redis_host` in the log.

Actual:

```
HTTP 503
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"...
```

Terminal log from the same request:

```
2026-09-26 18:56:38 [error] postgres_health_check_failed error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" ...
2026-09-26 18:56:41 [error] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'" ...
INFO: 127.0.0.1:57969 - "GET /health HTTP/1.1" 503 Service Unavailable
```

This matches the issue exactly: `/health` returns 503, redis reported unhealthy, and the log shows the same `AttributeError` (`Settings` has no `redis_host`).

Control check - confirmed Redis is actually reachable, so this is the health-check bug, not a real outage:

```
$ docker compose exec redis redis-cli ping
PONG
```

Note: the same response also shows `postgres: unhealthy`, from an unrelated SQLAlchemy `text()` issue - a separate defect, not part of what I'm claiming here.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. First full run (original rubric): 17/20 - below the bar. Misses: `pkg-03` (failed `comments`), `pkg-09` (failed `behavior`), `pkg-10` (failed `behavior`).
2. Second full run (after revising `behavior` and `comments`, plus canary re-checks on `pkg-02`, `pkg-07`, `pkg-13`, `pkg-19`, `pkg-20` via `--only`): 20/20 - every package agreed, category floor held.
3. Third full run, the confirming run saved with `--save-run eval-run.txt`: 19/20 - matches the agreement line in the committed `eval-run.txt`. `pkg-09` flipped back to a disagreement here (see Package analysis below); this is a legitimate pass, since the bar is 18/20 and the category floor still holds.

**Package analysis**

`pkg-09` (`sharkdp/fd#2033`). Gold: `accept` - an honest cannot-reproduce (a genuine, repeated attempt at the exact `--exec-batch` reordering scenario, with the marker-order log shown and the environment factors that likely mattered named). My rubric's verdict on this package was not stable across runs: it graded `accept` on several verification runs after I revised `behavior` to allow a faithful cannot-reproduce, but reverted to `reject` on the final, committed run. The evidence quoted by the grader on the reject was: "the fish-specific logical PWD resolution 'looks necessary to hit the `contract_repo_path` failure; I did not have one available for this attempt' - the attempt admittedly lacked the exact trigger condition." That is the same fact the grader read as a pass on every other run (the report's own honest admission of an environment gap). This is model grading variance on a genuinely borderline judgment call - whether an honestly-named environment deviation still counts as "the same configuration" - not a case where my rubric text changed between runs.

**Check rationale**

From `rubric.md`, the `behavior` check's pass condition: "The artifact shown is faithful to the report's own conclusion about the issue's exact trigger: either it reproduces the same failure signature the issue names (same error message/type, same exit code or crash mode, same observable symptom), or, when the report concludes it could not reproduce, the artifact documents a genuine attempt at the issue's same configuration/command sequence within the report's own recorded environment (any environment deviation from the issue is named, not quietly substituted - consistent with the `environment` check) and faithfully shows the failure did not occur. Fails when the artifact shows a different or adjacent failure narrated as a match, or when no artifact/vague description is given."

I revised this after the first full run: my original wording only allowed a pass when the artifact matched the issue's failure signature, with no path for a genuine cannot-reproduce to pass at all. That correctly caught every `wrong-target` package (a different failure narrated as a match), but it also meant `pkg-09` and `pkg-10` - both honest, evidenced cannot-reproduce reports that gold scores `accept` - could never pass `behavior`, no matter how good the report was. I rejected simply deleting the "must match" language, since that would let a `wrong-target` package's mismatched artifact slip through under a loosened rule; instead I added an explicit carve-out for a faithful cannot-reproduce, tied to the same environment-deviation language the `environment` check already uses, so the two checks stay consistent rather than re-litigating the same fact differently.

**Trade-offs**

Fixing the `comments` check's disclosure clause (to make `pkg-20`, the single-package `disclosure` category, correctly reject) meant adding: "treat every candidate comment as AI-assisted work by course convention... a comment fails this half unless it explicitly states that assistance." Applied too broadly, this risked flipping `pkg-03` - a `clear-accept` package whose repo policy reads "comments must be written by humans in their own words," which is an authenticity requirement, not a disclosure requirement — into an incorrect reject. I accepted the added complexity of writing the two policy types out separately in `references/evidence-guide.md` (an explicit disclosure requirement vs. a human-voice requirement) rather than one blanket rule, and verified the trade held: re-ran `pkg-20` (correctly rejects), `pkg-07` (a real disclosure-satisfying package, correctly still accepts), and `pkg-03` (correctly still accepts) together with `pkg-02`, `pkg-13`, and `pkg-19` as canaries from the other three categories — all five held their prior correct verdicts, so the fix was scoped to the one case it needed to change.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
