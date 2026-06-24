---
name: blog-bold-link-color-gotcha
description: Bold/italic text inside blog-post links renders dark unless components.css line 1583 (.ix-post__body a strong/b/em/i{color:inherit}) is deployed + LiteSpeed purged
metadata:
  type: project
---

Blog post links are usually wrapped in `<a><strong><em>…</em></strong></a>`. The bold/italic
child has its **own** color rule — `.ix-post__body strong, .ix-post__body b{ color:var(--text-dark) }`
(components.css ~1582) — and a child's direct color **always beats the inherited color from the
parent `<a>`** (even an inline `style="color:#50269E"` on the `<a>`). Result: the link text renders
**dark/black instead of purple**, and adding inline color to the `<a>` does NOT fix it.

**The generic fix (already in the theme):** components.css **line 1583** —
`.ix-post__body a strong, .ix-post__body a b, .ix-post__body a em, .ix-post__body a i{ color:inherit; }`
This makes bold/italic text inside post-body links inherit the link's purple. It is **scoped to
`.ix-post__body`** (single blog-post template only) — pages, homepage, header/footer are unaffected.
Plain (non-bold) links are already purple via theme.json global `a{color:primary}`. So NO per-post
inline styles are ever needed — never hand-edit post content for link color.

**Why it looked broken on prelive (2026-06-24):** the fix was committed locally (`7c92c81 Update
Color CSS`) but **not pushed** — local `main` was ahead of origin by 2. prelive's deploy pulls
origin, so it never got line 1583, and its **LiteSpeed combined CSS** kept serving the old rule.
Pushed `7c92c81` to origin/main this session.

**Deploy checklist for any theme CSS change to reach prelive:**
1. Commit + `git push origin main` (theme repo = `wp-content/themes/indexworld-blocks/`, its own git
   repo; deploy webhook pulls origin/main onto prelive).
2. **Purge LiteSpeed cache** on prelive (WP Admin → LiteSpeed Cache → Toolbox → Purge → Purge All).
   There is **no MCP tool to purge LiteSpeed** — the owner must do it in admin. Without the purge,
   prelive serves the stale combined CSS even after deploy.
3. Browser also caches HTML 7 days (`x-litespeed-cache-control: public,max-age=604800`) → hard-refresh
   (Cmd+Shift+R) or incognito to verify.

Stopgap used while waiting on deploy: injected the same rule via prelive Additional CSS
(`wp_update_custom_css`, MCP) — remove it once the theme rule is confirmed live so the theme stays the
single source of truth. Related: [[text-color-rule-black-white]] (purple = links only),
[[multi-dev-collaboration]] (git + deploy), [[mcp-absolute-url-protocol-gotcha]].

⚠️ SECURITY: the theme repo's git remote had a **GitHub PAT embedded in the URL in plaintext**
(`.git/config`). Recommend rotating that token and re-adding origin without the token in the URL
(use a credential helper instead).
