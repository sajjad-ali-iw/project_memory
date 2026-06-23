---
name: text-color-rule-black-white
description: Site-wide color rule — body text and bullets are always black or white; purple is reserved only for clickable links
metadata: 
  node_type: memory
  type: feedback
  originSessionId: de938308-ec01-4e57-863c-e4f4c6bb456d
---

RULE (owner, 2026-06-23): All body text and bullet/list markers must be **black or white** only.
**Purple (`#50269E` / brand purple) is reserved exclusively for clickable links** — never for plain
text, headings, bullets, or list markers.

**Why:** purple is the site's link/interaction signal. Using it on non-clickable text makes static
copy look tappable and dilutes the affordance.

**How to apply:**
- Body copy, headings, and `<li>` markers → black (dark) on light backgrounds, white on dark/colored
  backgrounds. (Existing tokens: `--text-dark`, `--text-mid`, white on hero/footer.)
- Style purple ONLY on `<a>` links / clickable elements (e.g. `.ix-blog__more`, nav links,
  read-more, in-text links).
- When building or reviewing a component, if any non-link text or bullet is purple, change it to
  black/white. Check `::marker`, custom bullet glyphs, and `list-style` colors too.

Related: [[button-system]], [[no-eyebrow-in-hero]], [[component-catalog-doc]].
