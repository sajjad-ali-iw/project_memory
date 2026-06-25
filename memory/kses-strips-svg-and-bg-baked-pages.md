---
name: kses-strips-svg-and-bg-baked-pages
description: KSES on MCP-saved baked pages strips inline <svg> icons AND background-image:url() — blank icons/arrows/carousel images on prelive; theme fix + which pages it hit
metadata:
  node_type: memory
  type: project
---

**Symptom (found 2026-06-25 on `prelive.indexworld.net/wholesale-distribution/`):** carousel slide images blank, prev/next arrows blank, plus blank icons/arrows/checkmarks all over the page. Looks fine on **local** but broken on **prelive**.

**Root cause = KSES, same family as [[booking-page-and-iframe-kses]].** Baked industry/service pages store their full markup (including the theme's inline SVG icons + inline `background-image`) in `post_content`. When saved through the MCP connector (a non-`unfiltered_html` user), KSES strips what it disallows in post content:
- **Inline `<svg>`** → removed entirely → empty icon/arrow/check holders. (Audit proof: `0` inline svgs in page body vs 96 intact in the theme-rendered header/footer.)
- **`background-image:url(...)`** in inline `style` → stripped, leaving a mangled `style=")"`. (Proof: every `.ix-iccar__media` had `style=")"`.)

Local looks fine because local saves run as an admin **with** `unfiltered_html`, so KSES never strips; and the theme PATTERNS output the same markup at render time where KSES doesn't apply.

**Blast radius is the whole page, not just the carousel.** On wholesale-distribution, KSES emptied: `.ix-iccar__arrow` (6), `.ix-ichal__icon` (6), `.prod-icon` (16), `.ix-svc__arrow` (20), `.ix-ihow__check` (5) + the 6 carousel bg images + prev/next buttons. **What SURVIVES KSES:** `<img src>` images (all loaded fine — beware false "broken image" reports from `loading="lazy"` offscreen imgs; force `img.decode()` to check), `data-*` attributes (so tabs/carousel JS still works), header/footer (theme-rendered).

**FIX (theme, 2026-06-25):**
1. `functions.php` — added a `wp_kses_allowed_html` filter (context `post`) that allow-lists a SAFE subset of inline SVG (svg/path/g/defs/rect/circle/ellipse/line/polyline/polygon + presentational attrs; NO script/foreignObject/on* — KSES still drops those). This restores **all** stripped inline-SVG icons/arrows/checks site-wide on save. Trusted-editors-only tradeoff.
2. `patterns/image-card-carousel.php` + `components.css` — carousel slides switched from inline `background-image` to `<img class="ix-iccar__img">` (KSES-safe). New `.ix-iccar__img{position:absolute;inset:0;object-fit:cover}`. NOTE: inline `background-image:url()` does NOT reliably survive KSES even with filters — prefer `<img>` for any baked-content image.
3. Theme changes must be **deployed to prelive** (git/ChemiCloud) before recreating pages, or recreation re-breaks.

**Pages affected = every baked page using these components.** On 2026-06-25 the 7 baked **industry** detail pages were **trashed** (recoverable) at user request to be recreated post-deploy: Cannabis (28), Chemical (640), Furniture (364), Manufacturing (369), Mechanical (380), Self Storage (365), Wholesale Distribution (367). The **Industries** listing (#26) was kept. **Service pages were NOT touched but likely have the same KSES damage** (Print on Demand, Web Development, Odoo Implementation/Migration/Audit/Customization/Training, Hire Odoo Developers, Blockchain, BPA, Odoo Integration Services, etc.) — recheck after deploy. Related: [[icon-cards-baked-pages-fix]], [[mcp-absolute-url-protocol-gotcha]], [[industries-build]].
