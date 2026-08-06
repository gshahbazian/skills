---
name: gh-stack
description: Create and manage stacked pull requests with the gh stack CLI. Use when asked to start, extend, push, submit, or inspect a PR stack.
---

# gh stack

Stacked PRs are a chain of branches where each PR targets the branch below it.

## Commands

- `gh stack init` — Start a stack and create its first branch.
- `gh stack add BRANCH-NAME` — Add a new branch on top of the current stack.
- `gh stack push` — Push every branch in the stack.
- `gh stack submit` — Push the branches, create their PRs, and link them as a stack.
- `gh stack view` — Show the stack's branches, PRs, statuses, and latest commits.

## Typical workflow

```bash
gh stack init

# Make and commit the first change.
git add .
git commit -m "First change"

# Add another layer and commit its change.
gh stack add BRANCH-NAME
git add .
git commit -m "Second change"

# Create the stacked PRs.
gh stack submit
```

Use this shortcut to stage all changes, commit them, and add the next layer in one step:

```bash
gh stack add -Am "Commit message"
```

Run `gh stack view` before and after changing a stack so you know which layer is current.

For more commands and current usage details, run:

```bash
gh stack --help
```
