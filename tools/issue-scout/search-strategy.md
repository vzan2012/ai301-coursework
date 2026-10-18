# Search strategy: where your scout looks, and why

Each query traces to a sentence of my fit profile in scope.md (lines a to d).
Queries are wide on purpose; the ranking filters do the narrowing.

## Queries

# a, b: JS/TS comfort, small bug fixes
gh search issues --label bug --label "good first issue" --language typescript --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# a, b: same, JavaScript
gh search issues --label bug --label "good first issue" --language javascript --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# a: NestJS
gh search issues nestjs --label "good first issue" --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# a: React
gh search issues react --label "good first issue" --language typescript --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# a: NodeJS ("node.js" gave a cleaner spread than "nodejs": more repos, fewer assigned)
gh search issues "node.js" --label "good first issue" --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# a: Python, some exposure (fallback)
gh search issues --label bug --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# c: test suites and CI
gh search issues test --match title --label "good first issue" --language typescript --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

## Ranking filters

Applied in this order:

- drop any result with an assignee (in the wild, an assignee is someone who got there first)
- drop any result not updated in the last 90 days
- keep only the best-ranked issue per repo (repeat repos flooded the raw results)
- order what is left by updatedAt, newest first

Line d (avoid issues that need deep domain-specific math or specialized subject
knowledge) cannot be judged from the result fields, so it is not a filter here;
the rubric handles it when the top results are graded.

## How many to grade

Grade the top **5** ranked results per scan.
