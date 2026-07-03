# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A WordPress install on **Local by Flywheel**. The only thing being actively built here is a
**block-native theme** at `wp-content/themes/indexworld-blocks/` for **IndexWorld** (an Odoo
implementation partner). The site is a pixel-perfect rebuild of **staging.indexworld.net**. No page
builder / Elementor — the whole point is a fast theme that is "controlled by AI" through plain block
markup, patterns, and `theme.json`.

Primary working directory: `app/public/`. The theme is the project; almost all edits happen under
`wp-content/themes/indexworld-blocks/`.

## Hard constraints

- **Staging is READ-ONLY.** Never write to staging through any tool. It is the source-of-truth
  design reference, fetched over HTTP only.
- **Two MCP connectors:** `royal-mcp` = staging (READ-ONLY, never write); `royal-mcp-local` = the
  local WP site (safe to create posts/pages/media).
- **Beginner pacing.** The site owner is not a developer. Guide one small, concrete step at a time
  and wait for confirmation before moving on. Match staging pixel-for-pixel.
- **Build scope is fixed:** finish homepage → Contact Us page → About Us page → then STOP and wait.
  (See `memory/build-scope-plan.md`.)

## Architecture

Block theme following the standard WP template hierarchy (`front-page` → `home` → `index`;
`page` → `index`). Key pieces:

- `theme.json` (v3) — all design tokens: palette (primary `#50269E`), font families
  (heading = "index happy font" / HappyDisplay; body = "AlmadenSans para"), radii, button styles.
  **Prefer adding design via tokens here over custom CSS.**
- `templates/` — `front-page.html` (homepage; composes patterns in order), `page.html`, `index.html`.
- `parts/` — `header.html` (mega-menu + nav + hamburger), `footer.html` (just pulls the footer pattern).
- `patterns/*.php` — one PHP file per homepage section. Each is a `wp:pattern` slugged
  `indexworld/<name>`. Images are referenced with `get_theme_file_uri('/assets/images/...')`, which a
  `render_block` filter in `functions.php` automatically **repoints to the Media Library** at render
  time (see Image handling). Pixel-locked components are written as raw markup inside `wp:html`
  (Custom HTML) blocks — these preview imperfectly in the editor but render perfectly on the front end.
- `assets/css/components.css` — the ONE custom stylesheet (~650 lines). All `.ix-*` classes live
  here (mega-menu, hero gradient, section layouts, footer, mobile). It is enqueued both on the
  front end (`wp_enqueue_scripts`) and in the editor (`add_editor_style`) — keep both in sync.
- `assets/js/*.js` — one IIFE per interactive component (mega-menu, mobile-menu, services-tabs,
  industry-tabs, hero-carousel, case-studies). Each is enqueued individually in `functions.php`
  with a `filemtime()` cache-bust. **Adding a new JS file requires adding an enqueue block.**
- `assets/images/` — the **source** imagery localized from staging. Every file here has also been
  imported into the WordPress Media Library (raster as WebP, vectors as SVG) and **the front end serves
  the Library copies**, not these files (see Image handling). Zero external image dependencies.

### How a homepage section gets added (the standard flow)

1. View the staging design (see "Viewing staging" below).
2. Create `patterns/<name>.php` with the section markup (raw HTML in `wp:html` for pixel-locked bits).
3. Append the `.ix-<name>` CSS to `assets/css/components.css`.
4. If it needs JS, create `assets/js/<name>.js` and add an enqueue block in `functions.php`.
5. Add `<!-- wp:pattern {"slug":"indexworld/<name>"} /-->` to `templates/front-page.html` in order.
6. Verify against the staging mirror.

> Images: drop the source in `assets/images/`, reference it via `get_theme_file_uri('/assets/images/<rel>')`,
> and import it into the Media Library tagged `_iw_src=<rel>` — the repoint filter then serves the
> Library copy automatically (see Image handling).

## Conventions

- Custom classes are namespaced `ix-` (BEM-ish: `.ix-cases__intro`, `.ix-cases__arrow--prev`).
- `.home` body class drives the transparent-header overlay. Note the compound selector gotcha:
  use `.admin-bar.home .ix-site-header` (both classes are on the same `<body>`, not descendant).
- `WP_DEVELOPMENT_MODE` is set to `'theme'` in `wp-config.php` so new pattern files load without
  the pattern cache getting in the way.

## Validation

There is no build step. Lint before considering an edit done:

```bash
# PHP (bundled with Local — adjust the php-8.2.x folder if the version differs)
/Applications/Local.app/Contents/Resources/extraResources/lightning-services/php-8.2.29+0/bin/darwin-arm64/bin/php -l patterns/<file>.php

# JS
node --check assets/js/<file>.js
```

## Viewing staging (the "mirror trick")

The controlled browser is blocked from navigating to `staging.indexworld.net` directly. To screenshot
a staging section: `curl` the page, save the HTML to the local webroot (e.g.
`app/public/_staging_mirror.html`) so it renders with staging's absolute-URL CSS, then screenshot
that. Strip `#custom-loader` / `[id*=loader]` elements and any `scrollTo(0, el.offsetTop)` to defeat
staging's scroll-reset.

## Image handling

**Images are managed in the WordPress Media Library** (the owner manages all images via MCP, across
environments). Every file in `assets/images/` was imported into the Library — **raster as WebP
(quality 90), vectors kept as SVG** — and each attachment is tagged with `_iw_src` meta = its path
relative to `assets/images/`. The front end serves the Library copies, not the theme files:

- A `render_block` filter in `functions.php` (`ix_repoint_images` / `ix_media_map`) rewrites any rendered
  `…/assets/images/<rel>` URL to its Media Library URL (jpg/jpeg/png→webp; svg/webp kept; unmatched
  paths fall back safely). So patterns keep using `get_theme_file_uri('/assets/images/...')` and it
  auto-repoints. **New image:** drop the source in `assets/images/`, import it to the Library tagged
  `_iw_src=<rel>` (mirror the existing import), and the filter wires it up. Replacing a Library image
  (same attachment) auto-reflects on the site.
- Client-side JS (e.g. `industry-tabs.js`) isn't block-rendered, so its image paths are repointed **at
  the source** to root-relative `/wp-content/uploads/...` URLs.
- **SVG** uploads are enabled + sanitized in `functions.php` (`upload_mimes` + `ix_sanitize_svg`); for
  untrusted uploads prefer the Safe SVG plugin.

**Conversion tooling:** no `cwebp`/ImageMagick and `sips` can't write WebP here — use **Local's bundled
PHP + GD** (`imagewebp` q90; `imagepalettetotruecolor()` for palette PNGs; downscale only if longest
edge >2000px, which also avoids a giant-WebP white-render bug).

**Migration:** environments sync via a **full-site export (DB + the `uploads/` folder)** — a DB-only
`.sql` would leave images broken (the files live on disk, not in the DB). One-way: author on one site,
export to the others.

See `memory/image-webp-media-library.md` for the full record (counts, the `_iw_src` mapping, the recipe).

## Persistent memory (in-repo)

Project context that isn't in the code lives in the committed folder **`./project_memory/memory/`**
(indexed by `project_memory/memory/MEMORY.md`). It ships with the repo so Claude reads the same
context on every clone.

**Rules for memory:**
- **At session start, read `project_memory/memory/MEMORY.md`** and check it for build scope, working
  style, and how prior sections were ported.
- **When you learn something worth persisting, write it into `project_memory/memory/`** (a new
  `<slug>.md` file) and add a one-line pointer to `project_memory/memory/MEMORY.md`. Do NOT write
  project memory to the per-user hidden `~/.claude/projects/.../memory/` path — that copy is not shared.
- The committed copy is the source of truth. After changing memory, **commit and push** to keep it
  in sync.
