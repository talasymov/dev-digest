# server — @devdigest/api

Fastify API: repos, PRs, agents, review runs, repo-intel indexing. Port :3001.

## Commands
```sh
pnpm dev                                            # tsx watch
pnpm typecheck
pnpm exec vitest run --exclude '**/*.it.test.ts'    # unit, no Docker
pnpm exec vitest run .it.test                       # integration, needs Docker
pnpm db:generate                                    # after editing src/db/schema/*
pnpm db:migrate && pnpm db:seed
```

## Where things live
| Path | What |
|------|------|
| `src/modules/<name>/` | feature plugin: `routes.ts` → `service.ts` → `repository.ts` |
| `src/modules/index.ts` | static module registration (one import + one `app.register`) |
| `src/platform/` | config, DI container, errors, jobs, SSE, model router, price book |
| `src/adapters/` | ports: llm, github, git, astgrep, secrets, …; `mocks.ts` for tests |
| `src/db/` | Drizzle schema, migrations, seed |
| `src/vendor/shared/` | `@devdigest/shared` Zod contracts (also used by reviewer-core) |
| `test/` | `helpers/pg.ts` = testcontainers Postgres |

## Rules
- Routes declare Zod `params`/`body`/response via `fastify-type-provider-zod`; never hand-parse
  `req.body`. Invalid input → 422 before the handler.
- Throw `AppError` (`src/platform/errors.ts`); the shared handler builds the error envelope.
- External IO only through adapters resolved from `platform/container.ts`.
- A test that imports `test/helpers/pg.ts` **must** be named `*.it.test.ts`.
- New module = `modules/<name>/` + register in `modules/index.ts`. DB schema already has tables for
  all course lessons — check `src/db/schema/` before adding one.

## Gotchas
- `@devdigest/shared` here and in `client/src/vendor/shared` are separate copies — sync both.
- `server/package.json` may be `skip-worktree` locally (see TESTING.md); CI calls vitest directly.
- Global rate limit 120/min is off only when `NODE_ENV=test`.

## Read when relevant
`README.md` (API map, env vars, review context) · `../TESTING.md` · `docs/` · `specs/` · `insights.md`
