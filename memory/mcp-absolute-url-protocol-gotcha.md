---
name: mcp-absolute-url-protocol-gotcha
description: MCP-generated pages bake absolute https:// image URLs that break on the http local site (blank images look like extra space); fix = root-relative URLs
metadata: 
  node_type: memory
  type: project
  originSessionId: de938308-ec01-4e57-863c-e4f4c6bb456d
---

GOTCHA (found 2026-06-23 on /mechanical-industry/, page id 380): MCP-generated page content
hard-codes **absolute `https://indexworld-revamp.local/...`** URLs for images (both
`<img src>` and inline `style="background-image:url(...)"`). But the site's `siteurl`/`home` is
**`http://indexworld-revamp.local`** and Local's HTTPS cert is NOT trusted (Chrome shows a Privacy
error), so every `https://` image **fails to load → renders blank**. In the Image-Card Carousel
(`.ix-iccar`) the blank square cards looked like a big band of **"extra space"** under the heading —
the layout/CSS was fine; the images were just broken.

**Why it matters:** any MCP-authored page (industry pages: furniture 364, manufacturing 369,
self-storage 365, wholesale 367, odoo-implementation 362, mechanical 380, etc.) is likely to carry
the same broken absolute-https URLs.

**How to fix / apply:**
- Make image URLs **root-relative** (`/wp-content/uploads/...`, `/wp-content/themes/...`) so they
  inherit the page's protocol — matches the CLAUDE.md image convention and survives http→https
  migration. NOT absolute `https://` or `http://`.
- **When generating pages via MCP, emit root-relative image URLs** (never absolute with protocol+host).
- One-off DB fix used: a PHP bootstrap script (Local's bundled php) — `ini_set('mysqli.default_socket', '<Local run .../mysql/mysqld.sock>')` then `require wp-load.php`, `kses_remove_filters()` (so inline style/SVG isn't stripped), `str_replace('https://indexworld-revamp.local','',$content)`, `wp_update_post`. (Plain `wp-load` from CLI fails DB connect until the socket is set — DB_HOST is just `localhost`.)
- Browser/MCP edits to live content: the royal-mcp-local connection works; CLI PHP needs the socket.

Still open on that carousel: the card image ratio is a fixed **1:1** (`.ix-iccar__media aspect-ratio:1/1`)
— owner wants it more dynamic (the source images are tall 1100×1955, cropped to square). Not changed yet.

Related: [[image-slot-aspect-ratios]], [[image-webp-media-library]], [[booking-page-and-iframe-kses]], [[industries-build]].
