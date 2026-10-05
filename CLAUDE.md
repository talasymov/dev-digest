# DevDigest

Local-first AI PR review: import PR → `reviewer-core` (diff + repo map → LLM → grounded findings).

## Stack
- Node **22** (default v20 is too old), pnpm in `server/` + `client/`, npm in `reviewer-core/` + `e2e/`
- server: Fastify 5 · Drizzle · Postgres 16 + pgvector (Docker) · Zod
- client: Next.js 15 (App Router) · React 19 · TanStack Query · next-intl
- tests: Vitest · testcontainers (server `*.it.test.ts`) · agent-browser (e2e)

## Commands
```sh
./scripts/dev.sh                 # docker → migrate → seed → API :3001 + web :3000
./scripts/dev.sh --db-only       # Postgres + migrate + seed only
./scripts/e2e.sh                 # hermetic e2e on :5433/:3101/:3100
```

## Where things live
| Path | What |
|------|------|
| `server/` | Fastify API, DB, repo-intel, DI adapters |
| `client/` | Next.js studio |
| `reviewer-core/` | pure review engine, no IO except the injected LLM |
| `e2e/` | deterministic browser flows, no LLM |
| `docs/` | long-form docs, agent prompts, ADRs |
| `specs/` | feature specs (one per feature/lesson) |

## Non-default conventions
- **No monorepo workspace.** Each package has its own lockfile; cross-package code goes through
  tsconfig path aliases (`@devdigest/shared`, `@devdigest/reviewer-core`), never published packages.
- Don't mix package managers between packages (see Stack).
- Zod contracts in `@devdigest/shared` are the single source for route validation, response
  serialization and client types. Change the contract first, then code.
- Hermetic by default: LLM/GitHub/git go through adapters; tests use `server/src/adapters/mocks.ts`.

## Gotchas
- `@devdigest/shared` exists as **two copies**: `server/src/vendor/shared` (also used by reviewer-core)
  and `client/src/vendor/shared`. They have already drifted. A contract change must land in both.
- Migrations are **not** applied on API boot → `cd server && pnpm db:migrate`.
- The API imports reviewer-core **source**; without `reviewer-core/node_modules` it crashes with
  `ERR_MODULE_NOT_FOUND` → `cd reviewer-core && npm ci`.
- Secrets live in `~/.devdigest/secrets.json` (Settings UI) with `process.env` fallback — not in DB/git.

## Do not touch
- Applied migrations in `server/src/db/migrations/*.sql` — add new ones via `pnpm db:generate`.
- `docker compose down -v` — deletes the `devdigest_pgdata` volume with all imported repos.
- `server/clones/` — runtime data (git-ignored).

## Session protocol
- **Before working in a module, read its `INSIGHTS.md`** (and the root one for cross-package work).
  Treat it as high-confidence guidance unless I say otherwise. Confirm you read it by naming the
  3 entries most relevant to the task (or saying it is empty) before the first change.
- **At the end of the session, run `/engineering-insights`** to append what was learned. Do not skip it;
  if nothing non-obvious came up, say so instead of inventing entries.

## Read when relevant (not auto-loaded)
- Architecture & review flow: `README.md` · test strategy: `TESTING.md`
- Agent system prompts & model choice: `docs/agent-prompts/README.md`
- New feature: write `specs/<id>-<name>.md` from `specs/_template.md` first (`/new-spec`)
