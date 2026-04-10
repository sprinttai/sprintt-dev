---
name: sprintt-dev:brand-sync
description: "Scan brand assets and update brand.md with file inventory. Use when the user says 'sync brand assets', 'update brand', 'I added logo files', or 'scan the brand folder'."
argument-hint: "[optional brand folder path]"
license: MIT
metadata:
  author: sprinttai
  version: "0.1.2"
allowed-tools: Read Write Edit Bash Glob
---

Scan `/public/brand/` and update `.claude/brand.md` with an accurate inventory of all brand assets.

## Step 1: Scan Assets

List all files in `public/brand/` recursively. For each file, note:
- File name and path relative to project root
- File type (SVG, PNG, JPG, ICO, PDF)
- Purpose (inferred from name and location)

Common file purposes:
- `logo.*` → Primary logo
- `logo-dark.*` → Dark mode logo variant
- `logo-icon.*`, `mark.*`, `icon.*` → Icon/mark version
- `favicon.*` → Browser favicon
- `og-image.*`, `og.*`, `social.*` → Social media / Open Graph image
- `headshots/*`, `team/*`, `people/*` → Personal photos

## Step 2: Update brand.md

Read `.claude/brand.md`. Find the `## Brand Assets` section (or create it if missing).

Replace that section with an updated inventory:

```markdown
## Brand Assets

Available in `/public/brand/`:

### Logos
- Primary logo: `/public/brand/logo.svg` — use for navbar, footer, documents
[list each logo file found]

### Icons
- Favicon: `/public/brand/favicon.ico` — browser tab icon
[list each icon file found]

### Social
- OG image: `/public/brand/og-image.png` — 1200x630, social sharing meta tags
[list each social image found]

### Photos
- Ricardo headshot: `/public/brand/headshots/ricardo.jpg` — about page, author cards
[list each photo found]
```

Only include sections that have files.

If `/public/brand/` is empty or doesn't exist, tell the user and provide the recommended assets checklist:

```
Recommended brand assets to add to /public/brand/:
- logo.svg — primary logo (SVG preferred)
- logo-dark.svg — dark mode variant (optional)
- logo-icon.svg — icon-only mark for small spaces
- favicon.ico — browser tab icon
- og-image.png — social sharing image (1200x630px)
- headshots/[name].jpg — personal photos for about/team pages
```

## Step 3: Confirm

Report how many assets were found and cataloged, and note any recommended assets that are missing.
