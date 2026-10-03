# Procedure: how this skill grades a plan package

## Read order

1. Read the issue first: title, body, and thread highlights. This is the question the plan is supposed to answer.
2. Read the repro-evidence block in full, before looking at the plan at all. Write down, in your own words, exactly what was reproduced - what was run, what happened, what was expected, and under which conditions things differed. This fact pattern is what every later check measures the plan against.
3. Read the repo-facts block: the bug-report template, the contribution policy, and any stated AI-use disclosure requirement. Note anything a plan comment should be responding to.
4. Only then read the candidate plan in full, then the candidate plan comment. Reading the evidence before the plan keeps you from grading the plan against its own claims instead of against what actually happened.

## Evidence gathering

- `diagnosis-grounded`: pull the plan's stated cause (usually under a Diagnosis or Cause heading). Pull every concrete fact noted from the repro-evidence block in step 2. Write down whether the stated cause accounts for all of those facts, or only some of them, or is contradicted by one.
- `bounded-scope`: pull the plan's scope statement, or its changes section if there's no separate scope heading. Write down the file(s) or area named, and anything explicitly excluded.
- `executable-by-a-stranger`: pull the plan's approach/changes section. Write down whether it names concrete files and concrete steps, or only describes an outcome.
- `test-decisive`: pull the plan's test plan section and the repro-evidence block's steps side by side. Write down whether the test plan's steps and stated expected result actually map onto the repro steps, or describe something else entirely.
- `comment-thread-and-convention-aware`: pull the thread highlights and repo-facts block first, then the plan comment. Write down any real signal present (a maintainer's stated preference, a contribution-policy note, an AI-disclosure requirement) and whether the comment responds to it.
- `honesty`: pull any risks or unknowns the plan states, and compare that against how certain the rest of the plan sounds. Write down whether anything stated as certain isn't actually backed by the diagnosis evidence gathered above.

## Check execution

1. Grade the checks in the table's order: `diagnosis-grounded`, `bounded-scope`, `executable-by-a-stranger`, `test-decisive`, `comment-thread-and-convention-aware`, `honesty`.
2. Grade each check using only the evidence already gathered for it in the previous stage - don't re-read the whole package for every check.
3. If the evidence-gathering step found nothing for a check (for example, the plan has no scope statement anywhere), grade that check `fail`, not `unclear`. `unclear` is reserved for evidence that exists but is genuinely ambiguous, not for evidence that's simply missing.
4. If a check's gathered evidence is ambiguous (for example, the plan names a file but it's unclear whether it's the only one touched), grade `unclear`.
5. For each check, write one line: its name, its grade, and the specific fact or quote that decided it. "Looks fine" is never a reason.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passed.
2. Any required check graded `fail` or `unclear` holds the plan - reject.
3. `honesty` is preferred and is reported but never changes the verdict.
4. If the verdict is reject, name every required check that failed or came back unclear, not just the first one.
5. Quote the exact fact or line from that check's evidence - not a restatement of the check's name - so the verdict traces back to something actually in the package.
