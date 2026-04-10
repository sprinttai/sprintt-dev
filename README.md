# sprintt-dev

SDLC toolkit for Sprintt projects. Scaffolds new projects, enforces code quality, scans for security issues, manages git workflow, and provides UI/UX design guidance — all through Claude.

## Install

```
/plugin install sprinttai/sprintt-dev
```

Or from a local directory:
```
claude --plugin-dir /path/to/sprintt-dev
```

## Requirements

These tools should be installed on your machine:

- **git** — version control
- **gh** — GitHub CLI (for creating repos and PRs)
- **node + npm** — for JS/TS projects
- **python 3** — for Python projects (optional)

All project-level tools (ESLint, Prettier, Husky, Commitlint, Vitest) are installed automatically by `/sprintt-dev:init`.

## Skills

| Skill | What it does |
|-------|-------------|
| `/sprintt-dev:init [name]` | Scaffold a new project — folder structure, git, linting, brand setup, GitHub repo |
| `/sprintt-dev:push [message]` | Stage, commit (conventional commits), and push to current branch with pre-commit checks |
| `/sprintt-dev:pr` | Create a GitHub PR from current branch into main with auto-generated description |
| `/sprintt-dev:new-feature [name]` | Create a feature branch from latest main |
| `/sprintt-dev:check` | Run security scan + code quality checks without committing |
| `/sprintt-dev:component [name]` | Generate a polished React component following brand guidelines |
| `/sprintt-dev:brand-sync` | Scan `/public/brand/` and update `.claude/brand.md` asset inventory |

## Agents

| Agent | Color | Purpose |
|-------|-------|---------|
| `@security` | Red | Scans for hardcoded secrets, API keys, dependency vulnerabilities |
| `@qa` | Green | Runs linting, formatting, type checking, and tests |
| `@reviewer` | Blue | Reviews code changes for logic issues, edge cases, and style |
| `@architect` | Cyan | Validates project structure and naming conventions |
| `@designer` | Magenta | Reviews UI/UX — hierarchy, spacing, typography, color, accessibility |

Agents can be invoked directly (e.g., "ask @security to scan this") or are triggered automatically by hooks.

## Hooks

Hooks run automatically during development:

| Event | What happens |
|-------|-------------|
| **Session start** | Loads project context, reminds about available commands |
| **Before git commit** | Scans for secrets (blocks if found), validates commit message format, warns about debug code |
| **Before git push** | Full security scan, warns if pushing directly to main, reviews diff for issues |

## Brand System

Every project scaffolded with `/sprintt-dev:init` includes:

- **`.claude/brand.md`** — colors, typography, spacing, and visual tone. Referenced automatically by Claude before any UI work.
- **`/public/brand/`** — directory for brand assets (logos, favicons, OG images, headshots).
- **`/sprintt-dev:brand-sync`** — scans assets and updates `brand.md` with file paths so Claude knows what images exist.

### Recommended Brand Assets

Drop these into `/public/brand/` and run `/sprintt-dev:brand-sync`:

```
public/brand/
├── logo.svg              ← Primary logo (SVG preferred)
├── logo-dark.svg         ← Dark mode variant (optional)
├── logo-icon.svg         ← Icon-only mark for small spaces
├── favicon.ico           ← Browser tab icon
├── og-image.png          ← Social sharing image (1200x630px)
└── headshots/
    └── ricardo.jpg       ← Personal photo for about/team pages
```

## Free Tools Used

Every tool in this plugin is free for business use:

| Tool | License | Purpose |
|------|---------|---------|
| ESLint | MIT | Code linting |
| Prettier | MIT | Code formatting |
| Husky | MIT | Git hooks |
| Commitlint | MIT | Conventional commit enforcement |
| Vitest | MIT | Unit testing |
| gh CLI | MIT | GitHub operations |

Security scanning is done by Claude directly via pattern matching — no external paid tools required.

## Project Types

`/sprintt-dev:init` supports:

- **Next.js web app** — Next.js 15 + TypeScript + Tailwind CSS + shadcn/ui patterns
- **Python API/agent** — Python project with ruff/flake8, pytest
- **Bare** — Minimal structure with just git, CLAUDE.md, and brand setup

## Git Workflow

This plugin enforces a simple git workflow:

```
main (production, always deployable)
├── feature/add-auth
├── feature/dashboard
├── fix/login-bug
└── chore/update-deps
```

- Branch from main with `/sprintt-dev:new-feature`
- Commit with conventional messages via `/sprintt-dev:push`
- Open PR with `/sprintt-dev:pr`
- Squash merge to main

## Publishing to GitHub

To push this plugin to your GitHub org:

```bash
cd sprintt-dev/
git init && git add . && git commit -m "feat: initial sprintt-dev plugin"
gh repo create sprinttai/sprintt-dev --private --source=. --push
```

Then install from anywhere with `/plugin install sprinttai/sprintt-dev`.
