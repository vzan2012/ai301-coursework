# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62

**Verdict output**

Live-mode run, `codepath/pathreview-ai301-fa26-s3`, candidates: issues #73, #62, #37.

```
## Live-mode grading: codepath/pathreview-ai301-fa26-s3

#62 — Health check references `settings.redis_host`, which does not exist on Settings
- maintainer-alive: pass — last default-branch commit 2026-09-16 (3 days before capture), by a human (Aburke225).
- repo-used: pass — same commit, well within 180 days.
- scope-fits: pass — one bounded bug in one file (api/routes/health.py), concrete reproduction steps (GET /health -> 503, specific error), no design debate, no core-internals warning.
- unclaimed: pass — 0 assignees, 0 comments, no linked PRs.
- policy: pass — no AI-restriction in docs/CONTRIBUTING.md, silence passes.
Verdict: accept.

#73 — README and `.env.example` disagree about which LLM API key to set
- maintainer-alive: pass (same repo-level evidence). repo-used: pass. unclaimed: pass.
- scope-fits: pass — one coherent inconsistency between two named files, 1-2h estimate, no ambiguity.
- policy: pass.
Verdict: accept.

#37 — API reference doc is missing the `POST /profiles` request body schema
- maintainer-alive/repo-used/unclaimed/policy: pass, same reasoning as above.
- scope-fits: pass — one coherent docs task (add request-body schemas) with two named concrete sub-steps (POST /profiles, POST /reviews) for one feature, not an umbrella; 2-3h estimate.
Verdict: accept.

Ranked by fit (profile: comfortable in Python/JS, want to get better at reading unfamiliar code and working with existing tests/CI, prefer small bounded bug fixes or docs tasks):

1. #62 — top fit. It's an actual code bug (not just docs), small and self-contained, and docs/CONTRIBUTING.md names it directly as a seeded bug tied to a suppressed mypy finding (attr-defined) — fixing it means practicing exactly what the profile wants: touching a real file, removing a stale suppression, and running it through CI.
2. #73 — smallest, cleanest task (1-2h), pure docs/config, very low risk for a first PR.
3. #37 — good docs fit but the largest scope of the three (two endpoints, multipart-form nuance to get right).

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last default-branch commit 2026-09-16 by Aburke225 (human), 3 days before capture"},
      {"name": "repo-used", "grade": "pass", "evidence": "same commit, well within 180 days"},
      {"name": "scope-fits", "grade": "pass", "evidence": "one bug in api/routes/health.py with explicit repro steps (GET /health -> 503, AttributeError for redis_host)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "0 assignees, 0 comments, no linked PRs in the timeline"},
      {"name": "policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI-generated-code restriction"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last default-branch commit 2026-09-16 by Aburke225 (human), 3 days before capture"},
      {"name": "repo-used", "grade": "pass", "evidence": "same commit, well within 180 days"},
      {"name": "scope-fits", "grade": "pass", "evidence": "README.md vs .env.example disagreement on one variable, 'estimated effort: 1-2 hours'"},
      {"name": "unclaimed", "grade": "pass", "evidence": "0 assignees, 0 comments, no linked PRs in the timeline"},
      {"name": "policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI-generated-code restriction"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/37",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last default-branch commit 2026-09-16 by Aburke225 (human), 3 days before capture"},
      {"name": "repo-used", "grade": "pass", "evidence": "same commit, well within 180 days"},
      {"name": "scope-fits", "grade": "pass", "evidence": "one docs task (add request-body schemas) with two named sub-steps for POST /profiles and POST /reviews, 'estimated effort: 2-3 hours'"},
      {"name": "unclaimed", "grade": "pass", "evidence": "0 assignees, 0 comments, no linked PRs in the timeline"},
      {"name": "policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI-generated-code restriction"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

1. First full run, initial draft rubric: 12/20 agreement (bar: 18/20). Category
   floor unmet: `clear-accept` 0/8 — every issue was rejected because
   `maintainer-alive`'s evidence source was limited to the issue's own comment
   thread (most fresh issues have 0 comments, so the check could never pass),
   and `scope-fits` capped the pass condition at "three or fewer files," which
   penalized ordinary multi-file docs and bug work.
2. Second full run, after rewriting `maintainer-alive` to also read repo-wide
   signals (recent default-branch commits, the maintainer first-response
   sample) instead of only this issue's thread, and dropping the file-count cap
   from `scope-fits` in favor of judging boundedness directly: 13/20. `clear-accept`
   improved to 4/8, but `policy` and `scope` regressed (0/1 and 2/4) — loosening
   the checks let through a repo whose contributing docs ban AI-generated code
   outright, and an issue with two abandoned closed PRs behind a friendly
   label, because nothing in the rubric checked contribution policy at all.
3. Third (confirming) full run, after adding a `policy` check (outright
   AI-generated-code bans fail; conditions and silence pass) and sharpening
   `scope-fits` to name what actually indicates hidden difficulty (abandoned
   linked PRs, years of unresolved design debate, a vague one-line request
   hiding a product decision) versus what doesn't (a docs task touching
   several related files, several named instances of one defect): **18/20
   agreement (bar: 18/20: PASS)**, category floors all met — `claimed 4/4`,
   `clear-accept 6/8`, `dead-repo 3/3`, `policy 1/1`, `scope 4/4`. This is the
   run committed in `eval-run.txt`.

**Issue analysis**

`issue-09` (`conda/conda#7617`, "conda config clear option") — gold label:
**accept**. My rubric's result: **reject**, failed on `scope-fits`.

The issue is a small, well-scoped CLI feature (add a `--clear` option to
`conda config`) opened by a maintainer, with no current assignee and no open
linked PR. It does carry one **closed, unmerged** linked PR
(`conda/conda#11627`) and a stale 2022 exchange where a contributor
volunteered, the maintainer said "give it a try," and a bot later marked the
issue stale with no follow-through. My `scope-fits` pass condition treats
"the thread shows closed/unmerged abandoned linked PRs" as evidence the issue
is harder than it looks — wording written for a different case in the eval set
(`issue-15`, zulip's `#19589`, which shows years of real design debate and
repeated claim churn). Here it is just one stale, abandoned claim on an
otherwise trivial feature, not evidence of hidden difficulty. The check as
written can't distinguish those two situations, so it rejected a genuinely
good first issue.

**Check rationale**

The `scope-fits` row, quoted as currently written in `rubric.md`:

> Issue describes one coherent bug or feature, even if broken into several
> concrete related sub-steps (e.g., several files for one docs feature,
> multiple named instances of one defect, or several named causes/approaches
> for one bug), specific enough that a newcomer could implement it without
> making a real product/design decision themselves. Fails if: the issue is an
> explicit tracking/index issue bundling separate unrelated features or bugs
> meant to become their own issues, or asks for a codebase-wide/sweeping
> change; the thread shows closed/unmerged abandoned linked PRs or years of
> unresolved design debate; the request is a vague one-liner leaving a real
> product decision unresolved; a maintainer states it touches core internals;
> or it is a pure usage/support question.

It is written this broadly because the first version of this check only
counted files touched, which rejected ordinary bounded multi-file docs work
(`issue-01`) and multi-instance bug fixes (`issue-04`). Naming the actual
failure patterns from `references/evidence-guide.md` — real tracking issues,
abandoned attempts, unresolved product decisions, core-internals warnings —
instead of a file count fixed those false rejections.

**Trade-offs**

The clause "the thread shows closed/unmerged abandoned linked PRs or years of
unresolved design debate" is what the check gives up. It was written to catch
`issue-15` (years of real design debate plus two abandoned PRs), and does. But
it also changes the result on `issue-09`: one stale, abandoned claim from 2022
on an otherwise trivial feature is not the same signal as years of repeated
failed attempts, and the check can't yet tell the two apart. I left the
stricter wording in for the committed run rather than re-splitting the clause,
since the run still clears the 18/20 bar and every category floor — but a more
precise version would separate "one old unfollowed claim" from "a genuine
history of abandoned attempts."

---

## Selection rationale

**Selection rationale**

1. **Fit to interests and time available.** I'm comfortable in working with Python and JavaScript want to get better at understanding unfamiliar codebases and with an existing test suite and CI, rather than just writing docs. #62 is a small backend bug (one file, one wrong attribute name) - a realistic first-PR time commitment - that still forces me through the project's real workflow: locating the actual `Settings` field, fixing the issue, and dealing with whatever tests or checks are already connected to this bug.

2. **What the verdict caught vs. what I weighed myself.** The rubric confirmed the basics: the repo is active, the issue is one bug related to the file with repro steps, nobody's claimed it, and there's no policy against AI-assisted work. What it couldn't see is that `docs/CONTRIBUTING.md` names this exact bug as tied to a suppressed mypy finding, not just a bug that happens to look simple. That's a call I made myself; the rubric has no way to check it.

3. **Anticipated difficulty claiming it.** The core fix looks small: swap `settings.redis_host`/`settings.redis_port` for the existing `settings.redis_url` in the health probe. The real work is closing it out per the repo's own contribution rules: `docs/CONTRIBUTING.md` says fixing this bug also means removing its baseline mypy suppression, and likely a `strict=True` `xfail` marker on its unit test (the same pattern it describes for other seeded bugs), then getting all five CI jobs (`lint`, `typecheck`, `test-unit`, `test-integration`, `frontend`) green. I expect the fix itself to be quick; finding and removing the right suppression/marker without breaking the existing code.

---

Related paths: `eval-run.txt` in this directory; my skill's files in `tools/issue-select/`.
