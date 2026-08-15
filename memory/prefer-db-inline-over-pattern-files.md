---
name: prefer-db-inline-over-pattern-files
description: "Owner preference (2026-08-15): service-page content lives INLINE IN THE DB, not in theme patterns/*.php — DB edits show instantly, pattern edits need an OpCode purge. Includes the pattern→DB conversion recipe."
metadata:
  type: feedback
---

**Rule:** for Odoo/ERP service pages, put the page content **inline in the WordPress database**
(edited with the `royal-mcp-prelive` `wp_*` tools), **not** in a theme pattern file.

Stated by the owner 2026-08-15 — *"i don't want odoo-customization.php"* — and acted on: the whole
odoo-customization page (id 646) was converted out of `patterns/odoo-customization.php` into inline
block markup, and the pattern file was deleted (theme commit `bb35683`).

**Why:**
- A **DB edit auto-purges that page's LiteSpeed cache → the change is visible immediately.**
- A **pattern/PHP edit needs a manual OpCode Cache purge** on prelive before it appears
  (`opcache.validate_timestamps=0` there). This was the single biggest recurring time-sink of the
  session — repeated "it's not updating" reports that were always the opcode cache.
- It is also **consistent**: every other Odoo page (odoo-implementation 362, odoo-migration 645,
  erp-* pages) is already DB-inline. odoo-customization was the odd one out.

**How to apply:** default to `wp_replace_in_page` / `wp_update_page` for content edits. Only reach for
a pattern file when markup genuinely must be shared across many pages AND the owner has agreed to the
purge cost.

**Converting an existing pattern to DB (the recipe that worked):**
1. Render the pattern to static block markup with a small PHP harness: stub `esc_url`/`esc_attr`/
   `esc_html`/`wp_strip_all_tags`/`get_theme_file_uri`/`ix_badge`, `eval` any `ix_*` helper pulled out
   of `functions.php`, then `include` the pattern and capture the output buffer.
2. **Bake image URLs to their final resolved form** (`/wp-content/uploads/...`), because the
   `render_block` repoint filter that rewrites `assets/images/...` paths does NOT apply to inline
   DB content the same way. Get the real URLs by curling the live page and reading the `data-src`s.
3. Leave nested `<!-- wp:pattern {"slug":"…"} /-->` references alone — those still resolve fine from DB
   content (the case-study carousel kept working).
4. `wp_replace_in_page` the old pattern ref → the rendered markup, with `dry_run: true` +
   `expected_count: 1` first.
5. Verify on the live page: count each section's classes, confirm inline `<svg>` survived (KSES can
   strip SVG on some paths — see [[kses-strips-svg-and-bg-baked-pages]]), and confirm the JSON-LD
   schema block is intact.
6. Then `git rm` the pattern file and push.

**Trade-off to state honestly:** DB-inline content is static HTML, so it cannot call the shared PHP
helpers (`ix_srv_hero()`, `ix_trust_bar()` — see [[shared-service-component-helpers]]). Responsiveness
is unaffected either way, because that lives in the one shared `components.css`.
See also [[prefer-media-library-over-theme-assets]], [[litespeed-purge-and-edge-cache]].
