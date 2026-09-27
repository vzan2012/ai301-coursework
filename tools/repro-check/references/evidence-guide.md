# Evidence guide: where the proof lives in a reproduction package

## Environment

Where it lives: the `Environment:` line (or equivalent block) at the top of the repro report - in an eval bundle, the `repro_report` field/section; live, the student's draft repro comment. Compare it against the issue's own stated environment (its body, or a version/commit named in the thread) and against the repo-facts block's stated "latest release"/target commit.

What good looks like: names a specific tool or library version (or commit hash), the OS/platform, and how it was built if relevant - and either that version matches what the issue targets, or the report says explicitly why it's different (e.g. "tested against the version named in the issue, not latest"). "Reproduced locally" with no version is not sufficient.

## Steps

Where it lives: the repro report's steps section - in an eval bundle, the `repro_report` field; live, the draft comment. Read alongside the issue body for the exact trigger (command, input, or user action) it describes.

What makes them followable: each step is a concrete command or action, in order, starting from a named starting state (a fresh clone, a named commit/tag, a built binary) - not "set up the project" or other steps that assume prior undocumented context. The steps end in the precise action that triggers the issue's behavior, and the report states what was expected versus what happened.

## Behavior shown

Where it lives: the output excerpt, log, or screenshot embedded in the repro report, read against the issue's own description of the failure (error text, exit code, stack trace, or observed symptom) - in an eval bundle, both live in their respective fields/sections.

What it means to match: the artifact shows the same failure signature as the issue - the same error message or type, the same exit/crash behavior, the same observable symptom - not merely an error in the same file or feature area. A different error, a different code path, or a symptom that "sounds similar" but doesn't match the specifics is a miss, not a match.

A genuine cannot-reproduce also counts as showing behavior faithfully: if the report attempts the issue's exact configuration/command sequence and the artifact shows that the failure did not occur, that is evidence, not a gap - as long as the attempt is real (actually run, environment recorded) and any environment deviation from the issue is named rather than quietly substituted. A named, honestly-flagged environment gap (e.g. a different OS/shell than the issue, with the difference stated) does not by itself disqualify a cannot-reproduce - that gap is what the `environment` check already accounts for.

## Integrity

Where it lives: the report's own concluding statement (accept/could-not-reproduce/partial), read against the artifacts that same report presents as backing.

What tells them apart: a report that states exactly what it observed - including a "could not reproduce," as long as it shows a genuine attempt (environment recorded, steps run, no match found) - is honest. A report is not honest when its conclusion claims more than its artifact demonstrates: e.g. asserting "confirmed, matches the issue exactly" while the attached log shows a different error, or omitting a step that failed.

## Comments

Where it lives: the claim comment and the repro comment's text, read against two things - the repo-facts block's stated contribution policy (including any AI-use disclosure requirement) and the issue's own specifics (its title, its named behavior/version).

What specific-and-honest looks like next to boilerplate: the comment names the issue's actual behavior/version (not a copy-pasted generic line that could apply to any issue), discloses AI assistance if and only if the repo's policy requires it (silence in the policy passes), and - for the claim comment specifically - promises the next artifact (a reproduction report) rather than a fix, a PR, or a delivery date.

On disclosure specifically, a repo's policy can ask for two different things - tell them apart:

- An explicit **disclosure requirement**: the policy asks contributors to state that AI was used (often naming the tool and extent), e.g. "you must disclose all AI usage." Treat every candidate comment in this evaluation as AI-assisted work by course convention - never read silence about AI as evidence none was used. A comment fails this half unless it explicitly states that assistance somewhere in its text, no matter how specific or well-written the rest of it reads.
- A **human-voice / authenticity requirement**: the policy asks that comments be "in your own words" or human-written, without banning AI-assisted work itself, e.g. "comments must be written by humans in their own words; AI-generated comments may be hidden." This is not a disclosure requirement - it does not ask the contributor to state anything about AI use. It is satisfied the same way ordinary specificity is: a comment tied to the issue's actual facts, not a generic or bot-sounding line. Do not fail this half of the check for missing a disclosure statement the policy never asked for.
