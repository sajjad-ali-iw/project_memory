---
name: faq-accordion-smooth
description: "How the FAQ (.ix-ifaq) got a smooth animated single-open accordion, and the <details> transition gotcha"
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

The FAQ (`patterns/faq.php`, `.ix-ifaq`) is a native `<details>`/`<summary>` accordion (staging's cannabis-industry FAQ is the same — and opens **instantly**, no animation). Made it **smooth + single-open** 2026-06.

**How:** `assets/js/faq-accordion.js` (enqueued `indexworld-faq-accordion`) + CSS scoped to a JS-added class `.ix-ifaq--js`:
- JS adds `.ix-ifaq--js` to `.ix-ifaq__list`, sets every `<details>` `open` (always rendered), and on summary click `preventDefault()` + toggles an `.is-open` class (removes it from all others first = single-open).
- CSS: `.ix-ifaq--js .ix-ifaq__a{ overflow:hidden; max-height:0; transition:max-height .35s }`. The `+` icon (`.ix-ifaq__ico-plus`) rotates 45° → `×` when `.is-open`.
- **SMOOTHNESS FIX (2026-06-23):** the old `.is-open{ max-height:600px }` rule made open/close feel "hard" — content is ~150px so it raced to height instantly then snapped shut (animating empty space). Now JS sets the **exact** height: `expand()` → `a.style.maxHeight = a.scrollHeight+'px'`; `collapse()` → `a.style.maxHeight=''` (reverts to CSS `max-height:0`). Plus a `window.resize` handler re-measures the open item. No more guessed max → genuinely smooth both ways.
- Answer markup needs an inner wrapper: `.ix-ifaq__a > .ix-ifaq__a-in > p` with the padding on the `<p>` (a grid/clip child's own padding isn't clipped by its own overflow).
- No-JS fallback: animation is scoped to `.ix-ifaq--js`, so without JS it's a plain `<details>` accordion.

**Gotchas (cost a lot of debugging):**
1. **A CSS transition won't fire on content revealed the same frame.** Toggling native `<details>` `open` reveals the answer from a hidden state, and the max-height/height/grid transition just jumps. Fix = keep `<details>` permanently `open` and animate only via a class; never rely on the native open/close for the animated state.
2. `grid-template-rows: 1fr` resolves to **0px** inside `<details>` content in Chrome (won't size to content there) — grid-rows accordion technique fails here; use `max-height`.
3. **`getComputedStyle(el).maxHeight`/`.height` reports the *mid-transition* value**, so measuring height right after a click reads ~0 even though it's animating fine — don't trust point-in-time measurements to verify animations; screenshot instead.

Related: [[section-port-method]], [[carousel-guidelines]].
