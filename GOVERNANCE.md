# RoundTable Framework Governance

This document describes the governance model for the RoundTable Framework project. It defines how decisions are made, who has authority, and how contributors can grow within the project.

## Built & Used in Production

> RoundTable Framework is not a theoretical exercise. It is the **actual governance system** used by [Unicorn Tech Integration Co., Ltd.](https://github.com/VarakornUnicornTech) to build production software with Claude Code. The framework you see here is the same one that orchestrates our AI development teams daily — a human Commander directs the work, and the RoundTable team structure (Overseer, Monolith, Syndicate, Arcade, Cipher) executes under this framework's policies and quality gates.
>
> **When you use RoundTable, you're using the same tool we use. When you contribute, you're improving the tool we depend on.**

## Project Leadership

### Owner — Commander

The **Commander** (project owner) is the highest authority in the RoundTable Framework project.

- Final authority on all decisions — technical, organizational, and strategic
- Sole merge authority for the `main` branch
- Controls repository settings, access, and team membership
- Can veto any decision made by maintainers
- GitHub: [@VarakornUnicornTech](https://github.com/VarakornUnicornTech)

### Principal Manager — AM (AstonMartin)

The **Principal Manager** handles day-to-day project operations within the framework itself.

- Coordinates PR reviews and community interaction
- Merge authority for `dev` and `staging` branches
- Manages issue triage and milestone planning
- All work is presented to the Commander for final approval
- Operates within the RoundTable Overseer team structure

### Maintainers

Maintainers are trusted contributors with merge access to `dev` and `staging`.

- Review and merge community PRs
- Triage issues and manage labels
- Respond to security reports
- Follow the governance rules in this document
- See [MAINTAINERS.md](MAINTAINERS.md) for current list

## Decision Making

### Lazy Consensus (Minor Changes)

For small, non-controversial changes (typo fixes, documentation improvements, minor bug fixes):
- A single maintainer approval is sufficient
- If no objection is raised within the review period, the change is accepted

### Explicit Approval Required (Significant Changes)

The following changes require **explicit Commander approval**:

| Change Type | Why |
|-------------|-----|
| Core framework changes (`.claude/CLAUDE.md`) | This is the AI's brain — changes affect all users |
| Policy amendments (`.claude/policies/`) | Governance rules affect the entire workflow |
| New agent or skill definitions | Extends the framework's capabilities |
| Breaking changes (any path/API change) | Affects all existing users |
| Governance document changes | Changes the rules themselves |
| Security policy changes | Affects trust and safety posture |
| Release tagging on `main` | Production impact |

### Commander's Final Authority

- The Commander's decision is final and binding
- Once a decision is made, the team executes without further debate
- The Commander may delegate specific decisions to maintainers on a case-by-case basis

## Contributor Tiers

Contributors grow through a trust-based ladder. Progression is earned through consistent, quality contributions.

| Tier | Role | Permissions | How to Earn |
|------|------|------------|-------------|
| **1** | **Contributor** | Fork, submit PRs, comment on issues, participate in discussions | Create a GitHub account and submit your first PR |
| **2** | **Trusted Contributor** | Assigned issues, advisory PR reviews, `good first issue` recommendations | 3+ merged PRs with consistent quality, active for 1+ months |
| **3** | **Reviewer** | Binding review approval on non-core files | Invitation by maintainer, 10+ quality reviews, demonstrated domain expertise |
| **4** | **Maintainer** | Merge to `dev`/`staging`, manage issues/labels, triage | Long-term trust, deep framework knowledge, Commander approval |
| **5** | **Owner** | Merge to `main`, repository settings, releases, team management | Commander only — not open for application |

### Progression Rules

- Progression is by **invitation**, not application (except Tier 1)
- Quality matters more than quantity — 5 excellent PRs outweigh 50 trivial ones
- Maintainers may recommend Tier 2-3 promotions; Tier 4 requires Commander approval
- Tier regression is possible if a contributor becomes inactive (12+ months) or violates the Code of Conduct

## Branch Governance

| Branch | Merge Authority | Requirements |
|--------|----------------|-------------|
| `main` | Owner (Commander) only | 1 review + CI pass + Owner approval |
| `staging` | Maintainer | 1 review + CI pass |
| `dev` | Maintainer | CI pass |
| `feature/*`, `fix/*` | Anyone (via PR) | PR to `dev` |
| `hotfix/*` | Anyone (via PR) | PR to `staging` (critical fixes only) |

See [docs/BRANCH_STRATEGY.md](docs/BRANCH_STRATEGY.md) for the complete branch flow.

## Release Process

1. **Semantic Versioning** — `MAJOR.MINOR.PATCH` ([semver.org](https://semver.org/))
   - MAJOR: Breaking changes (path changes, removed features)
   - MINOR: New features, non-breaking additions
   - PATCH: Bug fixes, documentation updates
2. **CHANGELOG.md** must be updated for every release
3. **template-version.json** must be updated with new SHA-256 hashes
4. **Release tags** are created on `main` branch only
5. **Release notes** are published via GitHub Releases
6. Only the Commander creates releases

## Conflict Resolution

When disagreements arise:

1. **Discussion** — Debate in the relevant PR or issue comments. Present evidence and reasoning.
2. **Maintainer Mediation** — If no consensus, a maintainer facilitates resolution.
3. **Commander Decision** — If mediation fails, the Commander makes the final call. This decision is binding and not subject to appeal.

Conflicts about the Code of Conduct follow the enforcement process in [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## Amendments to This Document

- Changes to this Governance document require **Commander approval**
- Proposed changes must be submitted as a PR to `GOVERNANCE.md`
- A **7-day community comment period** is required for major governance changes
- Minor clarifications (typos, formatting) follow the lazy consensus process

---

*This document is effective as of March 2026 and will be reviewed with each major release.*
