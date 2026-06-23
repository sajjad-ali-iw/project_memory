---
name: fonts-sourced-from-staging
description: The two custom theme fonts and where they came from
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

Index World theme fonts (self-hosted in `wp-content/themes/indexworld-blocks/assets/fonts/`, wired into theme.json fontFace, verified loading on local 2026-06-20):

- Headings = family **"index happy font"** → file `HappyDisplay-SemiBold.woff` (weight 400, normal)
- Body = family **"AlmadenSans para"** → file `AlmadenSans-Book-WebXL.woff2` (weight 400, normal)
- Buttons + fallback = Inter

Both were downloaded directly from public staging at `https://staging.indexworld.net/wp-content/uploads/2026/02/`. Staging exposes its `@font-face` rules and assets publicly, so brand assets (logo, icons, fonts) can be fetched over plain HTTP without the royal-mcp connector. Useful when the connector is down. See [[working-style-beginner-pacing]].
