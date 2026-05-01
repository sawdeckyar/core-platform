# Git Workflow

## Branch Strategy

```
main          ← production — auto-deploys to allinoneautoservice.com/portal/
staging       ← pre-production testing
feature/*     ← new features
fix/*         ← bug fixes
```

## Commit Convention

All commits must follow this format:

```
type(scope): description

Examples:
feat(estimates): add vehicle multiplier badge to builder
fix(invoices): email not sending from detail modal
refactor(settings): extract pricing form to separate card
style(nav): update active tab color
docs(schema): add shop_id column documentation
chore(cache): bump SW cache to v6-31
sql(estimates): add parts_cost and labor_cost columns
breaking(auth): replace custom bcrypt with Supabase Auth
```

### Types
| Type | When to use |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `refactor` | Code reorganization, no feature change |
| `style` | CSS/design only |
| `docs` | Documentation |
| `chore` | Maintenance, cache bumps, dependencies |
| `sql` | Database schema changes |
| `breaking` | Requires migration or staff notice |

## Workflow

### New feature
```bash
git checkout main
git pull
git checkout -b feature/appointment-calendar
# ... make changes ...
git add .
git commit -m "feat(appointments): add day/week calendar view"
git push origin feature/appointment-calendar
# Open PR → fill checklist → get review → merge to main
```

### Bug fix
```bash
git checkout -b fix/invoice-email-broken
git add .
git commit -m "fix(invoices): email function not callable from detail modal"
git push origin fix/invoice-email-broken
# Open PR → fill checklist → merge
```

### Hotfix (urgent production fix)
```bash
git checkout main
git pull
git checkout -b fix/urgent-login-broken
# fix it
git commit -m "fix(auth): login screen not loading on Safari"
git push origin fix/urgent-login-broken
# Open PR → merge immediately after testing
```

## Deployment

Merging to `main` auto-triggers GitHub Actions → FTP deploy to Bluehost.

**Never push directly to main.** Always use a PR.

## SQL Migrations

1. Write migration SQL in the PR description
2. Run on **staging Supabase** first
3. Test all affected features on staging
4. Get PR approved
5. Run on **production Supabase**
6. Merge PR

## Rollback

```bash
# See commit history
git log --oneline -20

# Revert last commit
git revert HEAD
git push origin main

# Revert to specific commit
git revert abc1234
git push origin main
```
