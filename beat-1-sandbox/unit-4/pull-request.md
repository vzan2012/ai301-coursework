# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/125

**Branch**

`fix/62-redis-health-check`

**pr-precheck on my draft**

I ran my own tool in live mode on the draft twice before opening the PR. The first run said reject. Six checks passed and `repo-template-asks` failed: `docs/CONTRIBUTING.md` says every commit message must use one of the scopes `ingestion`, `rag`, `agent`, `safety`, `api`, `frontend`, and my commit was `fix(health): build Redis client from settings.redis_url (#62)`. `health` is not on that list. The code and the description were fine. I amended the commit to `fix(api): build Redis client from settings.redis_url (#62)`, force-pushed my fork branch (`b30a202` became `728494a`, same files and same code), and ran the tool again. The second run accepted, with all seven checks passing. I opened the PR after that.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke test, `--limit 3`, 2026-10-07: 3/3 (pkg-01, pkg-02, pkg-03). A partial run, so it says nothing about the bar.
2. Full run, 2026-10-07: 20/20 (bar 18/20, PASS), every category matched. This was the first full run. I did not use `--save-run` on it.
3. Confirming full run with `--save-run eval-run.txt`, 2026-10-10: 20/20 (bar 18/20, PASS), every category matched.

The last score matches the `agreement:` line in `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. I made no changes to any tool file between the runs, so there was nothing to re-grade with `--only`.

**Package analysis**

pkg-12 (sharkdp/bat#3845, the `--line-range` capacity overflow). The gold label is `reject`, and my rubric also said `reject`. The tool failed it on three checks and passed the other four. The per-check detail below is from my run 2, which used the same tool files and was saved with `--out`; run 3 agreed with gold on all 20 packages but I did not save its per-check output.

- `reviewable-diff` failed. The saturating clamp itself is right, but the diff carries `// TODO: ring buffer?` on the import, a commented-out `// eprintln!("DBG buffer_size = {}", buffer_size);`, a commented-out first attempt, an unused `fn _unused_buffer_probe`, and a hunk that only re-indents the `read_line` loop. My condition says it "Fails on any one debris line or hunk, however small", so any one of those holds it.
- `description-matches-diff` failed. The description says "regression test and CHANGELOG entry included", but the diff touches only `src/controller.rs` and `tests/integration_tests.rs`. There is no `CHANGELOG.md` hunk.
- `repo-template-asks` failed for the same reason from the other side. The repo-facts block says the contribution guide asks for a CHANGELOG.md entry, and the diff has none.

The gold note for pkg-12 lists the debris but does not mention the missing changelog. My tool got to `reject` by the debris route as well, so it agrees on the verdict, and it found one more true reason on top. I read it this way because the tool grades what the diff contains and not what the description says it contains. The fix is correct and the evidence is decisive (`diff-within-plan`, `decisive-test-evidence` and `repo-checks-run` all passed), and the package still isn't ready to submit.

**Check rationale**

> | `reviewable-diff` | The unified diff, line by line, and the commit list as a pointer to where scratch work may hide | No line in the diff is debris: debug output left in (including commented out), commented-out code, an unused function or import, a TODO or scratch note, or a hunk that changes no behavior and is not a planned deliverable (re-indent, reordered imports, re-printed identical lines). A comment that explains why the new code is written the way it is does not count as debris. Fails on any one debris line or hunk, however small | required |

I settled this wording before the first run, and I have not revised it since. The choice I made was strict versus loose. I could have written the check to fail only churn that "buries the change". I rejected that because "buries" is a judgment call that two graders would split on, and the check's own rule is that it judges the thing and not the write-up. A list of named debris types is something someone else can apply and get my answer. I made the choice with Claude's help, and I went with its recommendation after it explained the trade-off. The one carve-out, that a comment explaining why the code is written that way is not debris, is there so a good explanatory comment (like the one in pkg-04's diff) does not get a PR rejected. The commit list is only a pointer. A commit named "wip" tells the tool where to look; the finding is always a line in the diff.

**Trade-offs**

The check gives up leniency. A PR that is right in every other way is held for one leftover `TODO`, and there is no weighting. I accept that because a stray line in a PR is the reviewer's cost, and the fix is a one-minute cleanup before opening. The package where this matters most is pkg-18 (gold-arguable, debris that is mechanical churn around a correct fix): the strict rule held it, and that matched gold. I did not re-run anything with `--only` and did not add canaries, because I never loosened a check. Runs 2 and 3 were both 20/20 on the same tool files, so nothing changed, and that is how I know. One case it will miss is a diff whose debris sits outside the hunks the package shows, since the tool can only grade the diff text it is given.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
