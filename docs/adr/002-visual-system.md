# ADR 002: Visual system and theme architecture

**Date:** 2026-08-27
**Status:** Accepted. The architecture is still in force; several details changed later (see [What changed since](#what-changed-since)).
**Affects:** the whole interface layer
**Source:** Visual & UX Design Brief, MakerHub

## Context

Until then, the interface was a minimal skeleton with a provisional palette. The brief set the final visual identity and added a product requirement: users should be able to personalise the look from Settings, through predefined themes and a configurable accent colour.

## Fit with the approved architecture

The brief's 36 points were checked against the existing architecture. **There was no architectural conflict.** The brief is a presentation layer, and the Main / Preload / Renderer split keeps it naturally decoupled.

Points that fitted without changes:

| Brief | Status |
| --- | --- |
| Desktop app, not web | Already Electron with a native window |
| Dark mode structured but not premature | Tokens already used `prefers-color-scheme` |
| Persist user preferences | `app_settings` and the `settings.get/set` IPC already existed |
| No new UI frameworks | Tailwind 4 and in-house components, already in place |
| Semantic tokens, nothing hard-coded | `@theme inline` over CSS variables, already in use |
| Keyboard shortcuts, Cmd+K | The renderer can capture them without touching the core |
| Context menus | Supported natively by Electron |
| File drag and drop | Compatible with the IPC contract: the renderer gets paths from the drop event and sends them to the import channel |

## Real conflicts found

### 1. The palette was terracotta-first; the brief asked for green-first

The first palette used terracotta (`#c65a1e`) as the action colour. The brief makes green the primary family and terracotta an **accent**, not a competing colour.

**Resolution:** replace the token layer. Not an architectural change: components already consumed semantic tokens (`bg-surface`, `text-accent`), not literal values, so rewriting the token file was enough. No component was touched for this.

### 2. `app.setName('MakerHub')` cannot be centralised

The brief asks for the visible name to be configurable. Two things happen to share the same text:

- **Visible name** (sidebar, window title, texts): centralised in `src/shared/branding.ts`.
- **Application identifier** (`app.setName`, `appId`, `productName`, package name): **stays fixed**. `app.setName()` decides the `userData` path, where the user's database lives. Changing it would make the app lose its own library.

The brief itself backs this: *"Do not rename the repository, package or application identifier unless explicitly instructed."*

### 3. Language

The brief's text examples were in English, while the interface was in Spanish at the time. The decision then was to keep Spanish and read the examples as guidance on *tone* (calm, concrete, actionable). This was reversed shortly after: see below.

## Dependency decisions

Both approved. Details in [branding.md](../branding.md).

1. **Inter typeface, bundled locally.** The `.woff2` files live in `src/renderer/src/assets/fonts/`, not on Google Fonts. Variable font (one file, weights 100–900) and two subsets via `unicode-range`: `latin` (47 KB) and `latin-ext` (83 KB, loaded only when a file name needs it). SIL OFL 1.1, licence included.
2. **Lucide icons as the main system.** Every icon goes through `components/Icon.tsx`, which sets size and stroke width once. No component imports an icon directly.
3. **Mascot: a snail with a spool for a shell.** Direction recorded in branding.md. Not implemented at the time.

## Theme architecture

Requirement: light / dark / system themes, several predefined palettes, a configurable accent with automatic variants, guaranteed contrast and a persisted preference.

### Layers

```
1. Raw palette        hex values, only inside the token file
2. Semantic tokens    --color-primary, --color-surface, --color-text...
3. Tailwind utilities @theme inline maps tokens to classes
4. Components         consume ONLY classes/tokens, never hex
```

A component cannot know which colour is "green". It only knows that something is `primary` or `surface`. That is what lets the theme change without refactoring the interface.

### Choosing a theme

Two independent attributes on the root element:

```html
<html data-theme="light|dark"   <!-- absent = follow the system -->
      data-preset="...">
```

- No `data-theme` → `prefers-color-scheme` decides. This is "System" mode.
- `data-theme` present → it wins over the system, in both directions.
- `data-preset` only changes the primary family tokens; everything else (surfaces, text, borders) is shared.

### Automatic accent variants

With CSS `color-mix()`, which Chromium supports natively. **No colour-manipulation dependency:**

```css
--color-primary-hover:  color-mix(in oklab, var(--color-primary) 88%, black);
--color-primary-active: color-mix(in oklab, var(--color-primary) 78%, black);
--color-primary-subtle: color-mix(in oklab, var(--color-primary) 14%, var(--color-surface));
```

Mixing happens in `oklab` rather than sRGB because it is perceptually uniform: it avoids the muddy tones you get when darkening a colour in sRGB.

### Contrast

`--color-primary-contrast` (the text that sits ON the primary colour) is set explicitly for each palette and theme. It is not derived automatically: deriving contrast blindly is exactly how accessibility breaks when a palette changes.

In dark mode, which green is the interactive one is swapped on purpose: a deep green has too little contrast on a dark background, so a light sage takes its place. It is not a mechanical inversion.

### Persistence

The source of truth is `app_settings`, with the keys `appearance.theme` (`light`, `dark` or `system`) and `appearance.preset`.

Reading settings over IPC is asynchronous, so applying the theme after the first paint would cause a flash. The plan was to mirror the preference in `localStorage` and apply it synchronously when the renderer starts, with SQLite as the source of truth and `localStorage` as a cache.

## What was done then, and what wasn't

**Then:** a complete semantic token layer with the brief's palette; the theme structure (`data-theme`, `data-preset`) with five palettes defined but no interface to switch them yet; the visible name centralised in `shared/branding.ts`.

**Not then:** a personalisation screen in Settings, a colour picker, redesigning screens outside the current phase, any new dependency.

## Consequence for later work

Every new interface is built against semantic tokens. A hex value inside a component is a review error, not a matter of taste.

## What changed since

Checked against the code in October 2026.

- **The interface is in English.** It switched on the same day this ADR was written (27 August 2026).
- **Three palettes: Forest, Terracotta and Rose.** The original five (sage, forest, olive, moss, terracotta) were reworked during August and September and cut down to three in September 2026. Retired palettes are still recognised: anyone who had one saved is moved to the closest survivor (sage, olive, moss and slate to Forest; clay and linen to Terracotta). See `RETIRED_PRESETS` in `src/shared/contracts/appearance.contract.ts`.
- **Each palette has a second accent.** Forest pairs green with terracotta, Terracotta pairs terracotta with green, and Rose pairs dusty pink with ochre. Values are in [branding.md](../branding.md).
- **Defaults:** theme `system`, palette `forest`.
- **No custom accent colour.** The planned `appearance.accent` setting and colour picker were never built; three well-separated palettes covered the need.
- **Settings › Appearance exists**, with the theme, the palette and a live preview.
- **The no-flash start-up works as planned:** `localStorage` caches the appearance and is applied before the first paint (`src/renderer/src/app/theme.ts`).
- **Accessibility became automated:** WCAG 2.2 AA contrast, the type scale and the separation of status colours for colour-blind users are checked by tests.
- **The mascot was implemented,** with AI-generated illustrations. See [branding.md](../branding.md).

---

*Translated from Spanish in October 2026, with the section above added.*
