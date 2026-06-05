---
description: Generate a new Astro page or component following project conventions
---

Generate a new $ARGUMENTS for TheHealingHome following these rules:

## For `page <name>`
Create `src/pages/<name>.astro`:
- Import BaseLayout from `@/layouts/BaseLayout.astro`
- Wrap all content in `<BaseLayout title="..." description="...">`
- Add a `<section aria-labelledby="<name>-heading">` with `<h1 id="<name>-heading">`
- Placeholder body text: `[Content — Phase 2]`
- No styling in Phase 1

## For `component <category>/<name>`
Create `src/components/<category>/<name>.astro`:
- Add `interface Props {}` with typed props in frontmatter
- Use `<slot />` for content areas
- Export nothing (Astro components have implicit default export)
- No inline styles

## Rules (always apply)
- Path alias `@/` for all src imports — never relative paths
- `class:list` for conditional classes, never string templates
- Props must be destructured from `Astro.props` with defaults where sensible
- Mark placeholder content areas with `[Content — Phase 2]` or `[Phase 2]` comments

After generating, remind the user: update PLAN.md route table if a new page was created.
