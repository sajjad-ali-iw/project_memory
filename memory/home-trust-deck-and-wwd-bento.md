---
name: home-trust-deck-and-wwd-bento
description: "New home sections (trust deck + What-We-Do bento), the pattern-manifest version-bump rule, and the seeded MCP page"
metadata:
  node_type: memory
  type: project
---

Home order since 2026-07-16: hero → case-studies → **trust deck** (`patterns/trust-deck.php`, `.ix-proofdeck*`) → **What-We-Do bento** (`patterns/what-we-do.php`, `.ix-wwd*`) → industries → about → … The old `indexworld/services` tabs section and the old `indexworld/trusted` strip are both retired from the front page (files remain in the repo). Full component notes: docs/COMPONENTS.md #18–#20.

**RULE learned the hard way: any commit that ADDS a pattern file must bump the theme `Version` in style.css.** WP 6.4+ caches the theme's pattern-file manifest keyed to the version; without the bump the new `wp:pattern` blocks render as NOTHING (files execute fine when hit directly — the block just resolves to an unregistered slug). Cost us a broken-looking homepage on 2026-07-16.

Bento rules: links point to PUBLISHED pages only, one link per target across the grid, grid always fills a perfect rectangle. Buttons follow [[button-system]] (chips Almaden 600 radius 12; CTA Large tier). Trust-deck chips/capsule are approved display pills, exempt from the button radius rule. Odoo + QuickBooks marquee logos must load the Media Library colored versions — the theme webp variants are white-on-white invisible.

**Seeded page:** `/odoo-mcp-server/` is created by a one-time seeder in functions.php (content: `inc/mcp-page-content.php`, guard option `ix_mcp_page_seeded`, KSES lifted for the single insert so SVGs + FAQPage JSON-LD survive). It will auto-create on production at the next theme deploy. Post-seed it is a normal WP page; edit it in WP, not the file.

Related: [[button-system]], [[frontpage-db-template-override]], [[component-catalog-doc]].
