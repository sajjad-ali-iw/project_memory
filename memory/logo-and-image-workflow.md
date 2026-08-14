---
name: logo-and-image-workflow
description: "RULE: owner supplies logos/images — do NOT source them yourself; upload to the PRELIVE Media Library first, then reference those URLs in local; owner pushes the code"
metadata:
  node_type: memory
  type: feedback
---

**RULE (owner, 2026-07-30): the owner supplies brand logos and imagery. Do NOT go hunting for
logos on the web.** When new imagery is needed, ask the owner for the files and wait.

**The order of operations is fixed:**
1. Owner provides the logo/image files.
2. **Upload them to the PRELIVE Media Library first** (`royal-mcp-prelive` → `wp_upload_media` /
   `wp_upload_media_from_url`). Prelive is the image source of truth.
3. **Then** wire the resulting prelive Media Library URLs into the LOCAL theme/section markup.
4. **The owner pushes the code** — do not commit/push the theme repo unless asked.

**Why:** the owner manages all media through the Media Library across environments
([[image-webp-media-library]]), and prelive is the shared environment other devs pull from. Logos
sourced ad-hoc into `assets/images/` bypass that pipeline, may be the wrong/unlicensed mark, and
create assets nobody else has. A 2026-07-30 session sourced ~12 logos from Wikimedia into the theme
before being corrected — that is the anti-pattern.

**How to apply:**
- Referencing prelive URLs from the local site is the ESTABLISHED pattern in `patterns/stack.php`
  (Odoo and QuickBooks already load `https://prelive.indexworld.net/wp-content/uploads/...`).
  Local is http and prelive is https, so an absolute https prelive URL loads fine — this is the
  one case where absolute URLs are correct, not the root-relative rule in
  [[mcp-absolute-url-protocol-gotcha]] (that rule is about baking `https://<LOCAL host>` URLs).
- Still respect the slot/pipeline rules: [[image-slot-aspect-ratios]], [[oversized-realistic-imagery-fix]]
  (resize to slot spec, WebP q90 — never upload raw multi-MB originals).
- Trademark note: vendor logos shown as "tools we implement" is nominative use, but some vendors
  (Salesforce, Twilio, Amazon) actively police their marks — owner decides what ships.

Related: [[image-webp-media-library]], [[always-supply-images-on-reuse]], [[multi-dev-collaboration]].
