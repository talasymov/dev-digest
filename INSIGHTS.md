# INSIGHTS — project-wide

Append-only engineering log for this module. Written by the `engineering-insights` skill
(`/engineering-insights`) and by hand. Never rewrite an entry — correct it with a dated note.
Pruning, merging and splitting happen only in `/engineering-insights review` (monthly), in a separate commit.

Entry format: `YYYY-MM-DD · what · why it matters · evidence (file:line / command)`.
Quality bar: actionable **cold** — an agent reads it and knows what to do, with no follow-up questions.
If it would be obvious to anyone reading the code, don't write it.
Promote a line to CLAUDE.md once it passes: "remove this line — would Claude start making mistakes?"

## What Works

_(empty)_


## What Doesn't Work

_(empty)_


## Codebase Patterns

- 2026-09-18 · `@devdigest/shared` lives as two hand-kept copies — `server/src/vendor/shared` (also used by reviewer-core) and `client/src/vendor/shared` — and they have already drifted (adapters, eval-ci, knowledge, productionize, trace) · a contract change applied to one copy type-checks locally and breaks the other side at runtime · `diff -rq server/src/vendor/shared client/src/vendor/shared`. **Promoted** → CLAUDE.md › Gotchas.


## Tool & Library Notes

- 2026-09-15 · the repo needs Node ≥22 while the machine default is v20 · `pnpm install` and `./scripts/dev.sh` run under the wrong runtime · `PATH="$HOME/.nvm/versions/node/v22.22.1/bin:$PATH"` before any pnpm/script here. **Promoted** → CLAUDE.md › Stack.


## Decisions

- 2026-10-05 · the learnings journal is named `INSIGHTS.md`, not `LEARNINGS.md` as in the course slides · the skill, `/promote-insight` and every CLAUDE.md point to `INSIGHTS.md`; a `LEARNINGS.md` would be a second, unread journal · never create `LEARNINGS.md` here.

## Recurring Errors & Fixes

- 2026-09-18 · `devdigest-postgres` sits in state `exited` after a host reboot, so migrate/seed fail to connect · Docker Desktop does not auto-start it · just run `./scripts/dev.sh`, it restarts the container before migrating.
- 2026-09-18 · `./scripts/e2e.sh` cannot bind Postgres on :5433 · another local container already holds that port · `E2E_PG_PORT=5533 ./scripts/e2e.sh`.


## Session Notes

- 2026-09-18 · set up the context layout: CLAUDE.md per package, `docs/`, `specs/`, `INSIGHTS.md`, `/new-spec` + `/promote-insight`. Open item: no automated check that the two `vendor/shared` copies stay in sync.


## Open Questions

- Should the two `@devdigest/shared` copies be enforced by a CI diff check, or merged into one source?
