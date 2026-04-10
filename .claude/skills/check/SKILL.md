---
name: check
description: >
  Run security and code quality checks on the project.
  Use when the user says "run checks", "scan for issues",
  "is this secure", "check quality", or "audit the code".
allowed-tools: Read Bash Grep Glob
---

Run a full security and quality scan on the project. Report findings without committing or pushing.

## Step 1: Detect Project Type

Check for `package.json` (Node/JS), `pyproject.toml` or `requirements.txt` (Python), or both.

## Step 2: Security Scan

### Secrets Detection
Scan all tracked files for hardcoded secrets. Search for patterns:
- AWS keys: `AKIA[0-9A-Z]{16}`
- Generic API keys: `api_key`, `apikey`, `api-key` followed by `=` or `:` with a quoted string
- Private keys: `-----BEGIN (RSA |EC |DSA )?PRIVATE KEY-----`
- Tokens: `ghp_`, `gho_`, `sk-`, `sk_live_`, `pk_live_`, `xoxb-`, `xoxp-`
- Connection strings: `postgresql://`, `mysql://`, `mongodb://`, `redis://`
- Password assignments: `password\s*=\s*["'][^"']+["']` (not empty strings)
- `.env` files committed to git: check `git ls-files` for `.env*` files

Use `grep -rn` across `src/` and project root config files. Exclude `node_modules/`, `.git/`, `dist/`, `build/`, `__pycache__/`.

### Dependency Vulnerabilities
**Node.js**: Run `npm audit --json 2>/dev/null` if `package-lock.json` exists.
**Python**: Run `pip audit --format json 2>/dev/null` if available, otherwise skip.

## Step 3: Code Quality

**Node.js/TypeScript:**
- Run `npx eslint src/ --format compact 2>/dev/null` if configured
- Run `npx prettier --check "src/**/*.{ts,tsx,css,json}" 2>/dev/null` if configured
- Run `npx vitest run 2>/dev/null` if configured

**Python:**
- Run `ruff check src/ 2>/dev/null` or `flake8 src/ 2>/dev/null` if available
- Run `pytest 2>/dev/null` if tests exist

If a tool isn't installed, skip it and note it in the report.

## Step 4: Report

```
## Security Scan
[PASS/FAIL] Secrets detection: [count] issues found
[PASS/FAIL/SKIP] Dependency audit: [count] vulnerabilities

## Code Quality
[PASS/FAIL/SKIP] Linting: [count] errors, [count] warnings
[PASS/FAIL/SKIP] Formatting: [count] files need formatting
[PASS/FAIL/SKIP] Tests: [passed]/[total] passed

## Issues Found
[List each issue with file path, line number, and what to fix]
```

Prioritize by severity: secrets first, then vulnerabilities, then lint errors.
