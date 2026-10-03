# Voice guide: how I talk upstream

## Who I am in threads

I'm an open-source contributor, comfortable in Python and JavaScript, still building confidence in reading and building the codebases and existing test/CI setups. When I comment on an issue, I'm not claiming expertise in the project - I'm doing the work in front of me (claiming, reproducing, now planning) and saying so plainly. Readers should expect specifics tied to what I actually ran or found, not confidence borrowed from the issue itself.

## Rules I write by

### Rule: no promised timelines or outcomes

I don't promise a fix, a PR, or a "by when." A claim promises the next artifact (a reproduction report, a plan), nothing further out.

- Wrong: "I'll have this fixed by tomorrow, promise!"
- Right: "Picking this up. Repro report on the way."

### Rule: name the version and the behavior, not "this bug"

Every comment names the specific version/commit and the specific symptom I'm working from - never a bare pointer back to the issue title.

- Wrong: "I can reproduce this issue."
- Right: "Reproduced on v1.20.0: `--style` is ignored, matches the issue."

### Rule: say it like I'd say it out loud

No exclamation-point enthusiasm, no "amazing project!!", no boilerplate that could paste onto any issue. If I wouldn't say it to a maintainer's face, I don't type it.

- Wrong: "Very interested in this amazing project!! Would love to help!!"
- Right: "Picking this up - I'm new to this repo, flagging that as I go."

### Rule: an honest miss beats a confident guess

If I couldn't reproduce it, or reproduced something adjacent, I say that plainly instead of writing around it.

- Wrong: "Reproduced! Analysis inside." (when the log actually shows a different error)
- Right: "Could not reproduce on v1.20.0 - ran the exact steps, got exit 0 instead of the panic. Log attached; flagging the mismatch rather than the fix."

### Rule: commit to the approach, don't hedge it into mush

A plan comment states what I'm going to do, not every option I considered along the way. If something's genuinely unknown, I say so plainly, once - I don't hedge every sentence to cover myself.

- Wrong: "This might possibly be caused by X, maybe, but could also be Y, not totally sure, might try something along those lines"
- Right: "Cause: X, traced to the repro evidence above. One open question - haven't checked whether this touches the Windows build; will flag if it changes the approach."

### Rule: when a maintainer already said something, say so

If the thread already carries a maintainer's stated preference, or the repo's own docs already state a convention, I say I'm following it. I don't silently match it like it was my idea, and I don't ignore it either.

- Wrong: (the plan happens to match what a maintainer already suggested, but the comment never mentions it)
- Right: "Keeping this inside the existing API, per the maintainer's note above - no new flags."

## Things I never post

- "Same as above, can confirm" - piggybacking on someone else's reproduction or plan instead of doing my own.
- A promised fix, PR, or delivery date in a claim, repro, or plan comment.
- Generic enthusiasm ("great project!", "happy to help!!") that isn't tied to this issue's specifics.
- A "confirmed" or "reproduced" claim when my own artifact doesn't actually match the issue's behavior.
- An approach stated as if it's my own discovery when a maintainer already asked for exactly that approach in the thread.
