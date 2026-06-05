# TheHealingHome — Agent Briefing

## Stack
- **Astro 5** — static output, all pages pre-rendered
- **Tailwind CSS 4** — via `@tailwindcss/vite` Vite plugin (no `tailwind.config.js`)
- **TypeScript** — `astro/tsconfigs/strictest`
- **pnpm 9** — package manager
- **Vercel** — hosting, auto-deploys on push to `main`

## Path Alias
`@/` maps to `src/` — always use it, never relative paths (e.g. `@/components/ui/Button.astro`)

## Component Conventions
- File name: `PascalCase.astro` in `src/components/<category>/`
- Every component has a typed `interface Props {}` block in frontmatter
- No default exports except pages/layouts (Astro enforces this)
- Use `class:list` not template literals for conditional classes
- Slots: use `<slot />` for single content areas, named slots (`<slot name="x" />`) for multiple

## Page Conventions
- File name: `kebab-case.astro` in `src/pages/`
- Every page imports and wraps content in `BaseLayout` from `@/layouts/BaseLayout.astro`
- Always pass `title` prop; pass `description` when it differs from the default

## Styling
- Tailwind utilities only — no inline `style=""` attributes
- Design tokens (colors, fonts, spacing): defined in `@theme {}` block in `src/styles/global.css`
- Custom CSS only when a utility cannot express the requirement

## TypeScript
- No `any`
- No `@ts-ignore` without a comment explaining why
- No unused imports or variables (ESLint catches this)

## Git
- Conventional commits: `feat:` `fix:` `chore:` `docs:` `style:` `refactor:` `test:`
- Every PR must pass CI (lint + typecheck + build) before merge

## Accessibility
- Every `<img>` needs `alt`
- Every form `<input>` and `<textarea>` needs a paired `<label>`
- Use semantic elements: `<header>`, `<nav>`, `<main>`, `<footer>`, `<section>`, `<article>`
- Sections use `aria-labelledby` pointing to their heading id

## Current Phase
**Phase 1** — Framework only. All pages have placeholder `[Content — Phase 2]` text.
Do not add real design or content during Phase 1.

## Agents (slash commands)
- `/scaffold <type> <name>` — generate new page or component
- `/integrate` — convert client HTML/CSS into Astro components
- `/review [file]` — check against coding standards

## Scripts
```
pnpm dev        # local dev server
pnpm build      # production build → dist/
pnpm typecheck  # tsc --noEmit
pnpm lint       # ESLint
pnpm format     # Prettier
```
