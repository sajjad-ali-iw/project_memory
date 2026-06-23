---
name: support-page-build
description: "The /support/ page (Unlimited Odoo Support) cloned pixel-perfect from staging, 2026-06-22"
metadata: 
  node_type: memory
  type: project
  originSessionId: 1fbae924-d65f-4e27-baae-f096fb774b3d
---

`/support/` (page id **361**, slug `support`, `_wp_page_template=industry` blank canvas) was cloned
pixel-perfect from `staging.indexworld.net/support/` ("Unlimited Odoo Support & Maintenance Services")
on 2026-06-22. Design matched to staging; **copy cleaned** (staging is placeholder-heavy — "HERO/STAR"
brand names, typos — replaced with real Index World copy, per the user's standing rule for these pages).

**One self-contained pattern** `patterns/support-page.php` (slug `indexworld/support-page`) holds all
sections; page content = just `<!-- wp:pattern {"slug":"indexworld/support-page"} /-->`. All CSS scoped
under `.ix-sup*` in components.css. Sections in order:
1. Hero — white, two-column: text + checklist (3 items) on left, branded support-card image right. **NO eyebrow** (per [[no-eyebrow-in-hero]] — staging had a "HERO Support Plans" badge; dropped it).
2. Plans — 4 cards (Saviour $250 / Champion $750 / Knight $1500 / Hero Custom). 4th is `--featured` (purple border + "Most Popular" pill + purple "Free Demo" button); others outline "Start Now". Check-icon feature lists.
3. Why — `#f7f7fb` band, `ix_badge('Why Index World')` + 3 purple line-icon cards (No clock-watching / One predictable fee / U.S. based experts).
4. Comparison table — "Hourly billing vs. flat-rate": label + Hourly Billing + Index World Support (dark-purple head, lavender-tinted hero column), Total row.
5–8. Four alternating media+text feature sections (`.ix-sup-feat`, `--rev` flips image side): Expert help / Get fluent / 500+ apps / Monitor your project.
9. FAQ — **reuses the shared `.ix-ifaq` component** (faq.php markup/classes + `faq-accordion.js` smooth single-open; the JS auto-inits any `.ix-ifaq__list` on the page) with 6 support Q&As. (Originally built a one-off `.ix-sup-faq`; refactored to reuse the shared component for consistency — reuse markup+behavior, supply your own content.)
10. CTA — reuses `indexworld/cta` via nested `wp:pattern` ref.
Images in `assets/images/support/` (hero.jpg, feat-expert.webp, feat-training.webp, feat-apps.png, feat-monitor.png). The support `$img()` helper appends `?v=filemtime` for cache-busting.

**GOTCHA — large WebP renders white in-page:** the hero image (a 2560px webp) loaded
(`img.complete=true`, had a layout box, was top/visible/opaque via elementFromPoint) yet **painted blank
white inside the page** — though it rendered fine when opened standalone. Fix: **convert oversized webp
heroes to JPG** (`sips -s format jpeg -Z 1200`). Smaller webps (the feature images) render fine. Also note
`decoding="async"` images paint white for ~1–2s after scroll-in — a screenshot-timing artifact, not a real bug.

**Verification:** desktop verified section-by-section (Chrome MCP screenshots — use a FRESH tab if the
screenshot CDP binding errors with "params.clip.scale missing"). **Mobile not auto-verifiable** (browser
pinned to desktop — [[mobile-responsiveness]]); responsive CSS written (breakpoints 1024/980/640) but USER
verifies phone visually. Related: [[industries-build]] (same blank-canvas page-composition model).
