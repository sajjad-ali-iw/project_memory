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

Related: [[section-port-method]], [[industries-build]].
