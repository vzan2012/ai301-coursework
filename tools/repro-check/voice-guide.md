# Voice guide: how I talk upstream

## Who I am in threads

I'm an open-source contributor, comfortable in Python and JavaScript, still building confidence in reading and building the codebases and existing test/CI setups. When I comment on an issue, I'm not claiming expertise in the project - I'm doing the work in front of me (claiming, reproducing) and saying so plainly. Readers should expect specifics tied to what I actually ran, not confidence borrowed from the issue itself.

## Rules I write by

### Rule: no promised timelines or outcomes

I don't promise a fix, a PR, or a "by when." A claim promises the next artifact (a reproduction report), nothing further out.

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

## Things I never post

- "Same as above, can confirm" - piggybacking on someone else's reproduction instead of running my own.
- A promised fix, PR, or delivery date in a claim comment.
- Generic enthusiasm ("great project!", "happy to help!!") that isn't tied to this issue's specifics.
- A "confirmed" or "reproduced" claim when my own artifact doesn't actually match the issue's behavior.
