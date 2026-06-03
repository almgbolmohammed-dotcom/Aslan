# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Car rental management system (نظام تأجير السيارات) — Arabic RTL app for managing car rentals in Yemen.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod, drizzle-zod
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Frontend**: React + Vite, Tailwind CSS, shadcn/ui, TanStack Query, wouter

## Artifacts

- **car-rental** (`/`) — Arabic RTL frontend app
- **api-server** (`/api`) — Express REST API
- **mockup-sandbox** (`/__mockup`) — Design prototyping sandbox

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)

## Features

### Car Rental App Features
1. **Dashboard** — Summary stats (active rentals, available cars, income, debt) + recent activity
2. **Cars** — Manage fleet with plate numbers, models, and rates (inside/outside Sanaa)
3. **Rentals** — Start/manage/complete active rentals with live timer, multi-step modal
4. **Income** — Track settled income records
5. **Debts** — Track outstanding debts with settle action

### Business Logic
- New rental: pick zone (داخل/خارج صنعاء), confirm/edit daily rate, optional customer name
- Timer tracks 24h cycles → full day billing
- Complete rental: paid → Income table, unpaid → Debt table
- Mid-trip rate/zone update supported
- Settle debt: marks debt settled and creates income record

## Important Notes

- After running codegen, the `lib/api-zod/src/index.ts` must only export from `./generated/api` (not types too — causes conflicts)
- Frontend imports types from `@workspace/api-client-react` (not from relative src paths)
- Use `en-US` locale for number formatting (not `ar-YE` which produces Eastern Arabic numerals)
