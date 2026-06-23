---
name: odoo-implementation-page
description: "The /odoo-implementation/ service page — structure, component reuse, SEO"
metadata: 
  node_type: memory
  type: project
  originSessionId: 2aa1cd17-8308-49ff-bc9f-c09f08dc803c
---

First **service page** built 2026-06-22 (model for future service pages). Page "Odoo Implementation", id **362**, `/odoo-implementation/`, `_wp_page_template`=`industry` (blank canvas, no post-title). Content = single ref `<!-- wp:pattern {"slug":"indexworld/odoo-implementation"} /-->`.

⚠️ **GOTCHA (hit 2026-06-22): page got FLATTENED.** Page 362's `post_content` had been expanded from the 62-byte pattern ref into ~65KB of static block HTML (happens when the page is opened+saved in the WP block editor — it inlines the pattern). Symptom: NEW edits to `patterns/odoo-implementation.php` stop appearing on the front end while older content (already baked into the snapshot) still shows; OPcache reset does NOT fix it. **Fix:** re-point the page to the live ref — `wp_update_post(['ID'=>362,'post_content'=>'<!-- wp:pattern {"slug":"indexworld/odoo-implementation"} /-->'])` (did it via a temp wp-load PHP in webroot, deleted after). Verify the served HTML, not just the editor. If file edits ever "don't show" again, check `get_post(362)->post_content` length first — 62 = live ref (good), big = flattened (re-point it).

**Built from a plan** (competitor analysis of 9 Odoo partners: Bista, O2b, Captivea, OBS, Synconics, much, Cudio, PortCities, Bayforward). Key competitor learnings baked in: almost none show pricing → we added a transparent value section; almost none use FAQ/Service JSON-LD → we added it; our 3 differentiators (documentation, AI, value-for-money) are absent across all 9 → they're the "Why Index World" wedge.

**One pattern `patterns/odoo-implementation.php`** holds all sections, REUSING existing component CSS classes (no new component CSS) + `wp:pattern` refs to shared patterns:
1. Hero `.ix-ihero` (Odoo copy, img `assets/images/odoo-impl/hero.jpg`)
2. Trust strip → ref `indexworld/trusted`
3. Why structured `.ix-iwhat` (img odoo-impl/why.jpg)
4. **Key Benefits** `.ix-services .ix-keyben` tabbed — "Run your whole business on one connected platform" (5 tabs: Sales&CRM, Inventory&MRP, Accounting&Finance, eCommerce&POS, HR&Payroll; reuses services/ images + services-tabs.js). **Moved here ABOVE How It Works + relabeled 2026-06-22** (was a separate "What we implement" / "The Odoo modules we implement" section below How It Works — user removed that and pasted this Key Benefits version on top of How It Works). Driven by `$oi_modules`.
5. How It Works / path-to-go-live — **Feature Tabs** component `.ix-ftabs` (vertical tabs + per-tab image + checklist; JS feature-tabs.js, already enqueued). **"component 4" = Feature Tabs per docs/COMPONENTS.md** (components numbered 1–13; #4 = feature-tabs, #5 = numbered-cards — I first mis-built numbered-cards, user said "not this one", corrected to feature-tabs 2026-06-22). **7 tabs = staging's exact 7 process steps** (user noticed I'd dropped steps; matched staging pixel-intent): Requirement Gathering / GAP Analysis / Business Process Mapping / Development & Testing / Deployment & Data Migration / Users Training / Support & Maintenance — each tab's checklist items are staging's literal sub-bullets. **Content deep-researched 2026-06-22** (3 parallel agents vs Odoo's official methodology + ERP best-practice sources; user said this is the most important section). Copy now uses accurate expert vocabulary woven in for a business audience: SPoC, fit-to-standard (70–85% met by standard Odoo), no-code Studio vs custom, BPMN/to-be flows, Conference Room Pilot + scripted UAT, upgrade-safe (never edit core), ETL migrate (master data → open transactions → opening balances, reconcile), RBAC (groups/access rights/record rules), train-the-trainer/super-users, hypercare + SLA + annual upgrades. Each step has a 2-sentence desc + 3–4 detailed checklist items. **Images** in `assets/images/how-it-works/`: discovery, design, mapping, build, deploy, training, optimize (all fresh Unsplash). go-live.jpg now unused. Was originally the two-column `.ix-ihow` checklist, briefly 4-tab/5-tab.
6. Why Index World `.ix-ichal` 6 icon cards (Documentation · AI · Value + Partner/US-support/Scale)
7. Cost & value `.ix-iwhat` (odoo-impl/cost.jpg) — value framing, NO invented prices, "Request a tailored quote"
8. Industries → ref `indexworld/industries-grid`
9. Case studies → **inline `.ix-iccar` image-card carousel** (9 real case studies, Odoo-first order, each card links to `/case-studies/<slug>/`; reuses image-card-carousel.js which auto-inits all `.ix-iccar`). Added `.ix-iccar__name a{color:inherit;text-decoration:none}` so the linked card title isn't blue. (Was the shared `indexworld/case-studies` ref; swapped 2026-06-22 at user request.)
10. FAQ `.ix-ifaq` (6 Q&A; note the answer needs `.ix-ifaq__a > .ix-ifaq__a-in > p` for smooth accordion)
11. Final CTA → ref `indexworld/cta`

**SEO:** Yoast title/description/og + focus_keyword "odoo implementation services usa" set via wp_update_seo_meta. JSON-LD (Service + FAQPage, areaServed USA) injected inline at top of the pattern. Images all Unsplash (free) in `assets/images/odoo-impl/`, ~45-75KB each.

**NOT invented (need real data from user):** Odoo Partner TIER (used "Official Odoo Partner", not "Gold"), project/client counts, ratings, actual price ranges. Trust strip uses existing partner logos. Update when the user supplies real numbers. Related: [[industries-build]], [[carousel-guidelines]], [[button-system]].
