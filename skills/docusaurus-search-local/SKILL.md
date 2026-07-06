---
name: docusaurus-search-local
description: Adds fully offline local full-text search to a Docusaurus v2/v3 + TypeScript project using @easyops-cn/docusaurus-search-local, with typed config and no external search service.
license: MIT
compatibility: "Claude Code, Cursor, or any agentic coding assistant with file read/write and terminal access inside a Docusaurus TypeScript project."
metadata:
  author: Luis Miguel Báez (LuchoBazz)
  version: "1.0"
---

# Docusaurus Local Search Integration

## Goal
Add `@easyops-cn/docusaurus-search-local` so the site has an offline search bar, no Algolia required.

---

## Steps

### 1. Detect package manager and install
Check for `package-lock.json` / `yarn.lock` / `pnpm-lock.yaml`, then run the matching command:
```bash
npm install --save @easyops-cn/docusaurus-search-local
# yarn add @easyops-cn/docusaurus-search-local
# pnpm add @easyops-cn/docusaurus-search-local
```

### 2. Register in `docusaurus.config.ts`
Import the type and append to the `themes` array (do not overwrite existing themes):
```ts
import type { PluginOptions as SearchLocalOptions } from '@easyops-cn/docusaurus-search-local';

// inside the config object:
themes: [
  [
    require.resolve('@easyops-cn/docusaurus-search-local'),
    {
      hashed: true,
      language: ['en'],       // match i18n.locales that have real content
      indexDocs: true,
      indexBlog: true,
      indexPages: false,
      searchBarShortcutHint: true,
      searchResultLimits: 8,
    } satisfies SearchLocalOptions,
  ],
],
```
Inspect the `i18n` block in the existing config — if it declares multiple locales with translated content, add all of them to `language`.

### 3. Key options reference

| Option | When to set |
|---|---|
| `hashed: true` | Always — enables long-term cache busting |
| `language` | All locales with real translated docs |
| `indexPages: true` | Only if static pages have meaningful searchable content |
| `docsRouteBasePath` / `blogRouteBasePath` | Only if docs/blog are on non-default paths |
| `highlightSearchTermsOnTargetPage` | Optional UX improvement |
| `explicitSearchResultPath` | Helps disambiguate similarly-titled sections |

### 4. Verify
```bash
npm run build   # must pass with no type errors
npm run start   # search bar must render; shortcut opens modal; results navigate correctly
```
If the build fails with `Module not found: Can't resolve '@docusaurus/useRouteContext'`, align the plugin version with the installed `@docusaurus/core` version per the [compatibility table](https://github.com/easyops-cn/docusaurus-search-local#readme).

### 5. Optional: style overrides
Only if requested — add to `src/css/custom.css`:
```css
:root { --search-local-modal-width: 480px; }
html[data-theme='dark'] { --search-local-highlight-color: #d23669; }
```

---

## Checklist
- [ ] Package in `package.json` dependencies
- [ ] Theme registered with typed options (`satisfies PluginOptions`, no `any`)
- [ ] `language` matches actual content locales
- [ ] `hashed: true` set
- [ ] Build and dev server pass with no errors
- [ ] Search bar renders, shortcut works, results are relevant
