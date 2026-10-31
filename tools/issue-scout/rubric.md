# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check              | Evidence                                                                                                    | Pass condition                                                                                     | Weight      |
| ------------------ | ----------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- | ----------- |
| `maintainer-alive` | Repo-facts "last 5 default-branch commits" (dates, non-bot authors) and "maintainer first-response sample," plus any Owner/Member/Collaborator comment or review in this issue's or a linked PR's thread (`references/evidence-guide.md`) | At least 1 of: a non-bot commit on the default branch within the last 45 days, or a maintainer/owner/collaborator comment or review (in the first-response sample or this issue's thread) within the last 60 days | required    |
| `repo-used`        | Committed dates in repo facts or default-branch commit history (`references/evidence-guide.md`)             | At least 1 commit on the default branch within the last 180 days                                   | required    |
| `scope-feasible`   | The issue body: what work it asks for | The issue describes one piece of work I could start on: a behavior, a gap, or a task I can point at. It fails only when there's no single piece of work in it: a sweeping "improve X everywhere", a checklist across the codebase, or a container issue tracking many sub-tasks. An open design question, an "etc.", or several files involved doesn't fail it; they go in the difficulty note instead | required    |
| `unclaimed`        | Issue sidebar assignees and open linked PRs, plus the thread's claim comments with their dates (`references/evidence-guide.md`) | 0 assignees and 0 open linked PRs attempting to solve the issue, and no comment in the last 60 days from a non-maintainer saying they are working on it, will take it, or have applied to work on it (an application through a program counts). Older claims do not count. Anything from me, the person running the grade (my own comments, or me as the assignee), doesn't count, so an issue I've already claimed doesn't fail against myself | required    |
| `policy`           | Repo-facts "contribution policy" line and any dedicated AI-policy docs it names (`references/evidence-guide.md`) | The contribution policy does not state an outright ban on AI-generated code or documentation. Conditions (disclosure, testing, personal-understanding requirements) are not bans and pass. Silence (no stated policy) passes | required    |

## Difficulty note

For each accepted issue, the read-out adds one line:

- **straightforward**: the issue says where the change lives, and the change is small.
- **moderate**: I'd have to find the location myself, or several files are involved.
- **ambitious**: there's an open design question or an unfamiliar part of the code. Name it, because I'd need to ask the maintainer about it in my claim.

The note helps me choose. It never changes accept or reject, and it isn't part of the JSON. In eval mode it is skipped.

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if every `required` check passes. Reject if any `required` check fails. `preferred` checks never affect the verdict; they only rank accepted issues. Any `unclear` or missing evidence counts as a `fail` for that specific check.
