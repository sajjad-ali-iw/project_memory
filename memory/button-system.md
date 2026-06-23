---
name: button-system
description: "Site-wide button standard — two tiers, Almaden Sans 600, radius 12"
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

Site-wide button standard (measured + finalized 2026-06). Supersedes the older "Inter 15/600 padding 14×26" note in [[industries-build]].

**Font:** `'AlmadenSans para'` (our only Almaden weight is Book/400 → 600 renders as faux-semibold, same as the announcement-bar text which already used it and looks fine). **Weight 600. Radius 12px** everywhere.

**Two tiers:**
- **Large** — content & hero CTAs: `font-size:16px; line-height:1; min-height:54px; padding:0 28px`.
  Classes: `.ix-btn` (+ `--primary`/`--light`), `.ix-hero__btn .wp-block-button__link`, `.ix-ihero__btn`, `.ix-csd__cta-btns a`, `.ix-acta__btn`.
- **Standard** — header/nav CTAs: `font-size:14px; line-height:1; min-height:46px; padding:0 20px`.
  Classes: `.ix-btn-outline` (nav Contact Us), `.ix-cta-btn` (nav Get free Demo). Announcement pill `.ix-annc__cta` = 14/600 Almaden, radius 12, small inline (keeps its FB→blue gradient).

Height is enforced via `min-height` + `display:inline-flex; align-items:center; justify-content:center` (line-height:1, no vertical padding) for fixed-height buttons. Mobile (≤600px) hero buttons go side-by-side: `flex:1`, `min-height:50px`, font 14px, `padding:0 10px`.

Reference values: standard = 46px/14px/600/radius12/pad 4px20px; large = 54px/16px/600/radius12. All buttons verified on homepage + cannabis-industry page.

Related: [[industries-build]] (button standard there is now stale), [[case-studies-component]].
