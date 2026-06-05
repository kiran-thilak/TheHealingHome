# TheHealingHome — Project Design Document

**Version:** 1.0 | **Date:** 2026-06-05 | **Stack:** Astro 5 + Tailwind CSS v4 + TypeScript + GitHub + Vercel

---

## 1. Project Overview

A production-ready marketing site for a client. Built in two phases:

- **Phase 1** — Framework: routing, CI/CD, tooling, coding standards. Minimal placeholder content on every route.
- **Phase 2** — Design: client-provided HTML/CSS reference integrated into Astro components.

---

## 2. Tech Stack

| Layer | Tool | Version |
|---|---|---|
| Framework | Astro | ^5.x |
| Styling | Tailwind CSS | ^4.x |
| Language | TypeScript | ^5.x |
| Package Manager | pnpm | ^9.x |
| Linting | ESLint + eslint-plugin-astro | latest |
| Formatting | Prettier + prettier-plugin-astro | latest |
| Source Control | GitHub | — |
| Hosting / CI | Vercel | — |
| Preview Deploys | Vercel PR previews | built-in |

**Why Astro:** Zero JS by default, content-first, perfect for marketing sites. Islands architecture for interactive components.
**Why Tailwind v4:** Native CSS variables, no config file needed, smaller output.
**Why pnpm:** Faster installs, strict dependency resolution, disk efficient.

---

## 3. Directory Structure

```
TheHealingHome/
├── .claude/
│   ├── settings.json          # Claude Code permissions
│   └── commands/              # Reusable slash commands (agents)
│       ├── scaffold.md        # /scaffold — generate page/component
│       ├── integrate.md       # /integrate — merge HTML design into Astro
│       └── review.md          # /review — standards check
├── .github/
│   └── workflows/
│       └── ci.yml             # Lint + type-check on PR
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Navbar.astro
│   │   │   └── Footer.astro
│   │   └── ui/
│   │       ├── Button.astro
│   │       └── NewsletterForm.astro
│   ├── layouts/
│   │   └── BaseLayout.astro   # <head>, Navbar, Footer, slot
│   ├── pages/
│   │   ├── index.astro        # / — Landing
│   │   ├── about.astro        # /about
│   │   ├── contact.astro      # /contact
│   │   └── 404.astro          # /404
│   └── styles/
│       └── global.css         # Tailwind imports + CSS custom properties
├── .eslintrc.cjs
├── .prettierrc
├── astro.config.mjs
├── tsconfig.json
├── vercel.json
├── pnpm-lock.yaml
├── package.json
├── PLAN.md                    # This file
└── CLAUDE.md                  # Agent briefing (coding standards + context)
```

---

## 4. Routes

| Route | File | Purpose |
|---|---|---|
| `/` | `index.astro` | Hero, value prop, CTA, newsletter |
| `/about` | `about.astro` | Who we are, team, mission |
| `/contact` | `contact.astro` | Contact form, address, social |
| `/404` | `404.astro` | Custom not-found page |

All pages use `BaseLayout.astro` which wraps content with `<Navbar>` and `<Footer>`.

---

## 5. CI/CD Pipeline

```
Developer pushes branch
       │
       ▼
GitHub Actions CI (ci.yml)
  ├── pnpm install
  ├── pnpm lint          (ESLint)
  ├── pnpm typecheck     (tsc --noEmit)
  └── pnpm build         (Astro build — catches SSG errors)
       │
       ▼ PR opened
Vercel Preview Deploy   ← automatic, unique URL per PR
       │
       ▼ Merged to main
Vercel Production Deploy → thehealinghome.com
```

**Vercel settings (vercel.json):**
- Framework: Astro
- Build command: `pnpm build`
- Output: `dist/`
- Node: 20.x

**Branch strategy:**
- `main` → production
- `dev` → integration (optional staging)
- `feat/*`, `fix/*`, `chore/*` → feature branches → PR → main

---

## 6. Coding Standards

### General
- TypeScript strict mode. No `any`. No `@ts-ignore` without comment explaining why.
- All files use named exports (no default exports except Astro pages/layouts — Astro requires default).
- No unused imports or variables. ESLint enforces this.
- Prefer `const`. Never `var`.

### Astro
- One component per file. File name matches exported concept (`Button.astro`, not `Btn.astro`).
- Props typed with `interface Props {}` inside the component frontmatter.
- No inline styles. All styling via Tailwind utility classes or `global.css` custom properties.
- Use `<slot />` in layouts, named slots for optional regions.
- Images via Astro's `<Image />` component for automatic optimization.

### Tailwind
- Utility-first. No custom CSS unless a design token or animation cannot be expressed as utilities.
- Custom colors/tokens go into `global.css` as CSS custom properties (`--color-brand-primary`), referenced in Tailwind config.
- No `@apply` except for highly-repeated base element resets.

### Git
- Conventional commits: `feat:`, `fix:`, `chore:`, `docs:`, `style:`, `refactor:`, `test:`
- PR title = commit message used on squash merge.
- Every PR must pass CI before merge.

### File Naming
- Astro components: `PascalCase.astro`
- CSS/config files: `kebab-case`
- TypeScript utilities: `camelCase.ts`

### Comments
- Only comment the non-obvious: a hidden constraint, a browser quirk, an Astro-specific workaround.
- No JSDoc blocks on Astro components (Props interface is self-documenting).

---

## 7. Agents (Claude Code Slash Commands)

Three custom commands live in `.claude/commands/`. They load project context from `CLAUDE.md` automatically.

### `/scaffold <type> <name>`
**Purpose:** Generate a new page or component from a template.
**Usage:** `/scaffold page about` or `/scaffold component NewsletterForm`
**What it does:**
1. Creates the file in the correct directory
2. Applies correct Props interface pattern
3. Wraps pages in `BaseLayout`
4. Adds to route table in `PLAN.md`

### `/integrate`
**Purpose:** Take a client-provided HTML/CSS design file and integrate it into an Astro component or page.
**Usage:** Paste raw HTML into chat, then `/integrate`
**What it does:**
1. Identifies reusable chunks → extracts into components
2. Converts inline styles → Tailwind utility classes
3. Replaces hardcoded text with Astro props/slots
4. Preserves semantic HTML

### `/review`
**Purpose:** Quick standards check on changed files.
**Usage:** `/review` (checks git diff) or `/review src/components/Navbar.astro`
**What it does:**
1. Checks against coding standards in this document
2. Flags TypeScript issues, accessibility gaps (`alt`, ARIA), missing Props types
3. Reports findings — does not auto-fix (run `/code-review --fix` for that)

---

## 8. Token-Saving Strategies

These reduce Claude Code token consumption during development:

| Strategy | How |
|---|---|
| **CLAUDE.md** | Project context loaded once per session — no need to re-explain stack or standards in chat |
| **Focused reads** | Tell Claude the exact file + line range, not "look at the project" |
| **`/scaffold` agent** | Generates boilerplate from template — no back-and-forth for repetitive structure |
| **Minimal Phase 1** | No design during framework phase — less code to generate and review |
| **Component isolation** | Work on one component at a time; don't load the whole codebase |
| **Type-first** | Define the Props interface first, then ask Claude to fill the template — short prompt, targeted output |
| **`pnpm build` fast feedback** | Astro's build catches errors before a long review loop |
| **Vercel previews** | Visual verification via URL — no need to describe desired layout in text |

---

## 9. Phase 1 Checklist

> Goal: Every route loads, CI passes, Vercel deploys. Zero design work.

- [ ] Init Astro project (`pnpm create astro@latest`)
- [ ] Add Tailwind CSS integration (`astro add tailwind`)
- [ ] Configure TypeScript strict mode
- [ ] Add ESLint + Prettier + Astro plugins
- [ ] Create `BaseLayout.astro` with `<Navbar>` and `<Footer>` stubs
- [ ] Create all 4 pages (`index`, `about`, `contact`, `404`) with placeholder `<h1>`
- [ ] Create `Button.astro` and `NewsletterForm.astro` stubs
- [ ] Configure `vercel.json`
- [ ] Write `ci.yml` GitHub Actions workflow
- [ ] Push to GitHub, connect Vercel, confirm production deploy
- [ ] Confirm 404 route works on Vercel
- [ ] Add `.claude/commands/` with the 3 agent files
- [ ] Write `CLAUDE.md` with project briefing

---

## 10. Phase 2 Checklist

> Goal: Client design integrated. Site is pixel-accurate and production-polished.

- [ ] Receive client HTML/CSS reference
- [ ] Extract design tokens (colors, fonts, spacing) → `global.css` CSS custom properties
- [ ] Run `/integrate` on each page design
- [ ] Implement responsive breakpoints (mobile-first)
- [ ] Implement newsletter form (static: form action or Resend/Mailchimp endpoint)
- [ ] Implement contact form (static: Formspree or similar)
- [ ] Add `<Image />` optimization for all photos
- [ ] Audit accessibility: headings hierarchy, `alt` text, focus states, color contrast
- [ ] Add Open Graph / Twitter meta tags in `BaseLayout`
- [ ] Add `sitemap.xml` via Astro sitemap integration
- [ ] Add `robots.txt`
- [ ] Run Lighthouse audit — target 95+ on all metrics
- [ ] Final `/review` pass on all components
- [ ] Tag release `v1.0.0`

---

## 11. Key Config Snippets (Reference)

### `astro.config.mjs`
```js
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  site: 'https://thehealinghome.com',
  integrations: [tailwind(), sitemap()],
  output: 'static',
});
```

### `tsconfig.json`
```json
{
  "extends": "astro/tsconfigs/strictest",
  "compilerOptions": {
    "baseUrl": ".",
    "paths": { "@/*": ["src/*"] }
  }
}
```

### `vercel.json`
```json
{
  "framework": "astro",
  "buildCommand": "pnpm build",
  "outputDirectory": "dist",
  "installCommand": "pnpm install"
}
```

### `CLAUDE.md` (summary — full file written during Phase 1)
```markdown
# TheHealingHome — Agent Briefing

Stack: Astro 5, Tailwind 4, TypeScript strict, pnpm, Vercel.
All pages use BaseLayout. Props typed with `interface Props {}`.
No default exports except pages/layouts. Conventional commits.
Coding standards: see PLAN.md §6.
When generating components, check PLAN.md §3 for file placement.
```

---

*Next step: confirm this plan, then execute Phase 1.*
