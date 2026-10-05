# modules/reviews

Runs a review: `run-executor.ts` gathers inputs (diff, agent prompt, repo-intel context) and calls
`@devdigest/reviewer-core`; persists runs, findings, traces (SSE via `/runs/:id/events`).

## Rules
- Strategy is `single-pass` (`constants.ts`) on purpose: map-reduce = one call per file, slower and
  one transient 5xx fails the whole run. Don't switch to `auto` without a spec.
- Grounding is mandatory: findings without a real diff line are dropped, score is recomputed from
  survivors — never trust the model's score.
- Prompt-injection defense is the shared `INJECTION_GUARD` in `reviewer-core/src/prompt.ts`.
  Do not add keyword filters over untrusted PR text.
- Repo-intel context is gated by `REPO_INTEL_ENABLED` **and** the per-agent `repo_intel` flag;
  an unindexed repo silently degrades to diff-only.

## Tests
`server/test/{reviews.it.test.ts,reviews-helpers.test.ts,grounding.test.ts}`
