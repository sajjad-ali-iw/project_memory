---
name: hero-and-frontpage
description: "State of the homepage hero + front page, and the dev-mode config"
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

**Homepage hero** rebuilt as a block pattern (no Elementor), now matching staging's layout:
- `patterns/hero.php` — PHP pattern (slug `indexworld/hero`, category `indexworld`). **Centered** layout: H1 "We implement AI-powered ERP…", centered subtext, then two buttons — "Let's Talk business" (dark navy `.ix-hero__btn--dark`) + "Odoo Partner" (white `.ix-hero__btn--white`). Below that, a FULL-WIDTH `wp:html` carousel.
- Cards: 9 client webp cards in `assets/images/hero-cards/` (~555KB), via `get_theme_file_uri()`. Pure-CSS marquee (`@keyframes ix-marquee`, two-pass track, pauses on hover). No JS.
- Hero bg = exact staging radial gradient (navy `#00031d` → `#50269e` → white glow at 50% 100%). Hero CSS near the end of `assets/css/components.css`.
- **GRADIENT WHITE-WASHOUT GOTCHA (fixed 2026-06):** the radial-gradient is sized in % of the container, so a SHORT hero pulls the bottom white glow up into the subtitle (washes it out). Staging's hero banner is **1171px tall**; ours was 642px → white rode up. Fix was structural, NOT a color change: `.ix-hero` padding-bottom `440px` (tall, content sits on purple) + `.ix-cases` `margin-top:-360px; background:transparent; z-index:2` so the Case Studies cards overlap up into the hero's lower white zone — exactly how staging builds it (its Case Studies block starts at 781 inside the 32–1203 banner, ~420px overlap). Mobile (≤900px): no overlap, `.ix-cases` reverts to white bg / margin-top:0. Don't "fix" the white by editing gradient stops — match staging's height instead. See [[case-studies-component]].
- `templates/front-page.html` = header part + `wp:pattern indexworld/hero` + footer.
- **Home-variant header**: `.home .ix-site-header` is transparent + absolute over the hero; dark logo flipped white via `filter:brightness(0) invert(1)`; nav text white. Done in CSS (scoped to `.home`), no markup change.

**CONFIG:** `define('WP_DEVELOPMENT_MODE','theme')` added to `wp-config.php` so new pattern/template files are picked up immediately (WP caches the `/patterns` listing by theme version otherwise). Turn off / bump version for production.

**VERIFIED against real staging screenshot (2026-06-20):** hero is centered with full-width card carousel below; header transparent white over hero. Match is close.

**DONE in this pass:** removed dead `#desktop_header_menu` CSS + the unused duplicate `.ix-main-item2` copy from components.css (30KB→12KB). Header restructured into `.ix-nav__left` (Services/Industries/Pricing) + `.ix-nav__right` (Blog/About/Contact Us/Get free Demo); added `.ix-btn-outline` "Contact Us" (white-filled on home via `.home` override). Added full-bleed `.ix-annc` announcement bar to the hero pattern (text + gradient "Explore" pill + inline-onclick dismiss ✕).

**GOTCHA — body classes:** WordPress puts `home`, `admin-bar`, `logged-in` etc. all on the SAME `<body>`. To target combinations use a COMPOUND selector (no space): `.admin-bar.home .ix-site-header`, NOT `.admin-bar .home ...` (the space means descendant and never matches). The transparent home header is `position:absolute; top:0`; on `.admin-bar.home` it's offset `top:32px` (46px ≤782px) so the admin bar doesn't clip it. Real visitors have no admin bar → top:0 is correct.

**OPEN FOLLOW-UPS:**
1. Announcement text: staging literally reads "HERO : Unlock Unlimited Odoo support." — I used the clean "Unlock Unlimited Odoo support." (the "HERO :" is an obvious CMS-label leak). Restore the prefix only if the user insists on exact replication.
2. Sections BELOW the hero not built yet (services/Odoo modules, stats, testimonials, CTA) — page jumps from carousel to footer.
3. Logo looks slightly clipped at very top ONLY when logged in (admin bar overlap) — not a real-visitor issue.
4. Mobile/hamburger nav + hero card-image optimization still pending.

See [[logo-variants]], [[header-megamenu-port]].

**DESIGN SOURCE = staging.indexworld.net.** Staging is the design reference for the homepage (hero structure, announcement bar, dark+white button pair, full-width card row). Heading font "Happy Face Semi Bold" == our "index happy font". Note the user flagged staging's title font size as "not accurate" in places — confirm type sizes with the user where staging looks off rather than copying blindly.

**HERO TYPE (current, staging values for the title):** H1 `font-size:2.9rem; line-height:60px; weight:700` (staging exact; mobile 2.125rem). Subtext `1.125rem; weight:400; letter-spacing:-0.035rem` AlmadenSans. "Let's Talk business" dark btn bg `#00053D`; "Odoo Partner" white.

**NAV BUTTONS (staging):** both 14px/600, padding 12px 22px, radius 10px. Contact Us = `.contact_us_nav_button` → bg `#ECE6FF` (light lavender) + black text (NOT plain white; IndexWorld secondary palette: primary `#4C2E86`, secondary `#905491`, CTA purple `#50269E`). Get free Demo = `#50269E` purple/white.

**HERO CAROUSEL (staging `slides_to_show:5`):** rebuilt from CSS marquee → STATIC cards navigated by prev/next **arrow buttons**. Markup: `.ix-hero__carousel > .ix-hero__arrow--prev + .ix-hero__viewport>.ix-hero__track>.ix-hero__card + .ix-hero__arrow--next`. Logic in `assets/js/hero-carousel.js` (enqueued in functions.php; advances 2 cards/click, disables arrows at ends). Card width 260px (~5 across), single set (no duplicate pass).

**LOCAL MCP — CONNECTED:** `mcp__royal-mcp-local__*` tools are live (local WP). `royal-mcp` (no suffix) = staging, READ-ONLY. Use royal-mcp-local to drive local WP directly (set front page via wp_update_option show_on_front/page_on_front, media, menus, options). Never write via plain royal-mcp (staging).
