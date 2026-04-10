---
name: push
description: >
  Stage, commit with conventional commits, and push to current branch.
  Use when the user says "push my changes", "commit and push",
  "save my work", or "push this".
allowed-tools: Read Bash Grep Glob
---

Commit and push the current changes to the active branch. Run quality checks first.

## Step 1: Check State

Run `git status` and `git branch --show-current` to understand:
- Which branch you're on
- What files are staged, modified, and untracked

If there are no changes, tell the user and stop.

## Step 2: Quick Quality Check

Before committing, review the staged/modified files for obvious issues:
- Hardcoded API keys, tokens, passwords, or secrets (strings starting with `sk-`, `ghp_`, `AKIA`, or matching patterns like `password = "..."`)
- `console.log` or `print()` debug statements that shouldn't ship
- Files that shouldn't be committed (`.env`, `node_modules/`, `__pycache__/`, `.DS_Store`)

If secrets are found, **block the commit** and tell the user exactly which file and line.
If debug statements are found, **warn** but don't block — ask if they want to proceed.

## Step 3: Stage Changes

Show the user what will be committed. Stage relevant files:
- Use `git add` with specific file paths — never `git add .` or `git add -A`
- Skip `.env`, credentials, and large binary files
- Ask the user if the file list looks right before proceeding

## Step 4: Commit

If `$ARGUMENTS` was provided, use it as the commit message (validate it follows conventional commits format).

If no message was provided, generate one by reading the diff:
- Format: `type(scope): description`
- Types: feat, fix, docs, style, refactor, test, chore
- Keep under 72 characters
- Show the message and ask for confirmation

Run: `git commit -m "[message]"`

## Step 5: Push

Run: `git push origin [current-branch]`

If the branch has no upstream, run: `git push -u origin [current-branch]`

## Step 6: Confirm

Tell the user:
- What was committed (file count, commit hash)
- Which branch it was pushed to
- If on a feature branch, remind them they can run `/sprintt-dev:pr` when ready
