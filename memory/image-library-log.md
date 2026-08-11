---
name: image-library-log
description: Running record of photos/icons sourced or reused for service-page image-replacement work, by page and section — check before sourcing a new photo to avoid duplicate use
metadata:
  type: project
---

Started 2026-08-07 during a multi-page image-replacement pass across service pages (`/odoo-integration-services/`, `/odoo-audit/`, `/odoo-rescue/`, `/odoo-training-services/`, `/hire-odoo-developer/`, `/erp-consultant/`). Photos are Pexels-sourced (free license, no attribution) unless noted "existing library asset" (reused, not newly uploaded). Update this file whenever a new image is uploaded and placed, or an icon grid is redesigned — same commit/step where the image itself is added.

**Before sourcing a new web photo for a task: check this log + `wp_search`/`wp_get_media` for anything already depicting a similar scene**, so a photo doesn't get placed on two pages by accident (almost happened once this session — see Gotchas).

## Photos — newly uploaded to Media Library this session

| Library ID | Filename | Depicts | Page → section |
|---|---|---|---|
| 1839 | odoo-rescue-hero-assessment.webp | Two consultants reviewing charts/data during a stalled-project assessment | /odoo-rescue/ hero |
| 1840 | odoo-rescue-step1-audit.webp | Magnifying glass over documents (read-only audit) | /odoo-rescue/ step 1 Audit |
| 1841 | odoo-rescue-step2-stabilize.webp | IT technician untangling network cables | **REJECTED** — user said it "completely missed the point" (too literal, not about data/workflow integrity). Orphaned in Library, not used anywhere. Do not reuse. |
| 1842 | odoo-rescue-step3-golive.webp | Team meeting around a laptop (phased go-live + training) | /odoo-rescue/ step 3 Go-live |
| 1843 | odoo-rescue-why-reviewing-config.webp | Two developers reviewing code together | /odoo-rescue/ why-section |
| 1848 | odoo-training-hero-role-training.webp | Colleague walking a teammate through role-specific training at a shared screen | /odoo-training-services/ hero |
| 1849 | odoo-training-adoption-dashboard.webp | Two colleagues reviewing training-completion progress on a laptop | /odoo-training-services/ "See who's trained, and drive real adoption" |
| 1853 | hire-odoo-dev-step1-requirements.webp | Two people discussing project requirements over notebooks/coffee | /hire-odoo-developer/ step 1 Share requirements |
| 1854 | hire-odoo-dev-step2-match.webp | Recruiter reviewing candidate documents | /hire-odoo-developer/ step 2 Match a developer |
| 1855 | hire-odoo-dev-step3-interview.webp | Video-call interview, candidate taking notes | /hire-odoo-developer/ step 3 Interview and approve |
| 1856 | hire-odoo-dev-step4-onboard.webp | Developer at a laptop getting set up with access/tools | /hire-odoo-developer/ step 4 Onboard fast |
| 1857 | hire-odoo-dev-step5-develop.webp | Developer intensely coding at a workstation | /hire-odoo-developer/ step 5 Develop and deliver |
| 1858 | hire-odoo-dev-step6-collaboration.webp | Two people on a video call gesturing, discussing progress | /hire-odoo-developer/ step 6 Ongoing collaboration |
| 1859 | hire-odoo-dev-step7-scale.webp | Developers at desks in an open-plan office | /hire-odoo-developer/ step 7 Scale the team (redone once on feedback) |
| 1875 | ec-requirements.webp | Requirements doc drafted, checked against candidate systems | /erp-consultant/ tab 2 Requirements & Fit |
| 1876 | ec-recommendation.webp | Written recommendation reviewed with client leadership | /erp-consultant/ tab 3 Recommendation |
| 1877 | ec-build-handover.webp | Implementation build phase, standard-first, against approved blueprint | /erp-consultant/ tab 4 Build or Handover |
| 1878 | ec-golive.webp | Role-based training on the configured system, ahead of a rehearsed cutover | /erp-consultant/ tab 5 Data, Training, Go-Live |
| 1879 | ec-advisory.webp | Standing quarterly review, usage + automation roadmap | /erp-consultant/ tab 6 Ongoing Advisory |
| 1880 | epi-discovery.webp | Team analyzing workflow data on a laptop during a discovery session | /erp-implementation/ phase "Discovery & Scoping" |
| 1881 | epi-fitgap.webp | Coworkers reviewing data charts on laptops, grading requirements | /erp-implementation/ phase "Fit-Gap & Design" |
| 1882 | epi-build.webp | Team doing a collaborative configuration review on laptops | /erp-implementation/ phase "Build & Configure" |
| 1883 | epi-migration.webp | Team collaborating on financial data reconciliation | /erp-implementation/ phase "Data Migration" |
| 1884 | epi-training.webp | One colleague walking another through training at a shared screen | /erp-implementation/ phase "Training & UAT" |
| 1885 | epi-golive.webp | Two-person support desk, post-launch hypercare | /erp-implementation/ phase "Go-Live & Beyond" |
| 1886 | ei-scan-warehouse.webp | Warehouse worker scanning inventory with handheld scanner + tablet | /erp-integration/ tabs "Payments, Banks & Accounting" AND "Shipping, 3PL & EDI" (same image, both tabs, per instruction) |
| 1887 | ei-crm-growth.webp | Two people reviewing a rising growth chart on a tablet | /erp-integration/ tab "CRM & Marketing Attribution" |
| 1888 | ei-data-dashboard.webp | Laptop showing a live analytics dashboard (active users, pageviews, top pages) | /erp-integration/ tab "Data, BI & AI" — **REVERTED**, this whole "What our ERP integration services cover" batch (1886-1888 + the ei-ecommerce-multichannel reuse) was undone; see Gotchas |
| 1889 | eint-audit.webp | Magnifying glass and calculator reviewing a chart | /erp-integration/ phase "Systems Audit" |
| 1892 | eint-reconcile.webp | Two people comparing printed reports side by side | /erp-integration/ phase "Test & Reconcile" |
| 2206 | cost-implementation.webp | Reviewing a printed cost-estimate report (finance charts) | /erp-implementation/ "How much do ERP implementation services cost?" |
| 2207 | cost-consultant.webp | Consultant and client going over a pricing proposal, calm two-person laptop discussion | /erp-consultant/ "How much does an ERP consultant cost?" |
| 2208 | cost-integration.webp | Two hands pointing at itemized line items on an invoice/quote | /erp-integration/ "How much does ERP integration cost?" |
| 2209 | cost-selection.webp | Presenter comparing pie-chart candidate data with a 4-person team | /erp-selection/ "How much does ERP selection consulting cost?" |
| 2210 | menu-preview-support.webp | Support agent with a headset helping a client | Mega-menu hover preview for "Odoo Support Plans" (`parts/header.html`) |
| 2211 | menu-preview-upgrade.webp | Developer at dual monitors with code on screen | Mega-menu hover preview for "Upgrade Odoo Version" — closest available fit; no real photo literally depicts a software version upgrade (phone-OS-update screenshots were rejected as wrong-platform/branded) |
| 2212 | menu-preview-partner.webp | Two professionals shaking hands with a growth chart on screen | Mega-menu hover preview for "Odoo Partner" |
| 2270 | menu-preview-payroll.webp | Hands counting cash with payroll spreadsheets and a calculator | Mega-menu hover preview for "Payroll Management" |
| 2271 | menu-preview-quickbooks.webp | Hands typing on a laptop with accounting software and binders | Mega-menu hover preview for "QuickBooks Services" |

"Bookkeeping" mega-menu preview reuses `bookkeeping-step4-monthly.webp` (id 2245, already in the Library from the bookkeeping-page pass) rather than a new upload — all 3 previously shared the generic `staging/3x-4.jpg`.
| 2213 | upgrade-step1-audit.webp | IT professional reviewing a report on screen | /odoo-upgrade-services/ step 1 "Audit current version" |
| 2214 | upgrade-step6-cutover.webp | Two developers at dual monitors monitoring a deployment | /odoo-upgrade-services/ step 6 "Cutover" |
| 2215 | upgrade-step7-support.webp | Support team with headsets and laptops working together | /odoo-upgrade-services/ step 7 "Support & maintenance" |
| 2216 | upgrade-partner-accountable.webp | Senior consultant presenting a plan to the team | /odoo-upgrade-services/ "An Odoo Partner accountable for a safe upgrade" |
| — | partner-hero.webp (theme file, not Library-uploaded) | Four-person tech team reviewing work at a screen | /odoo-partner/ hero (was a badge/logo graphic, `$p_logo`) |
| — | partner-why.webp (theme file, not Library-uploaded) | Senior consultant guiding two colleagues at their desks | /odoo-partner/ "Why choose Index World as your Odoo partner?" (was reusing one of the 3 credibility badge images) |
| 2232 | odoo-accounting-hero.webp | Overhead view of hands reviewing financial documents with a calculator | /odoo-accounting/ hero (was reusing the generic odoo-impl-hero.jpg shared with other pages) |
| 2233 | odoo-accounting-why.webp | Two colleagues reviewing a financial report together at a table | /odoo-accounting/ "An accounting bench your accountant can audit" (was reusing the generic odoo-impl-why.jpg) |
| 2242 | bookkeeping-step1-discovery.webp | Advisor and client discussing needs in an office meeting | /bookkeeping/ process step 1 "Discovery & Financial Assessment" |
| 2243 | bookkeeping-step2-setup.webp | Two accountants setting up software and tools at a shared table | /bookkeeping/ process step 2 "System Setup & Integration" |
| 2244 | bookkeeping-step3-cleanup.webp | Hands with a calculator reviewing financial ratio worksheets | /bookkeeping/ process step 3 "Data Migration & Cleanup" |
| 2245 | bookkeeping-step4-monthly.webp | Bookkeeper focused on calculations with documents at a desk | /bookkeeping/ process step 4 "Monthly Bookkeeping" |
| 2246 | bookkeeping-step5-reporting.webp | Presenter pointing at financial charts on a finance review board | /bookkeeping/ process step 5 "Financial Reporting" |
| 2247 | bookkeeping-step6-support.webp | Advisor reviewing documents with a client, ongoing support discussion | /bookkeeping/ process step 6 "Ongoing Support" |
| 2224 | upgrade-step3-plan.webp | Two people reviewing a migration plan checklist document, overhead view | /odoo-upgrade-services/ step 3 "Migration plan & backup" (final — superseded the erp-migration-servers.webp reuse, which was just server racks with no planning visual) |
| 2225 | upgrade-step4-code.webp | Developer typing real code on a laptop at an office desk | /odoo-upgrade-services/ step 4 "Migrate & refactor code" (final — superseded the cust-development.webp reuse, whose AI-brain-hologram overlay read as an AI-marketing stock shot rather than real refactoring work) |

These 3 previously shared the generic `menu-preview-services.jpg` hover-preview image in the desktop mega-menu (`parts/header.html`, `data-image` attribute on `.ix-item` links) — same file is still shared by 2 other menu items ("Odoo Integration", "Hire Odoo Developers") which weren't in scope for this pass.

All 4 previously shared one generic `odoo-impl-cost.jpg`/`.webp` (theme path `assets/images/odoo-impl/`) across the `.ix-iwhat` "How much does X cost?" section on all 4 Lane-A pages (erp-consultant, erp-implementation, erp-integration, erp-selection) — each now has its own image and file (`assets/images/odoo-impl/cost-*.webp`), matched to that page's specific pricing copy. The old shared file is still in the theme repo but no longer referenced by any of the 4.
| 1893 | eint-monitor.webp | Two support agents with headsets at a screen | /erp-integration/ phase "Monitor & Support" |
| 1894 | eint-fields.webp | Hands with a calculator and notebook beside a laptop showing a data table | /erp-integration/ phase "Data Mapping" (final — superseded 1890 `eint-mapping.webp`, a server-room photo the user said "don't look good") |
| 1895 | eint-flowchart.webp | Reviewing a flowchart on a whiteboard | /erp-integration/ phase "Method Choice" (final — superseded 1891 `eint-method.webp`, a "Use APIs" whiteboard shot) |
| 1896 | eint-build.webp | Two developers coding side by side | /erp-integration/ phase "Build & Connect" (final — first real image for this phase; earlier kept as `how-04-build.jpg` by choice, then changed after the user asked for 2/3/4 to be redone) |

1890 (`eint-mapping.webp`) and 1891 (`eint-method.webp`) are now unused/orphaned in the Library — superseded by 1894 and 1895 above, same as the 1841 "cables" precedent. Don't reuse.

Theme-repo copies of the 5 `ec-*` files and the 6 `epi-*` files also live at `assets/images/how-it-works/` in `indexworld-blocks` (those patterns' tabs loops read a theme-relative filename, not a Library URL — see `patterns/erp-consultant.php` `$ec_steps` and `patterns/erp-implementation.php` `$ep_steps`).

## Photos — existing Library assets reused (not new uploads)

| Filename / slug | Depicts | Page → section |
|---|---|---|
| cust-understand.webp (id 1737) | Team analyzing requirements together before a customization | Originally /odoo-customization/ step "Understand the need"; **also now used on** /erp-consultant/ tab 1 Workflow Mapping |
| industry-multi-channel-ecommerce.webp (id 1758) | Multi-channel ecommerce industry graphic | Already used /odoo-integration-services/ hero; **also now used on** /erp-integration/ tab "eCommerce & Marketplaces" (copied into theme as `ei-ecommerce-multichannel.webp`) — third-known placement, watch before reusing again |
| industry-multi-channel-ecommerce.webp (id 1758) | Multi-channel ecommerce industry graphic | /odoo-integration-services/ hero |
| configuration-and-quick-start (slug) | Config/quick-start scene | /odoo-integration-services/ step 5 Testing & validation |
| how-04-build (slug) | Build-phase scene | /odoo-integration-services/ step 6 Go-live & cutover AND step 7 Support & monitoring (same image, two slots, per explicit instruction) |
| index-world-consultant-with-erp-dashboard (slug) | Consultant with ERP dashboard | /odoo-audit/ hero → saved into theme as `about-cta-realistic.png` |
| odoo-implementation-consultants-reviewing-an-erp-dashboard (slug) | Consultants reviewing a dashboard | /odoo-audit/ step "Discovery call" → theme file `audit-discovery.png` |
| index-world-integration-developers-collaborating (slug) | Developers collaborating | /odoo-audit/ step "System review" → theme file `audit-system-review.jpg` |
| supply-chain-manufacturing-go-live (slug) | Go-live floor scene | /odoo-audit/ step "Implement & support" → theme file `audit-implement-support.jpg` |
| real-estate-go-live (slug) | Go-live scene | /odoo-audit/ closing image ("An audit that ends in action") |
| migrate-from-legacy-erp / em-legacy.webp (id 1717) | Team at computers on an enterprise system | /odoo-rescue/ step 2 Stabilize — swapped in after 1841 was rejected |
| epi-fitgap.webp (id 1881) | Coworkers reviewing data charts on laptops | Also /odoo-upgrade-services/ step 2 "Compatibility analysis" |
| erp-migration-servers.webp (id 1705) | Row of tower servers | Also /odoo-upgrade-services/ step 3 "Migration plan & backup" |
| cust-development.webp (id 1738) | Developer laptop with code, glowing AI-brain hologram overlay | Also /odoo-upgrade-services/ step 4 "Migrate & refactor code" — the AI-hologram styling is a slight mismatch (this is about refactoring, not AI) but was approved as-is |

## Icon sets redesigned (no generated art — sourced from the site's existing icon library or Lucide)

| Page | Grid | What changed |
|---|---|---|
| /odoo-audit/ | `.ix-ichal__grid` under "Spot issues early, fix them with confidence" | `$a_ic` in `patterns/odoo-audit.php`: 6 icons (gauge, layers, code, lock, db, plug) redrawn with an added negative-context mark (+/− dial, cross badge, lock-breach dot) per issue |
| /odoo-rescue/ (baked page, id 1080) | "Sound familiar?" checklist | Tick boxes replaced with context-matched icons (negative/neutral per line); "Nobody trusts the numbers" icon revised again after a rejected first pass — exact SVGs live in the page content, fetch via `wp_get_page(1080)` if reworking |
| /odoo-training-services/ (baked page, id 678) | "Training built for every role on your team" | Role icons checked against copy; Accountant / Sales Manager / HR Manager redone once, rest approved as-is — exact SVGs via `wp_get_page(678)` |
| /odoo-partner/ | `$p_ic` in `patterns/odoo-partner.php`, "Simple, effective Odoo services" grid | `migr` (Odoo Migration) and `hire` (Hire Odoo Developer) replaced with Lucide `database-backup` and `user-plus` — both keys were previously the same generic arrow/person icons reused sitewide (header mega-menu, etc.); this edit only touches this page's own `$p_ic` array, not the shared header.html SVGs |
| /odoo-accounting/ (baked page, id 1120) | "Where DIY setups go wrong, and what closes each gap" | All 6 icons replaced with existing site icons only (no generation): `layers` (odoo-audit.php, duplication badge) for "chart of accounts copied", `tdown` (erp-consultant.php) for "costing method left on default", `eqnot` (erp-consultant.php, not-equal) for "bank feeds never reconciled", `shx` (erp-consultant.php, broken shield) for "sales tax on defaults", `db` (odoo-audit.php, warning-triangle badge) for "stock and ledger disagree", `calx` (erp-consultant.php, calendar-X) for "month-end never closes" |
| /bookkeeping/ (baked page, id 690) | 3 grids: "Comprehensive bookkeeping solutions", "Industry-specific bookkeeping solutions", "Why businesses choose Index World" | The same 6 generic icons (receipt/shield/box/chart/store/sprout, from `icon-cards.php`'s cannabis-industry set) were cycling across all 3 grids regardless of content. Reassigned 15 icons total: existing-site reuses — `users` (erp-implementation.php) for Payroll Support, `shopping-cart`/`server`/`building`/`factory` (industries.php) for Ecommerce/SaaS/Real Estate/Manufacturing, `store` (icon-cards.php, kept — was already correct) for Retail, `badgecheck`/`team`/`value`/`shield` (erp-consultant.php) for Accurate Records/Experienced Professionals/Cost-Effective/Secure; Lucide fallbacks (nothing fitting existed) — `credit-card` (AP), `hand-coins` (AR), `file-chart-column` (Financial Reporting), `briefcase-business` (Professional Services), `cloud` (Cloud-Based Solutions), `trending-up` (Scalable Support, swapped in after `sprout` was rejected) |
| /payroll-services/ (baked page, id 1076) | `.ix-ichal__grid` under "Where payroll quietly goes wrong" (6 failure-mode cards, all negative context) | All 6 icons were generic/neutral (clock, bars, shield-check, ribbon, sparkle, squiggle) and didn't signal "this is a problem." Reassigned via `wp_replace_in_page`, reusing the exact icon+badge set already proven on `/odoo-accounting/`'s matching grid: not-equal `✕` (odoo-accounting "bank feeds never reconciled") for "Attendance data nobody reconciles"; `layers` + cross-badge (odoo-audit.php/odoo-accounting, literal "copied, not designed" phrase match) for "Structures copied, not designed"; `trending-down` (odoo-accounting "costing method left on default", literal "left on defaults" phrase match) for "Tax settings left on defaults"; `db` + caution-triangle badge (odoo-audit.php "db", odoo-accounting "stock and ledger disagree") for "Payroll divorced from the ledger"; `calendar-✕` (odoo-accounting "month-end never closes") for "Year-end surprises". The 6th, "Payday lives in one person's head," had no existing site icon for a single-person/key-person risk — built new: Lucide `user` + the same caution-badge construction as the `lock` icon in odoo-audit.php (solid circle, white "!" glyph), so it matches the established visual language (base Lucide icon + small negative badge) rather than introducing a new style. |

## Gotchas

- Pexels photo 6803525 was almost picked for two different pages in the same session before being caught — always check this log first.
- Very-low Pexels photo IDs (e.g. 7374) use an older URL scheme — `pexels-photo-<id>.jpeg` 404s as "Source image is unreachable"; fetch the photo page and read the real filename slug (e.g. `startup-photos.jpg`) from its og:image/src instead.
- `pexels.com` is blocked by policy in the Browser pane (`navigate` fails outright) — approval previews use plain markdown links in chat instead, see [[image-approval-preview-method]].
- Always view a candidate photo directly (download + `Read`, or the Artifact grid) before proposing it from a text description alone — a Pexels search description called a photo a "workflow strategy discussion" when the actual on-screen content was a "Workflow Wars" gamified-training mockup, and another called a stock-trading candlestick terminal a fit for "Data, BI & AI." Both looked right in text and wrong in the image (2026-08-07, /erp-integration/ tabs 4-5).
- ⚠️ **2026-08-10: Bunny CDN pull zone (`bunny-wp-pullzone-7yxxoq4cy0.b-cdn.net`) is down** — the live page HTML rewrites image `data-src` to this CDN host, and every request to it (even old, previously-working files, even the zone root) returns Bunny's own `504 Gateway Timeout` error page (`ErrorCode: 106`, `CDN-Cache: MISS` — the CDN can't reach its origin). This is sitewide, not caused by any image swap — direct requests to `prelive.indexworld.net` still return 200 for the same files. Landed via one of the "site updates via SHIP" commits merged during this session; needs the site owner / hosting dashboard to fix, no MCP tool covers CDN config. Re-check before trusting a "curl works" verification as proof images render for real visitors.
- `mcp__royal-mcp-prelive-indexworld__wp_replace_in_page` (and `_in_post`) does a literal find/replace on a page/post's content server-side — no need to fetch, PHP-decode, and resend the whole body for a baked-page image swap. Dry-run with `expected_count` first to confirm an exact single match, then apply. Much faster than the earlier PHP-reconstruction workflow for small, targeted edits (used successfully on /odoo-upgrade-services/, id 715).
- `/erp-integration/` served a stale cached page after this deploy (`X-LiteSpeed-Cache: hit`, `max-age=604800`); confirmed the real content was already correct via a `?nocache=<ts>` query-string bust. See [[litespeed-purge-and-edge-cache]] — programmatic purge needs an authenticated admin session this tool doesn't have.
- **This page has TWO tab-like sections with similar "tab 2/3" phrasing** — the 5-tab `.ix-svc` grid ("What our ERP integration services cover", `$ei_tabs`) at line ~283, and the 6-phase `.ix-ftabs` vertical tabs ("How an Integration Project Runs", `$ei_steps`) at line ~346. A request to swap "tab 2" or "tab 3" images on `/erp-integration/` needs the heading confirmed before editing — the first attempt edited `$ei_tabs` when the user meant `$ei_steps`, had to be git-reverted (commit 8548a3b → revert 65dd561), and redone against the right array.
- `wp_upload_media_from_url` needs a public HTTPS source; the working recipe is theme-repo commit → push → poll the deployed `assets/images/...` URL for 200 → upload from that URL.
- Pattern files using a `get_theme_file_uri('/assets/images/...')` + filename-in-array loop (`odoo-audit.php`, `erp-consultant.php`) don't automatically serve the Library copy — the Library upload is for the site's own image-management model (see project_memory `CLAUDE.md` "Image handling"), but what actually renders is the theme file. Baked DB pages (rescue, training, hire-dev) reference the Library URL directly instead.
