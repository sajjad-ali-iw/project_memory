# Index World — Block Theme Build: Handoff Context

Read this file first. It carries all decisions and design data from the planning session so you can continue without re-asking.

## ⭐ NON-NEGOTIABLE REQUIREMENT: PIXEL-PERFECT MATCH TO STAGING
The rebuilt local site MUST be a **pixel-perfect, 1:1 visual replica** of the staging site (`staging.indexworld.net`). Same fonts, same exact colors, same spacing, same sizes, same layout, same components, same hover/interaction states — on desktop AND mobile/responsive breakpoints.
- "Close enough" is NOT acceptable. Match the real values exactly (font families, weights, line-heights, letter-spacing, paddings, margins, radii, shadows, breakpoints).
- The underlying CODE changes (Elementor → native blocks/patterns), but the RENDERED RESULT must look identical to staging.
- Workflow expectation: build a section → compare it side-by-side against the same section on staging → fix any visual difference → only then move on. Verify visually before declaring a section done.
- When a value is unknown, READ IT from staging (computed CSS / theme files / rendered HTML). Do NOT guess or substitute approximate values.
- Capture exact font files from staging and self-host them; do not fall back to Inter in the final result (Inter is only a temporary placeholder until the real fonts are sourced).

## The goal
Build a fast, **block-native** WordPress theme for Index World (an Odoo implementation partner) with **no page builder** (no Elementor). Design tokens live in `theme.json`; page sections are real **block patterns** stored as files, built so the WordPress 7.0 AI stack can read theme context and assemble pages from the components.

## Environments
- **Local (build + test here):** a fresh, clean WordPress site in the Local app (`indexworld-revamp.local`). WP 7.0, PHP 8.2, MySQL 8.4, nginx. The old local site was deleted to start clean.
- **Staging (DESIGN SOURCE OF TRUTH — read only, do NOT modify):** `staging.indexworld.net`. Currently a **Hello Elementor child** theme. We extract its real design (fonts, colors, CSS, rendered components) and REBUILD them as clean block patterns on local. This is a rebuild, not a literal copy — Elementor markup != block markup.
- Connection to staging is via the `royal-mcp` WordPress connector. Keep it; it's how we read staging.

## Approach decided
- **Path A**: fresh custom block theme (NOT a Hello Elementor child). Clean slate on local, so no Elementor scaffolding to carry.
- Theme slug: `indexworld-blocks`.
- theme.json = single source of truth for tokens. Sections = unsynced block patterns in `patterns/`. Pixel-locked repeated components = custom (preferably **dynamic/server-rendered**) blocks to dodge block-validation errors. Site-wide identical elements = synced patterns.
- Editors can edit: unsynced patterns (free per-page), synced (global), custom blocks (content fields only, markup protected). AI can read theme context + assemble/edit pages on WP 7.0.

## Staging design data ALREADY EXTRACTED (from custom CSS)
- **Brand purple:** `#50269E` (primary). Lighter `#6b3fc4`. Purple bg `#f0ebff`. Purple icon bg `#EDE7FF`.
- **Neutrals:** navy/text-dark `#0d1224`, text-mid `#4a5568`, text-light `#696b6e`, border `#e8eaf0`, white `#ffffff`.
- **Shadow:** `0 24px 80px rgba(13,18,36,.18)`.
- **Radii tokens:** lg `20px`, md `12px`, sm `8px`. Buttons use `0.75rem`.
- **Fonts:** headings (h1–h6) = **"index happy font"** (fallback Inter). Body/paragraph/span = **"AlmadenSans para"** (fallback Inter). Buttons = Inter 600, 1rem, line-height 19.2px.
  - ACTION NEEDED: "index happy font" and "AlmadenSans para" are custom/licensed faces. Find the actual font files in the staging/Local theme folder (or wherever hosted) and self-host them via theme.json fontFace. Until then, fall back to Inter.
- **Buttons:** bg `#50269E`, white text, radius `0.75rem`, padding ~`14px 20px`, Inter 600.
- **Major custom component:** a large **mega-menu** (`.ix-*` classes — `.ix-dropdown`, `.ix-tab-strip`, `.ix-panel`, `.ix-item`, `.ix-footer-bar`, etc.). Two variants: home (white text triggers) and inner pages (dark text triggers). Tabbed dropdown, 4-col link grid, preview card with image, footer bar with icon links. This is the most complex thing to rebuild — likely a custom block or a template part with the CSS ported.
- There's also a custom full-screen loader (`#custom-loader`, white bg, logo max-width 140px) and LiteSpeed lazy-load image fade rules.

## What still needs extracting from staging (NOT yet done)
- The actual **fonts** (files + exact family names/weights).
- The real **page components** beyond the menu: hero, services/Odoo modules grid, stats, testimonials, CTA, footer — read rendered pages to capture structure, copy, and images, then rebuild as patterns.
- The **logo** and brand SVG icons (many in /wp-content/uploads/2024/04/ etc.).
- Color usage in context (which colors for which sections).

## theme.json draft already started (in planning sandbox)
A v3 theme.json was drafted but with PLACEHOLDER colors (indigo/amber) BEFORE the real staging purple was known. **Rebuild theme.json with the real staging tokens above** (purple #50269E primary, the real neutrals, radii 8/12/20, real font families). Don't reuse the placeholder palette.

## Suggested first steps in Claude Code
1. Confirm the local theme folder path: `app/public/wp-content/themes/indexworld-blocks/`.
2. Locate and copy the real font files into `assets/fonts/`.
3. Build `theme.json` with the REAL staging tokens.
4. Build `style.css`, `functions.php` (register `indexworld` pattern category), minimal `templates/` (index, page) + `parts/` (header, footer).
5. Read 2–3 real staging pages via the connector; rebuild each section as a pattern in `patterns/`.
6. Activate theme on local, test in Site Editor, iterate to match staging visually.

## Guardrails
- Never modify staging — read only.
- Keep new pages as drafts; confirm before publish.
- Prefer dynamic blocks for pixel-locked repeated components (avoids block validation errors on AI edits).
