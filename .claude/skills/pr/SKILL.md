---
name: sprintt-dev:pr
description: "Create a GitHub pull request from current branch into main. Use when the user says 'create a PR', 'open a pull request', 'submit for review', or 'merge this'."
argument-hint: "[optional PR title]"
license: MIT
metadata:
  author: sprinttai
  version: "0.1.2"
allowed-tools: Bash
---

Create a pull request from the current branch into main.

## Step 1: Verify State

Run `git branch --show-current` to get the current branch.

If on `main`, tell the user: "You're on main — switch to a feature branch first. Use `/sprintt-dev:new-feature [name]`." Stop.

Check if there are unpushed commits: `git log origin/[branch]..HEAD --oneline 2>/dev/null`
If there are unpushed commits, push them first: `git push origin [branch]`

## Step 2: Gather PR Content

Get the commits that will be in this PR:
```bash
git log main..[branch] --oneline
git diff main...[branch] --stat
```

From the commits and diff, generate:
- **Title**: Short, under 70 characters, describes the change
- **Body**: Summary formatted as:

```
## Summary
[2-3 bullet points describing what this PR does]

## Changes
[List of key files/areas changed]

## Test Plan
[How to verify this works — manual steps or test commands]
```

Show the title and body to the user for confirmation before creating.

## Step 3: Create PR

```bash
gh pr create --title "[title]" --body "[body]" --base main
```

## Step 4: Confirm

Show the user the PR URL, title, and target branch.
