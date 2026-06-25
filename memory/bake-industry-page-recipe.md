---
name: bake-industry-page-recipe
description: How to render a section/scaffold pattern to KSES-safe baked page content + how to bootstrap WordPress from the CLI on THIS Mac (Local mysqli socket override)
metadata:
  node_type: memory
  type: reference
---

**Goal:** turn a PHP section/scaffold pattern into static, KSES-safe **baked** page content (the [[no-per-industry-php-patterns]] model). Used 2026-06-25 to rebuild the local Chemical Industry page (id 640) after `chemical-page.php` was deleted.

**Recipe (proven on Chemical, local):**
1. Source the markup. A deleted pattern is recoverable from git: `git show HEAD:patterns/<name>.php`. Or start from `industry-starter.php`.
2. Make it KSES-safe BEFORE rendering: convert any carousel `.ix-iccar__media` inline `background-image:url()` to a child `<img class="ix-iccar__img">` (CSS for it is in components.css). Inline `<svg>` icons are fine — the `wp_kses_allowed_html` filter in functions.php now keeps them ([[kses-strips-svg-and-bg-baked-pages]]).
3. Render to HTML with WordPress so PHP (ix_badge(), image helpers, loops) resolves: `do_blocks('<!-- wp:pattern {"slug":"indexworld/<name>"} /-->')`. The render_block image-repoint filter also fires, swapping `assets/images/...` → Media Library URLs.
4. Normalise for portability: strip the origin (`https?://<site>` → '') so URLs are root-relative, and drop `?v=\d+` cache-busters.
5. Save into the page via `wp_update_post(['ID'=>N,'post_content'=>$html])`. Keep shared sections (e.g. CTA) as pattern refs: append `<!-- wp:pattern {"slug":"indexworld/cta"} /-->`.
6. Delete the temp pattern file — the page no longer needs it.
7. Verify on the front end: section count, inline `<svg>` count (icons survived), `.ix-iccar__img` count (carousel images), and **0** empty icon holders / **0** `background-image`.

**⚠️ Don't get the browser to dump the baked HTML** — the in-Chrome `javascript_tool` privacy guard BLOCKS returning bulk page HTML ("Cookie/query string data"). Render+save server-side instead (below), or `curl` the page in Bash and parse with DOMDocument.

**CLI WordPress bootstrap on THIS Mac (the non-obvious part):**
`require wp-load.php` alone fails with "Error establishing a database connection" because wp-config's `DB_HOST='localhost'` makes PHP-CLI look for the default mysqld socket, which Local doesn't use. Fix = point mysqli/pdo at Local's socket via `-d`:
```
PHP=/Applications/Local.app/Contents/Resources/extraResources/lightning-services/php-8.2.29+0/bin/darwin-arm64/bin/php
SOCK=$(find "$HOME/Library/Application Support/Local/run" -name mysqld.sock | head -1)   # run-id (e.g. K00omuOLI) changes across Local restarts — find it, don't hardcode
$PHP -d mysqli.default_socket="$SOCK" -d pdo_mysql.default_socket="$SOCK" script.php     # script.php does: require './wp-load.php'; ...
```
Run from `app/public/`. `do_blocks` + theme patterns are available (init has fired). This makes full WP scripting work here without MCP. (Differs from the Windows-author machine in [[chemical-industry-page]] which used port :10005 + php_mysqli.dll.)

**Note:** the local baked page is a pre-push demo — it'll be overwritten when prelive's DB is pulled to local. Authoritative industry pages get baked on prelive (via MCP) AFTER the theme fix is deployed. Related: [[industries-build]], [[image-webp-media-library]].
