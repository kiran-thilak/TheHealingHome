---
description: Check files against TheHealingHome coding standards
---

Review $ARGUMENTS (or run `git diff --name-only HEAD` to get changed files if no argument given).

For each file, check the following and report as a checklist:

## TypeScript / Props
- [ ] No `any` type
- [ ] No `@ts-ignore` without explanation comment
- [ ] Components have `interface Props {}` in frontmatter
- [ ] All props are destructured from `Astro.props`

## Imports
- [ ] Uses `@/` path alias — no relative `../` imports
- [ ] No unused imports

## Styling
- [ ] No inline `style=""` attributes
- [ ] Conditional classes use `class:list`, not string templates
- [ ] Custom CSS is in `global.css`, not in `<style>` blocks (unless component-scoped and justified)

## Accessibility
- [ ] Every `<img>` has a non-empty `alt` attribute
- [ ] Every form `<input>` and `<textarea>` has a paired `<label for="...">`
- [ ] `<section>` elements have `aria-labelledby` pointing to their heading
- [ ] Heading hierarchy is logical (no skipped levels, one `<h1>` per page)

## Naming
- [ ] Component files: `PascalCase.astro`
- [ ] Page files: `kebab-case.astro`

## Output format
List each file. Under each, show only the checks that **fail** (skip passing ones to save space).
Label each finding: **BLOCKER** (must fix before merge) or **SUGGESTION** (nice to fix).
Do NOT auto-fix — user runs `/code-review --fix` for automated fixes.
