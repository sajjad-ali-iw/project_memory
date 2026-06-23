---
name: team-memory-git-repo
description: Project memory now lives in the committed project_memory/ git repo, shared with the team (not the per-user hidden path)
metadata:
  type: project
---

As of 2026-06-23, this project's memory is a **standalone git repo** at
`app/public/project_memory/` (remote: https://github.com/sajjad-ali-iw/project_memory.git, branch `main`).
It is the **shared team source of truth** — multiple people work on the project and pull/push memory via git.

Structure: `project_memory/memory/` holds `MEMORY.md` (index) + all the `<slug>.md` memory files;
`project_memory/` also keeps copies of `CLAUDE.md`, `COMPONENTS.md`, `INDEXWORLD_HANDOFF.md`, and a
`README.md` describing the team workflow.

**Why:** memory used to live only in the per-user hidden path
`~/.claude/projects/<encoded>/memory/`, which isn't shareable. The team needed one synced copy.

**How to apply:**
- Read `project_memory/memory/MEMORY.md` at session start.
- Write NEW memory into `project_memory/memory/` (new `<slug>.md` + a pointer line in `MEMORY.md`) —
  NOT the hidden `~/.claude/...` path.
- After changing memory, `git add/commit/push` from inside `project_memory/` so teammates get it.
- The earlier `project-memory` (hyphen) bundle folder was deleted; only `project_memory` (underscore) is used.
- Pushes use the `indexworld1` GitHub account (a collaborator on the `sajjad-ali-iw`-owned repo).

The repo-root `CLAUDE.md` "Persistent memory (TEAM, in-repo)" section documents these rules so every
teammate's Claude follows them automatically.
