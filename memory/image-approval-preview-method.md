---
name: image-approval-preview-method
description: How to show candidate photos for approval before uploading/deploying — speed over polish
metadata:
  type: feedback
---

Default to navigating the Browser pane directly to each Pexels candidate URL for approval previews, not the Artifact-page method (download → resize → base64-embed → publish).

**Why:** asked directly ("what is the more easy way for u to get the images u choosed approved by me?"), the trade-off was laid out — Browser-pane navigation costs nothing extra (that pane isn't CSP-blocked like the widget/Artifact sandboxes, so it loads external Pexels URLs as-is) but loses the labeled side-by-side grid. User chose speed: "i need speed" (2026-08-07).

**How to apply:** for future candidate-photo approvals, `navigate` the Browser pane to each Pexels photo URL (or a Pexels search-results page showing several at once) rather than building an Artifact preview page. Reserve the Artifact/base64-grid method only if the user explicitly asks for a side-by-side comparison view. See [[image-library-log]] for tracking what gets approved/placed afterward.
