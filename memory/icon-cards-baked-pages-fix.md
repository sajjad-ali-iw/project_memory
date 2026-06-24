---
name: icon-cards-baked-pages-fix
description: Fix for missing icons on baked industry pages — CSS nth-child background-image approach
metadata:
  type: project
---

**Problem (2026-06-24):** Industry pages with baked-in block markup (manufacturing, furniture, self-storage, wholesale, print-on-demand) had empty `<span class="ix-ichal__icon">` spans — no icons showing. Root cause: the `indexworld/icon-cards` pattern uses `<?php echo $ix_ic[$c[0]]; ?>` to inject inline SVGs, but baked page content is stored as raw HTML — PHP doesn't execute inside saved WP block content.

**Fix:** CSS-only, in `components.css`. Added `nth-child(1–6)` selectors targeting `.ix-ichal__icon:empty` with background-image data URIs of the 6 SVG icons (shield, sprout, box, receipt, store, chart — color hardcoded to `#50269E` since `currentColor` doesn't work in CSS background SVGs). Inline SVGs (pattern-ref pages like cannabis) still take priority as normal.

**Why:** One CSS change fixes all existing and future baked industry pages without editing every page's content.
**How to apply:** If adding more than 6 icon cards to a page, extend the nth-child selectors. If a page uses pattern refs (not baked), inline SVGs apply automatically — no CSS needed.
