---
name: multi-dev-collaboration
description: Multi-developer Git + ChemiCloud collaboration plan decided 2026-06-24
metadata:
  type: project
---

**Decision (2026-06-24):** Move from single-user Local by Flywheel to a multi-developer Git + ChemiCloud setup.

- **GitHub repo:** `indexworld-theme` (private) — theme folder only (`wp-content/themes/indexworld-blocks/`)
- **Shared server:** ChemiCloud (team already has it) — subdomain `prelive.indexworld.net` as shared dev/prelive environment
- **Auto-deploy:** ChemiCloud cPanel Git Version Control + GitHub webhook → every `git push` auto-deploys to ChemiCloud
- **MCP:** `royal-mcp-prelive` connector points to `prelive.indexworld.net` for shared content management
- **Memory in Git:** `project_memory/` folder lives inside the theme repo so all developers share the same Claude context

**Workflow:** Each dev clones the theme, pulls before starting, pushes after changes. Content (pages/media) managed by one person at a time via MCP with team communication. Full plan doc at `docs/COLLABORATION.md` in the theme.

**Why:** Multiple developers can't work simultaneously on one Local by Flywheel instance.
**How to apply:** Before pushing any changes, always `git pull origin main` first. After Claude saves a memory file, commit `project_memory/` and push so teammates get it.
