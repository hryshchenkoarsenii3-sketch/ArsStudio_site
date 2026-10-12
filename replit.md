# ARS Studio

Dark-tech landing page for ARS Studio — a Telegram bot development studio. Single-page Russian-language site with neon Telegram-blue branding, glassmorphism cards, typing-animation hero, scroll-reveal sections, and a floating Telegram button.

## Run & Operate

- `pnpm --filter @workspace/ars-studio-web run dev` — run the landing page (port assigned by workflow)
- `pnpm --filter @workspace/ars-studio-ds run dev` — run the design system preview
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite, Wouter, Framer Motion, React Hook Form + Zod
- Design system: `@workspace/ars-studio-ds` (shadcn/Tailwind v4, dark-first tokens)
- API: Express 5 (shared `artifacts/api-server`)
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/ars-studio-web/` — the landing page (preview path: `/`)
- `artifacts/ars-studio-ds/` — design system (preview path: `/ars-studio-ds/`)
- `artifacts/api-server/` — shared Express API (preview path: `/api`)
- `lib/api-spec/openapi.yaml` — API contract source of truth
- `artifacts/ars-studio-ds/tokens.json` — design tokens source of truth (never edit generated files)

## Architecture decisions

- Dark mode is applied globally: `document.documentElement.classList.add('dark')` in `main.tsx`.
- All UI primitives, `cn`, and toast come from `@workspace/ars-studio-ds` — never local copies.
- The index.css only contains `@import "@workspace/ars-studio-ds/styles.css"` plus app-specific overrides; no separate Tailwind import.
- Glassmorphism cards use `rgba(13,24,41,0.6)` + `backdrop-filter: blur(12px)` + neon border.
- Background animation is pure CSS keyframes — no external video dependency.

## Product

ARS Studio landing page — sections: sticky header, hero with typing animation and stat strip, services (4 glassmorphism cards), tech stack badges, portfolio/cases (3 interactive cards), work process timeline (4 steps), contact form with toast feedback, floating Telegram button, footer.

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Run `pnpm install` after adding a new workspace dependency before starting workflows.
- Never hand-edit `artifacts/ars-studio-ds/src/index.css` or `src/generated/tokens.tsx` — edit `tokens.json` and let the dev server regenerate.
- `pnpm run typecheck` (not `build`) is the correct check for artifacts — `build` requires workflow env vars.
