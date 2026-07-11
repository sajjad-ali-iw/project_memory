# IndexWorld — Component Catalog

The theme is a **component library**: pages are composed from reusable section **patterns**
(`patterns/*.php`) dropped onto the blank **`industry`** page template (`_wp_page_template=industry`).
Page content is just an ordered list of `<!-- wp:pattern {"slug":"indexworld/<name>"} /-->` refs.
All custom CSS lives in `assets/css/components.css` (namespaced `.ix-*`); per-component JS in `assets/js/`.

**House rules**
- **No eyebrow/badge in a hero section** (eyebrows are fine in non-hero section headers).
- Reuse a component's **markup + behaviour**; supply page-specific **content** (don't fork behaviour).
- **Components are multi-use.** Name + describe each by its *shape*, not one page's content (a numbered-card block = steps OR benefits OR phases). Every entry lists **"Use for"** with several scenarios.
- **Always supply real images.** Every image slot must point to a real file in `assets/images/...`. When you reuse a component on a new page, **swapping in that page's own images is part of the job** — never ship the previous page's images, a placeholder, or an empty `src`. Each entry below lists its **image slots** so none get missed; before calling a section done, confirm every slot loads.
  - **Images are served from the WordPress Media Library.** Components still reference `get_theme_file_uri('/assets/images/...')`, but a `render_block` repoint filter in the theme `functions.php` swaps each to its Media Library copy at render time (raster = WebP, vectors = SVG). When adding a new image: drop the source in `assets/images/`, then import it to the Library tagged `_iw_src=<path>` (see `CLAUDE.md` → Image handling). Replacing the Library image later updates the site automatically.
- Section eyebrows use the `ix_badge('Label')` helper.
- Buttons use the shared `.ix-btn` / `.ix-hero__btn` system (Almaden 600, radius 12).

Status legend: ✅ finalized · 🔧 needs work · 🔁 rename/merge pending

**How this file is organised:** Section A lists the reusable components, **grouped by family**
(Heroes, Tabs, Carousels, Grids, Content sections, Proof & Trust, FAQ & CTA) so similar pieces sit
together. Section B lists page-specific patterns that are intentionally *not* components.

---

## A. Reusable section components

### Heroes

#### 1. `split-hero` ✅  — **Hero + Image** (plain variant)  (class `.ix-ihero`)
- **Family:** the **Hero + Image** family = two-column hero (title + intro + 2 CTAs + image). Two variants: **plain** (`split-hero`, this one) and **with stats** (`industries-hero`, #2). Pick by whether you need a stat strip.
- **Use for:** interior-page heroes — industry pages, service pages, any "text + image" hero.
- **Layout:** two-column — left: H1 + intro + two buttons (`--primary` / `--ghost`); right: image.
- **Markup/CSS:** `.ix-ihero` → `.ix-ihero__grid` ( `__copy` + `__media` ); classes `.ix-ihero__title/__sub/__actions/__btn`.
- **Image slot:** `__media img` — **required**, each page supplies its own (Cannabis uses `assets/images/cannabis/hero.jpg`).
- **Notes:** no eyebrow (house rule). Self-contained example copy → duplicate + re-text + re-image per page.

#### 2. `industries-hero` ✅  — **Hero + Image (with stats)**  (class `.ix-indhero`, no JS)
- **Family:** the "with stats" variant of the **Hero + Image** family (see #1). Same two-column hero + image + 2 CTAs, **plus a 4-stat strip**.
- **Use for:** landing/section heroes that want key stats up front — Industries, About, Company, a service landing.
- **Image slot:** one (right column) — present (`assets/images/about/overview.png`). Supply a page-specific image.
- **Decision (2026-06-22):** kept as a separate variant rather than merged into `split-hero` (owner's call). No eyebrow ✓.
- ⚠ `/book-a-free-appointment/` CTA link to verify (link-audit pass).

#### 3. `hero` ✅  — **Homepage Hero**  (class `.ix-hero`, no image)
- **Use today:** homepage only (front-page).
- **Future use:** reusable as a bold centered *statement* hero for landing / campaign / product-launch pages — when you want a full-bleed gradient statement rather than the two-column `split-hero` (#1).
- **Layout:** dismissible announcement bar + centered headline + subtext + two CTAs, on a CSS dark gradient. No eyebrow (house rule), no image slot.
- **Reuse:** re-text headline + announcement + button labels/links per use.
- **Naming:** finalized **"Homepage Hero"** (was "Hero"). Distinct from `split-hero` (#1, two-column interior hero with image).
- ⚠ link to fix: first CTA → `/book-a-free-appointment/` (verify in link-audit pass).

### Tabbed sections

#### 4. `feature-tabs` ✅  — **Feature Tabs**  (class `.ix-ftabs`, JS `assets/js/feature-tabs.js`)
- **Use for:** anywhere with multiple grouped feature sets — **any number of tabs** (4, 5, 6+). Each tab = image + checklist of points.
- **Layout:** vertical tabs (desktop, active = dark-navy pill) / dots + swipe (mobile) — mobile follows the house centered-peek recipe (full-bleed, 86vw slide + 14px gap, 7vw inset; JS steps in px). Scales as tabs are added.
- **Image slots:** one image per tab.
- **Distinct from `Service Tabs` (#5):** this one is *vertical* side-tabs + *icon-checklist* items.
- **Naming:** finalized **"Feature Tabs"** (dropped the "(slider)").

#### 5. `services` + `key-benefits` ✅  — **Service Tabs**  (class `.ix-services` / `.ix-svc`, JS `assets/js/services-tabs.js`)
- **One component, two ready presets.** `key-benefits` is `services` re-textured with a tiny `.ix-keyben` spacing modifier — same markup, same JS.
- **Use for (multi-use):** service suites · per-industry **key benefits / capabilities** · solution categories · product modules · "what we do" groups — anything that splits into 4–6 tabbed groups, each a list of titled, link-able points.
- **Layout:** horizontal tabs across the top (active = lavender pill) → each panel = a list of "**Title:** description ↗" rows (left) + an image (right) that swaps per tab.
- **Presets:** `services` ("Services Suite" — sparkle + "Our Services" eyebrow, ODOO/Accounting/Data Analytics/ERP AI); `key-benefits` ("Key Benefits" — eyebrow + subhead, industry capability tabs). Drop the closer preset, then re-text + re-image.
- **Image slots:** **one image per tab** (4–6 images) + (services preset only) a sparkle icon. **Every tab needs its own real image** — this is the component most likely to ship with a stale or missing tab image, so verify each one loads.
- **Distinct from `Feature Tabs` (#4):** that one has *vertical* side-tabs + *icon-checklist* items; this one is *horizontal top tabs* + *arrow-linked text rows*. Keep both.

### Carousels  *(all follow the house carousel recipe: dots `#D6D1E8` / active `#060038` pill, ~54px `#ECE6FF` round arrows, centered-peek mobile)*

#### 6. `image-card-carousel` ✅  — **Image Card Carousel**  (class `.ix-iccar`, JS `assets/js/image-card-carousel.js`)
- **Use for (multi-use):** use-cases / "proven results" showcase · service categories · industry highlights · portfolio / case-study teasers · product or feature galleries — any swipeable row of labeled image cards.
- **Layout:** square image cards (photo + label) in a row; prev/next round arrows + dot pagination (active = dark pill).
- **Hover (2026-07-11):** the per-card round ↗ arrow was removed (owner request); hovering a card 3D-flips it to a white face showing the card's text. The back-face text comes from the media anchor's `aria-label` (CSS `content:attr()`), so it works on baked pages too — **every `.ix-iccar__media` must carry an `aria-label`**. Keep `.ix-iccar__media` `overflow:visible` (overflow:hidden flattens the 3D and kills the flip); corner radius lives on the faces.
- **Image slots:** one square photo per card (each page supplies its own set).
- **Naming:** finalized **"Image Card Carousel"**.

#### 7. `blog-carousel` ✅  — **Blog Carousel**  (JS `assets/js/blog-carousel.js`)
- **Use for:** "latest from the blog" on any page or post (homepage uses it; `blog.php` is a thin wrapper around it).
- **Layout:** swipeable carousel of the latest 9 posts using the `/blog/` card design — arrows + dots + autoplay.
- **Dynamic / image source:** runs a `WP_Query` automatically; **card images = each post's featured image** → every published post must have a featured image set or its card shows blank.
- **Naming:** **"Blog Carousel"** (already shape-based — kept).

#### 8. `case-studies` ✅  — **Case Studies Carousel**  (class `.ix-cases`, JS `assets/js/case-studies.js`) — *less-used*
- **Use today:** homepage only. **Future:** a "featured results" carousel on landing / service pages.
- **Layout:** white intro card + row of client-result image cards, dots + arrows.
- **Image slots:** the client result-card images (`$ix_cases` keys, each mapped to a case-study slug). Swap per use.
- **Naming:** **"Case Studies Carousel"**. Don't confuse with `case-studies-archive` (the full /case-studies/ listing page).

### Card grids

#### 9. `icon-cards` ✅  — **Icon Card Grid**  (class `.ix-ichal`)
- **Use for (multi-use):** challenges / features / benefits / "why us" grid — flat cards (purple line icon + title + text), 3-col.
- **Header:** eyebrow + heading always; **optional subtext** via the `ix-ichal--withsub` modifier (adds `.ix-ichal__sub` under the heading).
- **Image slots:** none (line icons are inline SVG).
- **Distinct from `Industries Grid` (#11):** that one is photo cards that link out; this one is flat icon cards.
- **MERGED IN (2026-06-22):** the old `industries-why` (boxed-card "Why" section) was folded into this component — it's now a content instance rendered in the Icon Card Grid style (see Section B). The boxed `.ix-indwhy` CSS was deleted.
- **Naming:** display name finalized = **"Icon Card Grid"** (slug stays `icon-cards`; class `.ix-ichal`).

#### 10. `numbered-cards` ✅  — **Numbered Cards**  (class `.ix-ibenefits`)
- **Use for (multi-use):** process / "how it works" **steps** · **key benefits** / reasons to choose us · implementation **phases / roadmap** · **what's included** in a package · ranked features / highlights. Any ordered set of 3–4 points.
- **Layout:** dark band, big index number (01/02/03) + title + text per card.
- **Image slots:** none.
- **Naming:** finalized **"Numbered Cards"** (was mislabeled "Key Benefits"). Distinct from `Process Steps` (#13, timeline/checklist look) and `Service Tabs` (#5, tabbed).

#### 11. `industries-grid` ✅  — **Industries Grid**  (class `.ix-indgrid` / `.ix-indcard`, no JS)
- **Use for (multi-use):** an *image-link card grid* — Index World use = industry cards → single-industry pages; also reusable for service categories, solution areas, locations, any set of photo cards that link out.
- **Layout:** responsive grid of cards (photo + bottom name bar + arrow). Distinct from `Icon Card Grid` (#9 = flat icon cards, no photo).
- **Image slots:** one photo per card (`$ix_industries` → `assets/images/staging/`) — all 11 present.
- **Reuse:** Industries page + Odoo Implementation.
- ⚠ **links:** cards target `/cannabis-industry/` … `/wholesale-distribution/` but **only `/cannabis-industry/` exists** — 10 target pages still to build (link-audit pass).
- **Naming:** **"Industries Grid"** (kept).

### Content sections

#### 12. `media-text` ✅  — **Media + Text** (two column)  (class `.ix-iwhat`)
- **Use for:** "What is [X]?" / about-this-solution explainer on industry & solution pages.
- **Layout:** image left, copy right (intro + a bold "Why it matters" sub-block).
- **Image slot:** one (left column) — supply a page-specific image.
- **Naming note:** slug `media-text` ↔ class `.ix-iwhat` (mismatch).
- **TODO:** fix mojibake em-dash in the Cannabis copy (`finance â€ so` → `finance — so`).

#### 13. `steps-timeline` ✅  — **Process Steps**  (class `.ix-ihow`)
- **Use for (multi-use):** "how it works" / your-path-to-go-live · onboarding or implementation **steps** · a service **process** · a project **roadmap** · what-to-expect timelines. Two-column: heading + intro + numbered checklist of steps on one side, supporting image on the other.
- **Layout:** desktop = text column + image column; ≤900px collapses to one column (text → image below). Heading colour = **black** (`#0b0b0b`).
- **Image slot:** one supporting image (right column). **Required** — supply a real, page-specific image.
- **Naming:** finalized **"Process Steps"** (was "Steps Timeline"). Distinct from `Numbered Cards` (#10, dark band of big-number cards) and `Service Tabs` (#5, tabbed).

### Proof & trust

#### 14. `trusted` ✅  — **Tools & Frameworks**  (section `.ix-trusted` + marquee `.zt-trust`, no JS — CSS animation)
- **Index World use:** the **tools & frameworks we work on** — Odoo, QuickBooks, Claude, Power BI, Gantt, etc. (not generic "partners").
- **Use for (multi-use):** tech-stack / platforms-we-build-on · client "trusted by" logos · integration logos · certifications — any auto-scrolling logo row.
- **Layout:** centered heading + subtext + marquee; the `$ix_logos` array is duplicated 2× so the scroll loops seamlessly.
- **Image slots:** the **logo set** (`$ix_logos` → `assets/images/logos/`). Every entry must be a real logo; swap the array per use. ⚠ filenames are obscure staging exports — confirm each logo is the intended tool.
- **Reuse:** on homepage, Odoo Implementation, landing-starter.
- **Naming:** finalized **"Tools & Frameworks"** (on-page heading + subtext updated to match; was "Built on Trusted Platforms").
- **Class note:** marquee uses a leftover `.zt-` prefix instead of `.ix-` — works; optional cosmetic rename later.

#### 15. `testimonial` ✅  — **Social Proof**  (class `.ix-proof`, no JS)
- **Use for:** a credibility / social-proof strip on homepage, landing, or service pages — third-party review ratings.
- **Layout:** a row of 3 rating cards (Clutch / GoodFirms / Google) — score + 5-star row + review count + platform logo.
- **Image slots:** the **review-platform logos** (`assets/images/reviews/`, SVG). Swap if you change platforms.
- **Reuse:** homepage + landing-starter.
- **CHANGED 2026-06-22:** the founder/CEO testimonial quote was removed site-wide (deemed a weak section). Only the review ratings remain. `.ix-quote*` CSS is now dead code (safe to strip later).
- **Naming:** finalized **"Social Proof"** (was "Reviews & Testimonial").

### FAQ & CTA

#### 16. `faq` ✅  — **FAQ Accordion**  (class `.ix-ifaq`, JS `assets/js/faq-accordion.js`)
- **Use for:** an FAQ section on any page (industry, service, landing, support). Also droppable *inside* another pattern — the JS auto-inits any `.ix-ifaq__list` on the page (Support page nests it this way).
- **Layout:** centered eyebrow + heading + accordion (question left, +/× icon right, no dividers). Smooth single-open (opening one closes the rest).
- **Image slots:** none.
- **Reuse:** one shared component — re-text the Q&A array per page. Already on Odoo Implementation, Support, both starters. See memory `faq-accordion-smooth.md` for the `<details>` transition gotcha.
- **Naming:** **"FAQ Accordion"** (already shape-based — kept).

#### 17. `cta` ✅  — **Closing CTA**  (class `.ix-ctap`, no JS)
- **Use for:** the closing call-to-action band at the bottom of almost any page (industry, service, landing, blog). Most-reused component on the site.
- **Layout:** centered navy two-line heading + two buttons (`.ix-btn--primary` / `.ix-btn--light`).
- **Image slots:** none.
- **Reuse:** one shared component, no presets — re-text the heading + button labels/links per page. Already on homepage, Support, Odoo Implementation, and baked into both starters.
- **Naming:** finalized **"Closing CTA"** (was "CTA Build Systems", named after example copy).

---

## B. Page-specific patterns — NOT reusable components (hidden from inserter)

These render a specific page and are **not** meant to be dropped elsewhere, so they carry an
`Inserter: no` header: they stay **registered** (so the page's `wp:pattern` slug ref still resolves)
but **don't appear** in the block inserter / Index World component list.

- **`industries-why`** ("Industries — Why") — **merged into Icon Card Grid (#9)** 2026-06-22 (owner: "merge"). Now just the Industries page's icon-card content instance (eyebrow + heading + subtext + 3 cards) rendered in the `.ix-ichal` style; hidden from inserter. Page-specific to **/industries/ (page id 26)**, verified still rendering. The Why cards changed from boxed → flat Icon-Card-Grid style as a result.
- **`blog-list`** ("Blog List (post grid)") — page-specific to **/blog/ (page id 351)**. Server-side filtered + paginated grid of blog post cards (purple banner design). Hidden from inserter 2026-06-22 (per owner: "should not be a component, but not removed from /blog/"). `/blog/` verified still rendering after the change. Image source = each post's featured image.

---

## C. Image-slot aspect ratios + sizes (MCP contract)

Every reusable component's image slot has a **fixed aspect-ratio + `object-fit:cover`** in
`components.css`, so an image of *any* pixel size renders in the same box (it center-crops to fill —
it never stretches the layout). **When generating a page via MCP, supply each slot an image already
cropped to roughly the ratio AND at the "Fetch source" size below.** Wrong-ratio images won't break
layout — they'll just crop harder; under-sized images look soft on retina; over-sized images waste
bytes (and risk the WebP white-render bug).

### Size ceiling (applies to every in-page image)

- **Hard max: 2000px on the longest edge.** Above this, GD's WebP encoder can render the image white
  in-page (documented bug) and files bloat. Never fetch/keep an in-page image larger than this.
- **Practical max: ~1600px wide.** Every slot displays at ≤600px, so 2× retina is ≤1200px — bigger
  is wasted.
- **Floor: the "Fetch source" width below.** Smaller than that = soft/blurry on retina.
- **Format:** fetch **JPG** (PNG if it needs transparency); the import pipeline converts to WebP q90.
  Do NOT pre-supply large WebP for in-page images.

### Per-slot ratio + ideal source size

"Displays at" = on-screen width (desktop) computed from the grid; "Fetch source" = 2× that, at the ratio.

| Component | Slot class | Ratio | Displays at | **Fetch source** |
|---|---|---|---|---|
| Split Hero | `.ix-ihero__media` | **3:2** | ~543px | **1100 × 730** |
| Media + Text | `.ix-iwhat__media` | **flex height** (image stretches to text-column height; no fixed ratio) | ~600px wide | **tall/portrait, ~1100 × 1500** (long images OK, ≤2000px tall) |
| Industries Hero | `.ix-indhero__media` | **3:2** | ~543px | **1100 × 730** |
| Steps / How-it-works | `.ix-ihow__media` | **680:480 (≈17:12)** | ~560px | **1100 × 780** |
| Services tabs | `.ix-svc__media` | **16:9** | ~552px | **1200 × 675** |
| Image-Card Carousel | `.ix-iccar__media` | **1:1** (bg image) | ~320px | **700 × 700** |
| Industry Card grid | `.ix-indcard__img` | **16:11** | ~380px | **780 × 540** |
| Case-Studies Archive card | `.ix-csa__media` | **433:400 (≈1:1)** | ~381px | **780 × 720** |
| Industries tabs main image | `.ix-industries .image-card img` | fixed **380px h** | ~600px | **1000 × 760** |
| Blog card (featured) | `.ix-blog__card …featured-image` | fixed **200px h** | ~380px | **760 × 400** |
| Mega-menu preview | `.ix-preview img` | fixed **180px h** | ~300px | **600 × 360** |
| Contact location photo | `.ix-loc__photo` | **16:10** | ~400px | **800 × 500** |
| About — overview | `.ix-aco__media` | **3:2** | ~560px | **1100 × 730** |
| About — closing CTA | `.ix-acta__media` | **3:2** | ~560px | **1100 × 730** |
| About — people photos | `.ix-about__photos` | **2:3** (portrait) | ~200px | **440 × 660** |
| Support hero | `.ix-sup-hero__media` | **3:2** | ~560px | **1100 × 730** |
| Support feature | `.ix-sup-feat__media` | **16:9** | ~560px | **1200 × 675** |

**⚠️ Image URLs must be root-relative.** When MCP generates a page, write every image URL as
`/wp-content/uploads/...` (or `/wp-content/themes/...`) — **never** absolute `https://indexworld-revamp.local/...`.
The site runs on **http** (Local's https cert isn't trusted), so baked `https://` URLs fail to load
and the image renders **blank** — which reads as a band of "extra space" (this is what broke the
mechanical-industry `.ix-iccar` carousel, fixed 2026-06-23). Root-relative inherits the page protocol.

**Logos** (`.zt-logo`, `.ix-fpartners`, `.ix-rating__logo`, `.ix-svc__spark`) and **inline blog-body
images** (`.ix-post__body img`) are intentionally **NOT** ratio-locked — they size to themselves.
Supply logos small (≈200–400px wide), transparent PNG or SVG.

**Homepage hero cards** (`.ix-hero__card`) are JS-built and homepage-only — not an MCP slot.

**Exception — Media + Text (`.ix-iwhat__media`):** on desktop the image is NOT ratio-locked; it
stretches to match the height of the (often long) text column and cover-crops to fill, with a
`min-height:360px` floor. So this is the one slot where a **tall/long image is encouraged** — supply
a portrait image (~1100×1500) and it fills the column nicely. On mobile (stacked) it falls back to a
fixed **4:3**. Section height is still driven by the copy, never by the image.

> **Known too-small/wrong-ratio image:** `/mechanical-industry/` hero `mech-hero.jpg` is 640×612
> (near-square, under-sized) in a 3:2 slot — replace with a 1100×730 (3:2) JPG when convenient.
