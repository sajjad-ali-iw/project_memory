---
name: footer-mobile-accordion
description: "Footer link columns collapse into tap-to-open accordions on mobile (≤600px), matching staging"
metadata: 
  node_type: memory
  type: project
  originSessionId: 1538bbf8-65b1-46a1-a769-ae9644ea0655
---

Added 2026-06-23. On mobile (≤600px) the footer link columns (Our Services, Our Industries ×2,
Company) collapse into **tap-to-open accordions** with a rotating chevron + divider lines — matching
how staging's Elementor footer behaves. Desktop/tablet (>600px) is unchanged (columns expanded).

**Implementation:**
- `assets/js/footer-accordion.js` (enqueued in functions.php as `indexworld-footer-accordion`) —
  wraps each `.ix-footer .ix-fcol`'s links (the `<a>`s after the `<h4>`) in a `.ix-fcol__body`
  div at runtime, then toggles `.open` on the `.ix-fcol` when its `<h4>` is clicked (independent
  toggles; collapsed by default; keyboard-accessible).
- `components.css`: base `.ix-fcol__body{ display:flex; flex-direction:column; }` (needed because the
  `<a>`s are `display:inline` and only stacked before by being flex items of `.ix-fcol`). Inside
  `@media(max-width:600px)`: `.ix-fcol` gets a bottom divider, `<h4>` becomes a flex row with a
  CSS-border chevron (`::after`, rotates on `.open`), `.ix-fcol__body{ max-height:0; overflow:hidden }`
  → `.ix-fcol.open .ix-fcol__body{ max-height:640px }` with a transition.

**Verified live** via Claude-in-Chrome on the Mac at 400px: default collapsed, tap opens (chevron
flips, links reveal), tap closes. NOTE: during debugging, `getComputedStyle`/`getBoundingClientRect`
read via the browser eval tool gave misleading stale values (reported max-height 0 / height 12px when
the element was actually expanded) — trust a screenshot over scripted measurements for layout here.
Also: `mcp__Claude_in_Chrome__resize_window` was flaky at growing the viewport back (innerWidth stuck);
reload after resize, or have the user drag the window. Related: [[footer-build]], [[mobile-drawer-menu]].
