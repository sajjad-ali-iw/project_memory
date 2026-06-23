---
name: case-studies-archive-page
description: "The /case-studies/ listing page (grid + category filters), distinct from the homepage carousel"
metadata: 
  node_type: memory
  type: project
  originSessionId: 2aa1cd17-8308-49ff-bc9f-c09f08dc803c
---

Built 2026-06-21 from staging `/case-studies/` ("Work we're over the moon with"). This is the LISTING page — separate from [[case-studies-component]] (the homepage carousel).

- **Page:** "Case Studies", id **38**, `/case-studies/`, `_wp_page_template`=`industry` (blank canvas, no post-title). Content = single ref `<!-- wp:pattern {"slug":"indexworld/case-studies-archive"} /-->`.
- **Pattern:** `patterns/case-studies-archive.php` (slug `indexworld/case-studies-archive`). Header = ix_badge('Case Studies') + h1 "Work we're over the moon with" + subtitle (my addition). Then a filter bar + 3-col responsive grid of 9 cards. Card data lives in a PHP `$ix_cs_items` array (title, cat, img, slug); filter pills auto-built from the distinct cats ("All" first).
- **9 cards** map to staging case-study slugs `/case-studies/{slug}/` (those detail pages NOT built locally yet → links 404 for now, same as other pending pages). Categories are **Shopify** / **Odoo** (staging's literal tags; I title-cased them).
- **Images:** localized to `assets/images/case-studies/Frame-*.webp` (9 files, ~620KB total, already-optimized webp from staging `/uploads/2026/04/`). NOT the homepage `hero-cards/` set (different Frame numbers; e.g. listing uses `Frame-2147228088-1-1.webp` vs carousel's `Frame-2147228088.webp`).
- **CSS:** `.ix-csa` block appended to components.css. Cards: image link `aspect-ratio:433/400` radius 16, hover = image scale 1.05 + white round ↗ arrow badge top-right + title→purple. Tag pill = `#ECE6FF`/`#50269E`. Title font = "index happy font". Responsive 3→2 (≤1024)→1 (≤640) cols.
- **JS:** `assets/js/case-studies-filter.js` (enqueued in functions.php as `indexworld-case-studies-filter`). Toggles `card.hidden` by `data-cat`, sets `.is-active` pill, shows `.ix-csa__empty` if none. Multi-instance, normal-DOMContentLoaded init.

Verified on local: renders, images load (lazy — paint as they enter view), filter All/Shopify/Odoo works (Odoo→3 cards). Related: [[section-port-method]], [[industries-build]].
