---
name: phase-status
description: Full project phase and ticket status report. Scans all ticket files for a project and returns a complete status breakdown by team and phase.
---

# /phase-status [Project]

## Arguments

- `[Project]` — Project name (must match an entry in `.claude/ProjectEnvironment.md`)

## Ground Truth Rule (CRITICAL — anti-hallucination)

**Every data point in this report MUST come from reading an actual file on disk using the Read tool or Glob tool.**

- Do NOT use ticket information from conversation context, prior messages, or memory.
- Do NOT assume a ticket exists because it was discussed — verify by reading the file.
- Do NOT fill in status, title, or team from what you "remember" — read the file.
- If a ticket file does not exist on disk, it does NOT appear in the report.
- If a Phase folder is empty or does not exist, report it as empty — do not fabricate tickets.

**Violation of this rule produces a false status report — a critical failure.**

---

## Steps

1. **Read ProjectEnvironment.md** (using Read tool):
   - Path: `.claude/ProjectEnvironment.md`
   - Extract `PROJECT_ROOT` for the specified project.
   - If project not found: report error and stop.

2. **List Phase folders** (using Glob or Bash `ls`):
   - Path: `[PROJECT_ROOT]/Development/01_Implementation Logs/INDEV v1.0.0/`
   - List all `Phase *` folders found.
   - If no folders exist: report "No phases found" and stop.

3. **For each Phase folder — scan ticket files from disk:**
   - Use Glob to list all `.md` files in the Phase folder and its team subfolders.
   - Exclude Briefing files (files matching `*_Briefing.md`).
   - **For each ticket file found:** use the Read tool to open it and extract:
     - Ticket ID (from filename or `# [ID]` header)
     - Title (from `# [ID] — [Title]` header)
     - Status (search for `**Status:**` line — extract `[ ]` / `[~]` / `[x]` / `[!]` / `[>]`)
   - Identify owning team from ticket ID prefix (MON = Monolith, SYN = Syndicate, ARC = Arcade, OVR = Overseer).
   - **If a ticket file cannot be parsed:** include it in the report with status "UNKNOWN — file could not be parsed" rather than guessing.

4. **Build the status report** (using ONLY data extracted from files in Step 3):

```markdown
## Phase Status Report — [Project]
**Generated:** DD-MM-YYYY

### Phase [N] — [Phase Name if available]

#### Monolith
| ID | Title | Status |
|----|-------|--------|
| MON-01 | [title] | ✅ Complete / 🔄 In Progress / ⬜ Pending / 🚫 Blocked / ➡️ Deferred |

#### Syndicate
| ID | Title | Status |
|----|-------|--------|

#### Arcade
| ID | Title | Status |
|----|-------|--------|

**Phase [N] Summary:** [X] Complete, [X] In Progress, [X] Pending, [X] Blocked, [X] Deferred

---

### Overall Project Summary
| Phase | Complete | In Progress | Pending | Blocked | Deferred |
|-------|----------|-------------|---------|---------|----------|
| Phase 1 | X | X | X | X | X |
| Phase 2 | X | X | X | X | X |
```

5. **Flag any blockers** prominently at the top of the report if any `[!]` tickets exist.

## Status Icon Legend
- `[ ]` PENDING → ⬜
- `[~]` IN PROGRESS → 🔄
- `[x]` Complete → ✅
- `[!]` BLOCKED → 🚫
- `[>]` DEFERRED → ➡️
