# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
| --- | --- | --- | --- |
| `diagnosis-grounded` | The plan's stated cause, read against every fact in the repro-evidence block (the steps, the timings/outputs, what happened under which conditions) | The stated cause explains _everything_ the repro evidence actually shows, not just part of it, and is not borrowed from an unverified thread theory the repro evidence never confirms. Fails if the repro evidence contains a data point the stated cause cannot account for, or contradicts | required |
| `bounded-scope` | The plan's scope statement (or its changes/approach section, if there's no separate scope heading) | Names the specific file(s) or area it will touch, and says at least one thing it deliberately won't touch. Fails on an unbounded "while I'm here" addition, or if there's no scope statement to find at all | required |
| `executable-by-a-stranger` | The plan's approach/changes section | Someone who's never seen the issue could start making the change from what's written, without asking the author a question first. Fails if the plan only names an outcome ("make it fast again") with no concrete file or step | required |
| `test-decisive` | The plan's test plan, read side by side with the repro-evidence block's own steps | The test plan re-runs, or directly maps onto, the repro evidence's steps, and states a specific expected result after the fix - specific enough that a stranger could tell pass from fail just by reading it. Fails on "add tests" or "verify it works" with nothing a stranger could check | required |
| `comment-thread-and-convention-aware` | The candidate plan comment, read against the thread highlights and the repo-facts block | When the thread or repo-facts block contains a real signal (a maintainer's stated preference, a contribution-policy note, an AI-disclosure requirement), the comment responds to it rather than reading like boilerplate that could paste onto any issue. Fails if it ignores a signal that was there to engage, or promises something the plan itself doesn't contain | required |
| `honesty` | The plan's stated risks/unknowns, read against how certain the rest of the plan sounds | Doesn't claim certainty the gathered evidence doesn't support. A plan that names a real unknown passes exactly like one with nothing left to flag - this check never holds a plan just for lacking a risks section | preferred |

## Verdict rule

Accept if every `required` check passes. Any `required` check graded `fail` or `unclear` holds the plan (reject) - `unclear` is never free. `honesty` is `preferred`: report it, but it never flips the verdict either way. When the verdict is reject, name every failing required check, not just the first one.
