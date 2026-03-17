---
name: briefing-create
description: Create a Phase Briefing Mail for a specific team. Provide team name and phase number (e.g., /briefing-create Monolith 2).
---

# Create Phase Briefing Mail

Create a Briefing Mail document for a sub-team following the RoundTable Briefing Mail Standard.

## Steps

1. **Parse arguments:** `$1` = Team name (Monolith/Syndicate/Arcade), `$2` = Phase number
2. **Read `.claude/ProjectEnvironment.md`** to get PROJECT_ROOT and active project info.
3. **Determine the briefing file path:**
   ```
   Development/01_Implementation Logs/INDEV v1.0.0/Phase $2/$1_Phase$2_Briefing.md
   ```
4. **Create parent directories** if they don't exist.
5. **Check if file already exists** — if yes, warn and do not overwrite.
6. **Create the briefing file** using this template:

```markdown
# Team $1 — Phase $2 Briefing
Issued by: AM | Date: [TODAY] | Project: [PROJECT_NAME] | Phase: $2 | Tickets: [to be filled]

## 0. Pre-Work: Load Your Roster
- Read CLAUDE.md: `.claude/CLAUDE.md`
- Read Team Roster: `.claude/Team Roster/[N]. Team_$1.md`
- Adopt voice, code names, coding style
- Create today's Team Chat log

## 1. Context
[What has already been decided — to be filled by AM]

## 2. Your Mission
[What this phase produces — to be filled by AM]

## 3. Tickets
[One section per ticket — to be filled by AM]

## 4. Parallel Execution — Start Now vs. Wait
- **Start immediately:** [list unblocked tickets]
- **Blocked on:** [list blocked tickets and what unblocks them]

## 5. Logging & Handoff Requirements
- Team Chat log: `.claude/team_chat/[N]. $1/DD-MM-YYYY_$1.md`
- OverseerReport: `.claude/team_chat/5. OverseerReport/DD-MM-YYYY_OverseerReport.md`
- File Dependency Signal in OverseerReport when a ticket others depend on completes

## 6. Boundaries — Do NOT Touch
[Explicit list — to be filled by AM]

## 7. If You Hit a Blocker
Stop and file a blocker in OverseerReport — do not workaround without AM sign-off.

---
Footer: Issued by AM (Overseer) — [TODAY]
```

7. **Report** the file path and list sections that need to be filled in.

## Arguments

- `$1` — Team name (required: Monolith, Syndicate, or Arcade)
- `$2` — Phase number (required: 0, 1, 2, etc.)
