---
name: prefer-media-library-over-theme-assets
description: "Owner preference (2026-08-15): upload new page images to the prelive Media Library — do NOT commit them into the theme's assets/images/ folder."
metadata:
  type: feedback
---

**Rule:** when adding a new image to a page, upload it to the **prelive WordPress Media Library** and
reference the root-relative `/wp-content/uploads/...` URL. Do **not** add the file to the theme repo
under `assets/images/`.

Stated by the owner 2026-08-15: *"you should upload no need to adding file to assets."* Acted on the
same day — the odoo-customization step-1 image was moved out of
`assets/images/how-it-works/cust-understand-brains.webp` into the Library and the theme copy was
deleted (theme commit `92876a9`).

**Why:** the owner manages imagery through the Media Library across environments; theme-asset images
add repo weight, need a theme deploy (and therefore an OpCode purge) to change, and depend on the
`render_block` repoint filter — which does not help pages whose content is inline in the DB
(see [[prefer-db-inline-over-pattern-files]]).

**How to apply:** follow [[fast-image-upload-method]] — public URL → `wp_upload_media_from_url`
(cheap); local-only file → resize/WebP first, then `wp_upload_media` base64. Then point the page at the
returned `/wp-content/uploads/...` path.

**Note:** this supersedes the older theme-asset staging step described in [[image-icon-workflow]]
("Stage in the theme git repo under a per-page `assets/images/<slug>/` folder") for NEW page images.
The historical `_iw_src`-tagged asset set described in [[image-webp-media-library]] stays as-is.
