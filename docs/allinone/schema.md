# Database Schema — All In One Tires & Auto

**Supabase project:** `pnvxncomfnbgwpvcxrky.supabase.co`
**Last updated:** May 2026

---

## customers
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| full_name | text | |
| phone | text | |
| email | text | |
| notes | text | |
| created_at | timestamptz | |

## vehicles
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| customer_id | uuid | FK → customers |
| year | text | |
| make | text | |
| model | text | |
| trim | text | |
| vin | text | |
| license_plate | text | |
| mileage | integer | |
| color | text | |

## estimates
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| estimate_number | text | e.g. EST-2026-0042 |
| customer_id | uuid | FK → customers |
| vehicle_id | uuid | FK → vehicles |
| inspection_id | uuid | FK → inspections (optional) |
| subtotal | numeric | |
| tax_rate | numeric | |
| tax_amount | numeric | |
| discount_amount | numeric | |
| discount_reason | text | |
| total | numeric | |
| notes | text | |
| status | text | pending, presented, approved, declined, converted |
| emailed_at | timestamptz | |
| parts_cost | numeric | supplier cost of all parts |
| parts_revenue | numeric | sell price of all parts |
| labor_cost | numeric | tech cost (hrs × $50) |
| labor_revenue | numeric | sell price of all labor |
| gross_profit | numeric | revenue − cost |
| gross_margin | numeric | gross_profit / subtotal × 100 |
| created_at | timestamptz | |

## estimate_items
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| estimate_id | uuid | FK → estimates |
| line_type | text | 'part' or 'labor' |
| description | text | |
| part_number | text | |
| quantity | numeric | |
| unit_price | numeric | sell price |
| unit_cost | numeric | supplier cost |
| markup_pct | numeric | markup percentage |
| hours | numeric | labor hours |
| labor_rate | numeric | $/hr |
| vehicle_class | text | e.g. 'european' |
| vehicle_multiplier | numeric | default 1 |
| age_multiplier | numeric | default 1 |
| total | numeric | |

## invoices
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| invoice_number | text | e.g. INV-2026-0088 |
| customer_id | uuid | FK → customers |
| vehicle_id | uuid | FK → vehicles |
| payment_method | text | Cash, Card, Check, Zelle |
| subtotal | numeric | |
| tax_rate | numeric | |
| tax_amount | numeric | |
| discount_amount | numeric | |
| total | numeric | |
| notes | text | |
| status | text | paid, unpaid, void |
| emailed_at | timestamptz | |
| parts_cost | numeric | |
| parts_revenue | numeric | |
| parts_margin | numeric | |
| labor_cost | numeric | |
| labor_revenue | numeric | |
| labor_margin | numeric | |
| gross_profit | numeric | |
| gross_margin | numeric | |
| created_at | timestamptz | |

## invoice_items
*(same structure as estimate_items)*

## inspections
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| customer_id | uuid | FK → customers |
| vehicle_id | uuid | FK → vehicles |
| inspection_type | text | express, complete, prepurchase |
| summary | text | AI-generated summary |
| mileage | integer | |
| status | text | in_progress, completed |
| created_at | timestamptz | |
| completed_at | timestamptz | |

## inspection_items
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| inspection_id | uuid | FK → inspections |
| section | text | |
| item | text | |
| status | text | good, fair, needs_attention, critical |
| notes | text | |

## shop_settings
| Column | Type | Default | Notes |
|---|---|---|---|
| id | uuid | PK | |
| shop_name | text | | |
| phone | text | | |
| email | text | | |
| address | text | | |
| tax_rate | numeric | 9.75 | |
| labor_rate | numeric | 150 | $/hr charged to customer |
| parts_margin | numeric | 35 | target gross margin % |
| tech_cost_hr | numeric | 50 | internal tech cost $/hr |
| working_days | integer | 7 | days/week |
| target_daily_rev | numeric | 3600 | daily revenue target $ |
| preset_services | jsonb | null | null = use hardcoded defaults |

## staff_users
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| username | text | unique |
| password_hash | text | bcrypt cost-12 |
| role | text | admin, manager, advisor |
| full_name | text | |
| created_at | timestamptz | |

---

## Phase 2 additions
```sql
-- shops table (new)
CREATE TABLE shops (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  name text NOT NULL,
  subdomain text UNIQUE,
  plan text DEFAULT 'starter',
  settings jsonb,
  created_at timestamptz DEFAULT now()
);

-- shop_id on every table
ALTER TABLE customers ADD COLUMN shop_id uuid REFERENCES shops(id);
ALTER TABLE vehicles  ADD COLUMN shop_id uuid REFERENCES shops(id);
ALTER TABLE estimates ADD COLUMN shop_id uuid REFERENCES shops(id);
ALTER TABLE invoices  ADD COLUMN shop_id uuid REFERENCES shops(id);
-- etc.
```
