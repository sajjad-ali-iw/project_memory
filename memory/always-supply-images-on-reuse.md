---
name: always-supply-images-on-reuse
description: "RULE: when reusing any component, fill EVERY image slot with a real page-specific image — never leave it blank/stale"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 1fbae924-d65f-4e27-baae-f096fb774b3d
---

When reusing a component on a new page/section, **every image slot must get a real,
page-specific image** in `assets/images/...`. Never ship an empty `src`, a placeholder, the
previous page's image, or a blank slot.

**Why:** the user has repeatedly caught me reusing components without putting images in
("whenever you try to use it you don't put images"). A section with a missing/broken image is
not done — it reads as half-built.

**How to apply:**
- Treat re-imaging as part of reuse, equal to re-texting. Re-text AND re-image, every time.
- Before calling a section done, confirm every image slot actually loads (screenshot/check the file exists).
- The component most at risk is **Service Tabs** (`.ix-svc` — `services` / `key-benefits` presets):
  it needs **one image per tab** (4–6 images), easy to forget a tab.
- This rule is documented as a house rule in `docs/COMPONENTS.md`, and each component entry there
  lists its **image slots**. See [[support-page-build]] (webp→jpg gotcha for in-page heroes too).
