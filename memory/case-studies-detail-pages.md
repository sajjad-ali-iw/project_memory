---
name: case-studies-detail-pages
description: The 9 individual /case-studies/<slug>/ detail pages and how they were bulk-imported from staging
metadata: 
  node_type: memory
  type: project
  originSessionId: 2aa1cd17-8308-49ff-bc9f-c09f08dc803c
---

Built 2026-06-21. The 9 case-study detail pages, replicated pixel-close from staging `/case-studies/<slug>/`, same URLs. Companion to [[case-studies-archive-page]] (the listing) and reuses its slugs so listing cards now resolve.

**Pages:** WP pages **ids 40–46, 48, 52**, all `post_parent`=38 (the "Case Studies" page) so permalinks are `/case-studies/<slug>/`. Each has `_wp_page_template`=`industry` (blank canvas, no post-title). Content = a single `wp:html` block holding `.ix-csd` markup (design lives in theme CSS; content is bespoke per page, like the Cannabis page model).

**Design — `.ix-csd` in components.css** (matches staging exactly):
- Gradient hero `linear-gradient(90deg, rgba(247,247,248,.42), rgba(76,46,134,.13))`, left h1 "index happy font" 50/60, intro 20px #00053d.
- Two-col body (`grid 396px 1fr`): LEFT sticky-ish info card (white, radius 12, **border-left 2px #4c2e86**, pad 40, pulled up `margin-top:-90px` to overlap hero) holding company logo + Industry: value + Challenges/Solutions/Outcomes bulleted lists (16px #666487). RIGHT prose: section `<h2>` (32px happy font) + body p (16/1.7 #252a3a); "Key Components" sub-items render as `.ix-csd__sub` h4 (bold term) + paragraph.
- CTA = lavender `#d1c8eb` rounded box, navy 32px heading + two buttons (purple `.is-light` lavender) → /contact-us/ and /pricing/.
- Responsive: columns stack ≤900px, card margin-top -56.

**HOW THEY WERE IMPORTED (reusable technique for bulk staging→local):** wrote a TEMP `_cs_gen.php` in webroot that `require`s wp-load.php, then for each slug: cURL-fetches the staging page, parses with **DOMDocument+DOMXPath scoped to `//div[@data-elementor-type="wp-page"]`** (excludes header/footer/popup), extracts structured content, localizes the company logo, and upserts the page (`get_page_by_path`/`wp_insert_post`, idempotent). Ran it by `curl -k https://indexworld-revamp.local/_cs_gen.php` (PHP makes the outbound HTTPS to staging — works server-side; my Bash sandbox needs `dangerouslyDisableSandbox` only for the trigger curl). **DELETED the script after** (it was an unauthenticated DB-writing endpoint). This PHP-DOMDocument approach beat browser extraction for 9 pages — no manual pasting.
- Extraction rules that mattered: staging section headings & CTA heading **differ per page** (e.g. "Why Armor Concept Chose Index World", CTA "Simplify Your Multi-Channel Operations") — so DON'T whitelist titles; treat every heading WIDGET (`div[@data-widget_type^=heading]`) as a section, **first=page title, last=CTA**, middle=sections. Card = ancestor of the `<p>Industry:</p>` that also contains `<p>Outcomes:</p>`; its labeled `<ul>`s feed the lists; Industry value is the `.information-block__item--title` span after the label.

**Logos:** localized to `assets/images/case-studies/logos/<slug>.<ext>` (svg/png/webp), referenced by root-relative URL. **OPTIMIZED 2026-06-21:** vape + wholesaler were 1MB embedded-raster SVGs (1024px PNG shown at 30px) and looseleaf 150KB — shrunk the embedded raster in-place (extract base64 → `sips -Z 200/256` downscale → re-embed via **python3 re.sub with a lambda replacement**, NOT perl `s///` which chokes on `/` in base64 and will zero the file). Now 81/81/59KB, same filenames, no page edits. Keep this technique for any giant embedded-raster SVG.

**Hero photo (ADDED 2026-06-21):** staging hero is two-column — title+intro LEFT, a 640×400 radius-20 `object-fit:cover` photo RIGHT. `.ix-csd__hero-inner` is now `grid 1fr minmax(0,560px)` (stacks ≤900px). Generator extracts the hero image = first `image.default` widget before any section heading & not in card; localized to `assets/images/case-studies/heroes/<slug>.<ext>`. **Photos >200KB are downscaled to 1280px + re-encoded JPEG q82 via PHP GD** in `localize($...,$photo=true)` (looseleaf 1.6MB→121KB, RTJ 855KB→112KB; all heroes now 28–162KB).
- **Screenshot gotcha:** these hero/logo images often show BLANK in the first Chrome screenshot after navigation (first-paint/compositing artifact) even though `complete:true, naturalWidth>0`. A tiny scroll forces the repaint and they appear — NOT a real bug; don't "fix" it.

CTA buttons route to /contact-us/ + /pricing/ (pricing page not built → may 404).

**PIXEL-TUNED to staging 2026-06-21 (measured at vp 1440 via mirror):** title 50/60 600 #000; intro 20px/**line-height 1.2** #00053d; section h2 32/38.4 600 #000, margin 28px 0 20px (staging gaps ~20px); body p+li 16px/**1.5** color **#00053d** (was #252a3a/1.7); card has **NO box-shadow** (staging has none — removed my earlier one), li 16/1.3; hero image 640×400-ish radius20 cover. **Container alignment:** hero-inner, body, and cta all use `width:min(1320px, calc(100% - 48px)); margin:0 auto` so the title-left, card-left, and CTA-left all line up at the same x (matches staging where titleX==cardX==60). All 9 pages share `.ix-csd` so every tweak applies uniformly = identical design across all.

**Info-card pixel-tuned to staging (measured):** border-left **1.5px** (not 2px) solid #4c2e86; card 396 wide, pad 40, radius 12, NO shadow. EVERYTHING in the card is **#666487** — label 16/600, **Industry value 16/400 #666487** (was wrongly dark #1f2330/500), list items 16/line-height **1.3 (20.8px)** /#666487 with **margin 0** (the 21px row pitch comes from line-height alone), and **li::marker also #666487** (not a lighter tint). Field block margin-bottom 26, label margin-bottom 12, logo max-height 30 margin-bottom 24. Staging card detection in JS: border-left width is **1.5px** so test `parseFloat(borderLeftWidth)>=1.4` (a `>=2` test misses it).

**Purple band:** staging's hero has `border-bottom:20px solid #4c2e86` — that's the full-width purple strip at the hero/body boundary that the white info card overlaps. Added to `.ix-csd__hero`. (Earlier "band:NONE" detection failed because it's a border, not a bg element.)
