---
name: header-active-nav
description: "Header nav single lavender \"active\" indicator — defaults to Contact Us, moves to the current page's menu link"
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

Header nav has a single lavender "active page" indicator (added 2026-06).

**Behavior (per owner):** by default **Contact Us** is the lavender pill; when you're on a page that matches a menu link (Blog `/blog/`, About Us `/about-us/`, Pricing `/support/`, Contact `/contact-us/`), the lavender **moves** to that link and Contact Us drops to an outline. Never two lavender pills at once. Home / pages not in the nav (e.g. industry pages) → Contact Us stays lavender (default).

**How:** `assets/js/nav-active.js` (enqueued in functions.php as `indexworld-nav-active`) — the header is a static `.html` template part (no PHP), so JS reads `location.pathname`, normalizes (strip trailing slash/query), and adds `is-current` to the header `<a>` whose href path matches. If a `.ix-plain` menu link matched (not Contact, not the current page), it adds `is-dim` to `.ix-btn-outline` (Contact Us).
- CSS: `.ix-nav .ix-plain.is-current` = lavender pill (#ECE6FF + purple text, 9px/16px pad, radius 10). `.ix-btn-outline.is-dim` = transparent + `inset 0 0 0 1px #d9ccff` border + dark text.
- Mega-menu parents (Services/Industries) use `javascript:void(0)` hrefs so they're never matched — only direct-link menu items participate.

Related: [[header-megamenu-port]], [[button-system]].
