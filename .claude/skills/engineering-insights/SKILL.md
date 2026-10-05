---
name: engineering-insights
description: Captures non-obvious engineering learnings into the INSIGHTS.md of the module the work touched (server, client, reviewer-core, e2e, or the repo root for cross-package findings), and periodically reviews those files to prune stale entries, resolve contradictions and keep them lean. Use at the end of any substantive session (>30 min that involved a problem, a decision, or a discovery), when the user asks to wrap up, run a retro, or capture learnings, mid-session the moment something non-obvious is found, and when the user asks to review or clean up INSIGHTS.md. Only appends; never modifies or deletes existing content without the user's explicit approval.
---

# Engineering Insights

## Overview

Turns what one session learned into notes the next session reads before it starts, so the agent stops
repeating the same mistakes and re-asking the same questions. Each module keeps its own append-only
`INSIGHTS.md` next to its code; the root `CLAUDE.md` makes every session read it first and run this skill
last. The file is a draft under human review, not a source of truth: the skill proposes, the user confirms.

## Approval Rule

**Nothing that already exists is overwritten without the user's explicit approval.** This covers every file the
skill touches (`INSIGHTS.md`, `INSIGHTS-<domain>.md`, any `CLAUDE.md`) and every kind of change to existing text:
editing or deleting an entry, fixing its wording or typos, reordering, merging, moving between sections or files,
adding a `**Promoted**` mark, and rewriting the file as a whole.

- Appending new entries also needs approval: show the exact lines first, write only after a clear "yes".
- Write by inserting lines under a section heading — never regenerate or re-save the whole file.
- Approval covers exactly the lines shown. Anything else that turns up mid-way is a new proposal.
- No answer, an unclear answer, or an approval given in an earlier session is **not** approval — ask again.
- An entry that looks wrong is not fixed in place: propose a dated correcting entry, or a review-mode change.

## When to Use

- End of a substantive session: >30 min that involved a problem, a decision, or a discovery (wrap-up)
- Mid-session, right after something non-obvious happened: a quirk, a dead end, a convention that was
  not guessable from the code (capture as you go)
- The user asks to wrap up, run a retro, capture learnings, or runs `/engineering-insights`
- Review mode: monthly, after a dependency upgrade, or when the user asks to clean up `INSIGHTS.md`

Do **not** use it for trivial edits (config tweaks, typos, renames), for a session that taught nothing
new, or to restate what README / CLAUDE.md already say.

## Quick Reference

| Work touched | Target file |
|--------------|-------------|
| `server/**` (incl. `src/modules/*`) | `server/INSIGHTS.md` |
| `client/**` | `client/INSIGHTS.md` |
| `reviewer-core/**` | `reviewer-core/INSIGHTS.md` |
| `e2e/**` | `e2e/INSIGHTS.md` |
| several packages, scripts, Docker, tooling | root `INSIGHTS.md` |

| Section | What goes there |
|---------|-----------------|
| What Works | approaches and solutions that proved themselves |
| What Doesn't Work | dead ends and antipatterns — most valuable, most often skipped |
| Codebase Patterns | conventions and structure that are not guessable from one file |
| Tool & Library Notes | dependency and tooling quirks, version-specific behaviour |
| Decisions | a choice that was made **and the reason**, so nobody re-litigates it |
| Recurring Errors & Fixes | an error that happened more than once + the fix |
| Session Notes | dated summary of a session: what was done, what is left open |
| Open Questions | what is still unresolved and needs a decision |

Entry format: `- YYYY-MM-DD · what · why it matters · evidence (file:line, command, or error text)`

## Instructions

**Capture mode** (default)

1. Pick the target file from the table above. Several modules touched → one entry per module file, each
   about that module only.
2. Review the session for each section in turn: what worked, what was a dead end, which decision was made
   and why, which quirk or convention was not guessable, which error repeated, what is still open.
3. Draft the entries in the entry format, one fact per entry, with evidence you actually saw this session.
4. Check the target file for an entry the new one contradicts; if found, draft a dated entry that cites the
   old one and states which holds now.
5. Show the drafted entries and wait for explicit approval (see Approval Rule). Then append them under their
   sections; replacing a `_(empty)_` placeholder with the first real entry is part of the approved append.
   Never edit or delete existing entries in this mode.
6. Apply the promotion test to each new entry — "remove this line from CLAUDE.md: would Claude start making
   mistakes?" — and offer `/promote-insight` for the ones that pass. Changing CLAUDE.md and adding the
   `**Promoted** → <file> › <section>` mark to an existing entry are edits: do them only after approval.

**Review mode** (`/engineering-insights review [module]`)

1. Read the whole file(s). Propose: stale entries to delete (fixed bugs, quirks of versions no longer used),
   duplicates to merge, contradictions to resolve.
2. Over ~200 entries → propose a split into domain files (`INSIGHTS-<domain>.md`) linked from the module's
   `INSIGHTS.md`.
3. Change nothing until the user approves, item by item — approving deletions does not approve merges or
   the split. Apply only the approved items and commit them on their own, so a bad review can be reverted.

## Examples

```markdown
## Codebase Patterns
- 2026-09-18 · `@devdigest/shared` is two hand-kept copies — `server/src/vendor/shared` (also used by
  reviewer-core) and `client/src/vendor/shared` — and they have drifted · a contract change in one copy
  type-checks and breaks the other side · `diff -rq server/src/vendor/shared client/src/vendor/shared`
```

❌ "keep the contracts in sync" — true, useless. ✅ the entry above: the next agent knows exactly what to
check and how. More good/bad pairs for every section and a sample review proposal:
[examples.md](examples.md).

## Best Practices

1. **Actionable cold**: an agent that reads only this entry knows what to do, with no follow-up questions.
2. **Not obvious**: if anyone reading the code would see it, don't write it.
3. **One fact per entry**, terse and declarative — written for an LLM to apply, not for a human to enjoy.
4. **Lead with why**: a rule without its reason gets ignored or broken at the first edge case.
5. **Don't skip What Doesn't Work and Decisions** — they save the most time and are the first to be omitted.
6. **Capture as you go**: write the entry when the fix is confirmed, not from memory at the end.

## Constraints and Warnings

- **No overwrite without approval** — see Approval Rule; it applies in both modes and to every file.
- **Append-only** outside review mode — rewriting entries causes merge conflicts and erased lessons in a team.
- **Never invent evidence**: a plausible-looking `file:line` that does not exist poisons the file.
- **No secrets**: no tokens, keys, passwords, or private URLs in entries — the file is committed to git.
- **The model can summarise wrongly**: every entry is a draft until the user confirms it.
- **Not documentation**: architecture and how-tos belong in `docs/` and READMEs; link to them instead.
- **Not a crutch for bad tooling**: if the agent keeps forgetting how to run something, fix the command or
  script, then record the fix.
- **Size**: past ~200 entries per file signal-to-noise drops — run review mode.
- **Manual triggering is unreliable**: this skill depends on being run at the end of a session; automatic
  capture via a Stop hook comes in L06.

## References

- **[examples.md](examples.md)** — good/bad entries for all 8 sections, a full sample block, a sample review proposal
- **[references.md](references.md)** — sources from the research and what each one contributed to this skill
- Session protocol that closes the loop: root `CLAUDE.md` › Session protocol
- Promotion of proven entries into CLAUDE.md: `.claude/commands/promote-insight.md`
