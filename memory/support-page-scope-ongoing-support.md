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

**Known mismatch to fix:** two FAQ items in `support-page.php` are mis-scoped for this
page — "What does an Odoo implementation with Index World include?" and "How long does a
typical Odoo implementation take?" — they answer implementation, not support. Reword to
support-focused questions (e.g. what's covered by a support plan, response times,
onboarding onto a plan).

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
