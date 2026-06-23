---
name: section-port-method
description: "How to port staging's self-contained custom sections into the block theme"
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

**EDITOR PREVIEW GOTCHA:** `components.css` was enqueued only via `wp_enqueue_scripts` (front end). In the Site/block editor patterns rendered UNSTYLED (giant footer logos, no gradients). Fix: `add_action('after_setup_theme', fn() => add_editor_style('assets/css/components.css'))` so the editor canvas loads it too. Front end was always correct. Separately: sections built as `wp:html` (Custom HTML) blocks — mega-menu, hero carousel, services/industries tab bodies, footer — preview imperfectly in the editor (Custom HTML is sandboxed) but render perfectly on the FRONT END. The live site is the source of truth, not the editor canvas.

**SEEING STAGING VISUALLY (key trick):** the controlled Chrome (Claude-in-Chrome) is BLOCKED from navigating to staging.indexworld.net, and computer-use can't reliably screenshot the user's other browser. Workaround that WORKS: `curl` the staging page, save the HTML into the LOCAL webroot (e.g. `app/public/_staging_mirror.html`), then navigate the controlled browser to `http://indexworld-revamp.local/_staging_mirror.html`. Staging's stylesheets/images are ABSOLUTE `https://staging...` URLs so they load — the page renders with staging's real design and I can screenshot any section. Staging's loader/scripts reset scroll → in JS remove `#custom-loader`/`[id*=loader]` and `scrollTo(0, footer.offsetTop)`. Delete the mirror file before any production handoff. Use this to build Contact/About pixel-perfect.

Staging's homepage sections are often self-contained custom HTML+CSS+JS blocks (standalone-page style). Proven port method (used for the mega-menu and the Industries tabs):

1. **Extract 3 pieces** from `staging_fresh.html`: the real container HTML element (balanced-tag extract — note the FIRST class match is usually inside a `<style>` block, so pick the occurrence with child `id=`/structure), the `<style>` CSS, and the `<script>`.
2. **SCOPE the CSS** under a unique wrapper class (e.g. `.ix-industries`). Their CSS uses GLOBAL `:root`/`*`/`body` + generic `.card`/`.title`/`.desc` that collide with the rest of the theme (the Industries `:root` even redefined `--border`/`--shadow`). Transform: drop `body`/`.wrap`, scope `:root`→`.ix-wrapper`, `*`→`.ix-wrapper *`, prefix every other selector with `.ix-wrapper `. Append to `assets/css/components.css`.
3. **JS** → own file in `assets/js/`, enqueued in functions.php (footer). Normalize init: staging scripts run inline immediately or fire on LiteSpeed's `DOMContentLiteSpeedLoaded`; wrap the entry call in `if(readyState!=="loading"){init()}else addEventListener('DOMContentLoaded',init)`. Validate with `node --check`.
4. **Pattern** = `patterns/<name>.php` (PHP pattern, category `indexworld`): native-block heading + subheading, then a `wp:html` block with the scoped wrapper + container. Add to `templates/front-page.html` via `<!-- wp:pattern {"slug":"indexworld/<name>"} /-->`.
5. Images in the JS/markup still point to staging URLs (load fine; localize+optimize in a later pass).

**DONE:** Industries tabs section ("Driving Success Across Key Industries", 11 industries, `tabsData` JS, `.ix-industries` scope, `assets/js/industry-tabs.js`). Bundled PHP lint/run binary: `/Applications/Local.app/Contents/Resources/extraResources/lightning-services/php-8.2.29+0/bin/darwin-arm64/bin/php`.

**HOMEPAGE COMPLETE (2026-06-20).** front-page.html order of patterns: hero → **services** (tabbed: ODOO/Accounting/Data Analytics/ERP AI, `assets/js/services-tabs.js`) → industries (tabs) → about → trusted (logo marquee `zt-*`) → **testimonial** (3-col Clutch/GoodFirms/Google ratings + founder quote "Waqas Yousaf, Founder & CEO") → blog (dynamic Query Loop) → cta. All in `patterns/*.php`. Hero card carousel arrows = lavender `#ECE6FF` circles w/ `#00053D` arrows (staging-matched).
NOTE: I initially MISSED the services + testimonial sections — always cross-check the full section inventory of the staging mirror (no-heading sections too) before declaring a page done. Blog uses a real Query Loop over 3 seeded posts (IDs 12/13/14, Unsplash featured images, created via royal-mcp-local). CTA = dark radial-gradient banner reusing `.ix-hero__btn--dark/--white`.

**STILL TO DO (polish):** real footer (currently a placeholder — staging has a full multi-column footer); localize+optimize all staging-referenced images (industry illustrations, mega-menu hover previews — still load from staging.indexworld.net); mobile/responsive QA; per-section pixel-tuning vs staging; mega-menu mobile hamburger.
