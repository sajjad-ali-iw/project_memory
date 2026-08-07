---
name: image-approval-preview-method
description: How to show candidate photos for approval before uploading/deploying — speed over polish
metadata:
  type: feedback
---

Default to plain markdown links (one per candidate, in a table with the phase/section + one-line reasoning) for approval previews, not the Browser pane and not the Artifact-page method (download → resize → base64-embed → publish).

**Why:** asked directly ("what is the more easy way for u to get the images u choosed approved by me?"), the trade-off was laid out — Browser-pane navigation seemed like the zero-setup option (that pane isn't CSP-blocked like the widget/Artifact sandboxes), but user chose speed: "i need speed" (2026-08-07). **Correction the same day:** `mcp__Claude_Browser__navigate` actually fails outright on `pexels.com` — "blocked by policy and cannot be opened in the Browser pane." So the Browser pane isn't usable for this at all; plain clickable links in the chat response (the user opens them in their own real browser) is what's actually fastest and works.

**How to apply:** for future candidate-photo approvals, skip both the Browser pane and the Artifact pipeline — just list each candidate as a markdown link (`[Pexels #id](url)`) with its target section and a short reason it fits, in a table. Reserve the Artifact/base64-grid method only if the user explicitly asks for a side-by-side comparison view. See [[image-library-log]] for tracking what gets approved/placed afterward.
