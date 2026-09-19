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
| `scope-fits`       | Issue body, comment thread, and the state (open/closed/merged) of any linked PRs (`references/evidence-guide.md`) | Issue describes one coherent bug or feature, even if broken into several concrete related sub-steps (e.g., several files for one docs feature, multiple named instances of one defect, or several named causes/approaches for one bug), specific enough that a newcomer could implement it without making a real product/design decision themselves. Fails if: the issue is an explicit tracking/index issue bundling separate unrelated features or bugs meant to become their own issues, or asks for a codebase-wide/sweeping change; the thread shows closed/unmerged abandoned linked PRs or years of unresolved design debate; the request is a vague one-liner leaving a real product decision unresolved; a maintainer states it touches core internals; or it is a pure usage/support question | required    |
| `unclaimed`        | Issue sidebar assignees and open linked PRs (`references/evidence-guide.md`)                                | 0 assignees and 0 open linked PRs attempting to solve the issue                                    | required    |
| `policy`           | Repo-facts "contribution policy" line and any dedicated AI-policy docs it names (`references/evidence-guide.md`) | The contribution policy does not state an outright ban on AI-generated code or documentation. Conditions (disclosure, testing, personal-understanding requirements) are not bans and pass. Silence (no stated policy) passes | required    |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if every `required` check passes. Reject if any `required` check fails. `preferred` checks never affect the verdict; they only rank accepted issues. Any `unclear` or missing evidence counts as a `fail` for that specific check.
