---
name: logo-variants
description: Which Index World logo to use where (dark vs white)
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

There are two logo variants for the Index World theme:

- **Dark logo** = `Indexworld_logo.png` (user's own, from Drive; 725×176, "iW INDEXWORLD" wordmark in black). This is the LIVE site logo (set via Site Logo block, displayed at width 165 ≈ 40px tall). Theme copy at `wp-content/themes/indexworld-blocks/assets/images/Indexworld_logo.png`. Use on light/white headers (inner pages, current bare header).
- **White logo** = `indexworld-logo.png` (downloaded from staging, 215×40, all white — invisible on white). Staging's HOME header uses this because it sits over a colored hero. Still in the local Media Library, unused for now. Use it when we build the colored/transparent home hero header.

The handoff notes the staging mega-menu has two trigger variants: home = white text, inner = dark text — the header likely swaps logo color by context. When building the home hero, wire the white logo for that header state. See [[fonts-sourced-from-staging]].
