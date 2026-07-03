---
name: service-pages-responsive
description: Service pages (.ix-srv-* family) responsive fixes — the WHY section never collapsed; added tablet/phone tiers (2026-06-25)
metadata:
  type: project
---

The 4 SERVICE pages all render the **`.ix-srv-*` component family** (NOT the older
`.ix-ihero/.ix-iwhat/.ix-svc` industry classes — even `/odoo-implementation/` was rebuilt onto
`.ix-srv-*`). Pages: `/odoo-implementation/`, `/odoo-audit/`, `/odoo-customization/`,
`/odoo-migration/`. They chain `service-hero/stats/deliverables/modules/process/why/packages` +
`service-plat` (migration only) + shared `.ix-ichal`/`.ix-ifaq`/`.ix-ctap`. Source patterns:
`patterns/service-*.php`, `patterns/odoo-{audit,customization,migration,implementation}.php`;
all `.ix-srv-*` CSS is in `assets/css/components.css` (~line 1980–2140).

**Responsive fixed 2026-06-25** (in the "Service family responsive" block, components.css ~2093+):
- **CORE BUG:** `.ix-srv-why__grid` was `grid-template-columns:1fr 1.05fr` with **NO single-column
  rule at any breakpoint** → the "Why Index World" media+text section stayed 2-column (image+text
  squished) on tablet & phone on ALL 4 pages. Added `grid-template-columns:1fr` at ≤860px. Markup
  order is image-then-copy, so it stacks image-on-top.
- Hero title was locked at 50px until 860px → cramped in the 861–1000px 2-col zone. Added
  `2.7rem` @≤1000, `2.2rem` @≤860, `1.95rem` @≤560, `1.75rem` @≤400.
- Packages (`.ix-srv-pack__grid`) stayed 3-up with full padding 860–1000px → tightened gap/padding
  @≤1000 (collapses to 1-col @≤860, already existed).
- Added a **≤400px small-phone tier** so long happy-font section titles don't overflow on 360–390px.
- Hero floating "What's included" card (`.ix-srv-hero__card`, absolute, max-width:72%) → made
  `position:static` below the image @≤560px so it's readable instead of overlapping.

Breakpoint scheme for the service family: **1000 / 860 / 560 / 400**. Global overflow guard
(`html,body{overflow-x:clip}`, components.css ~1647) already present.

**Verify:** CSS brace-balanced (Edit/lint), and the served `components.css` over `http://prelive.local`
contains the new rules. Could NOT screenshot mobile myself (project tooling pins desktop — see
[[mobile-responsiveness]]); USER should spot-check at 375/414/768/1024px in DevTools. This was a
LOCAL `prelive.local` edit — to reach the remote `prelive.indexworld.net` it must be pushed +
LiteSpeed purged (see [[blog-bold-link-color-gotcha]], [[multi-dev-collaboration]]).
Related: [[industries-build]], [[carousel-guidelines]].
