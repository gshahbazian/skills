# Instructions

## Reading pull requests

When I ask you to read, review, or look at a PR, use the `gh` GitHub CLI
(e.g. `gh pr view`, `gh pr diff`, `gh pr checkout`) to fetch it. Assume `gh`
is installed and authenticated — don't check for it or ask me to authenticate.

## Parallel investigation

If these are true:
- you are a top-level agent talking to a user
- the work splits into independent read-only investigations
- it will be faster to spin up subagents instead of doing the investigation yourself, subagents can be slow

Then:
load `herdr-pi-subagent` and fan out 1–5 pi workers
