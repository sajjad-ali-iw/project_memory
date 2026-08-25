---
name: industry-page-standardisation
description: "The 2026-08-25 pass that put all 16 industry pages on one hero treatment and cut the ix-iwhat explainer to one screen — the measured targets, and why mobile one-screen is not achievable"
metadata:
  node_type: memory
  type: project
---

Owner instruction, 2026-08-25: *"the same hero will be design will be use accross all industries"* and, for the explainer, *"need to make it really should like is should be able to view in single page."* Both applied to every industry page and both are DB-inline edits ([[prefer-db-inline-over-pattern-files]]) — no theme change was needed for either.

## The industry hero standard

**Industry pages keep `.ix-ihero`. Do NOT convert them to `.ix-srv-hero`** — that swap belongs to the Odoo *service* cluster only ([[odoo-service-page-recipe]]). "Make it responsive like the service pages" is a **modifier**, not a component swap: `/erp-consultant/` and `/erp-implementation/` are themselves `.ix-ihero` pages, they just carry `ix-ihero--svc`.

Add `ix-ihero ix-ihero--svc` to BOTH the wp:group JSON `className` and the `<section>` tag. That turns on, from components.css ~4086: copy + button row centred at ≤900px, and button side padding 28px → 18px at ≤600px. Then:

- **One** `<p class="ix-ihero__sub">`, roughly 160–220 chars. Several pages bolded the whole first paragraph in `<strong>` — drop it, the `/erp-consultant/` reference does not.
- **Short button labels.** The 18px padding is what makes both buttons share one mobile row; measured at 360px the row has 324px available. "Get a Free Demo" (162px) + "Who We Are" (126px) + 14px gap = 302px ✓. "Talk to an expert" (157px) also fits. **"Book a Free Consultation" does not** — it was replaced on equipment-rental-software and office-furniture. "Why Index World" (159px) did not fit either and became "Who We Are" on food-beverage-industry — same `/about-us/` target, so it was only ever an inconsistent label.

Applied to all 16: multi-channel-ecommerce 912, wholesale-distribution 923, retail-erp 1201, print-on-demand 657, manufacturing-industry 805, chemical-industry 1405, food-beverage-industry 1413, construction-erp-software 1291, aviation-erp-software 1189, logistics-software-development 1873, equipment-rental-software 1166, real-estate-erp 772, self-storage-software 1232, hotel-management 1324, office-furniture 760, education-erp 996.

⚠️ **1166 has no `className` in a wp:group** — its hero sits in a raw `wp:html` block, so only the `<section>` tag needed editing. Guard every hero edit with `expected_count`; a page that fails the className find is this case, not a mistake.

## The ix-iwhat one-screen rule

Target **3 paragraphs, no `ix-iwhat__h3`, ~1,100–1,450 chars.** The site median for this component was already 1,116 chars, so this is the existing norm rather than a new constraint. Measured result at 1440×900: every trimmed section lands 724–894px, i.e. **0.80–0.99 screens**. Logistics is the tightest at 0.99 — leave headroom if you add to it.

The image in `.ix-iwhat__media` **stretches to match the copy column**, so an over-long section also distorts its photo (multi-channel-ecommerce was rendering a 1338px-tall image). Shortening the copy fixes the image for free; never "fix" that with a height on the img.

⚠️ **One screen on MOBILE is not achievable and should not be chased.** At 375×812 the image alone takes 254px, heading ~60px, section padding 70px — leaving ~430px, about 480 characters, for the page's main keyword-bearing explainer. Trimmed sections sit at ~1.75 screens on mobile, down from ~2.9. If the owner pushes for one mobile screen, the honest options are hiding the section image on mobile (buys back 254px) or accepting a 3-sentence section.

Trimmed on 8 pages (912, 1291, 1405, 805, 996, 923, 1324, 1873) — 15,090 → 10,409 chars, −31%.

## What to cut, in priority order

1. **Cross-page boilerplate.** *"Anyone can install the software. Plants stay broken because…"* ran on both chemical and manufacturing barely reworded; *"That is the relationship we are looking for too."* closed both education and hotel identically. Duplicate content across pages is a liability, not depth.
2. **Same-page triplication.** On multi-channel-ecommerce the "follow one order through the day" narrative was told three times — in `ix-iwhat`, again by the `ix-ichal` day timeline (Morning → End of Day) and again by the `ix-iccar` carousel. Cutting one instance loses nothing.
3. **Generic H3s.** "Why it matters", "The result", "Keep what already works" carry no keywords and have nothing to subdivide once the section is three paragraphs.

**Never cut:** the definition sentence carrying the primary keyword, keyword variant lists (wholesale's "warehouse distribution software / retail distribution software / wholesale distribution ERP", logistics' "logistics software development services"), named client proof, or editorial links. All 13 inline `<a>`s in the trimmed sections were carried into the new copy — two of them (`/support/` on equipment-rental, `/odoo-accounting/` on education) lived in paragraphs that were being deleted and had to be re-homed. Aligns with the recipe's "aim for ~20+ editorial links" and its "keep it short" rule at the same time.

Related: [[odoo-service-page-recipe]], [[story-carousels-and-switch-sections]], [[shared-service-component-helpers]], [[mobile-responsiveness]].
