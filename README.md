# Project Memory — IndexWorld block theme (SHARED TEAM MEMORY)

This folder is the **shared, version-controlled memory** for the project. It ships with the
GitHub repo so **every teammate's Claude reads the same context**. Multiple people work here.

## What's inside

```
project-memory/
├── README.md                 ← this file
├── CLAUDE.md                 ← copy of project instructions (live copy is at the repo root)
├── COMPONENTS.md             ← copy of the component catalog (live copy in the theme's docs/)
├── INDEXWORLD_HANDOFF.md     ← project handoff doc
└── memory/                   ← MEMORY.md (the index) + all the memory files  ← THE memory
```

The `memory/` subfolder is the important one — `MEMORY.md` is the index, and each other
`.md` file is one piece of project knowledge (build scope, conventions, how sections were ported, gotchas).

## How the team uses it

1. **Pull the repo** → you automatically get all the memory in `project-memory/memory/`.
2. **Your Claude reads it automatically** — the repo-root `CLAUDE.md` tells Claude that team
   memory lives in `./project-memory/memory/`, so it loads `MEMORY.md` at the start of a session.
   (If it ever doesn't, just say: *"the project memory is in ./project-memory/memory — read it."*)
3. **When Claude learns something worth keeping**, it writes a new `<slug>.md` into
   `project-memory/memory/` and adds a one-line pointer to `MEMORY.md`.
4. **Commit & push** those memory changes so the rest of the team gets them.

## Rules (so the shared memory stays clean)

- The committed `project-memory/memory/` is the **single source of truth**. Don't keep project
  memory in the per-user hidden `~/.claude/...` path — that copy isn't shared with anyone.
- One fact per file. Update an existing file rather than creating a duplicate.
- Always update `MEMORY.md` when you add/rename/remove a memory file.
- Treat memory edits like code: review them in pull requests if you want quality control.
