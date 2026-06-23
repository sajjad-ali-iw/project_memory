---
name: image-slot-aspect-ratios
description: "All component image slots are aspect-ratio + object-fit:cover locked so MCP images can't break layout; supply per-slot ratios"
metadata: 
  node_type: memory
  type: project
  originSessionId: de938308-ec01-4e57-863c-e4f4c6bb456d
---

Fixed 2026-06-23: every reusable component image slot in `components.css` now has a fixed
`aspect-ratio` + `object-fit:cover` (it center-crops any image to fill, so the image's own pixel
size can never change the layout). Previously several slots were `height:auto`, so an MCP-supplied
image of a different shape stretched the section.

**Why:** when generating new pages via MCP, images of arbitrary dimensions were going to break
section layouts (e.g. the same `.ix-ihero__media` showed two different heights for the cannabis 3:2
hero vs the odoo near-square hero).

**How to apply:** when filling an image slot on a generated page, crop the image to roughly the
slot's ratio first. Full ratio + ideal-source-size table is in
`wp-content/themes/indexworld-blocks/docs/COMPONENTS.md` ("C. Image-slot aspect ratios + sizes").
Key ones: split-hero 3:2, services 16:9, industry card 16:11, case-study card ≈1:1,
about people 2:3 (portrait), support feature 16:9.

EXCEPTION — Media+Text (`.ix-iwhat__media`): desktop is NOT ratio-locked; the image stretches to the
text-column height (cover, min-height 360px) so this slot WANTS a tall/long portrait image
(~1100×1500). Mobile falls back to 4:3. Section height is driven by the copy, not the image.

**Size ceiling (every in-page image):** hard max **2000px longest edge** (above it GD's WebP encoder
white-renders the image in-page + bloat — see [[support-page-build]] white-render gotcha); practical
max ~1600px wide (all slots display ≤600px, so 2× retina ≤1200px); floor = the slot's 2×-display
"Fetch source" width or it looks soft. Fetch as **JPG** (PNG if transparency) — pipeline converts to
WebP q90; don't pre-supply large WebP. Ideal source ≈ 2× the slot's on-screen width: e.g. split hero
displays ~543px → fetch 1100×730. Logos: small (~200–400px), transparent PNG/SVG, not ratio-locked.

Known bad image: `/mechanical-industry/` hero `mech-hero.jpg` is 640×612 (near-square + under-sized)
in a 3:2 slot — should be replaced with a 1100×730 JPG.

Slots locked: `.ix-ihero__media`, `.ix-iwhat__media`, `.ix-indhero__media`, `.ix-sup-hero__media`,
`.ix-sup-feat__media`, `.ix-about__photos`, `.ix-aco__media`, `.ix-acta__media` (the ones that were
exposed) — plus the already-locked svc/ihow/csa/indcard/loc/blog/preview slots.

NOT locked (intentional): logos (`.zt-logo`, `.ix-fpartners`, `.ix-rating__logo`), inline blog-body
images (`.ix-post__body img`), and homepage hero cards (`.ix-hero__card`, JS-built, homepage-only).

Related: [[always-supply-images-on-reuse]], [[component-catalog-doc]], [[image-webp-media-library]].
