# e2e — @devdigest/e2e

Deterministic browser flows via the agent-browser CLI. No Playwright, no LLM, no keys.

## Commands (npm)
```sh
../scripts/e2e.sh        # hermetic: own Postgres :5433, API :3101, web :3100 — preferred
npm test                 # against running dev stack — only if DB has just the seeded repo
```

## Rules
- A flow is `specs/NN-name.flow.json`; each `cmd` is passed verbatim to agent-browser.
- Assertions = `wait --text` / `wait --url` (+ optional `assert.stdoutIncludes`).
- Deterministic locators only (`--url`, `--text`, `find role|text|label`); never the AI `chat` command.
- Flows use read-only seeded data (`acme/payments-api`, PR #482) — nothing may trigger a model call.

## Gotchas
- Against a dev DB with other repos, flows 02/04/05 land on the wrong repo → use the hermetic runner.

## Read when relevant
`README.md` (flow format, env knobs, coverage table) · `insights.md`
