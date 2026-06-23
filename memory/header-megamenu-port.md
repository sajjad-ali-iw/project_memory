---
name: header-megamenu-port
description: "How the Index World header mega-menu was rebuilt, and its open to-dos"
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

The staging mega-menu was a self-contained hand-built component (custom HTML + a 26KB `<style>` + ~2.5KB inline JS, NOT Elementor-dependent). Ported into the block theme as the agreed "pixel-locked component" pattern:

- **Markup** → `parts/header.html` inside a single `wp:html` block, alongside `wp:site-logo`. Plain links (Pricing→/support/, Blog, About Us) + purple CTA "Get free Demo!"→/book-a-free-appointment/ were rebuilt by hand; the `.ix-*` Services/Industries wrapper was ported verbatim.
- **CSS** → `assets/css/components.css` (self-contained, has its own `:root` brand vars). Theme-specific integration + overrides appended at the end (header bar layout, dark trigger text for the white header, dropdown re-anchored to full-width header so it centers instead of staging's hardcoded `width:1360px;left:-160px`).
- **JS** → `assets/js/mega-menu.js`, enqueued in functions.php. Fixed staging's LiteSpeed-only `DOMContentLiteSpeedLoaded` event to also run on normal `DOMContentLoaded`.

**Trigger variants:** markup uses `.ix-main-item` (white text, for a dark/hero header). On the current white header an override colors them dark; active pill = purple bg + white text. The future home hero should switch back to white triggers. See [[logo-variants]].

**OPEN TO-DOS:**
1. **Image optimization (speed)** — per-item hover preview images still load from `staging.indexworld.net` (the heavy 3x .jpg files, ~2.7MB each, on-hover only). Resize + webp + self-host before go-live. Default tab previews are light Unsplash URLs.
2. Mobile/hamburger nav not built yet.
3. Menu links point to pages not yet built locally (will 404 until those pages exist).

**GOTCHA — template-part DB override:** editing `parts/header.html` (or footer) on disk does NOT show on the front end if that template part was ever edited in the Site Editor (a `wp_template_part` DB row overrides the file). Fix: Site Editor → Patterns → All template parts → header → ⋮ → **Reset** (clears the DB copy so the file is used). The site logo survives Reset because it's stored in the `custom_logo` option, not the markup.
