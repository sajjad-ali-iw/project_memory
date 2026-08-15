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

## Process step imagery — ALL SIX replaced 2026-08-15 (Media Library WebP, ~203 KB total)

| Step | Image | Source |
|---|---|---|
| 1 Understand the need | `cust-understand-brains.webp` (34 KB) | generated — two bright brains connecting |
| 2 BPMN process mapping | `cust-bpmn-mapping.webp` (24 KB) | generated — ERP BPMN w/ Sales/Inventory/Finance swimlanes |
| 3 Design & approval | `cust-design-approved.webp` (33 KB) | generated — flow diagram + signature + APPROVED stamp |
| 4 Development & testing | `cust-development-testing.webp` (23 KB) | generated — code editor + passing test-results panel |
| 5 Deployment & data | `cust-deployment-data.webp` (52 KB) | generated — server → data records → live dashboard + permissions shield |
| 6 Support & maintenance | `cust-support-maintenance.webp` (37 KB) | generated — support agent, ticket resolved, upgrade-safe wrench+shield |

### Imagery direction for this page (owner's accumulated steer)

- **Bright, vibrant, colourful illustrations** — not muted stock photos. Photo candidates for step 3
  (blueprint review, pen-signing-a-document) were sourced and **rejected** in favour of illustration.
- **Purple is FINE** — it is the brand primary (`#50269E`). During generation the owner said
  *"recreate without purple"*, but immediately clarified afterwards: *"i mistakenly told you about
  purple — purple is fine"*; the real objection was to a **solid purple BACKGROUND** filling the
  frame, not to purple as a colour. So: use purple freely in elements; avoid a heavy purple
  background wash. Steps 1/2/4 contain purple and are staying as they are.
- **High contrast** matters to the owner; a low-contrast/minimal-colour variant was tried and
  rejected in the same breath (*"bright colors"*).
- All six were generated via the Higgsfield MCP (`nano_banana_pro`, aspect 3:2, 2 variants each shown
  for approval via `SendUserFile`), then uploaded through the weserv proxy for URL-side WebP+resize —
  see [[fast-image-upload-method]].

**Gotcha:** the first weserv-proxied upload of a freshly generated image **timed out** (cold cache).
Curl the weserv URL once to warm it, then re-issue `wp_upload_media_from_url` — it succeeds.

## Open / next

- Owner asked for the **Support & maintenance** step (step 6) to use odoo-implementation's support
  image `/wp-content/uploads/2026/06/support-hero.webp`, **and** for an internal link to `/support/`
  to be added on this page. Support page confirmed to exist at `/support/` (200);
  odoo-implementation already links to it. **Not yet done.**
