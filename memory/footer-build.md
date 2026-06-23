---
name: footer-build
description: "The site footer, pixel-matched to staging (done 2026-06-21)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 6bd1de6c-c378-4bbd-a503-0f6d0ccf22e7
---

The homepage **footer is built and pixel-matched to staging** (done 2026-06-21). It was NOT a placeholder by the time I got to it — an earlier hand-built dark `#0b0f24` version already existed; I rewrote it to match staging exactly.

- **Markup:** `patterns/footer.php` (slug `indexworld/footer`); `parts/footer.html` just pulls the pattern. All in one `wp:html` block.
- **CSS:** `.ix-footer*` block in `assets/css/components.css` (replaced the old flat-color version).

**Staging spec captured via the mirror trick (computed styles, not eyeballed):**
- Section bg = diagonal gradient `linear-gradient(346deg,#00053D 40.29%,#000111)` (navy → near-black), white text, container max-width 1360.
- Layout = 2-col grid: LEFT a tall bordered brand card (`400px`, border `2px rgba(255,255,255,.12)`, radius 24, pad 28, spans full height); RIGHT a flex column = 4 link columns → 1px divider → 3 office cards → bottom bar.
- Brand card: white logo `Frame-2147227941.png` (215×64, **tagline "Innovative ERP Solutions." is BAKED INTO the PNG — do NOT add a separate text line, that was my first mistake**), two contact rows with **`#50269E` purple 50px circle icons**, partner-logos strip, "Our Socials" + 5 bordered 51px circles.
- Column heading 18px/400 Inter white; links 15px/500 AlmadenSans white.
- Office cards radius 16, pad 22. UK/PK = `rgba(255,255,255,.04)` + `1px rgba(255,255,255,.1)` border + lavender ghost badge (`rgba(123,108,255,.16)` bg / `#cfc8ff` text / `1px rgba(123,108,255,.35)`). **US is the HQ highlight**: `rgba(245,166,35,.08)` amber tint bg + `rgba(245,166,35,.35)` amber border + solid amber badge `#F5A623`/text `#2a1c00`. Badges: radius 999, pad 6px 11px, 10px/700, .5px tracking, uppercase. Country name 19px/800 'index happy font'. Flags = emoji in a 48px `rgba(255,255,255,.06)` circle.
- Bottom bar: ©2026 + Privacy Policy, 12px muted white.

**PARTNER LOGOS GOTCHA (the time-sink):** 4 logos in `assets/images/footer/` — Claude (`Group-1618874345.svg`, WHITE wordmark + orange asterisk), Odoo (`Gemini...Photoroom-1024x640.png`, muted purple "odoo"+green "Partner"; low-contrast on dark but matches staging — it only LOOKS missing when zoomed out), Power BI (`Frame-1-21.png`), QuickBooks (`Frame-2.png`).
- They are LIGHT/COLOR versions made for a DARK bg → keep them directly on the navy footer (NOT on a white panel — white kills the white Claude/Odoo text). Staging does the same (filter:none, transparent parent).
- **Do NOT use `width:auto`** on them — the Claude SVG's natural size is 1704px and it overflows. Set explicit per-logo widths (Claude 96 / Odoo 72 / PowerBI 50 / QB 50) with `height:auto`, `flex-wrap:nowrap`, `justify-content:space-between` → single line. User asked for single line + nicer look, so I added an "OUR TRUSTED PARTNERS" eyebrow (`.ix-fpartners__head`).

**Verified on local** via Chrome MCP (`http://indexworld-revamp.local/`) against the staging mirror. Front end renders correctly (no DB-template override needed for footer). Responsive media queries added (≤1100 stacks brand card; ≤600 single column) but deep mobile QA still pending. See [[section-port-method]], [[build-scope-plan]].
