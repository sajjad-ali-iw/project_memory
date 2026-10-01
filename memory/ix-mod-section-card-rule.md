---
name: ix-mod-section-card-rule
description: "The blanket section[class*=ix-mod-] card rule in components.css silently whitewashes any direct-child div that sets its own inline background, and turns table scroll-wrappers into white cards. Fixed 2026-08-18; read before adding coloured panels to an ix-mod-/ix-cmp- section."
metadata:
  type: reference
---

`assets/css/components.css` (~line 3856, "INJECTED MODULE NORMALISATION") gives bare REST-injected
`<section class="ix-mod-*">` / `ix-cmp-*` blocks the site card treatment. The card rule is

```css
section[class*="ix-mod-"] > div:not([class*="eyebrow"]) { background:#fff !important; border:1px solid #e4ddf5 !important; border-radius:14px !important; padding:22px !important; }
```

**Everything is `!important`, so it beat the inline styles the markup already had.** Two failure modes,
both found live 2026-08-18:

1. **Coloured callouts rendered plain white.** A `<div style="background:#f0ebff;border-left:5px solid #50269E">`
   lost both the lavender fill and the purple left border. Seen on the old odoo-rescue page (2 panels)
   and on **/data-engineering/ (2 panels, and it is still worth eyeballing)**.
2. **Table scroll-wrappers became cards.** A plain `<div style="overflow-x:auto">` around a comparison
   table got wrapped in a white bordered box. Present on **~13 pages** — every page with a comparison
   table (netsuite-to-odoo, quickbooks-to-odoo-migration, odoo-crm, fractional-cfo, bookkeeping…).

**Fixed** in theme commit `f32e951` by adding two opt-outs that follow the file's own idiom (it already
had a `:has(> .iw-card)` exception so a grid wrapper is not treated as a card):

- `:not([style*="background"])` on the card, hover and reduced-motion rules — a div that declares its
  own background keeps it.
- a new `:has(> table)` exception mirroring the `:has(> .iw-card)` one.

⚠️ **Inline `!important` is the only thing that beats it from the markup side**, which is what the
integration-services table wrapper still carries (harmless belt-and-braces, and it also satisfies the
new `:not([style*="background"])` selector). If you ever need a per-page override again, plain inline
styles are NOT enough.

⚠️ **This is a CSS change, so it needs a LiteSpeed CSS/JS purge.** The source file updates on deploy
but pages keep serving the stale combined `/wp-content/litespeed/css/<hash>.css` until purged. See
[[prefer-db-inline-pages]] for why DB edits do not have this problem.
