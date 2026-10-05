---
description: Promote proven entries from INSIGHTS.md into the matching CLAUDE.md
argument-hint: [package]
---
Review `INSIGHTS.md` for `$ARGUMENTS` (empty → root `INSIGHTS.md`).

For each entry not yet marked **Promoted**:
1. Apply the test: "if this line were in CLAUDE.md, would Claude stop making this mistake?"
   Skip entries that a linter/tsc catches, that are standard language rules, or that go stale.
2. Propose a one-line rule for the right file (root, package, or `src/modules/<name>/CLAUDE.md`)
   and section (Gotchas / Rules / Do not touch). Show me the list, wait for confirmation.
3. After confirmation: add the lines, mark entries `**Promoted** → <file> › <section>`.
4. Keep every CLAUDE.md ≤100 lines (packages ~50); if over, suggest what to cut or move to docs/.
