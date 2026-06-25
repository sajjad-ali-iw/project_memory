---
name: no-per-industry-php-patterns
description: RULE — never add a per-industry *-page.php pattern for a new industry page; build new industry pages from the industry-starter scaffold as baked/MCP-managed page content
metadata:
  node_type: memory
  type: feedback
---

**RULE (owner instruction, 2026-06-25): To create a NEW industry page, do NOT add a per-industry `*-page.php` pattern file.** Build it the baked / MCP-managed way instead: start from the canonical scaffold pattern **`industry-starter.php`** (`indexworld/industry-starter`, the 11-section composition mirroring /cannabis-industry/), then re-text and swap images per industry and save the page content via MCP (template = "industry").

**Why:** the project moved off per-industry PHP patterns to a single scaffold + shared section components ([[industries-build]]). Dedicated `chemical-page.php` and `furniture-page.php` were the last two leftovers and were **deleted 2026-06-25** at the owner's request. Keeping per-industry PHP files duplicates the scaffold, drifts from the MCP-managed model, and means every industry needs a code change + deploy instead of a content edit.

**How to apply:**
- New industry page → reference `industry-starter` (or copy its section markup) into a page, edit content, manage via MCP. No new pattern file, no `functions.php` change.
- Only the shared section COMPONENTS (`.ix-ihero / .ix-iccar / .ix-iwhat / .ix-ichal / .ix-ftabs / .ix-keyben / .ix-ihow / .ix-ifaq`) live in code; industry-specific content does not.
- Remember baked pages go through KSES on MCP save — the theme already allows inline SVG + uses `<img>` carousels so icons/arrows/images survive ([[kses-strips-svg-and-bg-baked-pages]]). Deploy theme before recreating.

Related: [[industries-build]], [[kses-strips-svg-and-bg-baked-pages]], [[icon-cards-baked-pages-fix]].
