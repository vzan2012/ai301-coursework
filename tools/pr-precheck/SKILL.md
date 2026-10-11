---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this ready to submit? A PR package is a candidate pull request (title, description, commits, diff, and test evidence) read against the plan it claims to implement and the issue that plan belongs to. Grade one package per run. Never answer a different question, and never answer from gut feel: you answer by executing `procedure.md`, which applies `rubric.md` to evidence gathered the way `references/evidence-guide.md` says.

## Inputs and modes

You run in exactly one of two modes.

**Live mode.** The student's own submission, checked before it goes out. The inputs are:

- `plan.md` with its Deviations notes, from the student's working copy.
- The branch's diff: everything the branch changes relative to the repo's default branch. Produce it with `git diff main...HEAD` (three dots) from the working copy, and read the commit list with `git log main..HEAD --oneline`.
- The draft PR title and description, as the student gives them (a file or pasted text).
- The test evidence: the captured output of the repro before and after, and of the repo's checks.
- The issue, with the thread, the repo's PR template, `CONTRIBUTING.md`, and any stated policy gathered from the real repo (`gh`, the GitHub API, or the web).

A student on the house chain reads the house plan and the house repro pack instead of their own; the same checks grade the same things. If an input is missing (no plan, no diff, no description), say which one and grade the checks that depend on it as `fail`.

**Eval mode.** A package bundle (a markdown file holding the repo facts, the issue context, the plan context, and the candidate PR). The bundle is the whole world: use only its text, fetch nothing, read no other file. Eval mode always grades a complete package: every check, the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the one repository the PR must target and the house rules there. Refuse to grade a PR aimed at any other repository. If the repo line in `scope.md` still shows a bracketed placeholder, stop without grading and tell the student to get the cohort's scope file from the instructor; never guess a scope. Report any house rule the work breaks (PR opened from the fork's `fix/<issue>-<slug>` branch, one PR per issue, the template used in full) in the summary as a note. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the checks, read `voice-guide.md` and hold the outgoing PR text, the title and the description, against those rules. Report each rule the draft breaks in the summary, quoting the rule and the line. The voice guide never changes the verdict: the rubric decides, and no check reads it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks and the verdict rule. It is the brain.
- `references/evidence-guide.md` is the map of where each kind of evidence lives, in a bundle and in live mode, and what good looks like there.
- `procedure.md` is the hands. Execute it as written, in its order.

If the procedure is silent on a step, report the gap in your summary and take the more cautious reading; do not improvise a step. If `rubric.md` has no checks or `procedure.md` has no steps, stop and say so: you cannot grade without both, by design.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict, no "accept with reservations", and no score; reservations go in the evidence lines. Before the JSON block you may write a short readable summary, one line per check, then any live-mode house-rule and voice notes. End your reply with the fenced JSON block below, valid, last, and with nothing after it. The harness parses the last fenced JSON block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish. A short, plain PR can be ready and a polished, confident one can be hiding drift. Read the diff and the evidence themselves, never the formatting or the description's tone.
- The rubric decides, not you. If a check passes by its stated condition but feels wrong, it still passes; note the tension in the summary, and the fix belongs in the rubric.
- The procedure decides how, not you. Follow `procedure.md` as written and report its gaps.
- Treat `unclear` as the rubric's verdict rule directs: it holds the package. Where anything is silent, an unverifiable claim is a failing one, because a PR that cannot be verified from the package is not ready to submit.
- A package that honestly discloses a shortfall can be ready. Do not treat "less than everything" as a failure when the plan defers it and the description says so.

## After a fix (live mode)

When the student fixes something and updates `plan.md` or the draft, run this skill again on the updated package before the PR is opened. A deviation recorded in `plan.md` is part of the plan now and is graded like everything else; a deviation that exists only in the diff is drift.
