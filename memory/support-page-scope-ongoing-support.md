---
name: support-page-scope-ongoing-support
description: /support/ sells ongoing unlimited support & maintenance (monthly, post-go-live) — NOT implementation or its cost
metadata:
  type: project
---

The `/support/` page (id 361, `patterns/support-page.php`) sells **ongoing unlimited
Odoo support & maintenance**, billed **monthly** ($250 Saviour / $750 Champion /
$1500 Knight / Custom Hero) — the service you buy **after** implementation / go-live.

It is **NOT** the Odoo implementation project, and its prices are **not** implementation
cost. Keep all copy (hero, plans, FAQ, feature blocks) framed around ongoing post-go-live
support: unlimited help, maintenance/bug-fixing, upgrades, hosting, hypercare — never
"how long/how much to implement Odoo". Implementation lives on `/odoo-implementation/`
(id 362) and is cross-linked from the hero, that's fine.

**FAQ scope (FIXED 2026-08-05):** the two mis-scoped FAQ items were reworded to
support-focused ones ("What does an Index World support plan include?" / "How quickly do
you respond to support requests?").

**IMPORTANT — the 2026-07-13 layout below was a DECISION that was never actually built
until 2026-08-05.** Before then the local/prelive `support-page.php` was still the old
~2026-07-08 layout (old `.ix-sup-hero`, plans near top, `.ix-sup-cmp` comparison present,
4 alternating `.ix-sup-feat` blocks). The full redesign + everything below was built and
shipped on 2026-08-05 (commit "Support page: full build …").

**Layout history (2026-07-13):** reordered to explain-then-price
(hero → support types → services grid → why → plans → FAQ → CTA); the
"Flat-Rate vs Hourly Billing" comparison section + its `$ix_compare` array were removed
(dead `.ix-sup-cmp*` CSS still in components.css). The 4 alternating feature blocks
(`.ix-sup-feat`, expert help / training / 500+ apps / portal) were merged into ONE section
that REUSES the home page "Our Services" tabbed component (`.ix-services`/`.ix-svc`,
services-tabs.js) — 4 tabs (Expert help / Team training / 500+ apps / Client portal),
each panel = the feature's title:desc + image. Driven by the `$ix_feats` array (its 4th
element is now the short tab label, was the unused heading tag). Old `.ix-sup-feat*` and
the interim `.ix-sup-svc*` CSS were removed. See [[service-pages-responsive]].

**Hero:** now uses the shared `.ix-srv-hero` design (same as `/odoo-implementation/`) —
copy + `.ix-btn` primary/light + image with floating "What's included" card. `.ix-srv-hero__grid`
self-constrains (max-width:1200px) so it works as plain markup inside `wp:html` (no wp:group
needed, unlike `.ix-services`). Old `.ix-sup-hero*` CSS is now dead (kept, harmless).

**GOTCHA — .ix-services needs the constrained wp:group wrapper.** `.ix-services` has NO
max-width in CSS; its 1200px constraint comes from the `wp:group {"layout":{"type":
"constrained","contentSize":"1200px"}}` wrapper (WP injects the max-width style at
render). Reusing `.ix-services`/`.ix-svc` as raw markup INSIDE a `wp:html` (Custom HTML)
block skips that → content stretches full-width and looks broken. Fix: emit it as a real
`wp:group` block at pattern top level (close the surrounding `wp:html` first, reopen after)
— mirror `patterns/services.php` exactly. Same applies to any `.ix-svc`/`.ix-services`
reuse.

**2026-08-05 BUILD & SHIP (full redesign landed + deployed to prelive):**
- Services tabs `$ix_feats` order: **Client portal (tab 1, default) / Expert help / Team
  training / 500+ apps**. `$ft` slots: [img, title, desc, tabLabel, img2?, bullets?]. desc
  supports multiple paragraphs split on `||`. Panels with `$ft[5]` (bullets) hide the strong
  title line.
- **Client portal tab** = a *cluster* collage in the media cell (`.ix-svc__media--cluster`,
  scoped `> img:nth-of-type(1|2)` so chip logos aren't hit): Gantt (`feat-portal.webp`) base +
  dashboard (`feat-dashboard.webp`) overlapping lower-right + a "Also in your portal" chip
  (`.ix-svc__portalx`) with real **Google Drive + Zoom** logos (`drive.svg`/`zoom.svg`). Text
  column has a purple-dot bullet list (`.ix-svc__ul`): live tracking & planning / Zoom AI
  transcripts / auto ERP docs w/ diagrams / module-specific training material / dedicated secure
  Google Drive / team access.
- **500+ apps tab** = a 12-tile grid (`.ix-appgrid`, branch on `'500+ apps' === $ft[3]`) of real
  Odoo app icons (`assets/images/support/apps/*.webp`, sourced from Odoo GitHub via jsDelivr).
  `.ix-svc__media--apps .ix-appgrid__ic{width:34px}` needed to beat base `.ix-svc__media img`.
- **Support types** section rebuilt as the `.ix-srv-deliver--wx` "why an expert" card design
  (`$ix_types`, 6 cards, checklist bullets). Order: Functional / Technical / Server&Hosting /
  Role-based User Trainings / Upgrade / Monthly Audit&Health Report.
- **Plans**: dark gradient band (`#2a2160→#171236`), CTA "Start Now"→`/contact-us/` (featured
  "Free Demo"→booking), "Monthly Odoo Audit"→**"Annual Odoo Audit"** (linked `/odoo-audit/`),
  "500+ apps"/"Unlimited training and support"/"Any number of Odoo users"; Custom(Hero) adds
  version migration, role-specific e-learning, AI automations, unlimited new developments,
  Direct Print (all plans).
- **Stats strip** `.ix-sup-stats` after hero (Official / Unlimited tickets / $0 / Anytime).
- **Nav relabel** (`parts/header.html`): top-level & mobile "Pricing"→**"Support Plans"**;
  mega-menu item + drawer sub-item → **"Odoo Support Plans"**. "Get support" CTA left as-is.
- **Deploy**: theme pushed to GitHub `main` (prelive auto-pulls same repo). **Images →
  prelive Media Library** (owner convention): 14 rasters uploaded + tagged `_iw_src` (ids
  1793 feat-portal, 1794 feat-dashboard, 1795–1806 the 12 app icons). **SVGs (drive/zoom)
  can't be Library-uploaded — prelive REST rejects `.svg`** — so they stay theme-served
  (render fine via fallback). Verified prelive `/support/` serves the Library copies.
