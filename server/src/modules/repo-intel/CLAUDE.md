# modules/repo-intel

Indexer: clone → walk → ast-grep symbols → import graph → PageRank → cached repo map.
Indexing runs as jobs on clone/fetch; reviews only **read** through the facade.

## Rules
- Consumers use the facade in `service.ts` (`getRepoMap`, `getFileRank`, `getCallerSignatures`,
  `getBlastRadius`, …) — never the pipeline or repository directly.
- Facade must degrade, not throw, for unindexed/partial repos (covered by
  `server/test/repo-intel-facade-degraded.test.ts`).
- Changing the AST extractor or symbol schema → bump `INDEXER_VERSION` in `constants.ts`
  (forces a full reindex).
- Limits (`MAX_INDEXED_FILES`, `INDEX_SOFT_BUDGET_MS` < JobRunner 120s) live in `constants.ts`.

## Read when relevant
`README.md` (pipeline, facade, routes) · pipeline: `pipeline/{full,incremental}.ts`
