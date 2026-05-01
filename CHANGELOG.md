# Changelog

All notable changes to this project will be documented here.

Format: [Semantic Versioning](https://semver.org)
Auto-generated entries added by release-please on merge to main.

---

## [6.30.0] — 2026-04-30

### Added
- Two-Phase Contribution Pricing engine
- 62-item preset service library with categorized dropdown
- Vehicle class + age tier multipliers (7 classes, 5 age tiers)
- `getVehicleMultiplier(make, year)` — auto-adjusts part pricing by vehicle
- Vehicle pricing badge on estimate/invoice services card
- ↻ Reprice Parts button — recalculates all parts at target margin in one click
- Parts/Labor separated sections on customer-facing estimate email
- Parts/Labor separated sections on print estimate
- Tax clarified as "on parts only" in all customer documents
- Settings → Pricing Strategy card with live margin preview
- Finance tab: Gross Profit, Overall Margin, Labor Margin, Parts Margin stat cards
- Finance tab: Daily Revenue vs Target tracker with progress bar
- Cost tracking stored per estimate and invoice (parts_cost, labor_cost, gross_profit)

### Fixed
- Invoice email function was not callable from detail modal
- iOS Safari confirm dialogs replaced with custom modal

### Database
```sql
ALTER TABLE estimates ADD COLUMN IF NOT EXISTS parts_cost numeric DEFAULT 0;
ALTER TABLE estimates ADD COLUMN IF NOT EXISTS parts_revenue numeric DEFAULT 0;
ALTER TABLE estimates ADD COLUMN IF NOT EXISTS labor_cost numeric DEFAULT 0;
ALTER TABLE estimates ADD COLUMN IF NOT EXISTS labor_revenue numeric DEFAULT 0;
ALTER TABLE estimates ADD COLUMN IF NOT EXISTS gross_profit numeric DEFAULT 0;
ALTER TABLE estimates ADD COLUMN IF NOT EXISTS gross_margin numeric DEFAULT 0;
ALTER TABLE estimate_items ADD COLUMN IF NOT EXISTS vehicle_class text;
ALTER TABLE estimate_items ADD COLUMN IF NOT EXISTS vehicle_multiplier numeric DEFAULT 1;
ALTER TABLE estimate_items ADD COLUMN IF NOT EXISTS age_multiplier numeric DEFAULT 1;
-- (same for invoices / invoice_items)
ALTER TABLE shop_settings ADD COLUMN IF NOT EXISTS parts_margin numeric DEFAULT 35;
ALTER TABLE shop_settings ADD COLUMN IF NOT EXISTS tech_cost_hr numeric DEFAULT 50;
ALTER TABLE shop_settings ADD COLUMN IF NOT EXISTS working_days integer DEFAULT 7;
ALTER TABLE shop_settings ADD COLUMN IF NOT EXISTS target_daily_rev numeric DEFAULT 3600;
UPDATE shop_settings SET preset_services = NULL;
```

---

## [6.29.0] — 2026-04-15

### Added
- Parts/Labor line item toggle on estimates and invoices
- Tax on parts only (California: labor not taxable)
- Separate parts and labor subtotals in totals box
- `line_type` column on estimate_items and invoice_items

---

## [6.28.0] — 2026-04-01

### Added
- Three DVI inspection templates: Express, Complete Safety, Pre-Purchase
- Template picker shown before DVI builder opens
- `inspection_type` saved per inspection to Supabase
- AI summary via Edge Function (Claude Haiku)

---

## [6.27.0] — 2026-03-15

### Added
- Custom `showConfirm()` modal — replaces browser confirm() for iOS/Safari
- Email confirmation dialogs for estimates and invoices
- `emailed_at` timestamp tracked after each send

---

*Earlier versions pre-date this changelog.*
