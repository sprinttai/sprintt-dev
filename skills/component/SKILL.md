---
name: component
description: "Generate a polished UI component following brand guidelines. Use when the user says create a component, build a hero section, make a navbar, generate a form, add a card component, or asks for any UI element."
---

Generate a production-quality UI component based on the project's brand identity and design system.

## Step 1: Load Brand Context

Read `.claude/brand.md` if it exists. All generated components must follow these guidelines.

If `.claude/brand.md` doesn't exist, use professional defaults and warn the user to run `/sprintt-dev:init` or create a brand file.

Also read `${CLAUDE_PLUGIN_ROOT}/skills/design-system/references/design-principles.md` for design principles.

## Step 2: Understand the Request

Parse `$ARGUMENTS` to understand what component is needed. Common types:

- **Layout**: hero, header, navbar, footer, sidebar, page-wrapper
- **Content**: card, feature-grid, testimonial, pricing-table, FAQ, timeline
- **Forms**: contact-form, login, signup, search-bar, newsletter-signup
- **Navigation**: breadcrumb, tabs, pagination, dropdown-menu
- **Feedback**: alert, toast, modal, loading-spinner, empty-state
- **Data**: data-table, stats-card, chart-wrapper, avatar, badge

If the request is ambiguous, ask the user to clarify.

## Step 3: Generate Component

Create a React component (TypeScript + Tailwind CSS) that:

1. **Follows brand.md** — uses the documented colors, font sizes, spacing rhythm, and border radius conventions
2. **Is accessible** — proper semantic HTML, ARIA labels where needed, keyboard navigable, sufficient color contrast (4.5:1 minimum)
3. **Is responsive** — mobile-first, works at all breakpoints
4. **Uses shadcn/ui patterns** — if shadcn is installed, use its primitives. If not, build with Radix-like patterns
5. **Is self-contained** — component + types in one file, no external deps beyond React and Tailwind
6. **Has sensible props** — typed with TypeScript, defaults for optional props

**File naming**: PascalCase, placed in `src/components/` (or `src/components/ui/` for primitives).

Example output for a "hero section":

```tsx
interface HeroProps {
  title: string;
  subtitle?: string;
  ctaText?: string;
  ctaHref?: string;
}

export function Hero({
  title,
  subtitle,
  ctaText = "Get Started",
  ctaHref = "#",
}: HeroProps) {
  return (
    <section className="py-24 px-6 text-center">
      <h1 className="text-4xl font-semibold tracking-tight text-foreground sm:text-5xl">
        {title}
      </h1>
      {subtitle && (
        <p className="mt-6 text-lg text-muted-foreground max-w-2xl mx-auto">
          {subtitle}
        </p>
      )}
      <div className="mt-10">
        <a
          href={ctaHref}
          className="inline-flex items-center px-6 py-3 rounded-lg bg-primary text-primary-foreground font-medium hover:bg-primary/90 transition-colors"
        >
          {ctaText}
        </a>
      </div>
    </section>
  );
}
```

## Step 4: Confirm

Tell the user: file path created, props interface, usage example, and any missing dependencies.
