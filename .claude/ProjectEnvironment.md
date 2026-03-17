# ProjectEnvironment

> Active project registry. Check this file before constructing any Development folder path or PreExisting TechStack path.
> Maintained by AM (Overseer). Update when a project is added, put on hold, or its source path changes.

---

## Active Projects

### UniOpsQC
**PROJECT_MODE:** Centralized
**PROJECT_ROOT:** `C:/UnicornVibeCode/UniOpsQC`
**SOURCE_ROOT:** `C:/UnicornVibeCode/UniOpsQC`
**ACTIVE:** true
**Notes:** RoundTable Framework — the governance framework itself. Planning and source in same root. Framework management, policy updates, skill development, contributor PR review.

---

### HubVSCode
**PROJECT_MODE:** Decentralized
**PROJECT_ROOT:** `C:/UnicornVibeCode/UniOpsQC/Development/HubVSCode`
**SOURCE_ROOT:** `C:/UnicornVibeCode/UniOpsQC-vscode`
**ACTIVE:** true
**Notes:** UniOpsQC Hub VSCode Extension (TypeScript, esbuild, v1.6.1). Publisher: UnicornTech. Planning hub in UniOpsQC/Development/HubVSCode — source code in separate repo (GitHub: UniOpsQC-vscode, local folder: UniOpsQC-vscode). All BUG-01–06 fixed and shipped to Marketplace v1.6.1.

---

### UniOpsQC-command
**PROJECT_MODE:** Decentralized
**PROJECT_ROOT:** `C:/UnicornVibeCode/UniOpsQC/Development/UniOpsQC-command`
**SOURCE_ROOT:** `C:/UnicornVibeCode/UniOpsQC-command`
**ACTIVE:** true
**Notes:** UniOpsQC Command — PM Dashboard web app for AI Agent workflows (.NET solution, PostgreSQL, SignalR). Full-stack: Hub.Domain / Hub.Application / Hub.Infrastructure / Hub.Api / Hub.Client. Source at GitHub: VarakornUnicornTech/UniOpsQC-command (renamed from RoundTable-Hub). Planning hub in UniOpsQC/Development/UniOpsQC-command.

---

## Rules

- Check this file before constructing any Development folder path or PreExisting TechStack path
- If a project is not listed here, add it before beginning work
- `SOURCE_ROOT` and `PROJECT_ROOT` are the same in Centralized mode with no sub-components; different in Decentralized mode
- Set `ACTIVE: false` for projects that are on hold — do not delete entries
- Update the Notes field when significant project state changes (new deployment, mode change, source path change)

---

*Last updated: 17-03-2026 by AM (AstonMartin) — RoundTableHub renamed to UniOpsQC-command (Session 18)*
