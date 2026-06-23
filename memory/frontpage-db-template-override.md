---
name: frontpage-db-template-override
description: "If theme-file edits don't show on the homepage, the Front Page template is a flattened DB copy — Reset it"
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

The homepage stopped reflecting edits to `templates/front-page.html`, `patterns/hero.php`, and new patterns. Cause: the **Front Page template had been edited & saved in the Site Editor**, which writes a *flattened* copy (patterns resolved into static blocks) into the database as a `wp_template` post. That DB copy **overrides the theme's `front-page.html`** entirely, so theme-file edits (and `wp:pattern` references) are ignored — the frozen old markup renders instead (e.g. old hero-with-carousel `ix-hero__carousel` kept appearing).

**Diagnose:** `curl http://indexworld-revamp.local/ | grep -oE 'ix-hero--noscroll|ix-hero__carousel|ix-cases'` — if the served HTML shows markup that exists in NO theme file (`grep -rln` the theme confirms it's not there), the render source is a DB template, not the theme.

**Fix (drove it via Chrome MCP on the local site):** Site Editor → **Templates** → switch layout to **Table** → Front Page row **⋮ Actions → Reset** → confirm "Reset to default and clear all customizations". Reverts Front Page to the theme's `front-page.html`. Fully reversible. The `Reset` action is only enabled when the template is actually customized — its presence confirms the override.

**Takeaway:** keep theme files the single source of truth. Avoid saving templates in the Site Editor; if it happens, Reset. Related: [[section-port-method]], [[header-megamenu-port]].
