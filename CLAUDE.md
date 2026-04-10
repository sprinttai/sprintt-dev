# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Project Overview

Sprintt Dev is a Claude Code plugin providing a complete SDLC toolkit for scaffolding and managing new projects. It includes skills for project setup, git workflow, security, QA, code review, component generation, and brand identity management.

**GitHub:** https://github.com/sprinttai/sprintt-dev
**Author:** sprinttai

## Installation

```bash
/plugin marketplace add sprinttai/sprintt-dev
/plugin install sprintt-dev@sprintt-dev
```

## Skills

| Skill | Description |
|-------|-------------|
| `/sprintt-dev:init` | Scaffold a new project (Next.js, Python, bare) |
| `/sprintt-dev:push` | Stage, commit (conventional commits), and push |
| `/sprintt-dev:pr` | Create a GitHub PR with auto-generated description |
| `/sprintt-dev:new-feature` | Create a feature branch from latest main |
| `/sprintt-dev:check` | Run security scan + code quality checks |
| `/sprintt-dev:component` | Generate React/TS/Tailwind component following brand.md |
| `/sprintt-dev:brand-sync` | Scan /public/brand/ and update .claude/brand.md |
| `/sprintt-dev:design-system` | Design knowledge base for UI/UX decisions |

## Agents

| Agent | Color | Purpose |
|-------|-------|---------|
| `@security` | Red | Secrets scanning, dependency vulnerabilities |
| `@qa` | Green | Linting, formatting, type checking, tests |
| `@reviewer` | Blue | Code review — logic, edge cases, error handling |
| `@architect` | Cyan | Project structure and naming conventions |
| `@designer` | Magenta | UI/UX quality — hierarchy, spacing, color, accessibility |

## Hooks

- **SessionStart** — Prints project context and available skills/agents
- **PreToolUse (git commit)** — Scans for secrets, validates conventional commits, warns about debug code
- **PreToolUse (git push)** — Re-scans for secrets, checks branch, reviews diff

## Architecture

```
sprintt-dev/
├── .claude-plugin/
│   ├── plugin.json          # Plugin manifest with skills references
│   └── marketplace.json     # Marketplace publishing config
├── .claude/
│   └── skills/              # All skills live here
│       ├── init/            # Project scaffolding
│       ├── push/            # Commit and push workflow
│       ├── pr/              # Pull request creation
│       ├── new-feature/     # Feature branch creation
│       ├── check/           # Security and quality checks
│       ├── component/       # UI component generation
│       ├── brand-sync/      # Brand asset cataloging
│       └── design-system/   # Design knowledge base
│           └── references/  # Design principles reference docs
├── agents/                  # Sub-agents (security, qa, reviewer, architect, designer)
├── hooks/
│   └── hooks.json           # Session start + pre-commit/push hooks
├── CLAUDE.md                # This file
└── README.md                # Public README
```

## Git Workflow

Never push directly to `main`. Always:

1. Create a new branch: `git checkout -b feat/...` or `fix/...`
2. Commit with conventional commits: `type(scope): description`
3. Push branch: `git push -u origin <branch>`
4. Create PR: `gh pr create`

## Prerequisites

- Git + GitHub CLI (`gh`)
- Node.js (for JS/TS projects)
- Python 3.x (for Python projects)
