---
name: no-eyebrow-in-hero
description: User rule — never place an eyebrow/badge label in a hero section
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 1fbae924-d65f-4e27-baae-f096fb774b3d
---

Never put an eyebrow in a **hero** section. The "eyebrow" is the small badge label above
the heading — the `.badge-wrapper` output by `ix_badge()` (see [[industries-build]] for the helper).

**Why:** the user removed the `ix_badge('Industries')` eyebrow from the Industries hero
(`patterns/industries-hero.php`) and explicitly asked to never use it in a hero again (2026-06-22).

**How to apply:** when building or editing any hero (e.g. `.ix-indhero`, `.ix-hero`, `.ix-bloghero`,
or any `*hero*` section), start straight with the H1/title — do NOT call `ix_badge()` / add a
`__badge` wrapper. Eyebrows are still fine in NON-hero section headers (e.g. the industries
`.ix-indwhy` "Why Index World" badge stays). This rule is hero-only.
