---
name: measure-slot-ratios-before-generating
description: "Always measure an image slot's RENDERED ratio in a browser before generating for it — the width/height attributes lie, and the same component renders differently per page."
metadata:
  type: project
---

**Rule: never generate an image until you have measured the slot's real rendered ratio at BOTH
desktop and mobile.** The `width`/`height` attributes on the existing `<img>` are almost always
stale (they describe the file that happens to be there, not the slot), and the *same component*
renders at different ratios on different pages because the copy column's height drives the grid.

## Why it matters (real damage, twice)

- `/odoo-audit/` `.ix-iwhat.ix-iwhy` renders **0.54 desktop / 1.33 mobile**. A wide image with
  composited UI cards was sliced to a ~31% centre strip; the cards were cut in half. Rebuilt portrait.
- `/erp-consultant/` `.ix-ftabs` renders **0.65 desktop** while all six live images were landscape
  (1200×675) — visitors were seeing ~37% of each image. That was a live defect, not a taste issue.

## Measured values so far (desktop @1280 / mobile @375)

| Component | Page | Desktop | Mobile | Generate at |
|---|---|---|---|---|
| `.ix-ihero--svc` | all service pages | 1.33 | 1.33 | 4:3 |
| `.ix-services.ix-keyben` | all | 1.78 (locked 16/9) | — | 16:9 |
| `.ix-ftabs` | all | 0.65 | 1.15 | 4:5 |
| `.ix-srv-process__thumb` | odoo-audit | 4/3 locked | — | 4:3 |
| `.ix-iwhat` #1 | erp-consultant | 1.03 | 1.33 | 5:4 |
| `.ix-iwhat` #1 | erp-implementation | 0.79 | 1.33 | 1:1 |
| `.ix-iwhat` #1 | erp-migration | 0.79 | 1.33 | 1:1 |
| `.ix-iwhat` cost | erp-consultant / -implementation / -migration | 1.06 / 1.06 / 1.12 | 1.33 | 5:4 |
| `.ix-iwhat.ix-iwhy` | odoo-audit | 0.54 | 1.33 | 4:5 |
| `.ix-iwhat.ix-iwhy` | erp-integration | 0.34 | 1.33 | 2:3 |
| `.ix-iwhat.ix-iwhy` | erp-migration | **0.30** | 1.33 | 2:3 |

Note `.ix-iwhat` #1 is 1.03 on erp-consultant but 0.79 on the other two — **measure per page.**

## Pick the ratio with the geometric mean

With `object-fit:cover`, the fraction of the image that survives is `min(box,img)/max(box,img)`.
The ratio that loses least across two different boxes is `sqrt(desktop × mobile)`.
Example: why-slot 0.34 desktop × 1.33 mobile → sqrt = 0.67 → 2:3, leaving ~51% visible in each,
versus 31% if you had used a 4:3 landscape.

## The measuring recipe (no live-site requests, Browser pane is blocked for prelive)

1. `curl` the page to `/tmp/x.html`.
2. Rewrite for a local mirror: root-relative → absolute → `http://localhost:10003/`, strip the
   LiteSpeed base64 placeholder `src`, promote `data-src`→`src`, write into `app/public/_x_mirror.html`.
3. Open `http://localhost:10003/_x_mirror.html` in the Browser pane (`preview_start`), then inject
   the theme stylesheet — LiteSpeed's combined CSS does not survive the rewrite:
   `link.href='http://localhost:10003/wp-content/themes/indexworld-blocks/assets/css/components.css'`
4. `getBoundingClientRect()` each slot at 1280 wide, then `resize_window` preset `mobile` and repeat.
5. **Delete the `_x_mirror.html` scaffolding from the webroot when done.**

Gotchas: the Browser pane resets scroll, so isolate a section by hiding every other `body *` rather
than scrolling; and images may render blank until you clear the lazy-load class and force
`opacity:1`. See [[image-library-log]].
