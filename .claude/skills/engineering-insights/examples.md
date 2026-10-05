# engineering-insights — examples

Good/bad pairs for every section of `INSIGHTS.md`. Entries marked **(this repo)** are real facts verified in
DevDigest; entries marked **(article)** are the examples from the MindStudio wrap-up guide used in the
L01 slides — they illustrate the shape, they are not facts about this codebase.

## What Works

❌ "The e2e setup works fine."
✅ **(this repo)**
```markdown
- 2026-09-18 · `./scripts/e2e.sh` runs the flows on its own stack (Postgres :5433, API :3101, web :3100)
  and is safe while the dev stack is up · `npm test` in `e2e/` against the dev DB is not · e2e/README.md
```

## What Doesn't Work

❌ "Running e2e locally is flaky."
✅ **(this repo)**
```markdown
- 2026-09-18 · `cd e2e && npm test` against the dev DB fails flows 02/04/05 · flow 02 follows the home
  redirect to the *first* repo, which is not the seeded `acme/payments-api` once you imported others ·
  use `./scripts/e2e.sh` instead (e2e/README.md › Precondition)
```

## Codebase Patterns

❌ "Be careful with shared types."
✅ **(this repo)**
```markdown
- 2026-09-18 · `@devdigest/shared` is two hand-kept copies — `server/src/vendor/shared` (also used by
  reviewer-core) and `client/src/vendor/shared` — and they have drifted · a contract change in one copy
  type-checks and breaks the other side · `diff -rq server/src/vendor/shared client/src/vendor/shared`
```
✅ **(article)** "Checkout-flow state always goes through Zustand (`cartStore.ts`) — three components share
the cart; local state does not work here."

## Tool & Library Notes

❌ "Promises can be tricky." / "Careful with async."
✅ **(article)** "`Promise.all()` in the ingest pipeline times out past 30 items — use `Promise.allSettled()`
in batches of 10 for this module."
✅ **(this repo)**
```markdown
- 2026-09-15 · the repo needs Node ≥22 while the machine default is v20 · pnpm and `./scripts/dev.sh`
  run under the wrong runtime · prefix `PATH="$HOME/.nvm/versions/node/v22.22.1/bin:$PATH"`
```

## Decisions

❌ "We use INSIGHTS.md." (no reason → the next person "fixes" it back to the course name)
✅ **(this repo)**
```markdown
- 2026-10-05 · the journal is `INSIGHTS.md`, not `LEARNINGS.md` as in the course slides · the skill,
  `/promote-insight` and every CLAUDE.md point to `INSIGHTS.md`; a `LEARNINGS.md` would be a second,
  unread journal · never create `LEARNINGS.md` here
```

## Recurring Errors & Fixes

❌ "Postgres sometimes doesn't work."
✅ **(this repo)**
```markdown
- 2026-09-18 · migrate/seed cannot connect: `devdigest-postgres` is `exited` after a host reboot ·
  Docker Desktop does not restart it · `./scripts/dev.sh` starts the container before migrating
```

## Session Notes

❌ "Worked on docs today."
✅ **(this repo)**
```markdown
- 2026-09-18 · set up the context layout: CLAUDE.md per package, `docs/`, `specs/`, `INSIGHTS.md`,
  `/new-spec` + `/promote-insight` · left open: nothing checks that the two `vendor/shared` copies stay
  in sync
```

## Open Questions

❌ "Should we refactor shared?"
✅ **(this repo)**
```markdown
- 2026-09-18 · enforce the two `@devdigest/shared` copies with a CI diff check, or merge them into one
  source behind the tsconfig alias? · decides whether contract changes need a manual double edit
```

## What NOT to capture

| Candidate | Why it is skipped |
|-----------|-------------------|
| "Review strategy is single-pass, not map-reduce" | already explained in `server/src/modules/reviews/constants.ts` — obvious from the code |
| "Migrations are not applied on API boot" | already in root CLAUDE.md › Gotchas |
| "Use `const` instead of `let`" | a linter rule / standard language practice |
| "Fixed a typo in README" | trivial, no lesson |
| "Set `GITHUB_TOKEN=ghp_…` in `.env`" | contains a secret — never goes into a committed file |

## Sample review-mode proposal

Shape only — `<placeholders>` stand in for real entries, nothing here is a fact about this repo.

```markdown
<module>/INSIGHTS.md — 212 entries, review proposal:

Delete (stale):
- <date> · Tool & Library Notes · "<lib> v3 drops <feature>" — <module>/package.json now pins <lib> v4,
  the quirk no longer applies

Merge (duplicates):
- <date A> + <date B> · Recurring Errors & Fixes · both describe <error> → keep one entry with the fix
  from <date B>

Resolve (contradiction):
- Codebase Patterns <date A> "<rule X>" vs <date B> "<rule not-X>" → <date B> holds (evidence:
  <file:line>); append a dated resolution entry citing both

Split (>200 entries):
- move <n> <domain> entries to `<module>/INSIGHTS-<domain>.md`, link it from `<module>/INSIGHTS.md`

Nothing changed yet — confirm to apply as a separate commit.
```
