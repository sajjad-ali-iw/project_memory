---
name: office-furniture-industry-page
description: /office-furniture/ page built on prelive 2026-07-09 (id 760) — first Phase-4 industry vertical, uses industry.html template, content grounded in EOF + Skutchi Odoo project tasks
metadata:
  node_type: memory
  type: project
---

Built on prelive 2026-07-09 as the first Phase-4 industry vertical page.

**Page details:**
- URL: `https://prelive.indexworld.net/office-furniture/`
- Page ID: **760**
- Slug: `office-furniture` (matches the header mega-menu link — first attempt was `/furniture-industry/`, renamed to fix menu 404)
- Template: **`industry`** (via `_wp_page_template` meta) — NOT `page`, because `page.html` renders `<!-- wp:post-title -->` as an H1 above the hero (visible duplicate title). See [[industry-template-gotcha]].
- Yoast focus keyword: `office furniture ERP`
- Editable content mirror: `office-furniture-content.md` in webroot

**Composition (9 sections, all baked HTML — no per-industry PHP pattern, per [[no-per-industry-php-patterns]]):**
1. Split-hero (`.ix-ihero`) — H1 "ERP Built for Manufacturers" (Office Furniture removed from hero at owner's request; kept in section headings + FAQ)
2. Image-card carousel (`.ix-iccar`) — 6 capabilities
3. Media-text (`.ix-iwhat`) — "Your product isn't a SKU. It's a specification."
4. Icon cards / challenges (`.ix-ichal`) — 6-card grid "Where We Add Value"
5. Feature tabs (`.ix-ftabs`) — 4 tabs: Manufacturing & BOM / Sales & Approvals / Multi-Channel / Executive Dashboards
6. Key-benefits tabs (`.ix-services.ix-keyben`) — 5 tabs: Quote to Cash / Manufacturing / Inventory / Channels / Leadership
7. Steps timeline (`.ix-ihow`) — 6 phases from Discovery → Post-Launch Optimization
8. FAQ (`.ix-ifaq`) — 6 questions (`<details>` single-open per [[faq-accordion-smooth]])
9. Shared CTA — `<!-- wp:pattern {"slug":"indexworld/cta"} /-->`

**Content source (real Odoo project tasks, not marketing fluff):**
- **EOF Furniture** (Odoo project 288, S00206, Waycrop Advisors) — 349 tasks: bulk BOM imports, variant-level SKUs (SEF-79/80), dimension fields H×W×D (SEF-337), CUSTOM-item approval routing (SEF-92), discount-margin approvals (SEF-88), multi-template quotations (SEF-261), barcode-scanner stocktake (SEF-347/348), Live Sales Dashboard with KPI/YoY (SEF-345), MYOB→Odoo migration, eLearning video library.
- **Skutchi Designs** (Odoo project 470, S00302) — 83 tasks: full Odoo v17→v19 migration (SSDI-33/49/50/61), Braintree gateway, TaxJar automation, marketplace sync, shipping manager, Round-Robin CRM lead assignment, Microsoft Ads + Google Analytics integrations.

**Positioning decisions locked in with owner:**
- Target audience: **office furniture manufacturers** (both proof-point clients are in that vertical)
- Angle: **ERP + digital transformation** (broader than Odoo-only) — matches other IndexWorld industry pages
- Client naming: **anonymized** — "we've built this for office furniture manufacturers before", never Skutchi/EOF by name (see hero + FAQ wording)

**Images:** all Pexels URLs (`https://images.pexels.com/photos/...`). Prelive is HTTPS so no mixed-content issue. Owner may want to swap to Media-Library-hosted images later per [[image-webp-media-library]].

**What's still open:**
- **Furniture card missing from `/industries/` grid** — patterns/industries-grid.php hard-codes the 11 cards on the hub page (id 26). Needs theme edit + deploy OR baking the grid HTML into page 26 content (like we did for the industries-hero on 2026-07-09).
- **Bottom CTA image + copy** are shared with all other pages via `indexworld/cta` pattern — not customized for furniture.

Related: [[bake-industry-page-recipe]], [[industries-build]], [[no-per-industry-php-patterns]], [[industry-template-gotcha]], [[always-supply-images-on-reuse]].
