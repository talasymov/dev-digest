# reviewer-core — @devdigest/reviewer-core

Pure engine: diff + system prompt + repo map → prompt → LLM → structured output → grounded findings.

## Commands (npm, not pnpm)
```sh
npm test
npm run typecheck    # this IS the build — the package never emits JS
```

## Where things live
| Path | What |
|------|------|
| `src/index.ts` | public API — everything the server may import |
| `src/prompt.ts` | `assemblePrompt`, `wrapUntrusted`, `INJECTION_GUARD` |
| `src/grounding.ts` | `groundFindings` — citation gate vs the diff |
| `src/llm/` | `LLMProvider` (injected), structured output + repair |
| `src/review/run.ts` | run orchestration (single-pass default, `reduce`) |

## Rules
- No DB, GitHub, filesystem or env access. The only side effect is the injected `LLMProvider`.
- Contracts come from `@devdigest/shared` → `../server/src/vendor/shared` (tsconfig alias).
- Optional prompt slots (`skills`, `memory`, `specs`, `callers`) are empty in the starter; lessons fill them.

## Gotchas
- The server consumes this **source** via alias; breaking `src/index.ts` exports breaks server
  typecheck and CI `server-unit`.

## Read when relevant
`README.md` (pipeline, public API) · `docs/` · `specs/` · `insights.md`
