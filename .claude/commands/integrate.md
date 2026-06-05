---
description: Convert client-provided HTML/CSS into Astro components
---

The user has provided an HTML/CSS design. Integrate it into TheHealingHome following these steps in order:

## Step 1 — Analyse
Read the HTML structure. Identify:
- Layout regions (header, hero, feature sections, footer)
- Repeating elements (cards, buttons, nav links, list items)
- Design tokens (brand colors, font families, spacing values)

## Step 2 — Extract design tokens
Move all colors, font stacks, and spacing values into `@theme {}` in `src/styles/global.css`.
Use CSS custom property naming: `--color-brand-*`, `--font-*`, `--spacing-*`.

## Step 3 — Extract components
Any element that repeats or appears on multiple pages becomes a component in `src/components/ui/`.
Add typed `interface Props {}` and use `<slot />` for variable content.

## Step 4 — Convert styles
- Inline styles → Tailwind utility classes
- Custom CSS classes → Tailwind utilities where a direct mapping exists
- Remaining custom CSS → `src/styles/global.css` in `@layer components {}`

## Step 5 — Preserve semantics
Keep semantic HTML. Do not replace `<section>`, `<article>`, `<nav>` with `<div>`.
Ensure all `<section>` elements have `aria-labelledby` pointing to their heading.

## Step 6 — Accessibility pass
- Every `<img>` has `alt`
- Every form control has a paired `<label>`
- Heading hierarchy is logical (one `<h1>`, no skipped levels)
- Interactive elements are keyboard-accessible

## Step 7 — Output
Write modified/created files one at a time. After all files, run `pnpm build` to confirm no errors.
