---
name: hero-rotator-two-line-lock
description: Hero H1 rotator made the headline flip 2↔3 lines on phones (vertical jump); fixed by pinning the line count in markup — 3-line stack on phones, 2 lines on desktop
metadata:
  node_type: memory
  type: project
---

**2026-08-03 — homepage hero headline: line count pinned so the rotator can't shift the page.**

The rotating word in `.ix-hero__title` (`patterns/hero.php`, words `Automate / Scale /
Understand / Simplify / Grow`) sat on line 1: "`<rotator> your business`". Word widths
range 4→10 chars, so on a phone the short words held one line and the long ones wrapped —
the H1 flipped between **2 and 3 lines** and shoved the subtitle + CTAs down ~35px on every
cycle. Reported as "the spacing changes during the animation".

**Measured** in the real heading font at the ≤480 size (29.6px), vs 339px of content width
on a 375px phone (375 − 18px hero padding ×2):

| line 1 variant | width |
|---|---|
| Grow your business | 322px ✓ |
| Scale your business | 323px ✓ |
| Simplify your business | 368px ✗ |
| Automate your business | 400px ✗ |
| Understand your business | 429px ✗ |

**The fix — 2 CSS declarations, no type scaling:**

```css
.ix-hero__title .ix-hero__line{ display:block; white-space:nowrap; }   /* breaks are markup, not wrapping */
@media (max-width:600px){ .ix-hero__title .ix-hero__np{ display:block; } }  /* rotator gets its own line */
```

Markup is three nested spans: `.ix-hero__line`(`.ix-rot` + `.ix-hero__np`) + `.ix-hero__line`.
Desktop = 2 lines (unchanged); ≤600px = 3-line stack `Understand` / `your business` /
`with Index World` = 223/261/316px at the largest applicable font (34px), clearing a 375px
phone at **full design type size**.

**LESSON — don't force two lines on a phone.** The first attempt pinned it to two lines with
`white-space:nowrap` + a vw-fitted `font-size:min(<tier>, calc(6.8vw - 2px))`. It worked, but
two lines is only reachable by *shrinking the type ~20%* (the whole headline is 712px of text
vs 339px of phone width, so 2 lines is arithmetically impossible at the design size). Owner
correctly pushed back: **more lines is the cheap fix, smaller type is the expensive one.**
When a headline won't fit, add a line before you scale the font.

**RULE:** any headline with a rotating/typewriter word must have its line count pinned in
markup (block + nowrap). Never let the rotator sit on a line that can rewrap. Reserving a
fixed width for the word is also wrong — it leaves visible gaps around short words in
centred type.

**HOW TO MEASURE FONT WIDTHS ON THIS PROJECT (no browser needed):** the heading font is
`assets/fonts/HappyDisplay-SemiBold.woff` (CFF, 1000 upem). No fontTools here, so decode the
WOFF by hand — zlib-inflate each table from the 44-byte header + 20-byte directory entries,
then read `head` (upem @18), `hhea` (numberOfHMetrics @34), `hmtx` advances and the `cmap`
(format 4). Sum advances → exact px width at any font-size. Turns "does it wrap?" into
arithmetic, which matters because mobile screenshots aren't available (see
[[mobile-responsiveness]]). Related: [[hero-and-frontpage]].
