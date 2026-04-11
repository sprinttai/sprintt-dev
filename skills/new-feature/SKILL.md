---
name: new-feature
user-invocable: true
description: "Create a feature branch from latest main. Use when the user says new feature, create a branch, start working on a feature, or new branch."
---


Create a new feature branch and switch to it.

## Step 1: Validate

If no `$ARGUMENTS` provided, ask the user for a branch name.

Clean the name: lowercase, replace spaces with hyphens, remove special characters.
Prefix with `feature/` if not already prefixed. If the name starts with `fix-` or `fix/`, use `fix/` prefix instead. If it starts with `chore`, use `chore/`.

Examples:
- "add auth" → `feature/add-auth`
- "fix login bug" → `fix/login-bug`
- "update deps" → `chore/update-deps`

## Step 2: Update Main

```bash
git checkout main
git pull origin main
```

If there are uncommitted changes on the current branch, warn the user and ask if they want to stash them first.

## Step 3: Create Branch

```bash
git checkout -b [branch-name]
```

## Step 4: Confirm

Tell the user the branch name, that they're now on it, and remind about `/sprintt-dev:push` and `/sprintt-dev:pr`.
