---
name: fast-image-upload-method
description: "THE fast, low-token way to get an image into the prelive Media Library — always try a public URL first (~30 tokens); base64 costs ~140x more. Decision table by image source, plus the SVG and data-URI limits."
metadata:
  type: feedback
---

Established 2026-08-15 after the owner called out that image uploads were sometimes instant and
sometimes burned huge numbers of tokens: *"every time you consume tones of token can we save the
fastest way… we need to streamline this process."* This file is the streamlined process. Read it
BEFORE uploading any image. Companion to [[image-icon-workflow]] (which covers scope, sourcing and
approval); this file is specifically about the *upload mechanics*.

## The one number that decides everything

For a ~24 KB WebP:

| Method | Cost | Notes |
|---|---|---|
| `wp_upload_media_from_url` (public URL) | **~30 tokens** | ALWAYS prefer |
| `wp_upload_media` (`content_base64`) | **~4,200 tokens** | ~140× worse; last resort |

Base64 must pass through the model's context, so cost scales with file size (base64 chars ≈ bytes × 1.37,
tokens ≈ chars ÷ 4). A 1 MB PNG inlined is ~340k tokens — never do it. Additional trap: reading a
large `.b64` file back with `Read` blows the 25k-char cap and wastes a whole turn for nothing.

## Decision table — pick by where the image comes from

1. **Image already has a public URL** (Unsplash/Pexels/CDN, or a generator's result URL)
   → `wp_upload_media_from_url` with that URL directly. Done, ~30 tokens.
   Add resize/crop/format params in the URL so no local processing is needed:
   - Unsplash: `?w=1000&h=667&fit=crop&fm=webp&q=80`
   - Any other public URL, convert + resize via the free weserv proxy:
     `https://images.weserv.nl/?url=<HOST/PATH no scheme>&w=1000&output=webp&q=80`
     (This is how the step-1 brains PNG went from 1.2 MB → 34 KB WebP in ONE call.)

2. **Image was generated locally / only exists on disk** (hand-authored SVG, Chrome screenshot,
   GD output) → **shell-upload it to a temp host, then upload from that URL.** ~0 tokens, no git, no
   deploy wait. **Verified end-to-end 2026-08-15** (12,666-byte WebP arrived byte-identical):

   ```bash
   curl -s -F "reqtype=fileupload" -F "fileToUpload=@out.webp" https://catbox.moe/user/api.php
   # -> https://files.catbox.moe/xxxxxx.webp
   ```
   then `wp_upload_media_from_url` with that URL.

   Two gotchas:
   - The first attempt was **blocked by the Claude Code permission classifier**; it succeeded on
     retry after the owner asked for it explicitly. If it gets blocked, say so and ask — don't
     silently fall back to base64.
   - `files.catbox.moe` may **fail DNS from this machine** (curl exit 6). That does not matter —
     `wp_upload_media_from_url` is fetched by the **prelive server**, not locally. Don't treat a local
     curl failure as "the URL is dead"; just do the upload.

3. **Fallback if the temp host is unavailable/blocked — git as the transfer** (owner's own suggestion):
   commit the file to the theme repo → push → it is publicly served at
   `https://prelive.indexworld.net/wp-content/themes/indexworld-blocks/<path>` (verified 200) →
   `wp_upload_media_from_url` → then `git rm` it. ~0 image tokens; price is a push + deploy wait.

4. **Last resort, tiny files only (<30 KB)** → `wp_upload_media` + `content_base64`.
   Minimise the damage first: resize to ≤1000 px wide and WebP q78–82 (usually 12–35 KB), then emit
   the base64 with a single `base64 -i file | tr -d '\n'` and paste it straight into the tool call.
   Do NOT write it to a file and `Read` it back.

## Routes tested 2026-08-15 (so nobody re-derives this)

| Route | Image-byte cost | Verdict |
|---|---|---|
| Public URL → `wp_upload_media_from_url` | ~30 tokens | ✅ best |
| Git push → deployed theme URL → `upload_from_url` | ~0 tokens | ✅ best for local-only files |
| Shell `curl` upload to catbox.moe → URL | ~0 tokens | ✅ **VERIFIED WORKING** — fastest for local files (no git, no deploy wait). Was blocked by the permission classifier on first try, succeeded on retry |
| `wp_upload_media` `content_base64` | ~4,200 per 24 KB | ⚠️ last resort |
| `data:` URI into `upload_media_from_url` | — | ⛔ rejected |
| SSH / wp-cli / ngrok-style tunnel | — | ⛔ none installed (no ssh config, no wp-cli, no tunnel tools) |
| GitHub raw URL | — | ⛔ theme repo is private (404) |

## Hard limits discovered (do not retry these)

- **SVG upload is rejected** by the prelive endpoint: `Error: File type .svg is not allowed.`
  (Even though the theme's `functions.php` enables SVG for the WP admin uploader — the MCP endpoint
  has its own allow-list.) → Rasterize SVG to PNG, then convert to WebP, then upload.
- **`data:` URIs are rejected** by `wp_upload_media_from_url`: `Error: A url is required.`
  So a data URI cannot be used to dodge the base64 cost.
- **LiteSpeed does not auto-serve WebP** for a PNG in the Library — a PNG stays a 1.2 MB PNG on the
  wire. Convert before uploading; don't rely on the server.

## Rasterizing SVG here (no ImageMagick/rsvg/inkscape installed)

Chrome headless is available and works:

```bash
printf '%s' '<!doctype html><meta charset="utf-8"><style>html,body{margin:0;padding:0}svg{display:block}</style>' > wrap.html
cat art.svg >> wrap.html
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu \
  --hide-scrollbars --force-device-scale-factor=1 --window-size=1000,667 \
  --screenshot="out.png" "file://$PWD/wrap.html"
```

Then PNG → WebP with Local's bundled PHP+GD (see [[image-webp-media-library]] for the q90 recipe;
for illustrations q78–82 at 1000 px is plenty and keeps base64 small).

## After upload (unchanged)

`wp_replace_in_page` with `dry_run: true` + `expected_count: 1` first, replacing the COMPLETE
`src="..."` value; then verify by curling the resulting image URL for a 200. See [[image-icon-workflow]].

## Cleanup

If a heavier interim version was uploaded (e.g. the PNG before the WebP), delete it with
`wp_delete_media` once the final one is wired in — but only with the owner's go-ahead, since it is a
permanent delete.
