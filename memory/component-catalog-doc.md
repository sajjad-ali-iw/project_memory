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

**Still to walk one-by-one:** industries-why, odoo-implementation, about/about-page, contact,
case-studies-archive, footer, industry-starter, landing-starter. (blog-hero was skipped by owner.)
After components: write the WORKFLOW.md (how to build/edit future pages). Related rule:
[[always-supply-images-on-reuse]].
