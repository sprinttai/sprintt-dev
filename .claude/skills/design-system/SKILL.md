---
name: sprintt-dev:design-system
description: "Design system guidance for visual design, spacing, typography, color usage, accessibility, and responsive patterns. Use when the user asks to improve the UI, fix the design, make it look better, review the layout, or add a section."
---

# Design System Knowledge

Professional UI/UX design principles for building polished, accessible interfaces. This skill provides the design knowledge that powers the `@designer` agent and `/sprintt-dev:component` command.

## Core Principles

### 1. Hierarchy First
Every screen has one primary action and one primary message. Make them obvious. Everything else is secondary. Use size, weight, color, and spacing to create clear levels of importance.

### 2. Spacing Creates Quality
The single biggest difference between amateur and professional UI is whitespace. When in doubt, add more space. Use a consistent spacing scale (4, 8, 12, 16, 24, 32, 48, 64px) and never eyeball it.

### 3. Limit Your Palette
Three colors max for UI (primary, neutral, accent). Every additional color adds cognitive load. Use shades/tints of your primary color instead of introducing new hues.

### 4. Typography Is Design
Good type choices eliminate the need for decoration. Use size and weight contrast to create hierarchy — you rarely need more than 2 font weights (regular and semibold) and 4-5 sizes.

### 5. Consistency Over Cleverness
Every component should feel like it belongs to the same family. Same border radius, same shadow depth, same padding patterns. Break consistency only with intention.

## Quick Reference

### Spacing Scale (Tailwind)
| Use case | Class | Pixels |
|----------|-------|--------|
| Tight (between related items) | gap-1 to gap-2 | 4-8px |
| Default (between components) | gap-4 | 16px |
| Comfortable (between sections) | gap-6 to gap-8 | 24-32px |
| Spacious (section padding) | py-16 to py-24 | 64-96px |
| Page max-width | max-w-7xl | 1280px |
| Content max-width (readability) | max-w-prose | 65ch |

### Typography Scale
| Element | Size | Weight | Line Height |
|---------|------|--------|-------------|
| Page title (h1) | text-4xl / text-5xl | font-semibold | leading-tight |
| Section heading (h2) | text-3xl | font-semibold | leading-tight |
| Subsection (h3) | text-2xl | font-semibold | leading-snug |
| Card title (h4) | text-xl | font-medium | leading-snug |
| Body text | text-base | font-normal | leading-relaxed |
| Small/caption | text-sm | font-normal | leading-normal |
| Overline/label | text-xs uppercase | font-medium tracking-wide | leading-normal |

### Color Usage
| Purpose | Typical Tailwind Class |
|---------|----------------------|
| Primary action | bg-primary text-primary-foreground |
| Secondary action | bg-secondary text-secondary-foreground |
| Destructive action | bg-destructive text-destructive-foreground |
| Page background | bg-background |
| Card/surface | bg-card |
| Primary text | text-foreground |
| Secondary text | text-muted-foreground |
| Borders | border-border |

### Component Patterns

**Cards:** `rounded-xl border bg-card p-6 shadow-sm`
**Buttons (primary):** `rounded-lg bg-primary px-4 py-2 text-sm font-medium text-primary-foreground hover:bg-primary/90 transition-colors`
**Buttons (secondary):** `rounded-lg border bg-background px-4 py-2 text-sm font-medium hover:bg-accent transition-colors`
**Inputs:** `rounded-md border bg-background px-3 py-2 text-sm ring-offset-background focus:outline-none focus:ring-2 focus:ring-ring`
**Badges:** `rounded-full bg-primary/10 px-2.5 py-0.5 text-xs font-medium text-primary`

## Detailed Reference

For comprehensive design principles, layout patterns, and accessibility guidelines, read `references/design-principles.md`.
