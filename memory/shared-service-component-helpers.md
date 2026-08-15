---
name: shared-service-component-helpers
description: "ix_srv_hero() and ix_trust_bar() in functions.php — one definition each of the service hero and the white trust bar, so Odoo pages don't duplicate markup. Plus which hero/trust-bar component each page family actually uses."
metadata:
  type: project
---

Added 2026-08-15 (theme commits `6506d44`, `e556e4e`) when the owner asked for one hero + one trust bar
shared across the Odoo pages instead of a separate copy per page.

## The helpers (in `functions.php`, next to `ix_badge()`)

- **`ix_srv_hero( array $h )`** — renders the full `.ix-srv-hero` block. Takes
  `title`, `sub` (trusted HTML — may contain `<br>`/`<a>`), `actions[]` (`label`/`href`/`variant`),
  `image` (`src`/`alt`/`w`/`h`), `card` (`label`/`eyebrow`/`items[]`), and optional `grad` for the
  `--grad` variant.
- **`ix_trust_bar( array $args = [] )`** — renders the white `.ix-sup-stats` / `.ix-pstat` stat row that
  sits directly under the hero. Optional `stats[]` of `n` (big line) / `l` (label) / `href` (makes it a
  verified external link). Defaults to the partner stats (Official Partner · 600+ · 150+ · 5.0).

Both return a markup string — `echo` them from a pattern file. Output was verified byte-identical to
the inline markup they replaced before switching any page over.

## Which component each page family uses (surveyed 2026-08-15)

- **Odoo service pages** (implementation, customization, migration, accounting, crm, inventory,
  manufacturing, audit, partner, rescue, hire-odoo-developer, netsuite-to-odoo, ai-agents, pricing):
  `.ix-srv-hero`.
- **ERP consulting pages** (erp-consultant, erp-implementation, erp-integration, erp-migration):
  a different hero, `.ix-ihero ix-ihero--svc`.
- **odoo-training-services** and **odoo-mcp-server**: also on `.ix-ihero`.

## Important caveat

Most Odoo pages keep their content **inline in the DB** (see
[[prefer-db-inline-over-pattern-files]]), and DB content is static HTML — it **cannot call these PHP
helpers**. So today the helpers are available for pattern-based work but are not what most pages
render through. They are kept because they cost nothing and centralise the markup if a pattern is ever
the right tool again.

**What was never the issue:** responsiveness. Both the hero and the trust bar get their responsive
rules from the single `assets/css/components.css` (`.ix-srv-hero` mobile block ~L2700; `.ix-pstat`
2-column grid at `max-width:760px`), which every page shares regardless of where its markup lives.
A "make it responsive everywhere" request is a CSS edit in one place, not a markup refactor.
