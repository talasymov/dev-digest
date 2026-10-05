# insights — project-wide

Running log of pitfalls found while working here. Not auto-loaded.
Format: `YYYY-MM-DD · what happened · cause · what to do`.
Promotion test: "if this line were in CLAUDE.md, would Claude stop making the mistake?" — yes → `/promote-insight`.

## Log

- 2026-09-15 · default `node` is v20, project requires ≥22 · nvm default not switched · run with Node 22 (nvm). **Promoted** → root CLAUDE.md › Stack.
- 2026-09-18 · `devdigest-postgres` was `exited` after reboot · container not auto-started by Docker Desktop · `./scripts/dev.sh` restarts it; no action needed.
- 2026-09-18 · `client/src/vendor/shared` ≠ `server/src/vendor/shared` (adapters, eval-ci, knowledge, productionize, trace) · two hand-kept copies · change both; consider a sync check. **Promoted** → root CLAUDE.md › Gotchas.
- 2026-09-18 · hermetic e2e wants Postgres on :5433 · port may already be taken by another local container · set `E2E_PG_PORT`.
