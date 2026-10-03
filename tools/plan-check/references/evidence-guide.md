# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: the candidate plan's opening section, usually under a heading like "Diagnosis" or "Cause" (calib-03 uses "Diagnosis"; calib-01 just opens with "Cause:"). Check it against every concrete fact in the repro-evidence block above it - the steps, the timings/outputs, and anything it says happened differently under different conditions. Live mode: the student's own plan.md diagnosis section, checked against their posted repro comment on the issue (or, on a house issue, the repro pack as quoted in the drafts).

What good looks like: a cause that accounts for *every* data point the repro evidence shows, including the inconvenient ones. calib-03 is the cautionary example: the plan blames a pager key-binding bug, but its own repro evidence shows the slowdown persists even with the pager fully disabled (`--paging=never`) - a fact that diagnosis can't explain, because it was lifted from a thread commenter's unverified theory rather than from the evidence. calib-01's diagnosis is the good example: it names exactly the behavior the repro steps show (color doesn't update until the view is re-entered) and nothing more.

## Scope

Where it lives: usually a short "Scope" or in-scope/not-in-scope statement right after the diagnosis; some plans fold it into the changes section instead of giving it its own heading. Live mode: plan.md's scope section.

What good looks like: names the specific file(s) or area it will touch, and names at least one thing it deliberately won't touch. Worth knowing: calib-03's plan actually scopes itself well on paper ("in scope: `src/output.rs`; not in scope: the highlighting pipeline") - which is exactly why this check can't catch a wrong-cause plan on its own. A narrow scope drawn around the wrong fix still passes `bounded-scope`; that's `diagnosis-grounded`'s job, not this one's.

## Executability

Where it lives: the candidate plan's changes/approach section. Live mode: plan.md's approach/files section.

What good looks like: concrete files and concrete steps, specific enough that someone who's never seen the issue could start without asking a question. calib-01 is the positive example - "trigger a refresh of the commits context... in the push completion callback in `pkg/gui/controllers/sync_controller.go`" names the file and the exact change. "Fix the refresh bug" would not pass; it names an outcome, not a step.

## Test plan

Where it lives: the candidate plan's test plan section, read side by side with the repro-evidence block's own steps. Live mode: plan.md's test plan, read against the student's own posted week-2 repro steps.

What good looks like: the test plan re-runs, or directly extends, the exact repro steps and states a specific expected result afterward. calib-01 does this well - it re-runs its four repro steps and adds two direct extensions (the main commits panel, a force push) that share the same code path. calib-03's test plan fails here even though a test exists: its binding-registration smoke test never re-checks the actual reported symptom (the slow `shift+G` seek), because it was built to confirm the wrong diagnosis instead of the real one.

## Honesty

Where it lives: any risks or unknowns the candidate plan states, read against how certain its diagnosis and changes sections sound. Live mode: plan.md's risks/unknowns section, and later its Deviations section once filled in after the build.

What good looks like: claims no more than the gathered evidence supports. A plan that names a real unknown ("haven't checked whether this touches the Windows build") passes exactly as well as a plan with nothing left to flag - calib-01 has no separate risks section at all and that's fine, because nothing in it overclaims either. What fails this check is a plan stating something as settled fact when the diagnosis evidence only ever showed it was plausible.

## Comms

Where it lives: the candidate plan comment, read against the thread highlights and the repo-facts block's stated bug-report template, contribution policy, and any AI-use disclosure requirement. Live mode: the student's draft comment.md, read against the live issue thread and the repo's actual CONTRIBUTING.md or issue template.

What good looks like: the comment responds to something actually present rather than reading like it could paste onto any issue. calib-01's comment is the positive case - it explicitly ties its minimal scope to the repo-facts block's stated review-bandwidth note ("keeping it minimal given the review-bandwidth note in CONTRIBUTING"). Thread-aware doesn't require a thread comment to exist; calib-01 has zero thread comments, and the comment still passes by engaging the repo-facts signal instead. What fails this is ignoring a signal that was there to catch, or a comment that promises something (a timeline, a guaranteed PR) the plan itself doesn't contain. Disclosure is a separate, narrower question from tone: if the repo-facts block states an AI-use disclosure requirement, the comment must disclose somewhere in its text to pass that half, regardless of how thread-aware it otherwise reads.
