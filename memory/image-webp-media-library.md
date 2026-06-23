---
name: image-webp-media-library
description: All theme images converted to WebP + imported into the local Media Library (2026-06-23); refs not yet repointed
metadata: 
  node_type: memory
  type: project
  originSessionId: a85d9f1b-a80a-4009-9371-004be479765f
---

On 2026-06-23 every raster image in `wp-content/themes/indexworld-blocks/assets/images/` was converted to **WebP (quality 90)** and imported into the **local** WP Media Library (royal-mcp-local site; staging is READ-ONLY so never the target). Library went **225 → 402** (+177).

**What was imported (177 attachments):** 105 jpg→webp + 29 png→webp + 43 existing `.webp` copied as-is. The 2 PNGs `staging/3x-1.png` & `staging/3x-2.png` were skipped as exact format-duplicates of the `.jpg` twins (same 660×540 image; **both** the `.jpg` and `.png` are referenced in `parts/header.html` mega-menu, so both refs should later point to the single `3x-1.webp` / `3x-2.webp`). Total size 19.3 MB → 13.7 MB (~71%).

**SVGs (11) imported as SVG (2026-06-23):** kept as vector (not WebP). Added SVG support to `functions.php` — `upload_mimes` (svg/svgz), a `wp_check_filetype_and_ext` fix for `.svg`, a minimal `ix_sanitize_svg()` (strips `<script>`/`<foreignObject>`, `on*` handlers, `javascript:` URLs) wired to `wp_handle_upload_prefilter`, and a `wp_get_attachment_image_src` size fallback (80×80) so SVGs don't render 0×0. The 11 theme SVGs were pre-scanned clean and sanitized on import; all tagged `_iw_src`. For arbitrary untrusted SVG uploads prefer the Safe SVG plugin (stronger sanitizer). Total media now **413** (225 + 177 webp + 11 svg). Inline-SVG icons hardcoded in markup are untouched.

**Idempotency + mapping:** every imported attachment carries meta **`_iw_src`** = its path relative to `assets/images/` (e.g. `cannabis/hero.webp`). Re-running the import skips anything already tagged, and this meta is the lookup table for the **reference-repointing** step. Titles/alt derived from the path (`cannabis/hero.webp` → "Cannabis Hero", filename `cannabis-hero.webp`) so same-named files across industry folders don't collide into WP's `-1/-2` deduping. Overlong filenames capped to 80 chars + an 8-char md5 of the rel path.

**Conversion recipe (tooling notes):** no `cwebp`/ImageMagick and `sips` can't write WebP on this box — used **Local's bundled PHP + GD** (`imagewebp`, q90; `imagepalettetotruecolor()` for palette PNGs like `logos/claude-seeklogo.png`; downscale only if longest edge >2000px to dodge the giant-webp white-render bug). Imported via CLI bootstrap: `php -d mysqli.default_socket=<Local mysqld.sock> wp-load.php` + `media_handle_sideload` (admin includes required). The CLAUDE.md "No webp/cwebp tooling" note is effectively confirmed (GD is the path). Temp scripts/dirs deleted after.

**REFERENCES REPOINTED (2026-06-23) — site now serves images from the Media Library.** Mechanism (in `functions.php`):
- **`render_block` filter** `ix_repoint_images()` + `ix_media_map()`: rewrites any rendered URL matching `…/wp-content/themes/indexworld-blocks/assets/images/<rel>` → its Media Library URL, looked up live by `_iw_src` (extension normalised jpg/jpeg/png→webp; svg/webp kept). Covers EVERY server-rendered ref at once — PHP patterns (incl. `$img()` helper closures + filename arrays), the static `parts/header.html` mega-menu `data-image` previews, `templates/single.html`, etc. Unmatched paths fall back to the original (safe). Map is built once/request (static cache). Replacing a Library image later (same attachment, keeps `_iw_src`) auto-reflects. **Fully reversible — delete the filter block to revert to theme-file images.**
- **`assets/js/industry-tabs.js`** (22 image paths, injected client-side so NOT block-rendered) was repointed **at the source** to **root-relative** `/wp-content/uploads/...` URLs (root-relative = no domain/scheme baked in → migration-safe, no mixed-content). Backup at `/tmp/industry-tabs.js.bak` (kept off-tree so it doesn't ship).
- **Verified** across 12 page types (home, all industry pages, about, contact, support, odoo-impl, case-studies, blog): **0** remaining `assets/images` refs, all images served from `/wp-content/uploads/`, sampled URLs return 200, JS lints clean. (Couldn't screenshot — controlled Chrome hits a cert/error-page on the local HTTPS site; user verifies visually.)
- **Tradeoff accepted:** images inside pixel-locked `wp:html` blocks are URL-swapped only, so they don't get WP's automatic `srcset` (would need real `wp:image` blocks). The Library still stores the responsive sub-sizes if ever needed.

**Convention context (owner, this session):** owner wants ALL images in the Media Library, **WebP**, MCP-managed, and migrates environments via a **full-site export (DB + `uploads/` files)** — staging→live one-way. A DB-only `.sql` export would leave images broken (files live on disk, not in the DB). CLAUDE.md still says "use `get_theme_file_uri()` for portability" — **not yet updated** to the Media-Library-via-MCP convention. Related: [[blog-clone-from-staging]] (same sideload-via-PHP technique), [[always-supply-images-on-reuse]].
