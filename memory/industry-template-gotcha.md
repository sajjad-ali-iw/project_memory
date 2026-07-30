---
name: industry-template-gotcha
description: Industry pages MUST use templates/industry.html — page.html renders WP post-title as duplicate H1 above the hero
metadata:
  node_type: memory
  type: feedback
---

**RULE:** every industry vertical page (`/*-industry/`, `/office-furniture/`, `/print-on-demand/`, etc.) must have `_wp_page_template = industry` post meta set, so it renders via `templates/industry.html` and NOT the default `templates/page.html`.

**Why:** `page.html` contains `<!-- wp:post-title {"level":1} /-->` which outputs the WP page title (e.g. "Office Furniture") as an H1 ABOVE the hero — producing a visible duplicate title above your hero's H1. `industry.html` skips the post-title block:

```html
<!-- templates/industry.html -->
<!-- wp:template-part {"slug":"header","tagName":"header"} /-->
<!-- wp:group {"tagName":"main"} -->
<main class="wp-block-group">
    <!-- wp:post-content /-->        <!-- no post-title! -->
</main>
<!-- /wp:group -->
<!-- wp:template-part {"slug":"footer","tagName":"footer"} /-->
```

**How to apply:** after creating any industry page via MCP (`wp_create_page`), set the template via:
```
wp_update_post_meta(post_id=<id>, key="_wp_page_template", value="industry")
```
Value is `industry` (not `industry.html`) — WordPress adds the extension internally.

**Symptom you missed this:** two H1s on the page — the WP page title on top, then the hero H1 below. Owner spotted this on the Office Furniture page 2026-07-09; fixed by switching template.

Related: [[office-furniture-industry-page]], [[bake-industry-page-recipe]], [[no-per-industry-php-patterns]].
