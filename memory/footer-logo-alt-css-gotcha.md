---
name: footer-logo-alt-css-gotcha
description: Footer partner logos are sized by CSS attribute selectors on their alt text; changing the alt breaks the footer
metadata:
  type: project
---

`assets/css/components.css` (~L727-730, L795) sizes the footer "Our Trusted Partners" logos with
`.ix-fpartners img[alt="Claude"|"Odoo Partner"|"Sage"|"QuickBooks"]`. On 2026-10-02 an SEO alt-text pass
changed those alts and the footer broke; reverted in 76e7d7b.

**Why:** the alt text doubles as a styling hook.
**How to apply:** before changing any alt text, grep the CSS for `[alt=`. To improve those footer alts, first move
the sizing to classes (e.g. `.ix-fpartners__claude`), then change the alt.
