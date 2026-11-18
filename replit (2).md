# LIORA

A premium platform planned around earning opportunities, digital services,
products, referrals and membership benefits.

## Standing scope

Development is staged and requires specific user instructions for each feature.
The current deliverable is the technical foundation only. Do not independently
build a homepage, authentication, payments, a user dashboard or an admin dashboard.
Do not add dummy content. SEO is a major product priority.

## Run & Operate

- Use the managed `artifacts/liora: web` and `artifacts/api-server: API Server`
  workflows for development; they inject assigned ports and proxy routing.
- `pnpm --filter @workspace/liora run build` — Next.js production build
- `pnpm --filter @workspace/liora run typecheck` — Next route types + TypeScript
- `pnpm --filter @workspace/liora test` — foundation utility tests
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `DATABASE_URL` — managed server-only database setting; never expose to clients
- `SITE_URL` — non-secret canonical production origin, required before building
  actual indexable pages; intentionally not guessed from a preview URL

## Stack

- pnpm workspaces, Node.js, TypeScript
- Frontend: Next.js App Router + React + Tailwind CSS
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/liora` — Next.js frontend; see its README for routing/SEO policies
- `artifacts/api-server` — existing Express backend; owns `/api`
- `lib/api-spec/openapi.yaml` — source of truth for API contracts
- `lib/api-client-react`, `lib/api-zod` — generated API integrations
- `lib/db` — PostgreSQL/Drizzle package; no application schema defined yet
- `artifacts/mockup-sandbox` — separate existing canvas preview, retained

## Architecture decisions

- The user approved Next.js before frontend development because crawlable public
  HTML and flexible static/server rendering are priorities. Retain Express and
  the existing database rather than merging or replacing them during migration.
- No homepage is supplied during foundation work. Root and unknown paths return
  real 404 documents; startup health uses `/robots.txt` instead.
- No public content is approved yet, so the sitemap is empty and metadata defaults
  to noindex. Public pages must explicitly opt in with their own SEO definition.
- Social image metadata uses explicit absolute `SITE_URL` URLs and the shared
  `/share-image` asset endpoint. Automatic image-file metadata resolved to
  localhost during Next development despite metadataBase, so do not substitute
  that convention without checking the resulting HTML URLs.
- Private route groups are organization/cache boundaries, not authentication.
  Add authentication and API authorization before creating private pages.

## Product

No product features are implemented yet. Do not infer future feature details from
the high-level platform description.

## User preferences

Preserve a clean scalable architecture and work one approved stage at a time.

## Gotchas

- Keep `/api` owned by Express; do not introduce conflicting Next API routes.
- Production uses a Node server, not a static SPA fallback. Use the assigned PORT.
- Next development and production output directories are intentionally separate.
- Do not add secrets to `NEXT_PUBLIC_*`, frontend props or Next config `env`.
- The preserved starter CSS still has incomplete design tokens; do not choose a
  brand palette without a separately approved design stage.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
