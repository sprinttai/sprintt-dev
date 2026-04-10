---
name: sprintt-dev:init
description: "Scaffold a new project with full SDLC setup. Use when the user says 'start a new project', 'scaffold a project', 'create a new app', 'init project', or 'set up a new repo'."
argument-hint: "[project name]"
license: MIT
metadata:
  author: sprinttai
  version: "0.1.2"
allowed-tools: Read Write Edit Bash Glob Grep
---

Scaffold a new project named `$ARGUMENTS`. Walk through setup interactively.

## Step 1: Gather Requirements

Ask the user (use AskUserQuestion if available, otherwise ask directly):

1. **Project type**: Next.js web app, Python API/agent, or bare (minimal structure)
2. **Brief description**: One sentence — what does this project do?

## Step 2: Create Project Structure

Based on project type, create the appropriate folder structure.

**For Next.js web apps:**
```
[name]/
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── globals.css
│   ├── components/
│   │   └── ui/
│   └── lib/
│       └── utils.ts
├── public/
│   └── brand/
│       └── .gitkeep
├── .claude/
│   ├── CLAUDE.md
│   └── brand.md
├── .eslintrc.json
├── .prettierrc
├── commitlint.config.js
├── .gitignore
├── package.json
├── tsconfig.json
├── tailwind.config.ts
├── postcss.config.js
└── README.md
```

**For Python API/agent:**
```
[name]/
├── src/
│   ├── __init__.py
│   ├── main.py
│   └── config.py
├── tests/
│   └── __init__.py
├── public/
│   └── brand/
│       └── .gitkeep
├── .claude/
│   ├── CLAUDE.md
│   └── brand.md
├── .gitignore
├── pyproject.toml
├── requirements.txt
├── .flake8
└── README.md
```

**For bare:**
```
[name]/
├── src/
├── public/
│   └── brand/
│       └── .gitkeep
├── .claude/
│   ├── CLAUDE.md
│   └── brand.md
├── .gitignore
└── README.md
```

## Step 3: Write CLAUDE.md

Create `.claude/CLAUDE.md` with this template, filling in project-specific details:

```markdown
# [Project Name]

[One-line description from Step 1]

## Tech Stack
[Based on project type — e.g., Next.js 15, Tailwind CSS, TypeScript]

## Commands
- `npm run dev` — start dev server
- `npm run build` — production build
- `npm run lint` — run ESLint
- `npm run format` — run Prettier
- `npm test` — run tests

## Git Workflow
- Branch from `main` as `feature/[name]` or `fix/[name]`
- Use conventional commits: `type(scope): description`
- PR into main, squash merge

## Code Style
- ESLint + Prettier enforced
- Prefer named exports
- Components in PascalCase, utilities in camelCase
- Keep files under 200 lines — split if larger

## Brand
Read `.claude/brand.md` before any UI work. Brand assets live in `/public/brand/`.
```

## Step 4: Write brand.md

Ask the user for brand basics:
1. **Primary color** (hex code, or "I'll set this later")
2. **Font preference** (e.g., Inter, System fonts, or "I'll set this later")
3. **Style direction**: minimal, bold, playful, corporate, or other

Generate `.claude/brand.md`:

```markdown
# Brand Identity

## Colors
- Primary: [user's color or #2563EB as default]
- Primary hover: [10% darker variant]
- Background: #FFFFFF
- Surface: #F8FAFC
- Text primary: #0F172A
- Text secondary: #475569
- Border: #E2E8F0
- Success: #16A34A
- Warning: #D97706
- Error: #DC2626

## Typography
- Font family: [user's choice or "Inter, system-ui, sans-serif"]
- Headings: font-semibold
  - h1: text-4xl (36px)
  - h2: text-3xl (30px)
  - h3: text-2xl (24px)
  - h4: text-xl (20px)
- Body: text-base (16px)
- Small: text-sm (14px)
- Caption: text-xs (12px)

## Spacing
- Use Tailwind default scale: 4, 8, 12, 16, 24, 32, 48, 64
- Section padding: py-16 (desktop), py-10 (mobile)
- Card padding: p-6
- Component gap: gap-4 (default), gap-6 (spacious)

## Border Radius
- Buttons: rounded-lg (8px)
- Cards: rounded-xl (12px)
- Inputs: rounded-md (6px)
- Badges: rounded-full

## Visual Tone
[User's style direction — e.g., "Clean and minimal with generous whitespace. Let content breathe. Avoid visual clutter."]

## Brand Assets
Assets live in `/public/brand/`. Run `/sprintt-dev:brand-sync` after adding files.

[This section is auto-populated by brand-sync]
```

## Step 5: Set Up Tooling

**For Next.js projects**, create `package.json` with these free-tier dependencies:

```json
{
  "name": "[project-name]",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint src/ --ext .ts,.tsx",
    "format": "prettier --write \"src/**/*.{ts,tsx,css,json}\"",
    "format:check": "prettier --check \"src/**/*.{ts,tsx,css,json}\"",
    "test": "vitest",
    "test:run": "vitest run",
    "prepare": "husky"
  },
  "dependencies": {
    "next": "latest",
    "react": "latest",
    "react-dom": "latest"
  },
  "devDependencies": {
    "@types/node": "latest",
    "@types/react": "latest",
    "@types/react-dom": "latest",
    "typescript": "latest",
    "tailwindcss": "latest",
    "postcss": "latest",
    "autoprefixer": "latest",
    "eslint": "latest",
    "eslint-config-next": "latest",
    "prettier": "latest",
    "prettier-plugin-tailwindcss": "latest",
    "husky": "latest",
    "commitlint": "latest",
    "@commitlint/config-conventional": "latest",
    "vitest": "latest"
  },
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{css,json,md}": ["prettier --write"]
  }
}
```

Create `.eslintrc.json`, `.prettierrc`, `commitlint.config.js`, and `.gitignore` appropriate to the project type.

**For Python projects**, create `pyproject.toml` with ruff/black, `.flake8`, and `requirements.txt`.

## Step 6: Initialize Git + GitHub

```bash
cd [project-name]
git init
git add .
git commit -m "feat: initial project scaffold"
```

Then ask: "Want me to create the GitHub repo under sprinttai? (Y/n)"

If yes:
```bash
gh repo create sprinttai/[project-name] --private --source=. --push
```

## Step 7: Install Dependencies

Ask: "Want me to run `npm install` now? (Y/n)"

If yes, run the install and set up Husky:
```bash
npm install
npx husky init
echo "npx commitlint --edit \$1" > .husky/commit-msg
```

## Step 8: Summary

Report what was created: project structure, tooling installed, GitHub repo URL. Remind about brand assets and available skills.
