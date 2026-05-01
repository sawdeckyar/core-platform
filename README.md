# Core Platform — AllinOne (codename)

> Automotive shop management SaaS — built by Phineworks Inc.

## What This Is

A commercial-grade, multi-tenant shop management platform for independent auto repair shops, tire shops, and dealerships. Currently in active development alongside the live production deployment at All In One Tires & Auto Service (Sunnyvale, CA).

## Current State

| Component | Status | Location |
|---|---|---|
| Portal (v6.30) | ✅ Live | `apps/portal/index.html` |
| React migration | 🔜 Phase 3 | — |
| Multi-tenancy | 🔜 Phase 2 | — |
| Commercial launch | 🔜 Phase 4 | — |

## Tech Stack

| Layer | Tool |
|---|---|
| Database | Supabase PostgreSQL |
| Auth | Supabase Auth (Phase 2) |
| Frontend | HTML/JS → React + Vite (Phase 3) |
| Hosting | Bluehost → Vercel (Phase 3) |
| Email | EmailJS → Resend (Phase 3) |
| Payments | Stripe (Phase 4) |
| Monitoring | Sentry |
| CI/CD | GitHub Actions |

## Live Deployment

- **Production:** `allinoneautoservice.com/portal/`
- **Supabase project:** `pnvxncomfnbgwpvcxrky.supabase.co`

## Phases

| Phase | Name | Status |
|---|---|---|
| 0 | Foundation & Tooling | 🔄 In Progress |
| 1 | Code Organization | 🔜 Upcoming |
| 2 | Multi-tenancy + Auth | 🔜 Upcoming |
| 3 | React Migration | 🔜 Upcoming |
| 4 | Commercial Launch | 🔜 Upcoming |

## Quick Start (Portal)

The current portal is a single HTML file. To test locally:
1. Open `apps/portal/index.html` in a browser
2. Login: `admin` / `allinone2026!`

## Docs

- [Architecture](docs/core/architecture.md)
- [Git Workflow](docs/core/git-workflow.md)
- [DB Schema](docs/allinone/schema.md)
- [Pricing System](docs/allinone/features/pricing-system.md)
- [Changelog](CHANGELOG.md)

---

*Phineworks Inc. — Confidential*
