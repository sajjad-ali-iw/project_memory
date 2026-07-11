---
name: oversized-realistic-imagery-fix
description: "2026-07-11: 18 raw AI 'realistic' PNGs (~100 MB) in the prelive Library made the homepage a 23 MB download — replaced with slot-spec WebPs (commit 442c642) + Library retag; homepage now 2.2 MB. Old PNG attachments 841-872 still in Library, untagged."
metadata:
  node_type: memory
  type: project
---

**Incident (found 2026-07-11, owner reported "site slow in incognito"):** the July "realistic" imagery batch was uploaded to the prelive Media Library as RAW AI renders — 18 PNGs at 2528×1696 / 2560×1429, 3.3–8.1 MB each (~100 MB total) — and `_iw_src`-tagged over the theme keys, bypassing the image pipeline entirely. Homepage payload hit **23 MB** (menu previews alone 7.7 + 6.0 MB); it only *felt* fine in a normal browser because assets were browser-cached. Affected: homepage (menu previews + svc tabs), About, and the 4 service pages (how-01…08, odoo-impl-*, about-*).

**Fix (2026-07-11):** same pictures resized to slot spec + WebP q90 — menu previews 600w, all other slots 1200w (16–84 KB each, ~1 MB total). Committed as theme sources at their `_iw_src` paths (commit `442c642`, e.g. `assets/images/staging/menu-preview-services.webp`), auto-deployed, imported to the prelive Library via `wp_upload_media_from_url` (new attachments **956–973**, alts preserved), tags moved to the new IDs, and `_iw_src` DELETED from the old PNG attachments (860–872, 841–843, 851, 855–856). Homepage verified **2.2 MB**, zero `-realistic` refs on all 6 affected pages.

- `ix_media_map` builds `$map[$rel]=url` over get_posts(date DESC) — **duplicate `_iw_src` keys = oldest attachment wins**, so always untag the loser, never leave two attachments with the same key.
- The old giant PNG attachments are STILL in the prelive Library (untagged, unused, ~100 MB disk) — safe to delete whenever; owner not asked yet. Never re-tag them.
- Windows CLI image tooling: Local's PHP needs `-d extension_dir=<...>\ext -d extension=gd` (GD+WebP confirmed, php_gd.dll not autoloaded). Bulk base64 upload via MCP is a context-killer — host files (theme deploy) + `wp_upload_media_from_url` instead.
- Perf context: prelive TTFB is ~1.2 s even on LiteSpeed cache hits (server far from PK; ~0.3 s RTT) — that floor needs a CDN, not image work. Compression/page-cache/CSS/JS all verified healthy.
- LiteSpeed caches pages 7 days; after any imagery retag a **Purge All** is needed (owner's click) or stale HTML keeps old URLs.

**Rule:** any new imagery — AI-generated included — must go through the pipeline: resize to the slot's §C spec, WebP q90, source committed to `assets/images/`, Library import tagged `_iw_src`. Related: [[image-webp-media-library]], [[image-slot-aspect-ratios]], [[overhaul-2026-07-02]].
