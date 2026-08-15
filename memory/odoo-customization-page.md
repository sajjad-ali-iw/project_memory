---
name: odoo-customization-page
description: "State of /odoo-customization/ (page 646) as of 2026-08-15: converted from a pattern file to DB-inline content, new hero heading/image, shared white trust bar, and the process-step imagery that was replaced."
metadata:
  type: project
---

Page **646**, `https://prelive.indexworld.net/odoo-customization/`. Worked on heavily 2026-08-14/15.

## Architecture change (the big one)

Was a single `<!-- wp:pattern {"slug":"indexworld/odoo-customization"} /-->` ref pointing at
`patterns/odoo-customization.php`. On 2026-08-15 the whole page was **converted to inline DB block
markup** and the pattern file was deleted — see [[prefer-db-inline-over-pattern-files]] for the reason
and the conversion recipe. The page's JSON-LD `Service` schema block was preserved.

Section order: hero → trust bar → "What We Customize" (`ix-srv-deliver`) → "Why Odoo customizations
fail" (`ix-ichal`, 6 cards) → process (`ix-srv-process`, 6 steps) → "Why Index World customization fits
your business" (`ix-iwhat ix-iwhy` + `ix-whyx` icon cards) → case-study carousel (still a pattern ref,
`indexworld/service-case-studies`) → FAQ (`ix-ifaq`, 6 Q&A) → CTA (`ix-ctap`).

## Content decisions

- **Hero heading:** "Odoo Customization?<br>Have It Your Own Way, Literally!" (replaced "Odoo
  Customization Services in the USA").
- **Hero image:** grey McLaren in a customization workshop —
  `/wp-content/uploads/2026/08/odoo-customization-hero-sportscar.jpg` (Unsplash, Zoshua Colah).
  A brighter "vibrant paint-splatter car" version was tried and **explicitly reverted** by the owner.
- **Hero buttons:** "Book a Demo" / "Talk to Us" (matches odoo-implementation).
- **Trust bar:** switched off the old `.ix-srv-stats` onto the shared white
  `.ix-sup-stats`/`.ix-pstat` component (the responsive one odoo-implementation uses). Tailored stats
  kept: Odoo · Partner & dev team (links to the verified Odoo Partner listing) / USA · Focused
  delivery / Upgrade · Safe, documented code / 1 month · No-obligation start.
- Removed earlier: hero chips, the "Tools & Frameworks" section, a deliver-section tagline.

## Process step imagery (replaced 2026-08-15, all Media Library WebP)

| Step | Image | Source |
|---|---|---|
| 1 Understand the need | `cust-understand-brains.webp` (34 KB) | generated "two bright brains connecting" illustration |
| 2 BPMN process mapping | `cust-bpmn-mapping.webp` (24 KB) | generated bright ERP BPMN diagram w/ Sales/Inventory/Finance swimlanes |
| 3 Design & approval | `cust-design-approval-flow.webp` (13 KB) | hand-authored bright SVG (documented flow + green approval check), rasterized via Chrome headless |
| 4–6 | unchanged (`cust-development`, `how-05-deploy`, `how-07-optimize`) | |

Owner's steer on imagery for this page: **bright, vibrant, colourful** illustrations rather than muted
stock photos. Two photo candidates for step 3 (blueprints review, pen-signing-document) were sourced
and rejected in favour of the bright vector.

## Open / next

- Owner asked for the **Support & maintenance** step (step 6) to use odoo-implementation's support
  image `/wp-content/uploads/2026/06/support-hero.webp`, **and** for an internal link to `/support/`
  to be added on this page. Support page confirmed to exist at `/support/` (200);
  odoo-implementation already links to it. **Not yet done.**
