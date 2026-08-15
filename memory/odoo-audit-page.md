---
name: odoo-audit-page
description: "State of /odoo-audit/ (page 676) as of 2026-08-15: converted pattern→DB-inline, rebuilt to the odoo-customization recipe, then extended to beat competitor silverdaletech.com/audit. Lists the remaining competitive gaps that need the owner's real data."
metadata:
  type: project
---

Page **676**, `https://prelive.indexworld.net/odoo-audit/`. Rebuilt 2026-08-15.

## Architecture

Converted from `patterns/odoo-audit.php` to **inline DB block markup**; the pattern file and its
`.bak` were deleted (theme commits `a6f41eb`, `40eb0bd`). See
[[prefer-db-inline-over-pattern-files]] for the recipe. Step-3/4 process images had to be baked to
their repointed `uploads/*.webp` URLs — the `render_block` repoint filter does NOT apply to DB
content, so theme `assets/images/...jpg` paths would have silently served unoptimized files.

## Section flow (final)

Hero → trust bar (`ix_trust_bar`) → **Is an audit right for you?** → Issues We Catch (6) →
Our Odoo audit process (5 steps) → **What a finding actually looks like** → Pricing (3 tiers) →
Why Index World audits end in action → case-study carousel → FAQ (8) → CTA.

## Owner decisions (2026-08-15)

- **Keep the pricing/packages section** ($2k / $7.5k / $15k) — audit is fixed-scope, prices help
  buyers self-qualify. (Contrast: odoo-implementation had its vague pricing paragraph removed.)
- **Keep the factual SEO H1** "Odoo Audit Services in the USA" — do NOT make it catchy like
  odoo-customization's. Exact-match keyword is worth more here.
- Hero buttons → "Book a Demo" / "Talk to Us"; the 3 hero chips were removed.
- **Audit imagery must be REAL PHOTOS**, not illustrations — *"real work picture… like an audit is
  being done."* Rationale: audit is a credibility/trust sale. This is the OPPOSITE of
  [[odoo-customization-page]], which uses bright generated illustrations.
  **Image work resumed 2026-08-15 and the HERO IS DONE.** Library id 3361
  `/wp-content/uploads/2026/08/odoo-audit-hero.webp` (1100×730) replaced the generic reused
  `about-cta-realistic.png`. **Superseded the same day by id 3371
  `odoo-audit-hero-laptop.webp`** — same yellow flat-lay language (magnifying glass + twin-bell alarm
  clock) but with an open laptop showing a colourful dashboard instead of the lettered card.
  **Why the card version lost:** the hero's own white "WHAT WE CHECK" card is absolutely positioned
  bottom-left (`.ix-srv-hero__card{left:18px;bottom:18px;width:248px}`) and sat directly on top of the
  black card, hiding the word "AUDIT". **Rule for any future hero image here: keep the bottom-left
  quadrant empty** and put the subject right-of-centre. Flipping an existing photo to do this does not
  work when a clock is in frame — the dial mirrors and the numerals come out backwards; regenerate
  instead. The retired card version (id 3361) is still in the Library, unreferenced.
  **The 5 process-step images are now DONE too** (2026-08-15, Library ids 3387–3391, all 1000×750
  WebP): Discovery call / System review / Process review / Findings & report / Implement & support.
  Brief was *"like /odoo-implementation/ but better … real … preferably bright colors"*, so they are
  deliberately sunlit and saturated against that page's dim corporate set. Note steps 3 and 4 had been
  pointing at odoo-implementation's own files, which is why the owner read them as look-alikes. The 5
  Unsplash candidates approved before the earlier pause were never used. See [[image-library-log]].
  An Odoo-logo-on-the-card version was generated and rejected — the owner said *"forgot odoo logo"*.

## Competitor benchmark — silverdaletech.com/audit

Their pricing tiers are **identical** ($2,000 / $7,500 / $15,000), so the fight is on proof.

**Gaps CLOSED on our page 2026-08-15:**
- "Who is the audit for?" qualification → 3 `ix-ichal` cards
- Objection-handling FAQ → added *Do you need access to our Odoo?* / *What happens after?* /
  *What makes your audit different?* (FAQ 5 → 8)
- Sample-findings deliverable → new `ix-srv-deliver` section, 4 example findings each naming
  the action, the Odoo feature, what it stops costing, and the owner
- Hero sub rewritten to lead with the loss ("Most teams use a fraction of what they pay Odoo for")

**Gaps STILL OPEN — all blocked on the owner's real data (do NOT invent these):**
1. **Case study with before→after numbers.** Their strongest asset: *"The $75K Decision They Didn't
   Have to Make"* — custom modules 30→5, close time 15→6 days, user confidence 38%→92%, $75k rebuild
   avoided, plus a named COO quote. We have 9 case-study cards but **no numbers on the page**.
2. **Named founder/expert quote** (theirs: CEO Nick Foy, *"It's a forensic process"*).
3. **Concrete scope number** — they claim "over 1,000 settings and checks"; our trust bar says
   "10+ system areas reviewed", which undersells the work.
4. **Money-back guarantee + "Buy Now"** self-serve purchase — business decision, not a copy change.
5. **Inline email capture form** on-page (ours links out to /contact-us/).

**Where we already beat them:** clickable verified Odoo Partner link, visual 5-step process,
9 real client case studies, internal links to customization/upgrade/support/rescue, and a
diagnostic "Issues We Catch" grid.

## Gotcha fixed

The shared `indexworld/service-case-studies` carousel had customization-specific copy
("Real customizations, real results") which read wrong once the audit page reused it. Neutralised to
"Real projects, real results" (commit `b336376`). **Any future page reusing that pattern inherits
this heading** — keep it generic.
