# Branch Strategy

RoundTable Framework uses a **three-tier branch model** to ensure stability, quality, and a clear path from contribution to production.

## Branch Flow

```
  contributor fork
       |
       v
  feature/*  ──>  dev  ──>  staging  ──>  main
  fix/*      ──>  dev  ──>  staging  ──>  main
  docs/*     ──>  dev  ──>  staging  ──>  main
  hotfix/*   ────────────>  staging  ──>  main  (+ cherry-pick to dev)
```

## Branch Definitions

| Branch | Purpose | Stability | Who Merges | Protection Level |
|--------|---------|-----------|-----------|-----------------|
| `main` | Production releases. Published to users. | Stable | Owner (Commander) only | Strict: 1 review + CI + Owner approval |
| `staging` | Pre-release validation and testing. | Near-stable | Maintainer | Moderate: 1 review + CI |
| `dev` | Integration of features, fixes, and experiments. | Development | Maintainer | Light: CI pass |
| `feature/*` | New features in development. | Unstable | Author (via PR to `dev`) | None |
| `fix/*` | Bug fixes in development. | Unstable | Author (via PR to `dev`) | None |
| `docs/*` | Documentation improvements. | Unstable | Author (via PR to `dev`) | None |
| `security/*` | Security improvements. | Unstable | Author (via PR to `dev`) | None |
| `hotfix/*` | Critical production fixes. | Targeted | Author (via PR to `staging`) | None |

## Merge Strategies

| Merge Direction | Strategy | Reason |
|----------------|----------|--------|
| `feature/*` → `dev` | **Squash merge** | Clean single-commit history on dev |
| `fix/*` → `dev` | **Squash merge** | Clean single-commit history on dev |
| `dev` → `staging` | **Merge commit** | Preserve full commit context for review |
| `staging` → `main` | **Merge commit** | Full audit trail for production releases |
| `hotfix/*` → `staging` | **Squash merge** | Single targeted fix |
| `hotfix/*` cherry-pick → `dev` | **Cherry-pick** | Keep dev in sync with the fix |

## Workflow: Standard Feature

```
1. Fork the repository (external contributors)
   or create a branch from dev (maintainers)

2. Create your feature branch:
   git checkout dev
   git pull upstream dev
   git checkout -b feature/my-feature

3. Develop and commit your changes:
   git commit -m "feat(scope): description"

4. Push and create PR to dev:
   git push origin feature/my-feature
   → Create PR targeting 'dev' branch

5. Review and merge (by maintainer):
   → Maintainer reviews → Squash merge to dev

6. Promote dev → staging (by maintainer):
   → Create PR: dev → staging
   → Maintainer reviews and merges

7. Promote staging → main (by Owner):
   → Create PR: staging → main
   → Owner (Commander) reviews and merges
   → Tag release: vX.Y.Z
```

## Workflow: Hotfix (Critical Production Fix)

```
1. Create hotfix branch from staging:
   git checkout staging
   git pull upstream staging
   git checkout -b hotfix/critical-fix

2. Apply the minimal fix:
   git commit -m "fix(scope): critical fix description"

3. PR to staging (not dev):
   → Create PR targeting 'staging' branch
   → Maintainer reviews → Squash merge

4. Promote staging → main (by Owner):
   → Owner reviews and merges
   → Tag patch release: vX.Y.Z+1

5. Cherry-pick to dev:
   → Maintainer cherry-picks the fix to dev
   → Keeps dev in sync
```

## Branch Naming Convention

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feature/` | New functionality | `feature/add-review-skill` |
| `fix/` | Bug fixes | `fix/hook-path-resolution` |
| `docs/` | Documentation changes | `docs/update-getting-started` |
| `security/` | Security improvements | `security/input-validation` |
| `refactor/` | Code restructuring | `refactor/policy-consolidation` |
| `hotfix/` | Critical production fixes | `hotfix/template-version-integrity` |

## Rules

1. **Never push directly to `main`, `staging`, or `dev`.** All changes go through PRs.
2. **Never force-push to protected branches.** Force pushes are disabled on all three tiers.
3. **Always branch from the correct base:**
   - Features/fixes → branch from `dev`
   - Hotfixes → branch from `staging`
4. **Keep branches short-lived.** Merge or close within 2 weeks when possible.
5. **Delete branches after merge.** Keep the branch list clean.

---

*See [CONTRIBUTING.md](../CONTRIBUTING.md) for the full contribution workflow and [GOVERNANCE.md](../GOVERNANCE.md) for merge authority details.*
