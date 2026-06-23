---
name: blog-clone-from-staging
description: "All 30 staging blog posts cloned to local on 2026-06-21; featured + inline images fully localized; only internal href links still point to staging"
metadata: 
  node_type: memory
  type: project
  originSessionId: 1fbae924-d65f-4e27-baae-f096fb774b3d
---

On 2026-06-21 all 30 published blog posts were cloned from staging.indexworld.net to the local
site (indexworld-revamp.local). Local went from 4 → 32 published posts (30 cloned + the local-only
"Power BI Reporting" #14 + "Hello world!" #1).

Method: fetched all posts in one shot from staging's REST API
(`/wp-json/wp/v2/posts?per_page=100&_embed`), built a manifest, then ran a one-shot PHP import
script that bootstrapped WordPress from the CLI (Local's PHP binary + the site's mysqld.sock under
`~/Library/Application Support/Local/run/<hash>/mysql/`) and called `wp_insert_post` /
`media_sideload_image`. This avoids pasting 18KB articles through MCP. The MCP write tools
(`royal-mcp-local`) also work but are call-heavy for bulk content.

What was preserved: slug, publish date, excerpt, categories (`odoo Customization`,
`Odoo Implementation`, else Uncategorized). **Featured images were downloaded into the local media
library** (URLs now local). The 2 pre-existing rewritten/shortened posts (#12 Odoo 18
Implementation, #13 Odoo Support) were **overwritten** with the full staging originals at the
user's request.

Inline body images: swept on 2026-06-21 — 178 unique images downloaded into local media and all
in-article `src`/`srcset` references rewritten to local full-size URLs (sweep matched
`staging.indexworld.net/wp-content/uploads/...` image URLs only; downloaded the original, stripped
`-WxH` size suffixes). Two odd double-extension files (`.jpeg-900x500-1.webp`,
`.jpg.optimal-min.jpg`) failed `media_sideload_image` and were re-fetched via `download_url` +
`media_handle_sideload` with clean filenames. **Zero staging image refs remain.**

Internal `<a href>` links: **FIXED 2026-06-23** — all absolute `https://staging.indexworld.net/...`
links in post content were rewritten to **root-relative** (`/odoo-partner/` etc.) across 29 posts (94
links), so they're domain-agnostic (work on local + live, no staging dependency). Done via CLI as an
admin user (so KSES didn't strip body markup). **Caveat:** some targets still point to slugs that
don't exist yet or don't match our slugs (e.g. `/cannabis/` vs our `/cannabis-industry/`,
`/automotive/`, `/bookkeeping/`) → they'll 404 until those pages are built / the links are corrected
(that's the separate link-audit + build-remaining-pages task, not a staging issue anymore).

Build scope ([[build-scope-plan]]) is unaffected — this was a separate user-requested task.

## Blog LISTING page built 2026-06-21 (user-requested)
`/blog/` now exists as **page id 351** (title "Blog", `_wp_page_template=industry` blank canvas). Content = two pattern refs: `indexworld/blog-hero` + `indexworld/blog-list`.
- `patterns/blog-hero.php` — lavender `#ECE6FF` hero: H1 "The latest from the Index World blog" + subtitle + a search/Category bar (functional `wp:search`-style GET form + `<select>` of categories that navigates to the category archive) + a FEATURED latest post card (right). Featured uses `new WP_Query(posts_per_page=1, meta_key=_thumbnail_id)` so it always picks the latest post **with** a thumbnail (the newest post #14 Power BI had a thumbnail anyway). Rewrote staging's placeholder brand leaks to "Index World".
- `patterns/blog-list.php` — `wp:query` (queryId 20, **inherit:false**, perPage 9) → 3-col grid of `.ix-blog__card` (featured-image, `wp:post-terms` category pills, title, excerpt+"Read more →", `wp:post-date`) + `wp:query-pagination`. Pagination works via `?query-20-page=N` (4 pages for 30 posts). Used a PAGE (not the native posts-page) because `show_on_front`/`page_for_posts` are NOT in the royal-mcp-local option allowlist — can't set them via MCP.
- CSS: `.ix-bloghero*` + `.ix-bloglist*` / `.ix-blog__cats` / `.ix-blog__date` / `.ix-blog__pag` appended to components.css. The homepage `.ix-blog` section (3-post Query Loop) is separate and untouched.
- **Caveat:** the newest post shows both as the hero featured card AND first grid card (minor duplication). Staging's featured was a placeholder, not a real post.

## Blog filter — SERVER-SIDE with pagination (2026-06-22, final)
First tried a client-side JS filter (loaded all 100 posts, hid/showed) — user rejected it: it **killed pagination** and dumped all posts in a messy "random" wall. Replaced with proper **server-side filtering + pagination** (the user's actual goal: click a category → only that category, paginated, clean grid, still on the styled /blog/ page — NOT the plain default category archive).
- `blog-list.php` is now a **custom `WP_Query` loop** (NOT a Query Loop block): reads `$_GET['category']` (slug), `$_GET['search']`, `$_GET['bpage']`; queries `posts_per_page=9, paged=bpage, category_name=cat, s=search, orderby date DESC`. Renders the banner cards (`.ix-blog__banner` + title overlay + `.ix-blog__brandbar` IW+Odoo logos + `.ix-blog__cats` + `.ix-blog__title` + `.ix-blog__excerpt` + `.ix-blog__foot`). Pagination via `paginate_links()` into `<nav class="ix-blog__pag">`, base `home_url('/blog/')?category=…&search=…&bpage=%#%` so the filter is preserved across pages. Empty state `.ix-bloglist__empty`.
- `blog-hero.php`: the search/category bar is a real `<form method="get" action="/blog/">` — search `name="search"` (prefilled), category `<select name="category" onchange="this.form.submit()">` with **option value = slug** + `selected()` reflecting the current filter. Category PILLS on cards link to `/blog/?category=<slug>`.
- CSS: custom grid `.ix-bloglist__inner` (max 1240) + `.ix-bloglist__grid` (3-col, was the Query Loop's post-template). `.ix-blog__pag` set to `display:flex; justify-content:center`. `.page-numbers` styling already existed.
- Verified: `/blog/` → 9 cards + pages 1·2·…·4; `/blog/?category=odoo-customization` → 4 cards, dropdown shows it selected, no pagination (1 page); search works the same.
- Slugs: `odoo-customization`, `odoo-implementation`, `uncategorized`. Reusable JS filter is GONE (no instant-filter).
- Card design (banner) was built by another session; preserved. Hero featured card has no image (another session removed it; left as-is). `templates/single.html` exists (another session) for single-post view.

## CONSISTENT card design (2026-06-22, final)
Unified the blog cards across homepage `patterns/blog.php` ("Our Insights", 3 latest) and `patterns/blog-list.php` (/blog/ listing):
- **Card = purple gradient `.ix-blog__banner` (single title overlay + IW/Odoo brandbar) → `.ix-blog__cats` category pills → `.ix-blog__excerpt`.**
- **Removed the duplicate dark `.ix-blog__title`** from BOTH (was showing the title twice — once on banner, once below). Only the banner title remains (it's the clickable post link).
- **Homepage:** no footer (matches staging's `card-gradient-bg` homepage cards — no date, no read more). `wp:post-excerpt` uses `moreText:""` + CSS hides any `.wp-block-post-excerpt__more-link`.
- **/blog/ listing:** keeps `.ix-blog__foot` (date left + "Read more →" right) per user choice — useful on a blog index.
- **Comma separator removed at ROOT level:** `.wp-block-post-terms__separator{ display:none }` in components.css — hides the ", " between category pills on every `wp:post-terms` block site-wide (homepage used post-terms; /blog/ uses a custom foreach so was already comma-free).
- homepage blog.php is a Query Loop (perPage 3) with the banner card; /blog/ blog-list.php is a custom WP_Query loop (filter + pagination).

## Card polish (2026-06-22) — tag ON the banner
User chose to move the category tag onto the banner (was below it). Now `.ix-blog__cats` sits INSIDE `.ix-blog__banner` (first child, before `.ix-blog__banner-title`) in both blog.php and blog-list.php. CSS: `.ix-blog__banner .ix-blog__cats a{ background:#fff; color:#4a2a9e }` (solid WHITE pill, purple text — user's chosen color over frosted/green/amber), `padding:22px 26px 0` at the banner top; `.ix-blog__banner-title a` top padding reduced to 12px. Card radius bumped to 20px. `.ix-blog__foot` divider removed (`border-top:none; padding-top:0`). Excerpt sits directly under the banner now (no tag row in the body).

## Single blog post template added 2026-06-22 (staging two-column design)
The theme had **no `single.html`**, so single posts fell back to `index.html` (the query-loop LISTING) and rendered only title + excerpt — "content not showing". Fixed by creating `templates/single.html`, modeled on staging's blog post (`/odoo-vs-quickbooks.../`):
- **Two-column** `.ix-post__wrap` grid (820px content + 330px sidebar). Main `<article>` = `.ix-post__cover` (featured image with overlapping `.ix-post__datepill` date pill, bottom-left) → `.ix-post__title` h1 → `.ix-post__cats` lavender category pill (`wp:post-terms`) → `.ix-post__body` (`wp:post-content`).
- **Sticky "Recent Posts" sidebar** (`<aside>`): `wp:query` (queryId 30, inherit:false, perPage 4). Design = **staging's** recent-post card (extracted from staging Elementor loop CSS): a **white card (radius 12, hover shadow `0 0 10px rgba(70,70,70,.42)`) → purple title chip (`#50269E`, radius 9, 12px 'index happy font' white) on the left + excerpt (14px 'AlmadenSans para' weight 500) on the right**. NO featured image. `.ix-recent__item` (white card) + `.ix-recent__thumb` (purple chip) + `.ix-recent__ex` (excerpt). **Alignment fix 2026-06-23:** card uses `align-items:stretch` so chip + excerpt are equal height with vertically-centred text (the old version top-aligned the chip + used a `padding-top` hack → ragged). ⚠️ Do NOT swap this for image thumbnails — staging uses the purple-chip design (this was tried and reverted at owner's request).
- **"Related Blogs"** full-width row below: `wp:query` (queryId 31, perPage 3) → 3 cards (featured img + category pill + title + date).
- All `.ix-post*` / `.ix-recent*` / `.ix-related*` styles appended to components.css. Body styles scoped to `.ix-post__body` (incl. table styling — cloned posts have comparison tables). Responsive: 1-col ≤980px. Applies to ALL posts.
- Note: Recent/Related query loops don't exclude the current post (block query limitation) — acceptable. Featured images are the per-post cloned images (some tall illustrations, some purple branded banners).

## Single-post body typography — professional polish (2026-06-23)
`.ix-post__body` refined for readability (verified in browser via http://): lead paragraph (`> p:first-of-type`) larger 1.18rem; body p `#3c4150` line-height 1.78 margin-bottom 20; `p:has(+ul/+ol)` tightened to 8px (lead-in sits on its list); lists `padding-left:22`, `li` 9px gap, **`li::marker` purple `#50269E`** (branded bullets); `strong/b` 700 dark; inline `a` underline `rgba(80,38,158,.32)` → full purple on hover; `img` block + 30px margin. Prose kept full column width so edges align with the title (did NOT narrow the measure — narrower prose looked misaligned vs the title). Blockquote stays italic (intentional). Applies to ALL posts (shared CSS).

## Blog readability cleanup (2026-06-23)
- **Placeholder labels removed** site-wide: staging callout leaks `Success Box` / `Info Box` / `Pro Tip` (16 instances across posts) stripped, keeping the real sentence/heading. (`STAR` was a false positive = "start".)
- **FAQ → our component (all 21 FAQ posts, ~91 Q&A).** Staging FAQ markup was `<h2>Frequently Asked Questions</h2>` + repeating `<h2 data-toggle="htbcollapse" data-target="#htmega-...">Question</h2><p>Answer</p>` (HT Mega accordion plugin). Converted each to our **`.ix-ifaq` accordion**: a plain `<h2 class="ix-post__faqh">Frequently Asked Questions</h2>` (inherits article h2) + `<div class="ix-ifaq__list">` of `<details class="ix-ifaq__item">` (summary `.ix-ifaq__q` + `.ix-ifaq__ico-plus` svg, answer `.ix-ifaq__a > .ix-ifaq__a-in`). NO section band/eyebrow — the item styles aren't scoped under `.ix-ifaq`, and `faq-accordion.js` auto-inits any `.ix-ifaq__list` (enqueued site-wide), so it gets the smooth single-open accordion in-post. Verified rendering. Reusable detector: posts with `htbcollapse` = staging FAQ.
- **Image declutter (balanced) — SAMPLE done on post 110** (18→3 distributed images; all stock decorative repeats removed, text/headings/lists kept). Rollout to the other image-heavy posts (8–15 imgs) still PENDING owner go. Short 3-item bullet lists were left (they're scannable, not over-long).
- ⚠️ Editorial gotcha: the json-rpc post (id ~) has large blank gaps where code samples should be — separate content issue, not the FAQ.

## /blog/ listing images "not showing" — fixed 2026-06-22
Blog card + hero featured images used WP default `loading="lazy"`; the grid sits below the fold so it painted blank until scroll ("images not showing"). Fixed with a `wp_get_attachment_image_attributes` filter in functions.php **scoped to `is_page('blog')`** that forces `loading=eager` + `decoding=async` (lazy stays on elsewhere). All images now load on first paint. (Same lazy-load gotcha family as [[lazy-load-hidden-tabs-gotcha]].)
