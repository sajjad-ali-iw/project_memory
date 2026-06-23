---
name: mobile-responsiveness
description: State of mobile/responsive QA and the tooling limit for viewing mobile
metadata: 
  node_type: memory
  type: project
  originSessionId: 2aa1cd17-8308-49ff-bc9f-c09f08dc803c
---

Mobile responsiveness QA pass (2026-06-21).

**TOOLING LIMIT (important):** the Claude-in-Chrome browser is pinned to a ~1440px desktop viewport — `resize_window` resizes the OS window but the page still renders/screenshots at desktop width (full nav stays inline). So I **cannot take real mobile screenshots**. The Preview MCP needs a dev server from launch.json and can't point at the Local WP host. → Mobile visual verification must be done by the USER (real phone or Chrome DevTools device mode). Don't claim mobile visual QA was done from desktop.

**DESKTOP visual verify DOES work — over HTTP (2026-06-23).** The controlled Chrome can screenshot local pages if you navigate to **`http://indexworld-revamp.local/...`** (the site's canonical scheme; `siteurl` is http). Navigating to **`https://`** hits a "Privacy error" cert interstitial that CANNOT be scripted/bypassed (`Cannot attach to this target`) — so always use the **http://** URL for Claude-in-Chrome screenshots. This unblocks self-verifying desktop layout/CSS changes (used to confirm the single-post sidebar redesign).

**Foundation is already solid (audited in code):**
- Header collapses to a **hamburger** below ~900px: `.ix-burger` (parts/header.html) + `assets/js/mobile-menu.js` (toggles `.nav-open`, locks body scroll, closes on link/Esc).
- All fixed-column grids collapse to 1 col on mobile: footer (@1100), contact, about-cta, feature-tabs, industry-tabs, case-study hero+body (@900), single-post sidebar.
- Breakpoints in use: 1100 / 1024 / 980 / 900 (main) / 767 / 700 / 680 / 640 / 600 / 560 / 480.

**Added a MOBILE POLISH block at the very end of components.css (2026-06-21):**
- Safety nets: `html,body{ overflow-x:clip; max-width:100% }` (clip preserves position:sticky) + `img,svg,video,iframe{ max-width:100% }`.
- `@media (max-width:480px)` small-phone tier: trims hero H1 (1.85rem), `.ix-sec-title` (1.7rem), `.ix-csd__title`/prose h2/cta, `.ix-csa__title`, CTA/contact/bloghero titles, and tightens section vertical padding.
- Verified: braces balanced (847/847), css 200, no DESKTOP regression.

**Homepage hero (mobile) — match staging:** staging keeps the SAME desktop radial gradient on mobile (white bloom at bottom-center, rich #50269e ring, navy top) + a tall hero (min-height ~880) so buttons sit on rich purple and the bottom bloom is where the Case Studies cards overlap up. Do NOT swap to a flat/linear gradient (washes the purple out). `.ix-cases` mobile: `margin-top:-360px` to rise into the bloom.

**STANDARD MOBILE CAROUSEL PATTERN (use for ALL carousels):** centered active card with a peek of prev/next on BOTH sides + finger swipe + dots; no arrows on phone.
- CSS recipe: carousel must be **full-bleed** (no side padding on the section, or it offsets centering — `.ix-iccar` needed `padding:44px 0` + header/nav get their own inset). Card `width/flex-basis:80vw; max-width:none`. Track `padding-left/right:(100vw-cardW)/2` (e.g. 80vw card → 10vw each side; 78vw → 11vw). The existing `pos=i*step` snap then lands each card centered (math works out: max()==(n-1)*step). 
- **GOTCHA:** a desktop rule's `left:` (e.g. `.ix-cases__intro{ left:max(24px,…) }`) leaks onto a mobile `position:relative` element and shifts it — reset `left/right/top:auto` on mobile.
- Swipe JS (shared pattern in case-studies.js + image-card-carousel.js): direction-locked, NON-passive touchmove + preventDefault once horizontal (stops page fighting the gesture), rubber-band at ends, snaps ≥1 card per swipe, click suppressed after drag. Case-studies also autoplays on mobile (matchMedia ≤900, 3.5s, pause on touch) and starts on the 2nd card. Dots: build in EVERY `.ix-cases__dots` (querySelectorAll) so the in-card mobile dots + desktop nav dots stay in sync; mobile dots are `pointer-events:none` indicators.
- Done for: `.ix-cases` (home) + `.ix-iccar` (industry pages). "Explore" CTA renamed → "View all"; MWHQ moved last in the homepage carousel.

**STILL TO SPOT-CHECK on a real phone (375–430px):** case-study detail card overlap, feature-tabs slider, footer columns, contact form card, mega-menu dropdowns inside the hamburger panel. Related: [[hero-and-frontpage]], [[section-port-method]].
