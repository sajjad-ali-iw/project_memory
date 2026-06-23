---
name: mobile-drawer-menu
description: "Mobile header replaced with a slide-in drawer (.ixm-*) ported from staging's Elementor popup"
metadata: 
  node_type: memory
  type: project
  originSessionId: 1538bbf8-65b1-46a1-a769-ae9644ea0655
---

The mobile header menu was upgraded on 2026-06-23 from the old plain top-dropdown
(which just stacked the desktop mega-menu vertically) to a **slide-in drawer from the right**.

**Where it lives:**
- Markup: `parts/header.html` — drawer added as `.ixm-backdrop` + `.ixm-overlay` inside the
  existing `wp:html` block (sibling of `.ix-nav`). Three sliding panels in `.ixm-track`:
  Main → Services → Industries (`data-panel="1"/"2"`, slid via `translateX`).
- CSS: `assets/css/components.css` — all `.ixm-*` styles appended at the end. Drawer starts
  `transform:translateX(100%); visibility:hidden`, slides in on `.open`. Old mobile dropdown
  killed with `@media(max-width:1024px){ .ix-site-header .ix-nav{ display:none !important } }`.
- JS: `assets/js/mobile-menu.js` — fully rewritten. The existing `.ix-burger` toggles `.open`
  on overlay+backdrop, handles panel sliding, Services accordions (single-open), back buttons,
  close (X / backdrop / Esc / link-tap), and body scroll-lock.

**Origin & adaptations:** source was a self-contained Elementor-popup HTML/CSS/JS the owner
built on staging. Adaptations when porting into the block theme:
- Search box dropped (no search results page wired locally).
- Elementor `closePopup()` logic replaced with our hamburger open/close.
- All `https://staging.indexworld.net/...` links → root-relative local URLs; Pricing → `/support/`.
- Logo → local `assets/images/Indexworld_logo.png` (dark, no filter — repoint filter serves the
  Library webp).
- Footer buttons → "Get free Demo!" (`/book-a-free-appointment/`) + "Contact Us" (`/contact-us/`),
  matching desktop header; staging "Sign In" dropped (no sign-in page).
- Added `.admin-bar .ixm-overlay` top offset (32px / 46px ≤782) so the WP admin bar doesn't
  cover the drawer's top row.

**GOTCHA (fixed 2026-06-23):** the drawer markup must be a SIBLING of `.ix-nav` (direct child of
`.ix-bar`), NOT inside it. First port accidentally nested it inside `.ix-nav`; the
`@media(max-width:1024px){ .ix-nav{ display:none } }` rule then hid the whole drawer on mobile (a
`display:none` ancestor hides even `position:fixed` descendants → drawer computed 0×0 / transform
`none`). Worked on desktop, dead on mobile. Symptom of this class of bug: fixed overlay opens
(`.open` class toggles) but has zero size. Verify the overlay's `parentElement` is `.ix-bar`.

**Verified live 2026-06-23** via Claude-in-Chrome on the Mac's Chrome at 400px: drawer opens full
viewport, Services/Industries panels slide, accordions + footer links render. (Note: the WP admin
bar can flag a `.admin-bar` offset; tested logged-out.) Related: [[header-megamenu-port]], [[header-active-nav]].
