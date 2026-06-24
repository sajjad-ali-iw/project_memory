---
name: chemical-industry-page
description: "Chemical Industry page built on the prelive local site (2026-06-23) + how page-builds work on THIS machine (no MCP; CLI bootstrap)"
metadata:
  node_type: memory
  type: project
---

**Chemical Industry page built 2026-06-23 on the `prelive` local site** (page **id 640**,
`/chemical-industry/`, `_wp_page_template=industry`). Page content = two pattern refs:
`indexworld/chemical-page` + `indexworld/cta` (103 bytes — live refs, not flattened).

- **Pattern:** `patterns/chemical-page.php` (slug `indexworld/chemical-page`), built on the
  furniture-page model — ONE self-contained pattern reusing the shared industry class hooks
  (`.ix-ihero / .ix-iccar / .ix-iwhat / .ix-ichal / .ix-ftabs / .ix-services.ix-keyben / .ix-ihow /
  .ix-ifaq`), so it inherits all existing CSS + JS (carousel/feature-tabs/services-tabs JS is enqueued
  unconditionally). Canonical 9-section composition: hero → capabilities carousel (6) → what-is →
  challenges (6 icon cards) → feature tabs (4) → key benefits (5 tabs) → steps → FAQ → cta.
- **Content:** Odoo-for-chemicals angle — formula/recipe BOMs (flexible qty, substitutions,
  by-products/co-products), batch & lot genealogy, Quality control points + COA, lots/expiry + FEFO,
  UoM conversions, hazmat segregated storage, REACH/TSCA/OSHA/GHS compliance docs + SDS, landed cost,
  B2B pricing/portal, batch costing. Written from domain knowledge (not web-deep-researched like
  cannabis/odoo-impl, but accurate); verify partner tier/counts/prices if ever used for real.
- **Images (10):** free Pexels JPGs in `assets/images/chemical/` (hero=aerial refinery, what=plant,
  production=day plant, formula=distillation glassware, lab=test-tube rack, warehouse=blue drums,
  compliance=PPE worker, delivery=tanker truck, finance=accounting desk, go-live=scientist pipetting).
  GOTCHA: Pexels "chemical plant" search returned 2 textile-factory shots — always eyeball downloaded
  stock images before use. **NOT yet imported to the Media Library** — they render via the repoint
  filter's safe fallback to the theme file. FOLLOW-UP: import the 10 as WebP tagged `_iw_src` per the
  owner's all-images-in-Library convention (see [[image-webp-media-library]]).
- **Grid:** added a 12th card to `patterns/industries-grid.php` ("Chemical" → `/chemical-industry/`),
  thumbnail `assets/images/staging/chemical.jpg` (copy of production.jpg). Verified: /industries/ links it.
- **Verified** over http: all 9 sections render, hero title present, all 10 images + grid thumb HTTP 200.

## ⚠️ Building pages on THIS machine (prelive / Windows) — key differences from the old env
The memory was authored on `indexworld-revamp.local` (macOS, user waqasyousaf). On `prelive`:
- **No `royal-mcp` / `royal-mcp-local` MCP connector here.** Create/edit WP pages via a **CLI PHP
  bootstrap** instead. Recipe that works:
  - MySQL (Local) for this site listens on **127.0.0.1:10005** (find via `netstat`/my.cnf if it changes;
    site run dir `…/AppData/Roaming/Local/run/Vfg7ofp6B/`). DB `local`, user/pass `root`/`root`.
  - PHP binary: `…/AppData/Roaming/Local/lightning-services/php-8.2.29+0/bin/win64/php.exe`.
  - CLI php loads **no php.ini** → mysqli is off. Run with
    `-d extension_dir=<…/ext> -d extension=php_mysqli.dll`.
  - In the script, `define('DB_HOST','127.0.0.1:10005');` **before** `require wp-load.php` — PHP keeps
    the first define, so wp-config's `localhost` is ignored (a harmless "already defined" warning).
  - `wp_set_current_user(<admin id, 1>)` before insert so KSES doesn't strip markup.
- **`WP_DEVELOPMENT_MODE` was NOT set in `wp-config.php`** — a brand-new pattern file's `wp:pattern`
  ref renders EMPTY (only already-cached patterns like `cta` resolve). Symptom: page renders but the
  new section is missing. **Fixed 2026-06-23 by adding `define('WP_DEVELOPMENT_MODE','theme')`** to
  wp-config (turn off / bump theme version for production). Same pattern-cache family as
  [[frontpage-db-template-override]].
- Home URL is **http://prelive.local** (theme "Index World Blocks"). Verify renders with
  `curl http://prelive.local/...` (https has an untrusted Local cert).

Related: [[industries-build]] (industry-page model), [[image-webp-media-library]], [[component-catalog-doc]].
