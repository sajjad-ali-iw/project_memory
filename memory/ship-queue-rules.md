---
name: ship-queue-rules
description: "Hard rules for SHIP-CHANGES.json queue authors, learned from a failed commit and a near-double-apply"
metadata:
  node_type: memory
  type: project
---

Rules for anyone queueing edits in `SHIP-CHANGES.json` (applied by SHIP.bat → SHIP.ps1):

1. **Never put double quotes in the `message` field.** Windows PowerShell 5.1 splits `git commit -m "$msg"` on embedded quotes and git treats the rest as a pathspec — this killed a run at the commit step on 2026-07-29 with all edits already applied. SHIP.ps1 now sanitizes quotes out of the message, but do not rely on the net.

2. **Prefer self-extinguishing finds.** The apply loop skips an entry only when its `find` string is GONE. An append-style entry (`find` = anchor, `replace` = anchor + new content) leaves the find alive — so any re-run after a mid-flight failure (commit or push failing) applies it AGAIN and duplicates the content. Build finds that include the following element's opening bytes, so the replace breaks the adjacency and the find dies. When an anchor-style entry is unavoidable, note it in the message and never re-run a failed batch without checking the tree first.

3. **Content entries (`content` key) overwrite whole files.** Tracked files skip the new-pattern size/php checks — the entry is trusted as-is, so validate the full file yourself before queueing. Untracked new `patterns/*.php` files DO get checked (must start `<?php`, ≥1500 chars) and trigger the style.css Version bump (see [[home-trust-deck-and-wwd-bento]] for why the bump matters).

4. Sequential entries may anchor on text a previous entry inserts — the loop applies in order on the same tree — but a pristine-file anchor is safer if the batch could ever be re-run partially.

Related: [[multi-dev-collaboration]], [[home-trust-deck-and-wwd-bento]].
