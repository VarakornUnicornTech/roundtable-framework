---
name: sync-setup
description: Interactive sync connection wizard. Guides Commander through connecting a local project to a UniOpsQC-command server instance. Collects endpoint URL and API token, validates both, writes configuration to ProjectEnvironment.md and secrets.local, tests connection via /api/health.
---

# /sync-setup [project_name?]

You are performing the **UniOpsQC Sync Setup** wizard. Execute all steps in order. Show progress as `[N/9]` on each step.

## Arguments

- `[project_name]` — Optional. Project name matching an entry in `ProjectEnvironment.md`. If omitted, the wizard prompts for selection in Step 2.

## Help

If `$ARGUMENTS` is `help` (i.e., invoked as `/sync-setup help`), print the following and STOP -- do NOT execute any steps.

```
/sync-setup -- Connect a local project to a UniOpsQC-command server

SYNTAX:
  /sync-setup [project_name?]

ARGUMENTS:
  project_name   Optional. Must match an entry in ProjectEnvironment.md.
                 If omitted, you will be prompted to select a project.

WHAT IT DOES:
  1. Reads your current projects from ProjectEnvironment.md
  2. Asks which project to connect (if not specified)
  3. Collects server endpoint URL and token key name
  4. Collects the secret API token (uqc_xxx...)
  5. Validates format (URL + token prefix)
  6. Writes SYNC_ENDPOINT + SYNC_TOKEN_KEY to ProjectEnvironment.md
  7. Writes token to .claude/secrets.local (git-ignored)
  8. Tests connection: GET {endpoint}/api/health
  9. Reports success or troubleshooting tips

EXAMPLES:
  /sync-setup                      Interactive -- prompts for project selection
  /sync-setup UniOpsQC-command     Connect UniOpsQC-command directly
  /sync-setup help                 Show this help text
```

---

## Steps

### [1/9] Read ProjectEnvironment.md

**Use the Read tool to open `.claude/ProjectEnvironment.md` from disk.** (Ground Truth Rule -- never rely on conversation context or memory.)

- Parse the `## Active Projects` section.
- Build a list of all projects where `ACTIVE: true`.
- If the file does not exist or cannot be read, HALT and report: `[BLOCKED] ProjectEnvironment.md not found. Cannot proceed.`

---

### [2/9] Select Target Project

- If `$ARGUMENTS` contains a project name:
  - Match it against the active projects list from Step 1. Match is case-insensitive.
  - If no match: report `Project "[name]" not found in ProjectEnvironment.md. Available projects: [list]` and HALT.
- If no project name was provided:
  - Present the list of active projects using `AskUserQuestion`:
    > "[2/9] Which project do you want to connect to UniOpsQC-command?"
    - Options: one per active project (show project name + PROJECT_MODE)
    - Header: `Project`

Store the selected project's `PROJECT_ROOT` and section name for later use.

---

### [3/9] Collect Sync Endpoint

Ask Commander for the server endpoint using `AskUserQuestion`:
> "[3/9] What is the UniOpsQC-command server URL? (e.g., https://your-server.example.com or http://localhost:5201)"
- Header: `Sync Endpoint`
- Options: `http://localhost:5201` / `Other -- paste your server URL`

**Validation:**
- Must start with `http://` or `https://`.
- Must not end with a trailing slash (strip it if present).
- If invalid, re-ask: `The URL must start with http:// or https://. Please try again.`

---

### [4/9] Collect Token Key Name

Ask Commander for the token key name using `AskUserQuestion`:
> "[4/9] What name should identify this token in config? This is the key name, not the secret itself. (e.g., UQCMD_API_TOKEN)"
- Header: `Token Key Name`
- Options: `UQCMD_API_TOKEN` / `Other -- type a custom key name`

**Validation:**
- Must be uppercase alphanumeric with underscores only (regex: `^[A-Z][A-Z0-9_]*$`).
- If invalid, re-ask: `Token key name must be uppercase letters, digits, and underscores only (e.g., UQCMD_API_TOKEN).`

---

### [5/9] Collect Secret Token

Ask Commander to paste the secret API token:
> "[5/9] Paste your API token. It should start with `uqc_` and was provided when you created the token on the dashboard."
- Header: `API Token`
- Accept free-text input only (no predefined options -- this is a secret).

**Validation:**
- Must start with `uqc_`.
- Must be at least 20 characters long.
- If invalid: `Token must start with "uqc_" and be at least 20 characters. Please check the token from your dashboard and try again.`

**Security note:** Never log or echo the full token. If displaying for confirmation, show only `uqc_****...****` (first 4 chars + last 4 chars).

---

### [6/9] Write SYNC_ENDPOINT + SYNC_TOKEN_KEY to ProjectEnvironment.md

**Use the Read tool to re-read `.claude/ProjectEnvironment.md` from disk** (Ground Truth Rule -- always read before writing).

Locate the section for the selected project (matched by `### [ProjectName]`). Append the following two lines immediately after the existing fields in that project's section (before the `---` separator or `**Notes:**` line -- whichever comes last):

```
**SYNC_ENDPOINT:** `[endpoint_url]`
**SYNC_TOKEN_KEY:** `[token_key_name]`
```

- If `SYNC_ENDPOINT` or `SYNC_TOKEN_KEY` already exist in that section, **update** the values in place rather than duplicating.
- Use the Edit tool to make the change.

Output:
> "[6/9] ProjectEnvironment.md updated -- SYNC_ENDPOINT and SYNC_TOKEN_KEY written for [ProjectName]."

---

### [7/9] Write Token to secrets.local

1. **Check if `.claude/secrets.local` exists** using the Read tool.
   - If it does NOT exist, create it with the Write tool.
   - If it exists, read its current contents.

2. **Write (or update) the token line:**
   ```
   [TOKEN_KEY_NAME]=uqc_xxxxx...
   ```
   - If a line starting with `[TOKEN_KEY_NAME]=` already exists, replace it.
   - If not, append the new line.

3. **Verify .gitignore excludes secrets.local:**
   - **Use the Read tool to open `.gitignore` from the project root** (Ground Truth Rule).
   - Search for a line containing `secrets.local`.
   - If NOT found: append `# Sync secrets (local only -- never commit)\nsecrets.local\n` to `.gitignore` using the Edit tool.
   - If found: no action needed.

Output:
> "[7/9] Token written to .claude/secrets.local. .gitignore verified -- secrets.local is excluded."

---

### [8/9] Test Connection

Execute an HTTP request to test the connection:

```bash
curl -s -o /dev/null -w "%{http_code}" -H "Authorization: Bearer [TOKEN]" [SYNC_ENDPOINT]/api/health
```

- Also capture the response body for display:
```bash
curl -s -H "Authorization: Bearer [TOKEN]" [SYNC_ENDPOINT]/api/health
```

**Evaluate the result:**

- **HTTP 200:** Connection successful. Proceed to Step 9 with success path.
- **HTTP 401/403:** Authentication failed. Proceed to Step 9 with auth error.
- **HTTP 0 / connection refused:** Server unreachable. Proceed to Step 9 with connection error.
- **Any other code:** Unexpected response. Proceed to Step 9 with the raw status and body.

---

### [9/9] Report Result

**On success (HTTP 200):**

```
[9/9] Connection successful!

  Project ........... [ProjectName]
  Endpoint .......... [SYNC_ENDPOINT]
  Token Key ......... [SYNC_TOKEN_KEY]
  Status ............ Connected

  Config written to:
    - .claude/ProjectEnvironment.md  (SYNC_ENDPOINT + SYNC_TOKEN_KEY)
    - .claude/secrets.local          ([TOKEN_KEY_NAME]=uqc_****...****)

  .gitignore ........ secrets.local excluded

/sync-setup complete. Your project is connected to UniOpsQC-command.
```

**On authentication error (401/403):**

```
[9/9] Connection FAILED -- authentication error (HTTP [code])

  Endpoint .......... [SYNC_ENDPOINT]  (reachable)
  Token ............. uqc_****...****
  Error ............. [response body or status text]

  Troubleshooting:
    1. Verify the token was copied completely (no trailing spaces)
    2. Check the token has not been revoked on the dashboard
    3. Confirm the token has access to this workspace
    4. Try creating a new token on the dashboard

  Config was written but connection is not verified.
  Re-run /sync-setup to try again, or fix the token and test manually:
    curl -H "Authorization: Bearer $TOKEN" [ENDPOINT]/api/health
```

**On connection error (unreachable):**

```
[9/9] Connection FAILED -- server unreachable

  Endpoint .......... [SYNC_ENDPOINT]
  Error ............. Could not connect

  Troubleshooting:
    1. Is the server running? (check docker / process status)
    2. Is the URL correct? (verify port number)
    3. Is a firewall or VPN blocking the connection?
    4. For localhost: ensure the API is listening on the expected port
    5. Try: curl [ENDPOINT]/api/health  (without auth, to test reachability)

  Config was written. Connection test will pass once the server is reachable.
  Re-run /sync-setup to test again.
```

**On unexpected error:**

```
[9/9] Connection FAILED -- unexpected response (HTTP [code])

  Endpoint .......... [SYNC_ENDPOINT]
  Response .......... [body or status text]

  Config was written. Investigate the server response and re-run /sync-setup to test again.
```

---

## Output

Summary of what was written and connection test result. Config file paths listed for Commander reference.

## Notes

- This skill writes to the **UniOpsQC Framework** project (`.claude/ProjectEnvironment.md`, `.claude/secrets.local`, `.gitignore`) -- it does NOT modify UniOpsQC-command source code.
- The secret token is stored in `.claude/secrets.local` which MUST be git-ignored. The skill verifies this automatically.
- `SYNC_TOKEN_KEY` in ProjectEnvironment.md stores only the **key name** (e.g., `UQCMD_API_TOKEN`), never the secret value itself.
- The actual token value lives exclusively in `.claude/secrets.local`.
- Live connection testing (Step 8) requires the UniOpsQC-command server to be running with the `/api/health` endpoint deployed (CMD-09).
- Re-running `/sync-setup` on the same project will update existing values (not duplicate them).
