# engineering-insights — references

Sources from the course research (`engineering-insights-research.md`) and the L01 lab slides, with what each
one contributed and where it landed in this skill. The course calls the file `LEARNINGS.md`; this repo
calls it `INSIGHTS.md` (see root `INSIGHTS.md` › Decisions).

## Core guides on the learnings loop

**MindStudio — Self-Learning AI Skill System with Learnings.md + Wrap-Up Skill** (2026-03-24)
https://www.mindstudio.ai/blog/self-learning-ai-skill-system-learnings-md-wrap-up
- Fixed sections (What Works … Open Questions) → `SKILL.md` › Quick Reference, every `INSIGHTS.md`
- Vague vs useful examples (`Promise.all`, Zustand `cartStore.ts`) → `examples.md` (marked **article**)
- Three ways to trigger wrap-up; manual is unreliable → `/engineering-insights`, Constraints › manual triggering
- CLAUDE.md "Session Context" + "End of Session" → root `CLAUDE.md` › Session protocol
- Common mistakes (inconsistent wrap-up, generic entries, >200 entries, conflicts, skipping What Doesn't Work)
  → Best Practices, Constraints › Size, capture step 4, review mode
- Team mode (append-only, maintainer consolidates, commit to repo) → Constraints › Append-only, review mode
- Cadence (>30 min with a problem/decision/discovery, skip trivia) → When to Use
- FAQ: the LLM can summarise wrongly → Overview "draft under review", capture step 5

**MindStudio — How to Build a Learnings Loop for Claude Code Skills** (2026-03-19)
https://www.mindstudio.ai/blog/how-to-build-learnings-loop-claude-code-skills
- Session Protocol; append, never overwrite, correct with a dated note → capture steps 4–5
- Forced active reading ("confirm you've read it and summarize the top 3") → root `CLAUDE.md` › Session protocol
- LEARNINGS ≠ CLAUDE.md, ≠ chat replay → Constraints › Not documentation, Best Practices › one fact per entry

**MindStudio — Compounding Knowledge Loop in Claude Code**
https://www.mindstudio.ai/blog/compounding-knowledge-loop-claude-code
- Hook types; Stop hook as the capture point → Constraints › manual triggering (deferred to L06)

**MindStudio — Self-Learning Claude Code Skill with Learnings.md**
https://www.mindstudio.ai/blog/self-learning-claude-code-skill-learnings-md
- Plain markdown, no RAG/vectors; structured context beats re-deriving → Overview

**MindStudio — Self-Evolving Claude Code Memory with Obsidian + Hooks**
https://www.mindstudio.ai/blog/self-evolving-claude-code-memory-obsidian-hooks
- Capture categories Patterns · Mistakes · Decisions · Context → the **Decisions** section

**MindStudio — What Is Claude Code Auto-Memory**
https://www.mindstudio.ai/blog/what-is-claude-code-auto-memory
- What is worth keeping (commands, conventions, decisions, env quirks); review early → Quick Reference, capture step 5

## Self-improving CLAUDE.md

**dev.to / Aviad Rozenhek — Self-Improving AI: One Prompt That Makes Claude Learn From Every Mistake**
https://dev.to/aviad_rozenhek_cba37e0660/self-improving-ai-one-prompt-that-makes-claude-learn-from-every-mistake-16ek
- Lead with why, concise, NEVER/ALWAYS → Best Practices 3–4; compounding into rules → capture step 6 (promotion)

**dev.to / evoleinik — CLAUDE.md: Building Persistent Memory for AI Coding Agents**
https://dev.to/evoleinik/claudemd-building-persistent-memory-for-ai-coding-agents-5322
- One-line entries (Prisma Accelerate 5 MB limit) → entry format
- Add after the fix is confirmed; monthly prune of fixed bugs/duplicates → Best Practices 6, review mode
- Not a replacement for docs; not a crutch for bad tooling → Constraints

## Anthropic

**Lessons from building Claude Code: How we use skills**
https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills
- Skills are folders (scripts, assets, data), not one markdown file → `SKILL.md` + `examples.md` + `references.md`

**Skill authoring best practices**
https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
- `description` is the discovery interface: what it does + when to use it, third person → frontmatter
- The guide recommends gerund-form names; the course fixes the name `engineering-insights`, so it is kept

## Existing implementations

**glebis/claude-skills — retrospective** — https://github.com/glebis/claude-skills
- Per-session retro that extracts learnings → capture mode

**mcpmarket — Lessons Learned (AI Development Retro)** — https://mcpmarket.com/tools/skills/lessons-learned-retrospectives
- Reject generic platitudes, keep actionable transferable knowledge → Best Practices 1–2, `examples.md` › What NOT to capture

**mcpmarket — CLAUDE.md Lessons Manager** — https://mcpmarket.com/tools/skills/claude-md-lessons-manager
- Duplicate detection and rule consolidation → review mode

**omega-memory / Omega — real-world report (r/ClaudeAI)**
- 10–15 min per session re-explaining architecture; decisions like "PostgreSQL for ACID, not Redis" → the case for **Decisions**

## Context management

**MindStudio — Code Scripts vs Markdown Instructions** (2026-04-01) —
https://www.mindstudio.ai/blog/claude-code-skills-code-scripts-vs-markdown-instructions
**MindStudio — Skills vs Hooks** (2026-04-30) — https://www.mindstudio.ai/blog/claude-code-skills-vs-hooks-difference
**MindStudio — Context Compounding Explained** — https://www.mindstudio.ai/blog/claude-code-context-compounding-explained
- Hooks are called by the system, not by Claude → why L06 moves capture to a Stop hook
- CLAUDE.md is a fixed-size input; INSIGHTS.md is read on demand → per-module files, not one big file

## L01 lab slides

- Slide 6: per-module files, append-only, 5–8 lines + YAML → target-file table; the body grew past 8 lines
  once the control rules from slide 10 were added
- Slides 7–11: sections, "concrete, not banal", double trigger, control, closing the loop → as mapped above
- Slide 14 (exit checklist): the skill must already have recorded lessons from the rest of the lab
