---
name: story-carousels-and-switch-sections
description: "The ix-re-life story-carousel layer and the ix-restack Switch section: where they live, how to reuse them, and the integration-card restyle status"
metadata:
  node_type: memory
  type: project
---

Two site-wide visual layers shipped late July 2026 (CSS in components.css, content per page). Both were built by the SEO/content execution session; treat the section CONTENT (H1s, H2s, card copy, FAQ text) as owned by the keyword map in `SEO Audits/SITE-CHANGELOG.md` — do not retitle headings or rewrite card copy without checking there first (partition rows are binding on all sessions).

**Story carousels (`ix-re-life`).** Add the class alongside `ix-iccar` on BOTH the wp:group JSON `className` and the `<section>` tag. What it does: numbered stage chips (01, 02, …) via pure CSS counters, a purple-glass hover overlay that shows each card's `data-desc` text, and a gentle 1.05 image zoom. Card recipe: keep `aria-label` as the SHORT title (accessibility), put the narrative in `data-desc` (one continuing story across the cards — a unit, an order, a property — ~35-45 words each), and point each card's `href` at a real service page, never `/contact-us/`. Degrades gracefully: without the CSS the cards still work with the default lavender overlay. Live on: `/real-estate/` (Unit 4B), `/self-storage-industry/` (unit 3C), `/aviation/` (serial 8412), `/multi-channel-ecommerce/` (the 6:02am order). Older pages carry LONG aria-labels as hover text — when upgrading one, move the story to `data-desc` and shorten the aria-label.

**The Switch (`ix-restack`).** A standard section on the same four pages: left muted card "What the operation runs today" (5 pain rows, ✕ markers) → circular arrow node "one phased migration" → right purple-gradient card "What it becomes on one system" (5 mirrored rows, ✓ markers) → footer line linking `/odoo-migration/`. Reuse by copying the section markup from any of the four pages and rewriting the ten rows in that industry's vocabulary. Mobile stacks the columns and rotates the arrow.

**Integration-card sections — status frozen, do not restyle.** The v1 "connection tiles" (purple left spine + pulsing green dot, selectors `.ix-iintg` / `.ix-iconnect` / `.ix-iint`) are LIVE and the owner dislikes them. A v2 (soft lavender gradient card, 3px gradient top bar, icons in circular chips that flip solid purple on hover) is drafted and previewed but NOT approved or queued. Don't ship any restyle here without the owner's explicit go.

Related: [[carousel-guidelines]], [[component-catalog-doc]], [[bake-industry-page-recipe]].
