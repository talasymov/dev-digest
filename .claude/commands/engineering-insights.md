---
description: Capture this session's learnings into INSIGHTS.md, or review/prune the INSIGHTS files
argument-hint: [module] | review [module]
---
Run the `engineering-insights` skill with `$ARGUMENTS`.

- Empty or a module name → **capture mode**. Module empty → infer it from what this session touched.
  Review the session: what worked, what turned out to be a dead end, which decision was made and why,
  which convention or quirk was not guessable from the code, which error repeated. Show me the entries
  before appending them. Capture nothing if the session taught nothing new.
- `review` (optionally followed by a module) → **review mode**: propose deletions of stale entries,
  merges of duplicates, resolution of contradictions, and a split into domain files if a file exceeds
  ~200 entries. Change nothing until I confirm, then make it a separate commit.
