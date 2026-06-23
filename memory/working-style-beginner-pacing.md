---
name: working-style-beginner-pacing
description: How the user wants to be guided through the Index World theme build
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

The user wants to be guided like a beginner on the Index World block-theme build: one small action at a time, told exactly what to click/paste/drag, and waited for before moving on. The goal is a PIXEL-PERFECT match to staging (staging.indexworld.net), spec'd in `wp-content/themes/INDEXWORLD_HANDOFF.md`.

**Why:** User is not a developer and is driving a real rebuild via Claude as the driver.

**How to apply:** Do the file/tool work yourself; reserve the user's actions for things only they can do (browser admin, dragging files, visual confirmation). Break their actions into tiny numbered steps and stop. Prefer fetching assets from staging directly (it's a public site) over asking the user to source them. See [[fonts-sourced-from-staging]].
