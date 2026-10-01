---
name: pending-erp-migration-imagery
description: "PENDING work: /erp-migration/ (page id TBC) needs 15 new images. Analysis is done and recorded here; only generation + deploy remain."
metadata:
  type: project
---

**Status 2026-08-18: analysis complete, generation blocked** (the image-generation MCP server
disconnected mid-session). Nothing has been changed on the page. Pick this up by generating.

Owner's brief: **bright, like /erp-consultant/** — warm daylight, NOT the cool blue-teal used on
/erp-integration/. Real photos of people / industry / machinery, not diagrams
(see the photo-vs-artifact gotcha in [[image-library-log]]).

## Scope: 15 images. Exclude the 16 industry-grid thumbs and the 5 vendor logos, as on every other page.

| Section | Current file (all need replacing) | Generate at |
|---|---|---|
| `.ix-ihero--svc` hero | `2026/07/erp-migration-reconciliation-review.webp` | 4:3 |
| `.ix-iwhat` "What is ERP migration?" | `2026/07/erp-migration-weekend-cutover.webp` | **1:1** (slot 0.79 desktop) |
| `.ix-services.ix-keyben` ×5 | `2026/08/em-accounting · em-legacy · em-spreadsheets · em-datamig · em-cutover` | 16:9 |
| `.ix-ftabs` ×6 | `2026/07/how-01-discovery · how-02-design · how-04-build · how-05-deploy · how-06-training · how-07-optimize` | 4:5 |
| `.ix-iwhat.ix-iwhy` | `2026/08/erpsol-why.webp` | **2:3** (slot **0.30** desktop — narrowest on the site) |
| `.ix-iwhat` "Proof, and what a migration costs" | `2026/08/erp-migration-cost-accountant-workspace.jpg` | 5:4 |

⚠️ The six `.ix-ftabs` files are **odoo-implementation's shared `how-0*` set**, and `erpsol-why.webp`
is shared with erp-consultant/-implementation/-integration. Give this page its own files.

## Content to illustrate (from the page's own copy)

- **5 tabs are the sources you migrate FROM:** From Accounting Software · From Legacy ERP ·
  From Spreadsheets · ERP Data Migration · Cutover & After.
- **6 checklist steps:** inventory the source system (incl. the workflows living in spreadsheets) →
  field-by-field mapping with a **second CPA pass on the financial layer** (chart of accounts) →
  cleanse: duplicates merged, dead records retired, formats standardised → full migration run against
  **staging, reconciled, fixed, run again** → **cutover in a planned quiet-weekend window** with final
  delta, reconciliation, smoke tests, go decision, rollback path → hypercare then US-hours support.
- **Why:** "CPA-led reconciliation … totals matched between systems, documented and signed before
  go-live"; "named proof, not adjectives".
- **Cost:** references the Looseleaf QuickBooks migration, "balances moved reconciled to the cent".

The weekend-cutover window and the CPA reconciliation pass are the two most distinctive shots.

## Deploy rules (do not skip)

Replace the **complete** `/wp-content/uploads/YYYY/MM/<file>` path, never the filename alone —
originals here sit in both `2026/07` and `2026/08`. Verify by extracting each path from the rendered
page and fetching that exact string, asserting `200` + `image/*`.
See [[measure-slot-ratios-before-generating]] and [[image-library-log]].
