# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.x.x | Yes |
| < 1.0 | No |

## Reporting a Vulnerability

**DO NOT open a public GitHub issue for security vulnerabilities.**

### How to Report

1. **Preferred:** Use [GitHub Security Advisories](https://github.com/VarakornUnicornTech/roundtable-framework/security/advisories/new) to report privately
2. **Alternative:** Contact the project owner directly via GitHub at [@VarakornUnicornTech](https://github.com/VarakornUnicornTech)

### What to Include

- Description of the vulnerability
- Steps to reproduce
- Impact assessment (what could an attacker achieve?)
- Suggested fix (if you have one)

### Response Timeline

| Action | Timeframe |
|--------|-----------|
| Acknowledgment of report | 48 hours |
| Initial assessment | 7 days |
| Fix development | 30 days (depending on severity) |
| Public disclosure | 90 days (or upon fix release) |

## Security Scope

### In Scope

RoundTable Framework is a governance and orchestration layer for Claude Code. Because framework files are **executed as AI instructions**, they must be treated with the same rigor as source code:

| Threat | Description |
|--------|-------------|
| **Prompt Injection** | Malicious content in CLAUDE.md, policies, skills, or agent files that could manipulate AI behavior |
| **Policy Bypass** | Exploits that circumvent governance rules, quality gates, or approval workflows |
| **Hook Bypass** | Methods to evade PreToolUse hooks that protect sensitive files |
| **Malicious Skills/Agents** | Skill or agent definitions that execute unintended or harmful actions |
| **Supply Chain** | Compromised template-version.json hashes or tampered framework files |
| **Privilege Escalation** | Contributor gaining unauthorized merge or admin access |
| **Data Exfiltration** | Framework configurations that leak secrets, API keys, or private data |

### Out of Scope

| Item | Where to Report |
|------|----------------|
| Claude Code CLI vulnerabilities | [Anthropic](https://www.anthropic.com/responsible-disclosure) |
| Claude model behavior issues | [Anthropic](https://www.anthropic.com/responsible-disclosure) |
| GitHub platform vulnerabilities | [GitHub Bug Bounty](https://bounty.github.com/) |
| General AI safety research | Not applicable to this project |

## Security Design Principles

1. **Framework files are code.** Every `.md` file in `.claude/` is interpreted as instructions by an AI agent. Treat them with the same security posture as executable code.

2. **Defense in depth.** Multiple layers protect sensitive files:
   - CODEOWNERS enforces review requirements
   - PreToolUse hooks block unauthorized edits
   - Branch protection prevents direct pushes
   - CI/CD validates naming, paths, and secrets

3. **No secrets in framework.** The framework must never contain API keys, tokens, passwords, or any credentials. All sensitive data must be managed through environment variables or external secret managers.

4. **Principle of least privilege.** Contributors have the minimum access needed. Only the Owner (Commander) can merge to main. See [GOVERNANCE.md](GOVERNANCE.md) for the full permission model.

5. **Audit trail.** All changes go through PRs with review history. RoundTable session logs provide an additional governance audit trail within the framework itself.

## Hall of Fame

Security researchers who responsibly disclose vulnerabilities are credited here (with their permission):

| Researcher | Date | Description |
|-----------|------|-------------|
| *Be the first!* | — | — |

---

*This security policy is reviewed and updated with each major release.*
