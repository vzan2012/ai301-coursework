# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

This file is Assignment 6, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. It has been waiting here since you created the repo in unit 1;
unit 9 is when you write it.

**Only this page is read.** Your `tools/` folders and your installed skill folders are never
opened, so every piece of evidence you want seen has to live inline here: an output excerpt,
a before/after pair, a dated commit reference.

**Write every sentence yourself.** AI may gather diffs, dates and excerpts for you; it does
not draft entries.

**Keep every section header below under its exact name** — Featured Tools, Inventory, The
Arc, and Limitations and What's Next. Feature three or four tools, each in the entry shape
shown below; give every other tool one inventory line.

**Some lines below are labelled, and the label is how they are read.** Three places on this
page use a `Label: your words` shape — the arc's beats, the tool-limits list, and nothing
else. Keep those labels exactly as written, one per line, and never delete a labelled line:
where you have nothing to put after a label, write `none` and move on. A deleted label reads
as a deleted answer.

**Student:** Deepak Guptha Sitharaman

## Featured Tools

I featured the scout because it checked that #591 was free (0 assignees, 0 linked or open PRs, 0 comments), but it used a rule that only looked at assignees and open PRs, so I made it fail an issue if someone who isn't the maintainer said they were working on it in the last 60 days.

I featured voice-guard because it checked my words against my own voice rules before I posted my claim on #591.

I featured these because they caught a mistake in my own PR when I forgot to update plan.md after I changed what the focus test covers.

**issue-scout**

**What it is, and the judgment hole I wrote:** issue-scout ranks issues from a big search and checks the top ones against a rubric. The part that is mine is the rules I added later, such as the 60-day claim rule.

**How it evolved, dated:**

- On Oct 15, the scout passed two issues that strangers had already applied to, so I changed unclaimed to also look at claim comments from the last 60 days.
- On Oct 28, I claimed #591 and re-graded it, and the scout rejected my own issue so I changed unclaimed so it ignores my own claim on an issue I already claimed.
- On Oct 28, the old scope-fits check would have rejected most real issues for having one open edge, so I replaced it with scope-feasible, which fails only when there is no single piece of work.

**Evidence:** Before the own-claim fix, the scout rejected my own issue #591 on unclaimed alone. After it, the same issue was accepted again.

**voice-guard**

**What it is, and the judgment hole I wrote:** voice-guard reads my draft and checks it against my voice guide before it goes out. The part that is mine is the upstream rules I added for claims, questions and pings, each with a wrong and a right example.

**How it evolved, dated:**

- On Oct 28, my first post was stopped because the guard couldn't find my draft file, so I gave the full path in the --body-file argument.
- On my Windows computer, the hook command used python3, which my computer doesn't recognize, so I swapped python3 for python in the hook.

**Evidence:** My guard log shows the hook stopped my first post at 01:23 am on Oct 28 because it couldn't read the draft file, and then I posted the claim: https://github.com/Babali42/DrumBeatRepo/issues/591#issuecomment-6461382811

```
2026-10-28 01:12 [manual] pass | target: claim-draft.md
2026-10-28 01:23 [hook] stop: could not read the body file: [Errno 2] No such file or directory: (path left out)
2026-10-28 01:23 [hook] pass | target: gh issue comment 591 --repo
```

**pr-precheck + ship-gate**

**What it is, and the judgment hole I wrote:** ship-gate checks my branch and my PR draft together before the PR goes out. The part that is mine is my manifest, the 2 checks I chose (npm run test-ci and npm run lint).

**How it evolved, dated:**

- On Oct 31, my manifest ran npm run test-ci and npm run lint, with the PATH set to a newer portable Node because my system Node was too old. It left out Vitest, Scala and i18n because Vitest already fails on main on Windows, and Scala and i18n need sbt and jq, which I don't have.
- On Nov 1, my pr-precheck rejected my PR because my plan and my PR said different things about the focus test, so I wrote the deviations into plan.md, and the gate passed at 21:50 hrs.

**Evidence:** My gate log shows a BLOCK, because diff-within-plan failed: my plan didn't say the focus test covers only the mobile button at 20:44 on Nov 1, and then a PASS, after I wrote the deviations into plan.md at 21:50.

```
2026-11-01 20:44 [manual]
- verdict: BLOCK (block)
  - diff-within-plan [fail]: Plan test 3 (focus ring on the play button) is delivered for the mobile button only, and plan.md's Deviations section doesn't record it ...

2026-11-01 21:50 [manual]
- verdict: PASS
  - diff-within-plan [pass]: All 4 changed files are in the plan (utils.scss mixin, modal.base.scss, sequencer scss, spec); ...
```

## Inventory

- repro-check: it checks that my repro is real and clear, and it accepted my #591 repro, with small notes.
- plan-check: it checks that my plan can be tested, and it rejected my #591 plan twice before it accepted it.
- pr-monitor: it reads my PR's live state and drafts my next move to a file, and I edited the draft before I posted it.

## The Arc

**Beat 1**
Date: Oct 15, 2026
Friction: When I checked the scout's top results on GitHub, I saw that two of its issues already had strangers saying they applied, because it only looked at assignees and open PRs.
Change: So I made unclaimed fail on a claim comment from someone who isn't the maintainer, and I picked 60 days because an old claim should not block an issue forever.
Result: Afterwards I re-ran the three canaries and all three matched the gold answers, but the fix is only tested on the canaries, not on the scan.

**Beat 2**
Date: Oct 28, 2026
Friction: When I re-graded #591 after I claimed it, the scout failed the issue I was already working on, because it saw my own claim comment as someone else's claim.
Change: So I changed unclaimed to ignore my own claim on an issue I already claimed.
Result: Afterwards, the same issue #591 was accepted again, and the scout's difficulty for it was "moderate".

**Beat 3**
Date: Oct 28, 2026
Friction: When I tried to post my claim on #591, the guard blocked my post, because the --body-file path I gave it was not a full path.
Change: So I gave the full path in the --body-file argument.
Result: Afterwards, the guard passed at 01:23 and my claim was posted on #591.

**Beat 4**
Date: Nov 1, 2026
Friction: When I ran the gate on my PR draft, it blocked my PR because the focus ring test covers only the mobile button, and plan.md didn't say so.
Change: So I wrote the changes into the plan.md file.
Result: Afterwards, the maintainer merged it on Nov 2.

**Beat 5**
Date: Nov 2, 2026
Friction: When I ran pr-monitor on PR #625, it read the PR as already merged, so its thresholds never fired.
Change: So I rewrote the draft before I posted it.
Result: Afterwards, I posted the thank-you, and I opened follow-up issue #628.

## Limitations and What's Next

pr-monitor's thresholds never fired, so I don't know if they work on a stalled PR. Vitest still fails on Windows, so my manifest leaves it out. Next, I will work on follow-up issue #628 and see how the toolkit handles it.

**Tool limits.** One line per featured tool, in the same order as the entries above, naming
where that tool still stops or what it still gets wrong. A small honest limit is full credit;
"none really" is not a limit. If a tool genuinely has none you can name, write `none` after
its label rather than deleting the line.

issue-scout: I still can only see claims written in comments, not people who say they're working on it somewhere else.
voice-guard: It still needs the python3 to python swap by hand on Windows, and the setup template still says python3.
pr-precheck + ship-gate: It can catch a plan and PR mismatch, but it can't tell me if my fix is good.
