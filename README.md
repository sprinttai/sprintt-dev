# sprintt-dev

A Claude Code plugin that gives you a complete development toolkit — project scaffolding, git workflow, security scanning, code review, and UI/UX design guidance. Think of it as a senior dev + designer sitting next to you, catching mistakes and keeping your code clean.

## What This Plugin Does

When you install sprintt-dev, you get:

- **8 skills** you can run as slash commands (like `/sprintt-dev:push` to commit and push your code)
- **5 agents** that act as specialists you can call on (like `@security` to scan for leaked API keys)
- **Automatic hooks** that protect you behind the scenes (blocks you from accidentally committing secrets, enforces clean commit messages)

You don't need to memorize everything. Once installed, Claude will remind you what's available at the start of every session.

---

## Installation

This is a two-step process. Run these commands inside Claude Code:

```
/plugin marketplace add sprinttai/sprintt-dev
/plugin install sprintt-dev@sprintt-dev
```

The first command registers the plugin source (the GitHub repo). The second command installs it into your project.

### Prerequisites

Make sure these are installed on your machine before using the plugin:

- **git** — version control
- **gh** — [GitHub CLI](https://cli.github.com/) (for creating repos and PRs)
- **node + npm** — for JavaScript/TypeScript projects
- **python 3** — for Python projects (optional)

All project-level tools (ESLint, Prettier, Husky, Commitlint, Vitest) are installed automatically when you scaffold a project.

---

## Getting Started (Your First 10 Minutes)

Here's what a typical first session looks like after installing the plugin:

**1. Scaffold a new project:**
```
/sprintt-dev:init my-portfolio
```
Claude will ask you a couple of questions (project type, brand colors) and then generate your entire project structure — folders, config files, linting, git, and a GitHub repo.

**2. Create a feature branch:**
```
/sprintt-dev:new-feature hero-section
```
This pulls the latest `main`, creates `feature/hero-section`, and switches you to it.

**3. Build something:**
```
/sprintt-dev:component hero
```
Claude generates a polished, accessible React component that follows your project's brand guidelines (colors, fonts, spacing from `.claude/brand.md`).

**4. Commit and push:**
```
/sprintt-dev:push
```
Claude stages your changes, writes a conventional commit message, scans for secrets, and pushes. If it finds a hardcoded API key, it blocks the commit and tells you exactly where.

**5. Open a pull request:**
```
/sprintt-dev:pr
```
Claude reads your commits, generates a PR title and description, and creates the PR on GitHub.

---

## Skills Reference

Skills are slash commands you run in Claude Code. Each one handles a specific workflow.

| Skill | What to type | What it does |
|-------|-------------|--------------|
| **init** | `/sprintt-dev:init my-app` | Scaffolds a new project (Next.js, Python, or bare). Sets up folders, git, linting, brand identity, and optionally creates a GitHub repo. |
| **push** | `/sprintt-dev:push` | Stages your changes, generates a conventional commit message, scans for secrets, and pushes to your current branch. You can also pass a message: `/sprintt-dev:push fix auth redirect` |
| **pr** | `/sprintt-dev:pr` | Creates a GitHub pull request from your current branch into `main` with an auto-generated title and description. |
| **new-feature** | `/sprintt-dev:new-feature auth-flow` | Pulls latest `main` and creates a new branch. Automatically prefixes with `feature/`, `fix/`, or `chore/` based on the name. |
| **check** | `/sprintt-dev:check` | Runs a full security + quality audit: scans for secrets, checks dependencies for vulnerabilities, runs linting, formatting, and tests. Doesn't commit anything — just reports what it finds. |
| **component** | `/sprintt-dev:component pricing-card` | Generates a React/TypeScript component that follows your brand guidelines. Includes proper TypeScript types, accessibility, and responsive design. |
| **brand-sync** | `/sprintt-dev:brand-sync` | Scans your `/public/brand/` folder for logos, favicons, and images, then updates `.claude/brand.md` so Claude knows what assets are available. |
| **design-system** | `/sprintt-dev:design-system` | Opens the design knowledge base — spacing scales, typography rules, color theory, accessibility guidelines, and component patterns. Used by Claude when generating or reviewing UI. |

---

## Agents

Agents are specialists you can call on by name. They focus on one area and give you a structured report.

### @security (Red)

**What it does:** Scans your code for hardcoded secrets, leaked API keys, dangerous files, and dependency vulnerabilities.

**When to use it:**
```
"@security scan my changes before I push"
"@security check if I left any keys in the auth module"
"@security run a full audit on this project"
```

**What it catches:**
- AWS keys (`AKIA...`), GitHub tokens (`ghp_...`), OpenAI/Anthropic keys (`sk-...`), Stripe keys, Slack tokens
- Private keys, connection strings, password assignments
- `.env` files accidentally tracked in git
- Known vulnerabilities in your npm/pip dependencies

**Example output:**
```
## Security Scan Results

### CRITICAL (must fix before commit)
- src/lib/api.ts:14 — Hardcoded API key: `sk-proj-abc123...` → Move to .env and use process.env.API_KEY

### WARNING (should fix)
- src/config.ts:8 — Connection string with credentials → Use environment variables

### Summary
1 critical, 1 warning, 0 info
Recommendation: BLOCK — fix the hardcoded key before committing
```

---

### @qa (Green)

**What it does:** Runs your linting, formatting, type checking, and test suite, then gives you a clear pass/fail report.

**When to use it:**
```
"@qa run all checks"
"@qa is this ready to push?"
"@qa check if my tests pass"
```

**What it runs:**
- ESLint (linting errors and warnings)
- Prettier (formatting check)
- TypeScript compiler (type errors)
- Vitest or pytest (test suite)

**Example output:**
```
## QA Report

### Linting
[FAIL] 3 errors, 1 warning
  - src/components/Header.tsx:22 — 'React' is defined but never used → Remove unused import

### Formatting
[FAIL] 2 files need formatting
  - src/lib/utils.ts
  - src/app/page.tsx

### Tests
[PASS] 12/12 tests passed

### Summary
Overall: FAIL — fix 3 lint errors and run prettier before committing
```

---

### @reviewer (Blue)

**What it does:** Reviews your code changes like a senior developer would — checks for logic issues, edge cases, error handling, performance, and readability.

**When to use it:**
```
"@reviewer review my changes before I merge"
"@reviewer what do you think of this approach?"
"@reviewer look at the auth module for issues"
```

**What it checks:**
- Logic errors, off-by-one bugs, null pointer risks
- Missing error handling (unhandled promises, missing try/catch)
- Edge cases (empty arrays, null values, concurrent requests)
- Performance issues (unnecessary re-renders, N+1 queries)
- Code readability and duplication

**Example output:**
```
## Code Review

### Critical (must fix)
- src/hooks/useAuth.ts:34 — Promise rejection is unhandled. If the token refresh fails,
  the user gets a white screen. Wrap in try/catch and redirect to login on failure.

### Suggestions
- src/components/DataTable.tsx:67 — This filters the full array on every render.
  Wrap in useMemo to avoid re-filtering when unrelated state changes.

### What Looks Good
- Clean separation between API layer and UI components
- Good use of TypeScript generics in the form handler
```

---

### @architect (Cyan)

**What it does:** Validates your project structure — are files in the right folders? Are naming conventions consistent? Are there any missing pieces?

**When to use it:**
```
"@architect check if my project structure is clean"
"@architect am I putting things in the right place?"
"@architect review the folder organization"
```

**What it checks:**
- Components outside of `src/components/`
- Utility functions buried inside component files
- Missing `.claude/CLAUDE.md` or `.claude/brand.md`
- Mixed naming conventions (camelCase and snake_case in the same project)
- Files over 300 lines that should be split

**Example output:**
```
## Architecture Review

### Structure Issues
- src/app/page.tsx contains a `formatCurrency()` helper → Move to src/lib/utils.ts

### Naming Issues
- src/components/data_table.tsx → Rename to DataTable.tsx (components use PascalCase)

### Health
Overall structure: NEEDS ATTENTION
Two files are in the wrong place, but the overall organization is solid.
```

---

### @designer (Magenta)

**What it does:** Reviews your UI code like a senior designer — visual hierarchy, spacing, typography, color contrast, accessibility, and responsiveness.

**When to use it:**
```
"@designer does this look good? How can I make it more polished?"
"@designer review the landing page layout"
"@designer check accessibility on the form components"
```

**What it checks:**
- Visual hierarchy (is the most important content the most prominent?)
- Spacing consistency (using the spacing scale, not arbitrary values)
- Typography (readable font sizes, clear heading hierarchy)
- Color contrast (WCAG AA compliance — 4.5:1 for text)
- Responsiveness (works at mobile, tablet, and desktop)
- Accessibility (alt text, form labels, keyboard navigation, semantic HTML)

**Example output:**
```
## Design Review

### High Impact (fix these first)
- src/components/Hero.tsx:12 — Section padding is `py-8`, too cramped for a hero.
  Change to `py-24` for proper visual weight.
- src/components/Card.tsx:28 — Text contrast is 3.2:1 (below 4.5:1 minimum).
  Change `text-gray-400` to `text-gray-600`.

### Accessibility
- src/components/ContactForm.tsx:15 — Email input has no <label>.
  Add <label htmlFor="email">Email</label> above the input.

### What Works Well
- Clean heading hierarchy with clear size progression
- Consistent border-radius across all card components
```

---

## How Hooks Work (Automatic Protection)

Hooks run automatically in the background — you don't need to trigger them. They protect you from common mistakes.

### On Session Start

Every time you open Claude Code in a project that has this plugin, Claude automatically loads your project context and reminds you what skills and agents are available. You'll see something like:

```
## Project Context
Read .claude/CLAUDE.md for project setup, commands, and conventions.
Read .claude/brand.md before any UI work.
Available skills: /sprintt-dev:push, /sprintt-dev:pr, /sprintt-dev:check, ...
Available agents: @security, @qa, @reviewer, @architect, @designer
```

### Before Every Git Commit

When you (or Claude) runs `git commit`, the plugin automatically:

1. **Scans staged files for secrets** — API keys, tokens, passwords, private keys, connection strings. If it finds any, the commit is **blocked** and you're told exactly which file and line to fix.
2. **Validates the commit message** — Must follow conventional commits format (`feat(auth): add login flow`). If the message doesn't match, the commit is blocked and a corrected message is suggested.
3. **Warns about debug code** — Flags `console.log` and `print()` statements. Doesn't block, just warns.

This means you literally cannot accidentally commit a secret. The hook catches it before it ever touches git history.

### Before Every Git Push

When you push, the plugin runs a more thorough check:

1. **Re-scans the full diff against main** for secrets (catches anything that slipped through)
2. **Warns if you're pushing directly to main** — asks you to confirm or use a feature branch + PR instead
3. **Checks for uncommitted changes** — warns if you have work that won't be included in the push
4. **Quick code review** — flags obvious issues in the diff (unhandled errors, TODO comments, large files, commented-out code blocks)

---

## Brand System

Every project scaffolded with `/sprintt-dev:init` includes a brand identity system so Claude produces consistent designs across your entire project.

- **`.claude/brand.md`** — Defines your colors, typography, spacing scale, and visual tone. Claude reads this before generating any UI component.
- **`/public/brand/`** — Drop your logo files, favicons, and images here.
- **`/sprintt-dev:brand-sync`** — Run this after adding assets. It scans the folder and updates `brand.md` so Claude knows what images are available.

### Recommended Brand Assets

```
public/brand/
├── logo.svg              ← Primary logo (SVG preferred)
├── logo-dark.svg         ← Dark mode variant (optional)
├── logo-icon.svg         ← Icon-only mark for small spaces
├── favicon.ico           ← Browser tab icon
├── og-image.png          ← Social sharing image (1200x630px)
└── headshots/
    └── [name].jpg        ← Personal photos for about/team pages
```

---

## Supported Project Types

`/sprintt-dev:init` supports three project types:

| Type | Stack | What you get |
|------|-------|-------------|
| **Next.js web app** | Next.js 15 + TypeScript + Tailwind CSS | Full frontend setup with ESLint, Prettier, Husky, Commitlint, Vitest |
| **Python API/agent** | Python 3 + ruff/flake8 + pytest | API-ready structure with linting and testing |
| **Bare** | Git + CLAUDE.md + brand.md | Minimal starting point — add your own stack |

---

## Git Workflow

This plugin enforces a simple, clean branching strategy:

```
main (always deployable)
 ├── feature/add-auth        ← new features
 ├── fix/login-bug           ← bug fixes
 └── chore/update-deps       ← maintenance
```

The typical flow:

1. `/sprintt-dev:new-feature add-auth` — create a branch
2. Write your code
3. `/sprintt-dev:push` — commit and push (hooks scan for issues automatically)
4. `/sprintt-dev:pr` — open a pull request
5. Squash merge to main

Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/): `type(scope): description`

Valid types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `ci`, `build`

---

## Free Tools Used

Every tool in this plugin is free and open source:

| Tool | License | Purpose |
|------|---------|---------|
| ESLint | MIT | Code linting |
| Prettier | MIT | Code formatting |
| Husky | MIT | Git hooks |
| Commitlint | MIT | Conventional commit enforcement |
| Vitest | MIT | Unit testing |
| gh CLI | MIT | GitHub operations |

Security scanning is done by Claude directly via pattern matching — no external paid tools required.

---

## License

MIT
