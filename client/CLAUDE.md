# client — @devdigest/web

Next.js 15 studio (App Router, React 19). Port :3000. Talks to the API via `NEXT_PUBLIC_API_BASE`.

## Commands
```sh
pnpm dev
pnpm test          # vitest + jsdom, fetch mocked — no API needed
pnpm typecheck
```

## Where things live
| Path | What |
|------|------|
| `src/app/**/page.tsx` | thin routes; feature UI in colocated `_components/<Name>/` |
| `src/lib/api.ts` | the only fetch client |
| `src/lib/hooks/*` | every data hook (TanStack Query) |
| `src/components/app-shell` | nav, breadcrumbs, `g`-then-key shortcuts |
| `src/vendor/ui` | `@devdigest/ui` design system |
| `src/vendor/shared` | `@devdigest/shared` contracts (client copy) |
| `messages/<locale>/*.json` | next-intl strings |

## Rules
- Import UI only from `@devdigest/ui` (barrel), never from a layer file.
- Data access only via a hook in `src/lib/hooks` → `src/lib/api.ts`; no ad-hoc `fetch` in components.
- User-facing strings go to `messages/`, not inline.
- Each `_components/<Name>/` has its own `*.test.tsx`.

## Gotchas
- `src/vendor/shared` is a **copy** of `server/src/vendor/shared` and has drifted — when a contract
  changes, update both.

## Read when relevant
`README.md` (route map → API) · `src/vendor/ui/README.md` · `docs/` · `specs/` · `insights.md`
