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

Theme-repo copies of the 5 `ec-*` files also live at `assets/images/how-it-works/` in `indexworld-blocks` (that pattern's tabs loop reads a theme-relative filename, not a Library URL — see `patterns/erp-consultant.php` `$ec_steps`).

## Photos — existing Library assets reused (not new uploads)

| Filename / slug | Depicts | Page → section |
|---|---|---|
| cust-understand.webp (id 1737) | Team analyzing requirements together before a customization | Originally /odoo-customization/ step "Understand the need"; **also now used on** /erp-consultant/ tab 1 Workflow Mapping |
| industry-multi-channel-ecommerce.webp (id 1758) | Multi-channel ecommerce industry graphic | /odoo-integration-services/ hero |
| configuration-and-quick-start (slug) | Config/quick-start scene | /odoo-integration-services/ step 5 Testing & validation |
| how-04-build (slug) | Build-phase scene | /odoo-integration-services/ step 6 Go-live & cutover AND step 7 Support & monitoring (same image, two slots, per explicit instruction) |
| index-world-consultant-with-erp-dashboard (slug) | Consultant with ERP dashboard | /odoo-audit/ hero → saved into theme as `about-cta-realistic.png` |
| odoo-implementation-consultants-reviewing-an-erp-dashboard (slug) | Consultants reviewing a dashboard | /odoo-audit/ step "Discovery call" → theme file `audit-discovery.png` |
| index-world-integration-developers-collaborating (slug) | Developers collaborating | /odoo-audit/ step "System review" → theme file `audit-system-review.jpg` |
| supply-chain-manufacturing-go-live (slug) | Go-live floor scene | /odoo-audit/ step "Implement & support" → theme file `audit-implement-support.jpg` |
| real-estate-go-live (slug) | Go-live scene | /odoo-audit/ closing image ("An audit that ends in action") |
| migrate-from-legacy-erp / em-legacy.webp (id 1717) | Team at computers on an enterprise system | /odoo-rescue/ step 2 Stabilize — swapped in after 1841 was rejected |

## Icon sets redesigned (no generated art — sourced from the site's existing icon library or Lucide)

| Page | Grid | What changed |
|---|---|---|
| /odoo-audit/ | `.ix-ichal__grid` under "Spot issues early, fix them with confidence" | `$a_ic` in `patterns/odoo-audit.php`: 6 icons (gauge, layers, code, lock, db, plug) redrawn with an added negative-context mark (+/− dial, cross badge, lock-breach dot) per issue |
| /odoo-rescue/ (baked page, id 1080) | "Sound familiar?" checklist | Tick boxes replaced with context-matched icons (negative/neutral per line); "Nobody trusts the numbers" icon revised again after a rejected first pass — exact SVGs live in the page content, fetch via `wp_get_page(1080)` if reworking |
| /odoo-training-services/ (baked page, id 678) | "Training built for every role on your team" | Role icons checked against copy; Accountant / Sales Manager / HR Manager redone once, rest approved as-is — exact SVGs via `wp_get_page(678)` |

## Gotchas

- Pexels photo 6803525 was almost picked for two different pages in the same session before being caught — always check this log first.
- `wp_upload_media_from_url` needs a public HTTPS source; the working recipe is theme-repo commit → push → poll the deployed `assets/images/...` URL for 200 → upload from that URL.
- Pattern files using a `get_theme_file_uri('/assets/images/...')` + filename-in-array loop (`odoo-audit.php`, `erp-consultant.php`) don't automatically serve the Library copy — the Library upload is for the site's own image-management model (see project_memory `CLAUDE.md` "Image handling"), but what actually renders is the theme file. Baked DB pages (rescue, training, hire-dev) reference the Library URL directly instead.
