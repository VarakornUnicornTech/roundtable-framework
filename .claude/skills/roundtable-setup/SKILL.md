---
name: roundtable-setup
description: Interactive Commander onboarding. Collects callsign, name, pronouns, language, team preferences, and working style via structured questions. Saves to .claude/UserProfile.md. Auto-triggered on first session when UserProfile.md is missing. Re-runnable to update any section.
---

# /roundtable-setup

You are performing the **RoundTable Commander Onboarding**. Execute all steps in order. Use `AskUserQuestion` for every preference — never assume defaults without asking.

## Arguments

- None — run as `/roundtable-setup` for full onboarding
- `update` — run as `/roundtable-setup update` to select and update specific sections only

---

## Steps

### 0. Check for existing profile

- Read `.claude/UserProfile.md` if it exists.
- If it exists AND `$ARGUMENTS` is NOT `update`:
  - Present a one-line summary of current values.
  - Use `AskUserQuestion` to ask:
    > "A Commander profile already exists. What would you like to do?"
    - Options: `Full re-onboarding` / `Update specific sections` / `Cancel — keep current profile`
  - If **Cancel** → stop. Output: `Profile unchanged.`
  - If **Update** → jump to Step 1b (section selector).
  - If **Full re-onboarding** → proceed to Step 1a.
- If it does NOT exist → proceed to Step 1a (no prompt — onboarding is mandatory).

---

### 1a. Commander Identity

Use `AskUserQuestion` with these questions (send all at once, max 4):

**Question 1:**
> "What position title should AM and the teams use when addressing you?"
- Options: `Commander` / `Boss` / `Chief`
- Header: `Callsign`
- Description for each:
  - Commander — formal military-style rank
  - Boss — direct authority title
  - Chief — leadership-focused title

**Question 2:**
> "What is your name?"
- Header: `Name`
- Options: `Prefer not to say` / `Use callsign only`
- Note: The question text should make clear that typing a name is preferred. The two options are fallbacks for users who do not want to provide a name. Most users will type their name via the "Other" free-text input.

**Question 3:**
> "What are your preferred pronouns?"
- Options: `He / Him` / `She / Her` / `They / Them` / `No preference`
- Header: `Pronouns`

**Question 4:**
> "What language should RoundTable respond in for your messages? (In shared sessions, each user's language preference applies to responses directed at them.)"
- Options: `English` / `Thai` / `Both (bilingual — respond in whichever language the message was written in)`
- Header: `Language`
- Description for each:
  - English — all responses in English regardless of input language
  - Thai — all responses in Thai regardless of input language
  - Both (bilingual) — RoundTable mirrors the language of each individual message. If you write in English, the response is in English. If you write in Thai, the response is in Thai. This is the recommended setting for shared or multilingual sessions.

Save answers to profile section: `## Identity`

---

### 1b. Section Selector (update mode only)

Use `AskUserQuestion`:
> "Which sections would you like to update?" (multiSelect: true)
- Options: `Identity` / `Team & Orchestration` / `Working Style`
- Header: `Update`

Run only the selected steps (1a, 2, 3 as applicable). Skip the rest.

---

### 2. Team & Orchestration

Use `AskUserQuestion` with these questions (send all at once):

**Question 1:**
> "Which teams should be active by default?" (multiSelect: true)
- Options: `Overseer (always on)` / `Monolith` / `Syndicate` / `Arcade`
- Header: `Active teams`
- Note: Cipher is always available on-demand — not listed here.

**Question 2:**
> "Default orchestration mode?"
- Options: `Mode A — AM Direct (one prompt in, consolidated report out)` / `Mode B — Separate sessions (you manage each team directly)`
- Header: `Orchestration`
- Description for each:
  - Mode A — You give AM one instruction. AM spawns all sub-teams, collects results, and presents a single consolidated report. Best for efficiency.
  - Mode B — You open a separate Claude session per team and interact with each directly. Best for hands-on control and real-time course correction.

**Question 3:**
> "Commander Phase Acceptance Gate — do you want to personally test and accept each phase before teams advance?"
- Options: `ON — I will test each phase` / `OFF — trust team sign-off (default)`
- Header: `Phase gate`
- Description for each:
  - ON — No phase advances until you explicitly test and accept it. Teams enter a wait state after completing all tickets.
  - OFF — Teams advance based on internal Verification Scholar sign-off. You can still intervene at any time.

Save answers to profile section: `## Team & Orchestration`

---

### 3. Working Style

Use `AskUserQuestion` with these questions (send all at once, max 4):

**Question 1:**
> "How verbose should team responses be?"
- Options: `Concise — short and direct` / `Standard — balanced detail` / `Full — complete reasoning shown`
- Header: `Verbosity`
- Description for each:
  - Concise — minimal output. Key results, decisions, and blockers only. No reasoning trace.
  - Standard — balanced. Enough context to understand decisions without excessive detail.
  - Full — complete reasoning shown. Every decision includes the rationale, alternatives considered, and trade-offs.

**Question 2:**
> "How much autonomy should the team have?"
- Options: `No Trust — full report and explicit approval required for every action` / `Medium Trust — report and approval for major items, AM handles minor tasks independently` / `Full Trust — short concise report only, team executes autonomously without requiring approval`
- Header: `Autonomy`
- Description for each:
  - No Trust — every action requires your explicit sign-off before execution. Full detailed report presented before and after. Nothing proceeds without your approval.
  - Medium Trust — major decisions (architecture, new features, phase advances) require your approval. Minor tasks (formatting, small fixes, documentation updates) AM handles independently and reports after.
  - Full Trust — team operates autonomously. You receive a short concise summary of what was done. Team makes implementation decisions on their own. You intervene only when you choose to.

**Question 3:**
> "When AM or MT need to present you with a decision that has multiple valid options (architecture, stack choice, fix strategy), how should they present it?"
- Options: `Context-first choice UI (recommended)` / `Free text — describe options in prose and I will reply`
- Header: `Decisions`
- Description for each:
  - Context-first choice UI — Before opening any choice prompt, AM/MT will first deliver a full explanation: a diagram, comparison table, or written analysis of each option with trade-offs. Only AFTER you have reviewed the context will the structured AskUserQuestion choice UI appear. This ensures you make informed decisions, not blind picks.
  - Free text — AM/MT describe the options in prose within the response. You reply in free text with your choice. More conversational, less structured.

Save answers to profile section: `## Working Style`

---

### 4. Write UserProfile.md

Write all collected answers to `.claude/UserProfile.md` using this exact format:

```markdown
---
last_updated: DD-MM-YYYY
---

# Commander Profile

## Identity
- **Callsign:** [answer]
- **Name:** [answer]
- **Pronouns:** [answer]
- **Language:** [answer]

## Team & Orchestration
- **Active Teams:** [comma-separated list]
- **Orchestration Mode:** [A or B]
- **Phase Acceptance Gate:** [ON / OFF]

## Working Style
- **Verbosity:** [Concise / Standard / Full]
- **Autonomy Level:** [No Trust / Medium Trust / Full Trust]
- **Architectural Decisions:** [Context-first AskUserQuestion / Free text]
```

Replace `DD-MM-YYYY` with today's actual date.

---

### 5. Confirm

Output the following (adapt callsign and name from profile):

```
Commander profile saved to .claude/UserProfile.md

  Callsign ........... [title]
  Name ............... [name]
  Pronouns ........... [pronouns]
  Language ........... [language]
  Active Teams ....... [teams]
  Orchestration ...... Mode [A/B]
  Phase Gate ......... [ON/OFF]
  Verbosity .......... [level]
  Autonomy ........... [No Trust / Medium Trust / Full Trust]
  Decisions .......... [Context-first AskUserQuestion / Free text]

RoundTable is ready. Welcome, [callsign] [name].
```

---

## Notes

- This skill is **auto-triggered** by CLAUDE.md when `.claude/UserProfile.md` does not exist. Onboarding is the only permitted action until the profile is saved.
- AM reads `UserProfile.md` at every session start and applies all preferences immediately.
- **Callsign** is the position title (Commander/Boss/Chief) used by all team members when addressing the user in logs, reports, and responses.
- **Name** is the user's personal name. Used alongside callsign (e.g., "Chief Martin") or in informal contexts.
- **Pronouns** are used by all team members when referring to Commander in third person.
- **Language: Both (bilingual)** means RoundTable mirrors the language of each individual message — critical for shared sessions where different users may write in different languages.
- **Verbosity** overrides default response length for all team outputs.
- **Autonomy Level** controls how much approval the team needs:
  - **No Trust** = full report + explicit approval for every action
  - **Medium Trust** = approval for major items, AM handles minor independently
  - **Full Trust** = short report, team executes autonomously
- **Architectural Decisions: Context-first AskUserQuestion** means MT and team Conductors MUST deliver a full explanation (diagram, comparison table, or analysis) BEFORE opening the AskUserQuestion choice UI. The user must have full context before being asked to choose.
- **Phase Acceptance Gate** setting is applied as if Commander had toggled it in §2 policy.
- Project-specific settings (tech stack, project type, structure mode, debug mode) are collected separately via `/project-init` — not during onboarding.
- Profile can be updated at any time by running `/roundtable-setup update`.
