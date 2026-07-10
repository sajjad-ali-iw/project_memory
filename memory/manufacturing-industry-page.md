---
name: manufacturing-industry-page
description: "/manufacturing-industry/ (prelive id 805) is the live Manufacturing page — slug renamed FROM supply-chain-manufacturing 2026-07-10; old-slug redirect pending theme deploy"
metadata:
  node_type: memory
  type: project
---

**The Manufacturing industry page is LIVE on prelive as `/manufacturing-industry/` (page id 805, title "Supply Chain & Manufacturing").** It was created at slug `supply-chain-manufacturing`; on 2026-07-10 the owner had the slug renamed to `manufacturing-industry` so it matches the existing mega-menu / footer / mobile-drawer links (theme links were NOT changed — they already pointed there). Rename done via `wp_update_seo_meta` (the royal-mcp tool that supports `slug`; `wp_update_page`/`wp_update_post` do NOT).

- This supersedes the "Manufacturing trashed (id 369)" status in [[kses-strips-svg-and-bg-baked-pages]] — a NEW manufacturing page exists; id 369 stays trashed.
- **WP's old-slug 301 does NOT fire for pages** (core limitation; `_wp_old_slug` meta was set anyway, no effect) → `/supply-chain-manufacturing/` currently 404s. Fix staged in `functions.php` `$ix_canon` map (`'supply-chain-manufacturing' => 'manufacturing-industry'`) — **uncommitted in the theme repo, awaiting owner's go to push/deploy**.
- LiteSpeed caches 404s (`X-LiteSpeed-Cache: hit` on the 404) — after deploying the redirect, expect the cached 404 until purge/TTL.
- `manufacturing-industry` stays in the functions.php 404-fallback list — harmless on prelive (page resolves, no 404) and it keeps local graceful (local DB has no such page).

**How to apply:** to rename any page slug via MCP, use `wp_update_seo_meta` with `slug`; add a `$ix_canon` entry for the old slug because WP won't redirect old page slugs on its own.
