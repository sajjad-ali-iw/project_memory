---
name: image-icon-workflow
description: "The complete, current rules for image/icon-only work sessions — scope, sourcing, approval, deployment pipeline, and record-keeping. The authoritative reference; other image/icon memories are historical detail."
metadata:
  type: feedback
---

Established over an images/icons-only session (2026-08). When the user restricts a session to "only images/icons," these are the rules that apply end to end.

## Scope

- Only change images/icons where explicitly asked — never touch layout, design, or structure otherwise. An explicit new request naming a specific page/section (e.g. "check the icons on this page") is in scope for that task even if it's not literally a photo swap. See [[indexworld-images-only-scope-with-named-exceptions]].
- Icons are never generated from scratch. Reuse an icon already in the site's own library first — grep the pattern files' `$xx_ic` arrays by key name (`'receipt' =>`, `'value' =>`, etc.) rather than eyeballing raw SVG path data. Also check other live/baked pages already fixed this session (icon reuse across baked pages is normal, same as photo reuse — see [[image-library-log]]). Only fall back to a hand-picked Lucide (ISC license) SVG when nothing existing fits, and match the target page's stroke-width convention (usually `1.4`) when doing so.

## Sourcing photos

1. Derive search phrases from the actual heading/copy, not generic stock-photo terms. "Person + laptop + chart" reads as generic — pull literal actions/nouns from the section text instead (e.g. "reconciling bank accounts," "checklist audit," not "business analytics").
2. Check [[image-library-log]] first before sourcing, so the same photo doesn't get placed on two pages by accident.
3. Always visually verify a candidate by downloading it and using the `Read` tool — never propose from a text search-result description alone. Descriptions have been wrong before (a "workflow discussion" was actually a gamified-training mockup screenshot; a "Data, BI & AI" match was a stock-trading terminal).
4. Screen for content that reads wrong up close:
   - Stock-trading candlestick charts (unless the topic is literally trading)
   - Car dashboards (Pexels conflates "dashboard" searches with automotive)
   - Visible brand names/logos (Google Analytics UI, "Walls.io," "Firmbee.com," "Western Digital/WD Blue," ChatGPT branding)
   - Readable whiteboard/document text from the wrong domain (a "MARKETING" whiteboard used for a data-modeling step; a political-conflict chart used for a business hero)
5. Low Pexels photo IDs (roughly under 10000) use an old URL scheme — `pexels-photo-<id>.jpeg` 404s. Fetch the photo page and read the real filename from its og:image/src instead.

## Approval

- Present candidates via `SendUserFile` with plain captions — not an Artifact/base64 grid — unless the user explicitly asks to "show me" a side-by-side. (For icon SVGs specifically, the visualize-widget before/after grid has worked well and is the equivalent tool for that content type. See [[image-approval-preview-method]] for how this default evolved.)
- Wait for explicit approval before deploying anything.
- Vague rejections ("try again," no specifics) get resolved by swapping the weakest/most generic candidates for bolder, more literal ones — not a full reshuffle — then re-presenting. When a user gives several rounds of specific feedback in a row ("not that one," "simpler," "no strike-through"), each round narrows the target — track the accumulated constraints rather than resetting the search each time.

## Deployment pipeline (once approved)

**Photos:**
1. Download the full-res (`w=1600`) version of each approved photo.
2. Convert to WebP quality 90 via Local's bundled PHP+GD.
3. Stage in the theme git repo under a per-page `assets/images/<slug>/` folder.
4. `git fetch` + check `git log HEAD..origin/main` for conflicts before every commit (another session edits this repo concurrently).
5. Commit, push, then poll the deployed theme URL for HTTP 200.
6. `wp_upload_media_from_url` to import into the Media Library with a descriptive title/alt text.
7. `wp_replace_in_page` with `dry_run: true` + `expected_count: 1` first, then apply for real — always replace the complete `src="..."` value, never just the filename fragment (a filename-only replace can "succeed" while leaving the old, broken path prefix).
8. Verify by curling the actual resulting image URL for a 200 — grepping the page HTML for the filename substring only proves the string is present, not that the path resolves.

**Icons (inline SVG, no upload step):**
1. `wp_replace_in_page` with `expected_count: 1` per icon, matching the exact current SVG string literally (watch for `/>`  vs `></path>` closing-tag style — baked pages are inconsistent, match what's actually there).
2. Verify by curling the live page and grepping for the `.ix-ichal__icon` spans to confirm the new path data landed in the right card, in order.

## Record-keeping

- Update [[image-library-log]] after every deploy: Library ID, filename, what it depicts, and page/section for photos; the old-icon → new-icon mapping and reasoning for icon swaps — plus a Gotchas entry for anything reusable as a lesson.
- `git fetch`/diff-check the memory repo too before pushing (see [[memory-git-repo]], [[multi-dev-collaboration]]), then commit and push.
