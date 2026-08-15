---
name: odoo-service-page-recipe
description: "THE repeatable recipe for bringing an Odoo service page up to standard — the component swaps, copy rules and checks applied to odoo-implementation, odoo-customization, odoo-audit and odoo-training-services in Aug 2026. Read this before starting the next page."
metadata:
  type: project
---

The owner works through the Odoo cluster one page at a time and expects the **same treatment each
time** ("you have seen our work pattern"). Applied so far to **odoo-implementation (362)**,
**odoo-customization (646)**, **odoo-audit (676)** and **odoo-training-services (678)**.

## The component swaps (this is what "convert it like the previous pages" means)

| Old component | Convert to | Why |
|---|---|---|
| `ix-ihero` (plain white hero) | **`ix-srv-hero`** | Gives the purple radial-gradient panel + the mobile rules already built (one-row CTAs, taller image, overlapping card). Rename every child: `__grid/__copy/__title/__sub/__actions/__media`, buttons become `ix-btn ix-btn--primary` / `ix-btn ix-btn--light`, and the img needs `class="ix-srv-hero__img"`. |
| `ix-srv-stats` (old stat strip) | **`ix_trust_bar()`** → `ix-sup-stats`/`ix-pstat` | The responsive white bar. First stat links to `https://www.odoo.com/partners/index-world-llc-23192027`. |
| `ix-srv-why` (checklist + image) | **`ix-iwhat ix-iwhy`** + `ix-whyx__c` icon cards | Each reason needs an SVG icon as a 3rd array element. |
| `ix-ihow` (checklist + side image) | **`ix-srv-process`** numbered steps | `__head` (title + `__sub`), then `<ol class="ix-srv-process__track">` of `__step` → `__thumb` (img + `__num`) + `__body` (`__stitle` + `__sdesc`). |
| `indexworld/trusted` ("Tools & Frameworks") | delete | Owner removed it on customization and audit. |

Other standard moves: hero buttons → **"Book a Demo" / "Talk to Us"** (short enough for one mobile
row); remove hero chips; page content must be **DB-inline**, not a pattern file
([[prefer-db-inline-over-pattern-files]]).

## Copy rules the owner enforces

- **Say what the service actually does — don't over-promise the next engagement.** On audit the cards
  said *"We fix / We clean / We optimize"*; the owner caught it: *"it should be we identify or tell
  you problem because it is an audit not work job."* Verbs became *pinpoint · show · flag · map ·
  report · test*. **Check every page for this.**
- **Cross-check claims against the pricing tiers.** On audit, steps 3 and 5 were not in the $2,000
  Standard tier — they now carry a purple pill badge reading "Enhanced & Preferred". Always diff the
  process/feature copy against what each tier actually buys.
- **Keep it short.** Repeated instruction: *"no extra text"*, *"reduce the text a bit overall"*.
  Prefer one tight paragraph over two; trim any sentence added for linking down to a clause.
- **Never invent proof.** Client metrics, named quotes and check-counts must come from the owner.
  See the open gaps in [[odoo-audit-page]].

## Internal linking

Aim for **~20+ editorial links**. The sections that are always empty and worth checking:
the two `ix-iwhat` blocks, the **FAQ answers** (biggest untapped surface), and the process steps.
Weave links into existing sentences rather than adding new ones. Useful targets:
`/odoo-partner/` (proves "certified team" — often missing entirely), `/case-studies/`,
`/odoo-implementation/`, `/odoo-customization/`, `/odoo-upgrade-services/`, `/support/`,
`/odoo-rescue/`, `/hire-odoo-developer/`, `/odoo-audit/`.
**Card-heading links must carry `style="color:inherit;text-decoration:none;"`** or they render in
link-purple and no longer match the plain-black headings on other pages.

## Layout gotchas learned the hard way

- `.ix-ichal--withsub` drops the H2's bottom margin **50px → 14px** — use it whenever a sub-paragraph
  sits under the heading.
- `.ix-srv-deliver--wx` is the **3-column** variant of the deliver grid (base is 4-col), and it is
  already responsive 3→2→1.
- `ix-srv-process__body` has **no list CSS**. Bullets under a step need inline
  `style="margin:10px 0 0;padding-left:18px;color:#000;"`. Do **not** add `list-style-type:disc` —
  the owner compared both and preferred the browser's default hollow circles (the `ol ul` nested
  default).
- Anchor links work out of the box: the theme sets `scroll-behavior:smooth` and
  `scroll-padding-top:96px` for the fixed header. Add `id="…"` to the target section.
