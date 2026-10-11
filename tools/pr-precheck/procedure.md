# Procedure: how this tool grades a PR package

## Read order

Read in this order, and write down the listed facts from each part before moving on. The plan and the diff come before the description so that the description's claims are tested against what the diff does, not used to explain it.

1. Read the repo-facts block (live: the PR template and CONTRIBUTING.md). Write down every ask it states: template sections, checklist, issue link, changelog or whatsnew entry, title format, tests, and any AI-use disclosure rule (what it requires: that AI was used, the tool, the extent).
2. Read the issue: title, body, and thread highlights. Write down the symptom in one line.
3. Read the plan-context block in full (live: `plan.md`, including its Deviations section). Write down: the Files list; the scope sentence; the "not in scope" list; every deliverable promised (code, tests, docs, changelog); any deferral or deviation note and what it covers; and the test plan's scenarios, each with its expected-after result.
4. Read the diff (live: `git diff main...HEAD`) before the description. For every changed file and every hunk, write one line: what it does, and which plan item (or deviation note) it serves, or "none". Also write down every debris line (debug output, commented-out code, unused function or import, TODO, hunks that change no behavior). Note the commit list as pointers only.
5. Read the test-evidence section. Write down each scenario it shows, with the output before and after, and each repo check it names, with the outcome.
6. Read the title and description last. Write down every concrete claim (what changed, "exactly as planned", "no other changes", docs updated, tests added, changelog entry, checks run) and any shortfall or deferral it states.

## Evidence gathering

- `diff-within-plan`: take the hunk list from step 4 and the plan facts from step 3. Mark each hunk "in plan", "in deviation note", or "outside". Mark each promised deliverable "in diff", "absent, noted in deferral", or "absent, no note". Behavior-less hunks are set aside for `reviewable-diff`.
- `description-matches-diff`: take the claim list from step 6 and look each one up in the diff and evidence. Mark each "present", "absent", or "contradicted". Check whether the plan defers part of the issue; if it does, find where the description says so.
- `decisive-test-evidence`: set the test-plan scenarios from step 3 next to the scenarios from step 5. For each, mark whether it is the plan's repro (not a control), on the changed path, with before and after visible, and whether it matches the expected-after. Count a new test in the diff only for the behavior it asserts. Skip anything the plan defers.
- `repo-checks-run`: take the checks the plan's test plan and the repo-facts block name. For each, record the command, whether an outcome is visible, or whether the package says it was not run and why.
- `reviewable-diff`: take the debris list from step 4. Count it; the lines go in the evidence.
- `repo-template-asks`: go through the step 1 list of asks one at a time, and find where each is met in the title, description, or diff (a changelog ask is met only by a changelog hunk in the diff). Record each as met, empty, or absent. Look at each ticked checklist item for something behind it.
- `ai-disclosure`: take the step 1 disclosure rule. Search the description for a sentence saying AI was used; record whether it names the tool and the extent. If no rule exists in eval mode, record "no policy". In live mode the course's requirement counts as the rule.

## Check execution

1. Grade the checks in the rubric's table order: `diff-within-plan`, `description-matches-diff`, `decisive-test-evidence`, `repo-checks-run`, `reviewable-diff`, `repo-template-asks`, `ai-disclosure`.
2. Grade each check from the evidence gathered for it. Do not re-read the whole package for each one, except to settle an `unclear`.
3. If the gathering found nothing for a check (no test evidence section, no description claims to test, no disclosure sentence where one is asked for), grade it `fail`. `unclear` is for evidence that exists and is truly ambiguous (a hunk whose purpose cannot be told from the plan, a diff cut off before the end).
4. A check passes only when its pass condition is met as written. If it feels wrong but the condition is met, it passes; say so in the summary and leave the rubric alone.
5. Write one line per check: its name, its grade, and the specific fact or quote that decided it. "Looks fine" is never a reason.
6. Procedure gaps: if a step above does not tell you what to do, say so in the summary and apply the more cautious reading. Do not invent a new step.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if all seven checks pass.
2. `fail` and `unclear` both count as not passing, so the verdict is reject if any check is either.
3. In the readable summary, name every check that failed or came back unclear.
4. In the JSON block, put each check's one-line fact or quote in its `evidence` field. The deciding check is the first failing check in table order; quote the fact itself (the hunk, the claim, the missing line), not a restatement of the check's name.
5. Live mode only: after the checks, report house-rule notes from `scope.md` (fork branch, one PR per issue) and any voice-guide rule the title or description breaks, quoting the rule. These notes never change the verdict.
