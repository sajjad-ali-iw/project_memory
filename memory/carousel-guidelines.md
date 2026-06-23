---
name: carousel-guidelines
description: "The standard recipe for building ANY carousel on this theme (structure, dots, CSS, JS, mobile)"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 2aa1cd17-8308-49ff-bc9f-c09f08dc803c
---

THE carousel standard for IndexWorld. Build every new carousel this way so they're all consistent. Reference implementations: `.ix-iccar` (image-card, industry pages), `.ix-cases` (homepage Case Studies), and `.ix-blogcar` (latest-posts carousel — `assets/js/blog-carousel.js`, exact house dots/arrows + autoplay, built 2026-06-22). Pattern follows the staging design.

**Blog Carousel is a REUSABLE COMPONENT:** `patterns/blog-carousel.php` (slug `indexworld/blog-carousel`, title "Blog Carousel") = self-contained section (eyebrow + "From the Index World Blog" heading + carousel of 9 latest posts using the `.ix-blog__card` design). Drop it anywhere via `<!-- wp:pattern {"slug":"indexworld/blog-carousel"} /-->`. The homepage `patterns/blog.php` (slug `indexworld/blog`, referenced by `front-page.html`) is now just a thin wrapper that references it. Cards reuse the `/blog/` listing card markup so they stay in sync.

**MARKUP** (BEM `ix-<name>__*`):
```
.ix-x  (section)
  .ix-x__head   → ix_badge(...) eyebrow + 46px title + sub   (centered)
  .ix-x__viewport (overflow:hidden)
     .ix-x__track (display:flex; gap)  →  .ix-x__card × N
  .ix-x__nav  → .ix-x__abtn--prev + .ix-x__dots + .ix-x__abtn--next
```

**DOTS — the house style (use these exact values everywhere):**
- container: `display:flex; gap:8px; position:absolute; left:50%; transform:translateX(-50%)` (centered in nav).
- dot: `width:10px; height:10px; border-radius:50%; background:#D6D1E8; transition:width .3s, background .3s`.
- active: `width:26px; border-radius:5px; background:#060038` (dark-navy pill).
- JS adds `.is-active` to the dot at `Math.round(pos/step())`.

**ARROWS:** round 56px, `background:#ECE6FF; color:#060038`, `:hover #ddd2ff`, `:disabled opacity:.4`. Shown on desktop only.

**DESKTOP CSS:** card fixed width (e.g. 325px), `.ix-x__viewport{ overflow:hidden; max-width:1360px; margin:0 auto }`, track `gap:20px; transition:transform .55s cubic-bezier(.22,.61,.36,1)`.

**MOBILE (≤640 or ≤900) — centered active card with peek both sides:**
- carousel must be **FULL-BLEED**: remove the section's horizontal padding (`.ix-x{ padding:Y 0 }`) and re-add inset to `.ix-x__head`/`.ix-x__nav` only — otherwise the side padding offsets the centering.
- `.ix-x__card{ width/flex-basis:80vw; max-width:none }`
- `.ix-x__track{ padding-left/right:(100vw - cardW)/2 }`  (80vw card → 10vw; 78vw → 11vw). With this, the plain `pos = i*step` snap lands every card centered (math: `max() == (n-1)*step`).
- `.ix-x__abtn{ display:none }` (no arrows on phone — swipe + dots).
- GOTCHA: a desktop `left:` on an element that becomes `position:relative` on mobile (e.g. an overlapping intro card) leaks and shifts it — reset `left/right/top:auto` on mobile.

**JS (copy from case-studies.js / image-card-carousel.js):**
- `step()` = card width + gap; `max()` = `track.scrollWidth - vp.clientWidth`.
- Arrows: `pos ±= step()`, clamp [0,max]. Dots: click → `pos = i*step()`.
- **Drag to move — BOTH finger AND mouse** (essential): factor into shared `dragStart(x,y)`/`dragMove(x,y,e)`/`dragEnd()`. touch → touchstart/move(NON-passive)/end; mouse → `mousedown` on viewport (button 0, preventDefault) + `mousemove`/`mouseup` on **document** (so drag continues outside the viewport). dragMove direction-locks (horizontal only when |dx|>|dy| && |dx|>6, else let page scroll) and `preventDefault()`s when cancelable; rubber-band ×0.35 past ends; dragEnd snaps ≥1 card per swipe (threshold `step()*0.16`). Suppress the card link click if a drag happened (`click` capture: if moved, preventDefault) and `track.addEventListener('dragstart', e=>e.preventDefault())` to kill native img/link drag. CSS: viewport `cursor:grab` + `:active{cursor:grabbing}` + `touch-action:pan-y`; card img/media `-webkit-user-drag:none; user-select:none`.
- **Autoplay** (optional, home uses it): `matchMedia('(max-width:900px)')` only, `setInterval(advance, 3500)`, loop to 0 at end, pause on touchstart + resume ~4s after touchend.
- **Multiple dot containers:** build/update dots in ALL `.ix-x__dots` (querySelectorAll) so an in-card mobile dots row + the desktop nav dots stay in sync. Dots are **clickable** (jump to slide) on both desktop and mobile; a dot click also calls `stopAuto()` so autoplay doesn't yank you off the chosen slide.

Validate: `node --check` the JS; keep components.css braces balanced. Related: [[mobile-responsiveness]], [[industries-build]].
