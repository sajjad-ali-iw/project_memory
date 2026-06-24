---
name: component-catalog-doc
description: "The team-facing component catalog lives at theme docs/COMPONENTS.md — grouped by family, keep it updated as components are finalized"
metadata: 
  node_type: memory
  type: project
  originSessionId: 1fbae924-d65f-4e27-baae-f096fb774b3d
---

The component catalog is `wp-content/themes/indexworld-blocks/docs/COMPONENTS.md`. The site owner
**shares this file with their team**, so keep it clean and readable.

**Structure (decided 2026-06-22):**
- **Group similar components together by family**, with sequential numbering — Heroes, Tabbed
  sections, Carousels, Card grids, Content sections, Proof & trust, FAQ & CTA (Section A);
  page-specific non-components in Section B.
- When you add/rename a component, **re-fix cross-reference numbers** (`#N`) so they stay correct.
- Each entry: shape-based name, "Use for" (multi-use scenarios), layout, **image slots**, reuse notes.

**Finalized so far (#1–#17):** Hero+Image (split-hero) / Hero+Image with stats (industries-hero) /
Homepage Hero (hero) / Feature Tabs / Service Tabs (services+key-benefits) / Image Card Carousel /
Blog Carousel / Case Studies Carousel / Icon Card Grid / Numbered Cards / Industries Grid /
Media+Text / Process Steps / Tools & Frameworks (trusted) / Social Proof (testimonial) /
FAQ Accordion / Closing CTA. Section B: blog-list (hidden via `Inserter: no`).

**Section B — DONE 2026-06-23.** All page-specific patterns + scaffolds are now documented in
`docs/COMPONENTS.md` Section B: footer, contact, about-page, case-studies-archive, support-page,
odoo-implementation, furniture-page (whole-page patterns), plus industry-starter / landing-starter
(scaffolds that intentionally stay in the inserter). industries-why + blog-list were already there.
(blog-hero was skipped by owner.) ⚠️ NOTE: the 7 whole-page patterns are still registered WITHOUT
`Inserter: no`, so they currently show in the inserter — flagged in the doc; add `"Inserter":"no"` to
each header if the owner wants them hidden (the `wp:pattern` slug ref keeps resolving either way).

**WORKFLOW.md — DONE 2026-06-23.** `docs/WORKFLOW.md` written: mental model, build-a-new-page (2 routes),
add/edit a component, validation (Windows Local PHP path), image pipeline, staging mirror trick,
gotchas, env migration. Owner shares both docs with their team — keep them in sync. Related rule:
[[always-supply-images-on-reuse]].
