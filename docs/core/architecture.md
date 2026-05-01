# Architecture

## Current Stack (Phase 0–1)

```
Browser
  └── portal/index.html (single HTML file, 11,600+ lines)
        ├── Supabase REST API (direct from browser)
        ├── EmailJS (transactional email)
        └── Edge Function: generate-inspection-summary (Claude Haiku)
```

## Target Stack (Phase 3+)

```
Browser
  └── React + Vite app (Vercel)
        ├── Supabase PostgreSQL (database)
        ├── Supabase Auth (authentication)
        ├── Supabase Storage (photos, documents)
        ├── Resend (transactional email)
        └── Stripe (billing — Phase 4)

Cloudflare (CDN + DDoS + SSL)
GitHub Actions (CI/CD, backups, changelog)
Sentry (error monitoring)
```

## Database

**Project:** `pnvxncomfnbgwpvcxrky.supabase.co`
**Staging:** separate Supabase project (Phase 2)

### Tables
| Table | Purpose |
|---|---|
| `customers` | Customer records |
| `vehicles` | Vehicle records, linked to customers |
| `estimates` | Service estimates |
| `estimate_items` | Line items (parts + labor) |
| `invoices` | Completed invoices |
| `invoice_items` | Line items (parts + labor) |
| `inspections` | DVI inspection records |
| `inspection_items` | Individual inspection check items |
| `appointments` | Appointment scheduling |
| `staff_users` | Staff accounts (bcrypt) → Supabase Auth in Phase 2 |
| `shop_settings` | Shop configuration, pricing strategy |
| `shops` | Multi-tenant shop registry (Phase 2) |

### Key columns added in v6.30
See [CHANGELOG.md](../../CHANGELOG.md) for full SQL migration history.

## Authentication

**Current:** Custom bcrypt (staff_users table)
- Roles: admin, manager, advisor
- Session stored in localStorage

**Phase 2:** Supabase Auth
- Email/password with MFA
- JWT includes `shop_id` claim
- RLS policies enforce data isolation

## Pricing Engine

Located in: `apps/portal/index.html` → Phase 1: `apps/portal/js/pricing.js`

Key constants:
- `VEHICLE_CLASSES` — 7 classes with multipliers (1.0× to 2.2×)
- `AGE_TIERS` — 5 tiers by vehicle age
- `PRESET_SERVICES` — 62 pre-configured services
- `getVehicleMultiplier(make, year)` — combined multiplier

## Multi-tenancy (Phase 2)

Every table gets `shop_id uuid NOT NULL`.
RLS policy pattern:
```sql
CREATE POLICY "shops_isolation" ON invoices
  USING (shop_id = (auth.jwt() ->> 'shop_id')::uuid);
```

## Email

**Current:** EmailJS (`service_89sretb`)
**Phase 3:** Resend API

Templates: estimate email, invoice email, inspection customer report
All HTML templates are inline in the portal — extracted to `/packages/email-templates/` in Phase 1.

## Edge Functions

**generate-inspection-summary**
- Slug: `super-handler`
- Model: Claude Haiku via `ANTHROPIC_API_KEY`
- Called after DVI completion to generate AI summary
