# Design Principles — Complete Reference

Comprehensive design knowledge for building professional interfaces. Read this when generating components or reviewing UI.

## Visual Hierarchy

### Size Contrast
Adjacent text sizes should differ by at least 1 step on the type scale. If two pieces of text look like they might be the same size, they will feel ambiguous — make one clearly bigger or clearly smaller.

Bad: h2 at 24px and body at 20px (too close)
Good: h2 at 30px and body at 16px (clear distinction)

### Weight as Emphasis
Use font-semibold for headings and font-normal for body. Reserve font-bold for inline emphasis only. Too much bold makes nothing stand out.

### Color as Signal
Use your primary color sparingly — only for interactive elements (buttons, links, active states) and key data points. If everything is blue, nothing is blue.

### Visual Weight Order
Elements on screen should have a clear weight hierarchy:
1. Primary heading (largest, darkest)
2. Primary CTA button (color + size)
3. Secondary headings
4. Body text
5. Secondary/tertiary text (muted)
6. Borders and dividers (lightest)

## Layout Patterns

### The Content Width Rule
Body text should never span more than 65-75 characters per line. On wide screens, constrain content with `max-w-prose` (65ch) or `max-w-2xl` (42rem). Full-width text is exhausting to read.

### The Container Pattern
```
Full-width background color/image
  └── max-w-7xl mx-auto px-6 (content constrained to 1280px with side padding)
       └── Content grid/flex layout
```

This gives you edge-to-edge color sections with readable centered content.

### Grid Layouts
- 1 column: mobile default, long-form content
- 2 columns: side-by-side comparison, content + sidebar
- 3 columns: feature grids, card layouts, pricing tables
- 4 columns: icon grids, logo bars, footer link groups

Always collapse to 1 column on mobile (`grid-cols-1 md:grid-cols-2 lg:grid-cols-3`).

### Vertical Rhythm
Maintain consistent vertical spacing between sections. A common pattern:
- Between sections: `py-16` (desktop), `py-10` (mobile)
- Between section heading and content: `mt-8` or `mt-12`
- Between cards in a grid: `gap-6`
- Between text blocks: `space-y-4`

## Color Theory for UI

### Building a Palette
Start with one primary color. Derive everything else:
- **Primary shades**: Generate a 50-950 scale (use oklch or Tailwind's default scale as reference)
- **Neutral shades**: Use a slightly tinted gray (warm gray for warm primaries, cool gray for cool)
- **Semantic colors**: success (green), warning (amber), error (red) — keep these standard, don't reinvent

### Dark Mode
Swap, don't invert:
- Background: not pure black (#000), use #0A0A0A to #1A1A1A
- Text: not pure white (#FFF), use #FAFAFA to #E5E5E5
- Reduce elevation shadows, increase border visibility
- Primary color may need a lighter tint for dark backgrounds

### Contrast Requirements (WCAG AA)
- Normal text (under 18px): 4.5:1 contrast ratio minimum
- Large text (18px+ bold, or 24px+): 3:1 contrast ratio minimum
- UI components (borders, icons): 3:1 contrast ratio minimum
- Test with: WebAIM Contrast Checker or browser DevTools

## Responsive Design

### Mobile-First Approach
Write base styles for mobile (375px), then add breakpoints upward:
```
text-base md:text-lg lg:text-xl    ← starts small, grows
grid-cols-1 md:grid-cols-2 lg:grid-cols-3  ← starts single, adds columns
p-4 md:p-6 lg:p-8                  ← starts tight, grows
```

### Breakpoint Guide
| Breakpoint | Tailwind | Typical Device | Layout Shift |
|-----------|----------|----------------|-------------|
| Default | (none) | Phone (375px) | Single column, stacked |
| sm | sm: | Large phone (640px) | Minor adjustments |
| md | md: | Tablet (768px) | 2 columns, sidebar appears |
| lg | lg: | Laptop (1024px) | Full layout, larger spacing |
| xl | xl: | Desktop (1280px) | Max-width container kicks in |

### Touch Targets
Interactive elements on mobile must be at least 44x44px. This means:
- Buttons: min-h-[44px] on mobile
- Links in nav: adequate padding
- Form inputs: py-3 minimum on mobile
- Icon buttons: p-3 minimum

## Accessibility

### Semantic HTML
| Content | Use | Not |
|---------|-----|-----|
| Navigation | `<nav>` | `<div className="nav">` |
| Main content | `<main>` | `<div className="main">` |
| Sections | `<section>` with heading | `<div>` |
| Articles/cards | `<article>` | `<div>` |
| Lists | `<ul>` / `<ol>` | `<div>` with bullet characters |
| Buttons | `<button>` | `<div onClick>` |

### ARIA Essentials
- Images: `alt="descriptive text"` (empty `alt=""` for decorative images)
- Icon buttons: `aria-label="Close menu"`
- Form inputs: always a `<label>` with `htmlFor` linking
- Loading states: `aria-busy="true"`, `aria-live="polite"` for dynamic content
- Modals: `role="dialog"`, `aria-modal="true"`, trap focus inside

### Keyboard Navigation
- All interactive elements must be reachable with Tab
- Focus state must be visible (`focus:ring-2 focus:ring-ring focus:ring-offset-2`)
- Escape key should close modals, dropdowns, popovers
- Enter/Space should activate buttons and links

### Color Independence
Never use color as the only way to convey information:
- Error states: red color + icon + text message
- Success states: green color + checkmark + text
- Form validation: red border + error message text below the input
- Status indicators: color + label text

## Common Component Patterns

### Hero Section
- Full-width with constrained content
- Clear heading hierarchy: big title, medium subtitle, visible CTA
- Minimum `py-24` vertical padding (feels spacious, not cramped)
- On mobile, stack content vertically with `text-center`

### Navigation
- Sticky on desktop (`sticky top-0 z-50 bg-background/80 backdrop-blur`)
- Hamburger menu on mobile (below md: breakpoint)
- Logo left, links center or right, CTA far right
- Active page indicator (underline or color change)

### Card Grid
- Consistent card heights in a row (use grid, not flexbox)
- Same padding, border-radius, shadow across all cards
- Clear visual separation (border or shadow, not both)
- Image aspect ratio locked (`aspect-video` or `aspect-square`)

### Forms
- Labels above inputs (not inside as placeholder text)
- Error messages below the input, in red with icon
- Disabled submit button until required fields are filled
- Logical tab order
- Group related fields with fieldset

### Footer
- Simpler than the header — don't overwhelm
- Link groups in columns on desktop, stacked on mobile
- Copyright line at bottom
- Contrast with page body (slightly different background)

## Anti-Patterns to Avoid

1. **Walls of text** — Break up with headings, whitespace, images, or pull quotes
2. **Too many font sizes** — Stick to 4-5 from the type scale
3. **Inconsistent spacing** — Use the spacing scale, never arbitrary values
4. **Low-contrast text** — Gray on white is not classy, it's unreadable
5. **Centered everything** — Left-align body text; center only headings and short CTAs
6. **Decorative borders AND shadows** — Pick one visual separator, not both
7. **Icon overload** — Icons should clarify, not decorate. If the icon doesn't add meaning, remove it
8. **Tiny click targets** — Especially on mobile; pad generously
9. **Inconsistent hover states** — Every interactive element needs a visible hover/focus state
10. **No empty states** — Every list, table, and content area needs a "nothing here yet" state
