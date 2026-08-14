---
name: lazy-load-hidden-tabs-gotcha
description: "Images with loading=\"lazy\" inside initially-hidden tab panels never load when the tab is revealed via JS"
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

In any JS-tabbed component (services-tabs, feature-tabs, industry-tabs), an `<img loading="lazy">` that sits inside a panel which starts `display:none` will **not load** when the user switches to that tab — the browser deferred the lazy load while hidden and never re-triggers it on reveal. Only the first/visible panel's image loads; the rest stay blank (`naturalWidth === 0`, even though the file serves HTTP 200 and a fresh `new Image()` decodes it fine).

**Fix:** use `decoding="async"` (drop `loading="lazy"`) on images inside hidden tab panels so they all load on page load. Done for `patterns/services.php` (the .ix-svc tab media). Watch for the same pattern if adding images to other tabbed panels.

**Why:** the panels render server-side but only one is shown; lazy-load + initial display:none is a known browser quirk.

## LiteSpeed re-introduces this, so `decoding="async"` alone is NOT enough (2026-08-03)

`decoding="async"` fixes the *browser's* native lazy-load, but **LiteSpeed Cache's own image
lazy-load rewrites every `<img>` anyway** — it replaces `src` with a base64 SVG placeholder, moves
the real URL to `data-src`, and adds `data-lazyloaded="1"`, then loads it from JS on scroll. Images
in a `display:none` panel never enter the viewport, so the observer never fires and they stay blank.
Same symptom, different cause — and it survives any theme-side attribute change.

**Full fix = both attributes:** `decoding="async" data-no-lazy="1"`. LiteSpeed honours `data-no-lazy`
(and `data-skip-lazy`) per-image — see `plugins/litespeed-cache/src/media.cls.php` L1054 + L1208 — so
this stays in the theme and needs **no plugin-setting change**. Applied to all 41 tool pills in
`patterns/stack.php` on 2026-08-03; verify with:

```
curl -s http://indexworld-revamp.local/ | grep -c 'data-src'    # should not cover hidden-panel imgs
```

**How to check a section quickly:** fetch the page, isolate the section, and count imgs whose `src`
is still real vs. moved to `data-src`. An image that is *visible* on load (e.g. the eyebrow badge)
can keep LiteSpeed lazy-load safely — only images inside hidden panels need the opt-out.

**Watch for:** `patterns/stack.php` regressed to `loading="lazy"` even though `services.php` — the
same `.ix-svc` tab component — had already been fixed. When adding images to ANY tabbed panel, copy
the attribute pair, not just the markup.

Related: [[section-port-method]], [[industries-build]], [[blog-bold-link-color-gotcha]] (the other
LiteSpeed-masks-a-theme-change trap).
