# Contributing to RoundTable Framework

Thank you for your interest in contributing to RoundTable Framework! This project transforms a single AI assistant into a coordinated multi-team engineering organization using Claude Code.

## Built & Used in Production

> **This is not a theoretical framework.** RoundTable Framework is actively used in production by [Unicorn Tech Integration Co., Ltd.](https://github.com/VarakornUnicornTech) to build and ship real software — fully powered by Claude Code, governed by this very framework, and directed by a human Commander. Every policy, every workflow, every quality gate you see here has been battle-tested in live development. If you're looking for an AI governance tool that's built by practitioners who use it daily — you've found it.

## Code of Conduct

By participating in this project, you agree to uphold our [Code of Conduct](CODE_OF_CONDUCT.md). We are committed to providing a welcoming and inclusive experience for everyone.

## How Can I Contribute?

### Reporting Bugs

- Use the [Bug Report](https://github.com/VarakornUnicornTech/unicorn_roundtable_framework_repo/issues/new?template=bug_report.md) issue template
- Include your Claude Code version and OS
- Provide steps to reproduce the issue
- Include relevant log output if available

### Suggesting Features

- Use the [Feature Request](https://github.com/VarakornUnicornTech/unicorn_roundtable_framework_repo/issues/new?template=feature_request.md) issue template
- Describe the problem your feature would solve
- Explain your proposed solution
- Consider alternative approaches

### Submitting Pull Requests

All contributions are welcome — from typo fixes to new features. Follow the process below to ensure smooth reviews.

### Improving Documentation

Documentation improvements are highly valued. Both English and Thai translations are maintained. If you can contribute translations, that's especially appreciated.

## Development Setup

### Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed and configured
- Git 2.30+
- A GitHub account

### Fork & Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR_USERNAME/unicorn_roundtable_framework_repo.git
cd unicorn_roundtable_framework_repo
git remote add upstream https://github.com/VarakornUnicornTech/unicorn_roundtable_framework_repo.git
git fetch upstream
```

### Branch Naming Convention

Create branches from `dev` using these prefixes:

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feature/` | New functionality | `feature/add-review-skill` |
| `fix/` | Bug fixes | `fix/hook-path-resolution` |
| `docs/` | Documentation changes | `docs/update-getting-started` |
| `security/` | Security improvements | `security/input-validation` |
| `refactor/` | Code restructuring | `refactor/policy-consolidation` |

## Pull Request Process

### 1. Fork & Branch from `dev`

```bash
git checkout dev
git pull upstream dev
git checkout -b feature/your-feature-name
```

### 2. Make Your Changes

- Follow the [Style Guide](#style-guide) below
- Test your changes locally with Claude Code
- Update documentation if your changes affect user-facing behavior

### 3. Test Locally

```bash
# Verify your framework changes work with Claude Code
# Open Claude Code in a test directory with your modified framework
claude
```

### 4. Commit Your Changes

```bash
# Use conventional commit format
git commit -m "feat(skills): add new review workflow skill

Detailed description of what changed and why."
```

### 5. Submit PR to `dev`

```bash
git push origin feature/your-feature-name
```

Then create a Pull Request targeting the `dev` branch. Fill out the PR template completely.

### 6. Review Process

- A maintainer will review your PR within the SLA defined in [docs/REVIEW_PROCESS.md](docs/REVIEW_PROCESS.md)
- Address any requested changes
- Once approved, a maintainer will merge your PR

**Important:** Only maintainers can merge PRs. Do not expect auto-merge.

## Branch Strategy

```
feature/* ──> dev ──> staging ──> main
fix/*     ──> dev ──> staging ──> main
hotfix/*  ──> staging ──> main (+ cherry-pick to dev)
```

| Branch | Purpose | Who Merges |
|--------|---------|-----------|
| `main` | Production / published releases | Owner only |
| `staging` | Pre-release validation | Maintainer (with review) |
| `dev` | Integration of features and fixes | Maintainer |
| `feature/*` | Your work branch | You (via PR to dev) |

See [docs/BRANCH_STRATEGY.md](docs/BRANCH_STRATEGY.md) for full details.

## Protected Zones

The following paths require **Maintainer or Owner review** via CODEOWNERS. Changes to these files will automatically request review from the appropriate maintainer:

| Path | What It Is | Required Reviewer |
|------|-----------|-------------------|
| `.claude/CLAUDE.md` | Core framework brain | Owner |
| `.claude/agents/` | Team identity definitions | Owner |
| `.claude/settings.json` | Hook configurations | Owner |
| `.claude/policies/` | Governance rules | Maintainer |
| `.claude/skills/` | Skill definitions | Maintainer |
| `template-version.json` | Version integrity | Owner |
| `GOVERNANCE.md` | Project governance | Owner |
| `SECURITY.md` | Security policy | Owner |

You can still submit PRs that touch these files — they just require the designated reviewer's approval.

## Commit Message Convention

We use [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): short description

[optional body]

[optional footer]
```

**Types:**

| Type | When to Use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, whitespace (no logic change) |
| `refactor` | Code restructuring (no feature/fix) |
| `test` | Adding or updating tests |
| `chore` | Maintenance tasks, CI, tooling |
| `security` | Security improvements |

**Scopes:** `core`, `policies`, `skills`, `agents`, `docs`, `ci`, `hooks`

## Style Guide

### Markdown

- Use ATX-style headings (`#`, `##`, `###`)
- One sentence per line in paragraphs (for cleaner diffs)
- Use fenced code blocks with language identifiers
- Tables must be aligned for readability
- No trailing whitespace

### Framework Files

- All paths must be **relative** (never absolute like `C:\...` or `D:\...`)
- No hardcoded usernames or local machine references
- Policy section numbers must match filename numbers (e.g., `§5` in `05_PreExisting_Codebase.md`)
- Use forward slashes in all paths (even on Windows)

### Bilingual Requirement

User-facing documentation must be available in both:
- **English** (`*.md`)
- **Thai** (`*.th.md`)

When modifying `README.md`, `GETTING_STARTED.md`, or `CONTRIBUTING.md`, please update the Thai counterpart as well, or note in your PR that a translation update is needed.

## AI Agent Contributors

RoundTable Framework welcomes contributions from AI agents. See [docs/AI_AGENTS.md](docs/AI_AGENTS.md) for detailed guidelines.

**Key rules:**
1. AI-generated contributions must be declared in the PR template
2. A human must sponsor and take responsibility for every AI-generated PR
3. AI contributions go through the same review process as human contributions
4. No automated mass-PRs without prior maintainer approval

## Community

- **Questions:** Use [GitHub Discussions](https://github.com/VarakornUnicornTech/unicorn_roundtable_framework_repo/discussions)
- **Bugs:** Use [Issues](https://github.com/VarakornUnicornTech/unicorn_roundtable_framework_repo/issues) with the Bug Report template
- **Features:** Use [Issues](https://github.com/VarakornUnicornTech/unicorn_roundtable_framework_repo/issues) with the Feature Request template
- **Security:** See [SECURITY.md](SECURITY.md) — **never** open a public issue for vulnerabilities

## License

By contributing to RoundTable Framework, you agree that your contributions will be licensed under the [MIT License](LICENSE). This means your contributions can be freely used, modified, and distributed by anyone.

## Recognition

All contributors are credited in [CHANGELOG.md](CHANGELOG.md). Significant contributions are highlighted in release notes.

---

*Thank you for helping make RoundTable Framework better for everyone.*
