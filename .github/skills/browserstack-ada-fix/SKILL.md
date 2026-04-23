---
name: browserstack-ada-fix
description: Fix ADA/WCAG accessibility issues via BrowserStack scan results.
version: "1.4"
standards: "WCAG 2.2 · ADA Section 508 · EN 301 549"
triggers: "fixadaissues file · fixadaissues folder · fixadaissues results · fixadaissues preventive"
---

# ♿ BrowserStack ADA Compliance Fixer

> **Automatically fix accessibility violations identified by BrowserStack's scanner — directly in your codebase.**
>
> `WCAG 2.2` · `ADA / Section 508` · `EN 301 549`
>
> **Triggers:** `fixadaissues file` · `fixadaissues folder` · `fixadaissues results` · `fixadaissues preventive` · or mention ADA, WCAG, a11y, BrowserStack accessibility

---

## 🔄 Workflow Overview

### Standard Workflow (Fix Mode)

```
1. Branch Selection   (pre-Phase 0)
  Choose: git worktree | single branch
          |
          v
2. Setup              (Phase 0)
  Prerequisites + branch/worktree creation
          |
          v
3. Scan               (Phase 1)
  Load existing BrowserStack results
          |
          v
4. Triage             (Phase 2)
  Group, prioritize, exclude contrast
          |
          v
5. Fix                (Phase 3)
  Apply fixes + auto-capture new knowledge
          |
          v
6. Verify             (Phase 4)
  Summarize + validate/consolidate knowledge base
          |
          v
7. Review             (Phase 5)
  User approves, rejects, or inspects changes
          |
          v
8. GitFlow            (Phase 6)
  Commit, push, and create PRs when applicable
```

### Preventive Workflow (Prevention Mode)

**Separate detailed workflow** — See **[preventive-workflow.md](preventive-workflow.md)**

```
1. Branch Selection   (pre-Phase 0)
   Choose: git worktree | single branch | current branch
          |
          v
2. Setup              (Phase 0)
   Load prevention patterns from custom-fixes.md
          |
          v
3. Scan               (Phase 1)
   Check code against prevention patterns
          |
          v
4. Report             (Phase 2)
   Group and prioritize potential issues
          |
          v
5. Fix (optional)     (Phase 3)
   User chooses: fix now / report only
          |
          v
6. Verify             (Phase 4)
   Summarize fixes (if applied)
          |
          v
7. Review             (Phase 5)
   User approves, rejects, or inspects
          |
          v
8. GitFlow (optional) (Phase 6)
   Commit, push, and create PR if requested
```

---

## Ignored Issue Categories

> **⚠️ ALWAYS apply this filter — before triage, before fixing, and before reporting.**
> Contrast-related issues are permanently excluded from this workflow.

```
 Rule IDs / keywords to skip (case-insensitive match on rule ID or description)
 ───────────────────────────────────────────────────────────────────────────────
  color-contrast
  color-contrast-enhanced
  contrast
  non-text-contrast
```

**At every phase:**
- Do NOT triage, fix, validate, or report contrast issues.
- When a contrast issue is encountered, log: `[Triage] Skipping "<rule ID>" -- contrast issues are excluded from this workflow.`
- Do NOT count skipped contrast issues in the "Fixed" total. Count them separately in a "Excluded (contrast)" line in the Fix Summary.

---

## User Communication Rule

> **⚠️ CRITICAL — Logs are MANDATORY. Always print status messages before and after every step.**
> The Auto-Start Rule below means "start immediately without preamble" — it does NOT mean "work silently."
> Logs must always be printed using the prefixes below.
>
> **🌿 Worktree Mode Logging:** When processing multiple worktrees, logs are EVEN MORE CRITICAL.
> Users cannot see what's happening inside each worktree without logs. Print detailed progress
> for each worktree including: worktree switch, issue count, individual fixes, and completion status.

**Log Prefixes:**

```
 Prefix        Phase
 ─────────     ──────────────────────────────────────
 [Setup]       Prerequisites and configuration
 [Scope]       Input mode and target resolution
 [Scan]        Loading and parsing scan results
 [Triage]      Issue analysis and prioritization
 [Fix]         Code changes
 [Verify]      Summary report and next steps
 [Review]      User review and approval of changes
 [Worktree]    Git worktree creation and management
 [Excel]       CSV/Excel fix tracking (file and folder modes)
 [GitFlow]     Commit, push, and PR (branch already created in Setup)
```

**Format:** `[Phase] Description of what is happening...`

---

## ⚡ Auto-Start Rule

> **🚨 When `fixadaissues` is detected in the user’s message, START THE WORKFLOW IMMEDIATELY.**
>
> - Do **NOT** explain, describe, or summarize the workflow.
> - Do **NOT** ask the user what file or folder to use — it is already in their message.
> - Parse mode and target directly from the message.
> - Your **very first output** MUST be the `[Scope]` confirmation line followed immediately by the branch-strategy question.
> - No preamble. No "I will now...". No "Here’s what I’ll do". Just start.

**Required first output — copy this template exactly (file / folder / results modes):**

```
[Scope] Mode: <file|folder|results> | Target: <parsed target>
[Setup] Branch strategy required before Phase 0.
[Setup] Do you want to use:
  1. git worktree
  2. single branch
```

> **📝 Preventive mode** uses a three-option prompt (see [preventive-workflow.md](preventive-workflow.md)):
> `1. git worktree  2. single branch  3. current branch`

The user triggers this skill with a command-style prompt. Parse the mode
and target directly from the user's message, then ask one required branch-strategy
question before starting Phase 0.

> **📝 Do not ask interactively for mode or target** — extract those parameters from
> what the user typed. After mode/target are resolved, ask the user whether to use
> `git worktree` or a `single branch`, then continue into Phase 0 based on that selection.
> Keywords are case-insensitive. Natural phrasing is also accepted
> (e.g., *"fix ada issues in file src/App.tsx"*).

### Supported Commands

```
fixadaissues file   <filepath>                    ──▶  Fix using BrowserStack report file (CSV/Excel)
fixadaissues folder <folderpath>                  ──▶  Fix using BrowserStack reports in folder
fixadaissues results <url>                        ──▶  Fix from BrowserStack URL
fixadaissues preventive <path>                    ──▶  Check code BEFORE scan (see preventive-workflow.md)
```

> **⚠️ IMPORTANT:** File and folder modes require BrowserStack exported report files (CSV/Excel format).
> You cannot use source code files (.cshtml, .tsx, .jsx, etc.) as the target.
> The report file contains the violations; fixes are applied to the actual codebase.
>
> **💡 Preventive mode:** Scans code against known prevention patterns from custom-fixes.md.
> Use this proactively before running an actual BrowserStack scan. Full details in
> **[preventive-workflow.md](preventive-workflow.md)**.

After parsing the command, ask:

```
[Setup] Choose branch strategy before Phase 0:
  1. git worktree
  2. single branch
```

> **📝 Note:** `current branch` is available only in preventive mode. Do not offer it for `file`, `folder`, or `results` modes.

- If the user selects `git worktree`, ask one follow-up question:

```
[Setup] How many worktrees should be created? Enter a value from 1 to 4.
```

- If the user selects `single branch`, skip the worktree-count question and start Phase 0 immediately.
- Do not start Phase 0 until the branch strategy is explicitly selected.
- For users unfamiliar with worktrees, provide this optional reference:
  `https://www.youtube.com/watch?v=s4BTvj1ZVLM`

### Parsing Rules

```
                    ┌─────────────┐
                    │  User Input │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
               ┌────│  Keyword?   │────┐
               │    └─────────────┘    │
              YES                      NO
               │                       │
     ┌─────────▼─────────┐   ┌────────▼────────┐
     │ Use keyword+target │   │  Auto-detect:   │
     └───────────────────┘   │                  │
                              │ http → results  │
                              │ file → file     │
                              │ dir  → folder   │
                              │ else → STOP     │
                              └─────────────────┘
```

If nothing can be determined, show usage and **stop**:

```
[Setup] Could not determine mode. Usage:
  fixadaissues file   <filepath>
  fixadaissues folder <folderpath>
  fixadaissues results <url>
```

---

## 📂 Mode Details

> **⚠️ CRITICAL:** File and folder modes require **BrowserStack exported report files** (CSV/Excel format).
> Source code files (.cshtml, .tsx, .jsx, etc.) cannot be used as targets in these modes.
> Use `results` mode if you have a BrowserStack URL instead of an exported report.

### ▸ File Mode

**Required:** BrowserStack accessibility report file (`.csv`, `.xlsx`, or `.xls`)

```
Scope:       Violations are READ from the BrowserStack report file, row-by-row.
             Fixes are applied across the entire codebase — no path filtering.
Log:         [Scope] Mode: file (CSV/Excel) | Target: <filepath>
```

- ✅ **Valid** → `[Scope] Target BrowserStack report confirmed: <filepath>. Reading violations from rows...`
- ❌ **Invalid (not found)** → `[Scope] File not found: <filepath>. Please check the path.` → **Stop.**
- ❌ **Invalid (not CSV/Excel)** → `[Scope] File mode requires a BrowserStack report file (.csv, .xlsx, .xls). For source code files, use results mode with a BrowserStack URL.` → **Stop.**
- Load violations from the report in Phase 1 (row-by-row). Apply fixes across the codebase. In Phase 4, write the **"Fix"** column back to this file in-place (see CSV/Excel Fix Tracking).
- Do **NOT** treat the report file itself as a UI file to fix.

### ▸ Folder Mode

**Required:** Folder containing BrowserStack accessibility report files (`.csv`, `.xlsx`, or `.xls`)

```
Scope:       CSV/Excel BrowserStack reports in folder are violations sources.
             Fixes are applied across the entire codebase — no path filtering.
Log:         [Scope] Mode: folder | Target: <folderpath>
```

- ✅ **Valid** → `[Scope] Target folder confirmed: <folderpath>. Found X BrowserStack report file(s) (.csv/.xlsx/.xls).`
- ❌ **Invalid (folder not found)** → `[Scope] Folder not found: <folderpath>. Please check the path.` → **Stop.**
- ❌ **Invalid (no reports)** → `[Scope] No BrowserStack report files (.csv, .xlsx, .xls) found in <folderpath>. Please export scan results first.` → **Stop.**
- **CSV/Excel files** found in the folder are treated as **BrowserStack violation reports** (rows read as violations). They are NOT treated as UI files to fix.
- After all fixes, the **"Fix"** column is written back to each report file (see CSV/Excel Fix Tracking).
- Violations are matched and fixed across the entire codebase, not restricted to the folder path.

### ▸ Results Mode

```
Scope:       Entire codebase (no path filtering)
Log:         [Scope] Mode: results | Target: <url>
```

- ✅ **Success** → `[Scope] Loaded X violations from BrowserStack results. Fixes apply across the entire codebase.`
- ❌ **Failure** → `[Scope] Failed to load results from <url>. Please verify the URL.` → **Stop.**

### End-of-Phase Summary

At the end of Phase 3, always report:

```
[Fix] Mode: <mode>. Target: <target>. Fixed X issues. Skipped Y issues (outside scope).
```

---

## 🛡️ Preventive Mode

> **Check code against known patterns BEFORE running a BrowserStack scan.**
> Full details in **[preventive-workflow.md](preventive-workflow.md)**.

### Quick Overview

Preventive mode scans your code against prevention patterns from `custom-fixes.md`
to catch accessibility issues during development — before they appear in BrowserStack scans.

**Key concepts:**
- Each **WCAG criterion** is treated as one reviewable unit (all violations under that criterion grouped together)
- Summary of all applicable WCAG criteria is **always displayed first** before fixing
- User chooses: **Fix all** (all criteria) or **one-by-one** (one criterion at a time)

**Key differences from standard workflow:**
- ❌ No BrowserStack credentials needed
- ❌ No scan results required
- ✅ Works offline with local pattern matching
- ✅ Supports GitFlow and worktree options
- ✅ Review at WCAG criterion level (not individual violations)

### Triggering Preventive Mode

```
fixadaissues preventive <path>
```

**Path options:**
- `.` — Check uncommitted changes
- `src/views/` — Check specific folder
- `file.cshtml` — Check specific file
- Omit path — Check all uncommitted files

### Branch Strategy Options

Unlike standard workflow, preventive mode offers **three options**:
1. **git worktree** — Parallel branches (1-4 worktrees)
2. **single branch** — One branch for fixes
3. **no branch** — Fix in current branch or just report

### When to Use

✅ Before committing new code  
✅ During feature development  
✅ Before requesting QA review  
✅ On large UI refactors  
✅ When learning accessibility patterns  

### Full Documentation

See **[preventive-workflow.md](preventive-workflow.md)** for complete details including:
- All workflow phases (0-6)
- GitFlow integration
- Worktree distribution
- Pattern matching logic
- Report formatting
- Best practices and examples

---

## 📊 CSV/Excel Fix Tracking

> **Applies to `file` and `folder` modes only.**
> In these modes the target is a CSV/Excel file — it is both the **source of violations** (Phase 1)
> and the **tracking document** that receives the "Fix" column (Phase 4).
> Always update the provided file in-place. Never create a new file.

### "Fix" Column Setup

For each CSV/Excel file, inspect the first sheet/header row:

```
 State                          Action
 ─────────────────────────────  ─────────────────────────────────────────────────────────
 "Fix" column already exists    Use it as-is — do NOT add a duplicate column
 "Fix" column does not exist    Append "Fix" as a new column in the first empty column
```

- ✅ `[Excel] "Fix" column ready in <filepath>.`
- ❌ File locked / unreadable → `[Excel] Cannot open <filepath>: <error>. Fix tracking skipped for this file.`

### Row Matching

Match each row to a processed violation using the best available identifier column
(e.g., Rule ID, Element, Page/URL, Description). If no match can be made, leave the "Fix" cell blank.

### Fix Status Values

```
 Status                  When to write
 ──────────────────────  ──────────────────────────────────────────────────────────────
 Fixed                   Issue was found, a fix was applied, AND custom-fixes.md capture succeeded
 Skipped                 Issue element found but flagged out of scope
 Excluded (contrast)     Contrast issue — permanently excluded from this workflow
 Manual Review           No safe fix could be determined OR custom-fixes.md capture failed
 Not Found               Affected element could not be located in the codebase
```

> **🚨 Gating rule for file/folder modes:** Never write `Fixed` unless the corresponding
> knowledge-capture write to custom-fixes.md succeeded for that issue.

### Write Timing

All status values are written **once — at the end of Phase 4 (Verify)**, after all fixes are
complete. Do not write incrementally during Phase 3. Save after writing all rows.

### Tool Selection

Use the following approach in order of preference:

```
 Environment            Preferred tool
 ─────────────────────  ──────────────────────────────────────────────────────────────
 PowerShell available   ImportExcel module (handles .csv and .xlsx/.xls)
                        Auto-install: Install-Module ImportExcel -Scope CurrentUser -Force
 Python available       openpyxl for .xlsx/.xls; built-in csv module for .csv
                        Auto-install: pip install openpyxl
 Neither available      [Excel] No supported tool found. Log fix statuses in chat only.
```

> **Never use COM/Excel interop automation** — it requires desktop Excel and is unreliable
> in automated or headless environments.

---

## 🌿 Branch Strategy Selection

> **🚨 This happens before Phase 0.** Do not begin prerequisite checks until the branch strategy is confirmed.
> For `file`, `folder`, and `results` modes only `git worktree` and `single branch` are valid.
> `current branch` is available **in preventive mode only**.

**Log for file / folder / results mode:**

```
[Setup] Branch strategy required before Phase 0.
[Setup] Do you want to use:
  1. git worktree
  2. single branch
```

**Log for preventive mode** (see [preventive-workflow.md](preventive-workflow.md)):

```
[Setup] Branch strategy required before Phase 0.
[Setup] Do you want to use:
  1. git worktree
  2. single branch
  3. current branch
```

Handle the response as follows:

- `git worktree` → Ask: `[Setup] How many worktrees should be created? Enter a value from 1 to 4.` Then start Phase 0 after the count is confirmed.
- `single branch` → Set the workflow to single-branch mode and start Phase 0 immediately.
- `current branch` (preventive mode only) → Set the workflow to current-branch mode and start Phase 0 immediately.
- If `current branch` is selected in `file`, `folder`, or `results` mode → `[Setup] Invalid selection. Please choose option 1 (git worktree) or option 2 (single branch).` Repeat until a valid option is chosen.
- Unclear answer → Repeat the question once and stop until the user selects one option.

Branch strategy state to carry forward:

```
branch_strategy = current-branch | single-branch | git-worktree
worktree_count = 0 when current-branch or single-branch, otherwise 1..4
has_remote_tracking = false (set to true if current-branch mode and current branch has remote)
```

Optional help link for users who want background on git worktrees:

`https://www.youtube.com/watch?v=s4BTvj1ZVLM`

---

## ⚙️ Prerequisites — Auto-Setup

> **🚨 Always run these checks before starting the workflow. Do NOT skip this section.**
> Start this section only after the branch-strategy question above has been answered.

---

### Step 0 — PowerShell 7 (pwsh) Availability

Log: `[Setup] Checking for PowerShell 7 (pwsh)...`

Run in the terminal:

```powershell
powershell.exe -Command "Test-Path 'C:\Program Files\PowerShell\7\pwsh.exe'"
```

```
 Result    Action
 ───────   ──────────────────────────────────────────────────────────────────
 True      [Setup] pwsh (PowerShell 7) detected. Proceeding with pwsh.
 False     [Setup] pwsh not found. Attempting automatic installation via winget...
```

If `pwsh` is not found, immediately run the following command to install it:

```powershell
powershell.exe -Command "winget install --id Microsoft.PowerShell --source winget --silent --accept-package-agreements --accept-source-agreements"
```

- ✅ Install succeeds → `[Setup] PowerShell 7 installed successfully. Please restart your terminal, then re-run the workflow.` **Stop immediately.**
- ❌ Install fails (winget unavailable or error) → `[Setup] Automatic installation of PowerShell 7 failed. Please install it manually before continuing.` **Stop immediately.**

Show the manual install fallback if winget is unavailable:

```powershell
# Manual install — MSI installer
# Download from: https://aka.ms/powershell-release
# After installing, restart your terminal and re-run the workflow.
```

Handle Step 0 outcomes as follows:

- If `pwsh` is already present, continue to Step 1.
- If `pwsh` is absent and the automatic install succeeds, this step is a **hard failure** — stop and ask the user to restart their terminal and re-run the workflow. Do not continue to Step 1.
- If `pwsh` is absent and the automatic install fails, this step is a **hard failure** — stop immediately. Do not continue to Step 1 or any later step.

---

### Step 1 — Environment Variables

> **🚨 Only for results mode.** This step is skipped for `file` or `folder` modes
> because BrowserStack credentials are only needed to fetch scan results via MCP.

**Step 1a — Mode Check**

Log: `[Setup] Checking mode to determine environment variable requirement...`

- If mode is `file` or `folder` → `[Setup] Mode is file/folder. Skipping environment variable check.` → Jump to Step 2a.
- If mode is `results` → Continue to Step 1b.

**Step 1b — Environment Variables (results mode only)**

Log: `[Setup] Checking required environment variables...`

```
 Variable                    Required    Purpose
 ─────────────────────────   ────────    ───────────────────────────
 BROWSERSTACK_USERNAME       ✔ Yes       BrowserStack account username
 BROWSERSTACK_ACCESS_KEY     ✔ Yes       BrowserStack account access key
```

Read each variable from the shell using the correct command for the detected OS:

```
 OS                              Command to read a variable
 ─────────────────────────────   ────────────────────────────────────────────────────────
 Windows (pwsh, if available)    $env:BROWSERSTACK_USERNAME
                                 $env:BROWSERSTACK_ACCESS_KEY
 Windows (powershell.exe)        $env:BROWSERSTACK_USERNAME
                                 $env:BROWSERSTACK_ACCESS_KEY
 macOS / Linux                   echo $BROWSERSTACK_USERNAME
                                 echo $BROWSERSTACK_ACCESS_KEY
```

**Shell on Windows:** Use `pwsh` (PowerShell 7) if available (determined in Step 0); otherwise fall back to `powershell.exe` (Windows PowerShell 5.1).

**Critical: no workarounds if the shell check itself fails.**
If the shell command cannot be run at all (e.g. no shell is available), this is a hard failure.
Do NOT switch to file inspection, code reading, or any other alternative approach to infer the variable values.
Do NOT continue. Tell the user which shell command failed and stop.

- ✅ Set → `[Setup] Found <VAR_NAME> in environment.`
- ❌ Missing → `[Setup] Missing <VAR_NAME>. Please set it before continuing.`
- ❌ Shell unavailable → `[Setup] Could not run shell check. Please verify your terminal has powershell.exe or a POSIX shell available.` **Stop immediately.**

Handle Step 1b outcomes as follows:

- If either variable is missing, this step is a hard failure.
- If the shell check itself cannot be executed, this step is a hard failure.
- Do not continue to Step 2 or any later step.
- Do not switch to file inspection, reading source code, or any other method to proceed without confirmed env vars.
- Do not create or modify `.vscode/mcp.json`.
- Do not attempt Git, scan, fix, review, or GitFlow actions.
- End the workflow immediately after printing the failure message and the setup commands below.

If either is missing, print the message(s), show the setup commands, and **stop immediately**:

```bash
# PowerShell
$env:BROWSERSTACK_USERNAME="your_username"
$env:BROWSERSTACK_ACCESS_KEY="your_access_key"

# macOS / Linux
export BROWSERSTACK_USERNAME=your_username
export BROWSERSTACK_ACCESS_KEY=your_access_key
```

On all set: `[Setup] All required environment variables are set.`

Only continue to Step 2 when:
- Mode is `file` or `folder` (skip Step 1b automatically), or
- Mode is `results` and both required environment variables are present.

---

### Step 2 — BrowserStack MCP Server Configuration

> **🚨 Only for results mode.** This step is skipped for `file` or `folder` modes
> because the user provides or loads violations directly, not via BrowserStack MCP.

**Step 2a — Mode Check**

Log: `[Setup] Checking mode to determine MCP configuration requirement...`

- If mode is `file` or `folder` → `[Setup] Mode is file/folder. Skipping MCP configuration.` → Jump to Step 3.
- If mode is `results` → Continue to Step 2b.

**Step 2b — MCP Configuration (results mode only)**

Log: `[Setup] Checking for BrowserStack MCP configuration in .vscode/mcp.json...`

Read `.vscode/mcp.json` in the project root and handle each state:

```
 State                              Action
 ──────────────────────────────     ──────────────────────────────────────────────
 File does not exist                Create it with the template below
 File exists, no "browserstack"     Merge the entry into "servers" (preserve others)
 File exists with "browserstack"    Skip — already configured
 File exists but invalid JSON       STOP — hard failure
```

**MCP Configuration Template** (use `${env:…}` so credentials are never hardcoded):

```json
{
  "servers": {
    "browserstack": {
      "command": "npx",
      "args": ["-y", "@browserstack/mcp-server@latest"],
      "env": {
        "BROWSERSTACK_USERNAME": "${env:BROWSERSTACK_USERNAME}",
        "BROWSERSTACK_ACCESS_KEY": "${env:BROWSERSTACK_ACCESS_KEY}"
      }
    }
  }
}
```

> **🔒 Never hardcode real credential values in the file or in chat output.**

Handle Step 2b outcomes as follows:

> **⚠️ This step requires the agent to write files directly — do NOT instruct the user to create or edit files manually. Use your file-creation and file-edit tools to perform all writes yourself.**

- **If `.vscode/mcp.json` does not exist:**
  - Print: `[Setup] .vscode/mcp.json not found. Creating it now...`
  - **ACTION REQUIRED:** Use your file-creation tool to write `.vscode/mcp.json` at the workspace root with exactly the template content shown above. Do not ask the user to create it.
  - On success: `[Setup] .vscode/mcp.json created. BrowserStack MCP server configured.` Continue to Step 3.
  - On write failure: `[Setup] Failed to create .vscode/mcp.json: <error>.` **Stop immediately.**

- **If `.vscode/mcp.json` exists but does not contain a `"browserstack"` key under `"servers"`:**
  - Print: `[Setup] BrowserStack MCP entry missing. Adding it to .vscode/mcp.json...`
  - **ACTION REQUIRED:** Use your file-edit tool to merge the `"browserstack"` object into the `"servers"` section (create `"servers"` if absent). Preserve all other existing content.
  - On success: `[Setup] BrowserStack MCP server entry added to .vscode/mcp.json.` Continue to Step 3.
  - On parse or write failure: `[Setup] Failed to update .vscode/mcp.json: <error>.` **Stop immediately.**

- **If `.vscode/mcp.json` exists but is not valid JSON:**
  - Print: `[Setup] Failed to parse .vscode/mcp.json — invalid JSON. Please fix the file before continuing.`
  - **Stop immediately.** Do not continue to Step 3 or any later step.

- **If the `"browserstack"` entry is already present and the file is valid:**
  - Print: `[Setup] BrowserStack MCP server already configured in .vscode/mcp.json.`
  - Continue to Step 3.

---

### Step 3 — Node.js Availability

Log: `[Setup] Checking Node.js version...`

Run `node --version`. **Node >= 18.0** is required.

- ✅ `>= 18` → `[Setup] Node.js vX.X.X detected. Meets minimum requirement.`
- ❌ `< 18` → `[Setup] Node.js version is below the minimum requirement. Node.js 18+ is required.`
- ❌ Not found → `[Setup] Node.js is not installed. Please install Node.js 18+.`

Handle Step 3 outcomes as follows:

- If Node.js is missing or the version is below 18.0, this step is a hard failure.
- Do not continue to Step 4 or any later step.
- Do not continue to Git checks, scan loading, fixing, review, or GitFlow.
- End the workflow immediately after printing the failure message and the upgrade/install guidance below.

If Node.js is missing or below 18.0, show the guidance below and **stop immediately**:

```bash
# Windows
nvm install 22.15.0
nvm use 22.15.0

# macOS
brew upgrade node   # or:
nvm install 22.15.0
nvm use 22.15.0
```

Only continue to Step 4 when Node.js 18.0 or later is available.

---

### Step 4 — MCP Server Verification

> **🚨 Only for results mode.** This step is skipped for `file` or `folder` modes
> because scan results are provided directly by the user or via Phase 1 loading,
> not fetched from BrowserStack via MCP.

**Step 4a — Mode Check**

Log: `[Setup] Checking mode to determine MCP verification requirement...`

- If mode is `file` or `folder` → `[Setup] Mode is file/folder. Skipping MCP verification.` → Jump to Step 5.
- If mode is `results` → Continue to Step 4b.

**Step 4b — MCP Server Verification (results mode only)**

Log: `[Setup] Verifying BrowserStack MCP server can start...`

```
 Step   Action                                          Pass / Fail
 ────   ──────────────────────────────────────────────   ────────────────────────────
  1     Run npx -y @browserstack/mcp-server@latest      ✅ Package verified
        (kill after confirmation)                        ❌ Failed to start → STOP

  2     Check if VS Code Copilot loaded the MCP tools   (log message)

  3     Call accessibilityExpert with trivial request    ✅ Tools active & responding
                                                         ❌ Not responding → ask user
                                                            to reload VS Code, retry
```

Handle Step 4b outcomes as follows:

- If the MCP package fails to start, VS Code Copilot does not load the MCP tools, or `accessibilityExpert` is not responding, this step is a hard failure.
- Do not continue to Step 5 or any later step.
- Do not continue to scan loading, issue fixing, review, or GitFlow.
- Tell the user exactly which verification sub-step failed.
- If the failure is a tool-loading or response issue, ask the user to reload VS Code and retry.
- End the workflow immediately after printing the failure message.

Only continue past Step 4 (to Step 5) when:
- Mode is `file` or `folder` (skip Step 4b automatically), or
- Mode is `results` and all three verification sub-steps succeed.

---

### Step 5 — Git Configuration

Log: `[Setup] Checking Git configuration...`

Run `git config user.name` and `git config user.email`:

```
 Check         Pass                                      Fail
 ───────────   ───────────────────────────────────────    ──────────────────────────
 Identity      [Setup] Git user: <name> (<email>).       Show git config commands → STOP
 Repository    [Setup] Git repository detected.          GitFlow phase will be skipped
 Remote        [Setup] Git remote found: <url>.          GitFlow phase will be skipped
```

Store the `user.name` value — it is used later to construct the branch name.

**Step 5a — Remote Tracking Check (Current-Branch Mode)**

Only if `branch_strategy = current-branch`:
- Run `git rev-parse --abbrev-ref --symbolic-full-name @{u}` to check for remote tracking branch
- If remote tracking branch exists:
  - Set `has_remote_tracking = true`
  - Log: `[Setup] Current branch has remote tracking branch. Commit and push will be available; PR creation will be skipped.`
- If no remote tracking branch:
  - Set `has_remote_tracking = false`
  - Log: `[Setup] Current branch has no remote tracking branch.`

Handle Step 5 outcomes as follows:

- If `user.name` or `user.email` is missing, this step is a hard failure.
- Show the required `git config` commands, then **stop immediately**.
- Do not continue to Step 6 or any later phase.
- Do not load scan results, apply fixes, run review, or attempt GitFlow.

- If the directory is not a Git repository, this is not a hard failure.
- Log that GitFlow will be skipped, then continue with the accessibility workflow.

- If the repository has no remote, this is not a hard failure.
- Log that GitFlow will be skipped, then continue with the accessibility workflow.

Only continue to Step 6 when Git identity is configured.
If GitFlow is skipped because the repository or remote is missing, skip Steps 6 and 7 and continue directly into Phase 1.

---

### Step 6 — Branch Creation

Log: `[Setup] Constructing branch name(s) and creating branch(es)...`

> **🚨 All branches are created here — before any fixes are applied.**
> Use the branch strategy selected before Phase 0:
> - `current branch` (preventive mode only) → Skip branch creation entirely; work in the current branch.
> - `single branch` → create one branch in the current repository and do not create a separate worktree directory.
> - `git worktree` → create one or more worktrees as selected by the user.

#### Branch Name Convention

Derive `<git-username>` from `user.name` (lowercase, spaces/special chars → hyphens).

```
 Strategy        File mode branch name                  Folder mode branch name                Results mode branch name
 ─────────────   ───────────────────────────────────    ───────────────────────────────────    ───────────────────────────────────
 single branch   <user>/ada-fix-<filename-no-ext>      <user>/ada-fix-<foldername>           <user>/ada-fix-<YYYYMMDD-HHmmss>
 git worktree    <user>/ada-fix-<filename-no-ext>-wt<N> <user>/ada-fix-<foldername>-wt<N>    <user>/ada-fix-<YYYYMMDD-HHmmss>-wt<N>
```

> **Note:** `current branch` does not apply to file / folder / results modes and is excluded from this table. See [preventive-workflow.md](preventive-workflow.md) for preventive-mode branch naming.

---

#### Current-Branch Mode (Preventive Mode Only)

> **⚠️ This branch strategy is only available in preventive mode.** It is not a valid choice when mode is `file`, `folder`, or `results`.

If the user selected `current branch`, skip branch creation entirely:

- ✅ `[Setup] Current-branch mode selected. Working in current branch: <current-branch-name>.`
- No `git checkout -b` command is run
- All fixes will be applied directly to the current branch
- Git actions in Phase 6 are adjusted based on `has_remote_tracking` flag

Store context for later phases:

```
worktrees = [
  { path: ".", branch: "<current-branch-name>", issues: [] }
]
```

#### Single-Branch Mode

If the user selected `single branch`, create exactly one branch in the current repository:

```bash
git checkout -b <branch-name>
```

- ✅ `[Setup] Single-branch mode selected. Created branch <branch-name> in the current repository.`
- ❌ `[Setup] Failed to create branch <branch-name>: <error>.` **Stop immediately.**

Store context for later phases:

```
worktrees = [
  { index: 1, path: <current-repo-path>, branch: <branch-name>, issues: [] }
]
```

#### Worktree Mode

If the user selected `git worktree`, use the confirmed worktree count from Branch Strategy Selection.

**Step 6a — Prerequisite Check** *(runs before any worktree or branch is created — hard stop on any failure)*

```bash
git --version
git worktree list
```

```
 Check               Pass                                              Fail (hard stop — no branches created)
 ─────────────────   ───────────────────────────────────────────────   ──────────────────────────────────────────
 git --version       [Setup] Git vX.Y.Z detected. Worktrees supported. [Setup] git not found. Install Git 2.5+.
                     Minimum required: 2.5                             STOP immediately.
 git worktree list   [Setup] git worktree subcommand is available.     [Setup] git worktree unavailable: <err>.
                                                                        STOP immediately.
```

- N must be between 1 and 4. If N < 1, stop immediately: `[Setup] git worktree mode requires N ≥ 1. Please re-run with a valid value.`
- If N > 4, cap at 4 and log: `[Worktree] Capped at 4 worktrees (maximum supported).`
- If N = 1, always use `git worktree add` — do NOT skip worktree creation. Follow Step 6b normally for the single worktree.

**Step 6b — Create Worktrees**

Derive `<repo-name>` from the current working directory name.

```
 Worktree   Path                            Branch name
 ────────   ─────────────────────────────   ─────────────────────────────────────────────
 1          ../<repo-name>-ada-wt1/         <user>/<flow-name>-wt1
 2          ../<repo-name>-ada-wt2/         <user>/<flow-name>-wt2
 3          ../<repo-name>-ada-wt3/         <user>/<flow-name>-wt3
 4          ../<repo-name>-ada-wt4/         <user>/<flow-name>-wt4
```

For each worktree index 1 through N:

```bash
git worktree add ../<repo-name>-ada-wt<N> -b <branch-name>
```

- ✅ `[Worktree] Created worktree <N>: path=../<repo-name>-ada-wt<N>, branch=<branch-name>.`
- ⚠️ Branch name already exists → Append `-2`, `-3` to branch name and retry once.
- ⚠️ Path already exists → Append `-a`, `-b` to path and retry once.
- ❌ Hard failure → `[Worktree] Failed to create worktree <N>: <error>.`
  Run `git worktree remove <path> --force` for each already-created worktree, then **stop immediately**.

Log when all created: `[Worktree] <N> worktrees and branches ready.`

Store context for Phases 2, 3, and 6:

```
 worktrees = [
   { index: 1, path: <path1>, branch: <branch1>, issues: [] },
   { index: 2, path: <path2>, branch: <branch2>, issues: [] },
   ...
 ]
```

> Issues are distributed into `worktrees[].issues` at the end of Phase 2 (Triage), after severity is known.

---

Handle Step 6 outcomes as follows:

- **Single-branch mode:** create one branch in the current repository and continue without creating a separate worktree path.
- **Git worktree mode with N < 1:** stop immediately with the error message above.
- **Step 6a:** any prerequisite check failure → hard failure, no branches created. **Stop immediately.**
- **Step 6b:** any `git worktree add` failure → remove all created worktrees and **stop immediately.**
- Only continue to Step 7 when the selected branch strategy has been created successfully.

---

### Step 7 — Bitbucket MCP Server

Follow the full setup, auto-configure, and start procedure in
**[bitbucket-setup.md](bitbucket-setup.md)**.

That file handles the entire lifecycle automatically:

```
 1. Locate  ──▶  2. Auto-configure  ──▶  3. Verify  ──▶  4. Auto-recover  ──▶  5. Fallback
 ────────────    ─────────────────────    ───────────     ─────────────────     ────────────
 Find config    Check uvx/pip, install   Lightweight     Reload, fix config,   Branch+push
 in .cursor/    if needed, write config  tool call       reinstall, retry      only; PR manual
 mcp.json       and pre-warm package     to confirm      until working         if all fails
```

Handle Step 7 outcomes as follows:

- If Bitbucket MCP starts and verifies successfully, continue normally.
- If Bitbucket MCP cannot be configured or verified after the documented recovery steps, this is not a hard failure for the accessibility workflow.
- Log that Bitbucket MCP is unavailable and that automatic PR creation will be skipped.
- Continue with scan loading, fixing, verification, and review.
- In Phase 6 GitFlow, allow commit, push, and push if Git is available, but require manual PR creation when Bitbucket MCP remains unavailable.

Step 7 is complete when either:

- Bitbucket MCP is working, or
- Bitbucket MCP has been conclusively marked unavailable and the manual-PR fallback has been logged.

Log: `[Setup] Prerequisite checks complete. Starting accessibility workflow.`

---

## 📥 Phase 1 — Load Scan Results

All 3 modes work with **existing** BrowserStack scan results. No new scan is triggered.

**Input sources:**

```
 Source              Mode              Description
 ─────────────────   ───────────────   ────────────────────────────────────────────────────
 CSV/Excel file      file / folder     Read row-by-row from the target CSV/Excel file(s)
 Pasted text         any               Violations copied directly into the chat
 Results URL         results           BrowserStack results URL
 File attachment     any               Exported scan report
```

> **File and folder modes:** Open each CSV/Excel file and read every row as a violation.
> Each row's columns (Rule ID, Element, Page, Description, etc.) map to the violation fields below.
> Rows that cannot be parsed as violations are skipped and logged.
>
> **⚠️ Skip already-fixed issues:** If the CSV/Excel file contains a "Fix" column and a row
> has "Fixed" in that column, skip that row entirely — it was already processed in a previous run.
> Log each skipped row: `[Scan] Skipping row X — already marked as Fixed.`

Log: `[Scan] Loading existing scan results...`

**Extract from each violation:**

```
 Field                Example
 ────────────────     ──────────────────────────────────────
 Rule ID              image-alt, color-contrast, label
 WCAG criterion       1.1.1, 1.4.3, 4.1.2
 Affected element     CSS selector or description
 Current state        What's wrong
 Page / URL           Where the issue occurs
 Suggested fix        If provided by BrowserStack
 Fix status           Check "Fix" column if present; skip if "Fixed"
```

- ✅ Success → `[Scan] Loaded X violations (Y already fixed, skipped). X of remaining include a suggested fix.`
- ❌ Failure → `[Scan] Could not load scan results. Please provide them.` **Stop.**

Handle Phase 1 outcomes as follows:

- If scan results are loaded successfully, continue to Phase 2.
- If scan results cannot be loaded from the provided source, stop immediately after printing the failure message.
- Do not continue to triage, fixing, verification, review, or GitFlow without loaded scan results.

---

## 🔍 Phase 2 — Triage & Prioritize

Log: `[Triage] Analyzing scan results and organizing by severity...`

```
 Priority     WCAG Level    Impact                    Fix Order
 ──────────   ──────────    ────────────────────────   ─────────
 🔴 Critical  Level A       Blocks access entirely     First
 🟠 Major     Level A       Significant barrier        First
 🟡 Moderate  Level AA      Usability problem          Second
 🟢 Minor     Level AAA     Enhancement                Last
```

**MANDATORY STEP — Contrast Exclusion:**

Before grouping, **remove ALL contrast-related issues** (see Ignored Issue Categories above):

1. **Check EVERY violation** for these keywords (case-insensitive) in Rule ID or Description:
   - `color-contrast`
   - `color-contrast-enhanced`
   - `contrast`
   - `non-text-contrast`

2. **Log EACH excluded issue:**
   ```
   [Triage] Skipping "<rule-id>" -- contrast issues are excluded from this workflow.
   ```

3. **DO NOT include contrast issues** in Critical/Major/Moderate/Minor groups.

4. **Verify exclusion** before continuing: If any contrast issues remain in the fix queue after this step, STOP and re-filter.

Group remaining issues (e.g., all missing alt texts, all form label failures)
to batch fixes efficiently.

```
[Triage] Issue breakdown:
  🔴 Critical:            X issues (Level A violations)
  🟠 Major:               X issues (must fix)
  🟡 Moderate:            X issues (should fix)
  🟢 Minor:               X issues (nice to fix)
  ✅ Already Fixed:       X issues (skipped — marked Fixed in CSV/Excel)
  ⛔ Excluded (contrast): X issues (skipped — contrast issues ignored)
```

Log: `[Triage] Prioritization complete. Starting fixes with Critical issues first.`

> **🌿 Worktree mode — Issue Distribution:** Now that severities are known, distribute issues
> into the worktrees created in Setup Step 6. Update `worktrees[].issues` for each worktree:
>
> ```
>  Worktree   Default scope
>  ────────   ───────────────────────────────────────────────────────────
>  1          🔴 Critical issues
>  2          🟠 Major issues
>  3          🟡 Moderate issues
>  4          🟢 Minor issues
> ```
>
> Rules: if severity groups < N, combine adjacent groups; if a group is too large, use
> round-robin; if total fixable issues < N, reduce active worktrees accordingly.
>
> Log: `[Worktree] Issue distribution: wt1=X Critical, wt2=Y Major, wt3=Z Moderate, wt4=W Minor`

---

## 🔧 Phase 3 — Fix Issues

> **⚠️ CRITICAL — Contrast Filter Checkpoint**
> Before starting any fixes, verify that ALL contrast-related issues were removed in Phase 2.
> If any contrast issues remain in the fix queue, STOP and log:
> `[Fix] ERROR: Contrast issues detected in fix queue. These should have been excluded in Phase 2.`
> Then remove them immediately and restart Phase 3.

> **⚠️ Respect the target scope** from the selected mode.
> Only fix issues in files that fall within the user's specified file or folder.

> **🌿 Worktree mode — MANDATORY LOGGING:**
> Work through each worktree in sequence (1 → N). For each worktree:
>
> **REQUIRED logs for each worktree (print ALL of these):**
> ```
> [Worktree] Switching to worktree <N>: <path> (branch: <branch-name>)
> [Fix] Processing X issues assigned to worktree <N> (Severity: <severity>)
> [Fix] Issue 1/<X>: "<rule-id>" (WCAG <criterion>) -- Searching...
> [Fix] Found affected element in <file> at line <N>.
> [Fix] Applying <fix-source> fix for "<rule-id>" in <file>...
> [Fix] Fixed "<rule-id>" in <file>. (<M of X> issues resolved)
> ...(repeat for each issue)...
> [Worktree] Completed worktree <N>. Fixed M of X assigned issues.
> ```
>
> - Apply only the issues assigned to that worktree (from Setup Step 6)
> - All file edits must be made inside that worktree's path
> - Use `git -C <worktree-path>` for any git commands within that worktree
> - Print progress logs for EVERY issue (do not batch silently)

---

### Review Cadence (Workflow-Specific)

**Standard Workflow** (file/folder/results mode):
- Process **all issues automatically** without asking
- Work through the queue by severity (Critical → Major → Moderate → Minor)
- Review happens at the end in Phase 5
- Skip directly to Step 1 below

**Preventive Workflow** (preventive mode):
- Ask the user to choose review cadence **before** fixing issues
- See **[preventive-workflow.md](preventive-workflow.md)** Phase 3 Step 0 for full details

For preventive mode, ask once before fixing:

```
[Fix] How would you like to proceed?
  1. Fix all issues (all WCAG criteria)
  2. Fix one by one (one WCAG criterion at a time)
  3. Stop
```

Response handling:
- `Fix all issues` → Process all WCAG criteria automatically without stopping. Review happens at end (Phase 5).
- `Fix one by one` → Fix one WCAG criterion (all its violations), review it, then ask: "Next criterion? / Commit and next? / Push and next? / PR and next? / Stop?" Repeat for each criterion.
- `Stop` → End workflow with no edits.

**Important**: In preventive mode, each **WCAG criterion** is treated as one reviewable unit. All violations under the same criterion are fixed together, then reviewed as a batch.

State to carry forward:

```
review_cadence = one-by-one | fix-all
```

**One-by-one mode** provides:
- Per-criterion approval gates (all violations in that criterion reviewed together)
- Smaller incremental commits (one commit per WCAG criterion)
- Natural stopping points (after each criterion)
- Option to create PRs after each approved criterion

See **[preventive-workflow.md](preventive-workflow.md)** Phase 3 for complete workflow including the always-displayed summary and detailed one-by-one/fix-all flows.

---

For each issue, follow these three steps:

---

### Step 1 — Identify the Source File

Log: `[Fix] Issue: "<rule ID>" (WCAG <criterion>) -- Searching for affected element...`

Search the codebase for the element/component from the scan result.
If a scope is set, restrict the search.

- ✅ Found (in scope) → `[Fix] Found affected element in <file> at line <N>.`
- ⏭️ Found (outside scope) → `[Fix] Skipping "<rule ID>" -- outside target scope.` → Next issue.
- ❓ Not found → `[Fix] Could not locate element for "<rule ID>". Asking user for help.`

Handle Step 1 outcomes as follows:

- If the affected element is found in scope, continue to Step 2 for that issue.
- If the affected element is found outside scope, skip that issue and continue to the next issue.
- If the affected element cannot be located, ask the user for help and pause work on that issue until clarified.

---

### Step 2 — Apply the Fix

Fixes are applied in **priority order:**

**① Custom fix** — Check **[custom-fixes.md](custom-fixes.md)** first. This file contains
project-specific fixes that have been verified to resolve issues where BrowserStack
suggested fixes or generic patterns were insufficient.

Custom-fix lookup must be organized by **WCAG Criterion first**, then narrowed by
Rule ID, pattern scope, and contextual notes within that criterion section.
There must be only **one top-level knowledge-base section per WCAG Criterion**.
When multiple pattern scopes, rule IDs, or solution variants apply to the same criterion,
update that existing criterion section instead of creating a duplicate section.

```
[Fix] Found custom fix for "<rule ID>" in custom-fixes.md. Applying custom fix in <file>...
```

> **Why custom fixes take highest priority:**
> BrowserStack suggestions and generic WCAG patterns sometimes produce fixes that
> do not actually resolve the violation on re-scan. Custom fixes are recorded only
> after they have been proven to work, making them the most reliable source.

If no matching entry exists in `custom-fixes.md`, fall through to the next source.

```
[Fix] No custom fix found for "<rule ID>". Checking BrowserStack suggestion...
```

**② BrowserStack suggested fix** — Use the suggestion from the scan result if one is provided.

```
[Fix] Applying BrowserStack suggested fix for "<rule ID>" in <file>...
```

**③ Generic fix pattern** — Fall back to patterns in **[wcag-fix-patterns.md](wcag-fix-patterns.md)**:

```
 Category          Fix
 ────────────────  ──────────────────────────────────────────────────────
 Images            Add meaningful alt attributes
 Contrast          ⛔ SKIP — contrast issues are excluded from this workflow
 Forms             Add <label>, aria-label, or aria-labelledby
 Keyboard          Add tabindex, focus styles, keyboard handlers
 Headings          Fix heading hierarchy (no skipped levels)
 Links/Buttons     Add descriptive text, avoid "click here"
 ARIA              Add/fix role, aria-* attributes
 Language          Add lang attribute to <html>
 Tables            Add <caption>, <th>, scope attributes
```

```
[Fix] No suggested fix from BrowserStack. Using generic pattern for "<rule ID>"...
```

After applying: `[Fix] Fixed "<rule ID>" in <file>. (<N of total> issues resolved)`

> **📝 Recording new custom fixes:**
> When a BrowserStack suggestion or generic pattern fails to resolve a violation on
> re-scan, and a working fix is later found (manually or via `accessibilityExpert`),
> add the fix to **[custom-fixes.md](custom-fixes.md)** using the template in that file.
> This ensures the fix is available for future runs.

Handle Step 2 outcomes as follows:

- If a valid fix is applied (from any priority level), continue to Step 3 when validation is needed, or continue to the next issue.
- If no safe fix can be determined from custom fixes, the BrowserStack suggestion, or generic patterns, do not guess.
- Ask the user for clarification or use `accessibilityExpert` before continuing with that issue.
- If a working fix is discovered that was not in `custom-fixes.md`, record it there for future use.

---

### Step 3 — Validate (if uncertain)

Use the `accessibilityExpert` tool when the correct fix is not obvious.

```
[Fix] Consulting BrowserStack A11y Expert to validate "<rule ID>"...
[Fix] A11y Expert confirmed the fix is correct.
```

Handle Step 3 outcomes as follows:

- If validation confirms the fix, continue to Step 4 for knowledge capture.
- If validation raises concerns or the correct fix remains unclear, stop work on that issue, log that manual review is required, and continue with other issues when possible.

---

### Step 4 — Capture Knowledge (automatic)

> **🔄 MANDATORY — Automatically record every fix to custom-fixes.md for future reuse.**
> This step is NOT optional. It runs after **every** successful fix, without exception.
> The workflow MUST write to custom-fixes.md before continuing to the next issue.

**Capture Criteria** — **Always capture with idempotent write behavior.**

Every fix applied during Phase 3 must be reconciled with custom-fixes.md, regardless of whether it came from a custom override, a generic WCAG pattern, a BrowserStack suggestion, or any other source.

Use this exact write policy for the matching WCAG Criterion:

1. **Criterion section exists and content changed** -> **Update the existing criterion section** in place.
2. **Criterion section exists and content unchanged** -> **Skip write** (no-op) and log "no changes needed".
3. **Criterion section missing** -> **Append one new criterion section** under "Verified Custom Fixes".

Within a criterion section, keep a single consolidated record that may include:
- multiple Rule IDs for that criterion
- multiple reusable pattern scopes or modules
- multiple proven solution variants
- additional notes, prevention guidance, and product tags

Do not create a second top-level section for the same WCAG Criterion just because the
Rule ID, pattern scope, file, module, or code sample differs.

**What Gets Captured:**

```
 Field                   Source
 ─────────────────────   ──────────────────────────────────────────────
 WCAG Criterion          From violation
 Rule ID(s)              From violation; append to existing criterion section as needed
 Pattern Scope           Derived from reusable issue class or UI pattern
 Product Tags            Auto-detected from file path (e.g., [Enterprise], [Permit])
 Date Added              Auto-generated (current date)
 Verified In             Current product/module name
 Why Standard Fix Failed Detected reason (BS failed / generic modified / etc.)
 Fix / Solution Variants Actual code change made; keep multiple variants in same criterion section when needed
 Prevention              Auto-generated guidance based on the fix
 Notes                   Optional — ask user if they want to add context
```

**Capture Process:**

Log: `[Fix] Capturing fix to knowledge base...`

> **🌿 Worktree mode:** Always write to `.github/skills/browserstack-ada-fix/custom-fixes.md`
> relative to the current worktree's root path. Verify the file is written to the worktree,
> not the main repository, by checking the worktree path before writing.

1. **Detect product context** from file path:
   ```
   Path pattern          Product Tag
   ───────────────────   ─────────────────
   Enterprise/*          [Enterprise]
   Permit/*              [Permit]
   AutomatedRouting/*    [AutoRouting]
   *.cshtml              [CSHTML] [MVC-Layout]
   *.css (wwwroot)       [CSS-Theme]
   GIS/*                 [ArcGIS]
   (default)             [TNMCS-Core]
   ```

2. **Locate or generate the criterion section** using the Entry Template from custom-fixes.md

3. **Apply idempotent write policy** to custom-fixes.md:
  - Update existing criterion section when changes are needed
  - Skip write when no changes are needed
  - Append a new criterion section only when the WCAG criterion is missing

4. **Consolidate within the criterion section**:
  - Merge new Rule IDs into the existing Rule ID list for that criterion
  - Add newly encountered reusable pattern scopes, modules, or framework contexts to the same section
  - Add additional proven solutions as separate variants/subsections inside the same criterion section
  - Preserve prior working solutions unless they are incorrect or superseded

5. **Ask user** (optional): `[Fix] Would you like to add notes about when/where this fix applies? (Enter text or press Enter to skip)`

6. **Confirm capture**: `[Fix] Knowledge captured for WCAG <criterion> (rule: "<rule ID>"). This fix will be prioritized in future runs.`

7. **Emit engineer alert**:
   ```
   ⚠️  REVIEW BEFORE COMMIT — custom-fixes.md was updated for WCAG <criterion>.
       Please open .github/skills/browserstack-ada-fix/custom-fixes.md, verify the Before/After
       code block(s), solution variants, and Notes are accurate, then stage the file alongside your fix before committing.
   ```

Handle Step 4 outcomes as follows:

- If custom-fixes.md was updated successfully, continue to the next issue.
- If custom-fixes.md update fails for an issue, treat that issue as `Manual Review`
  (do NOT count it as Fixed), log the failure, and continue with the next issue.
- Do not silently continue as if capture succeeded.
- All captured knowledge is validated and consolidated in Phase 4.

**Required per-issue log checkpoint:**

```
[Fix] Knowledge capture checkpoint for "<rule ID>":
  - wcag criterion section: <criterion>
  - custom-fixes.md action: <updated-existing-criterion|skipped-no-change|appended-new-criterion|failed>
  - Result status for this issue: <Fixed|Manual Review>
```

---

## ✅ Phase 4 — Verify

Log: `[Verify] All fixes applied. Generating summary report...`

**Fix Summary:**

```
 ┌───────────────────────────────────────────────┐
 │              FIX SUMMARY                      │
 ├───────────────────────────────────────────────┤
 │  Mode:      <file|folder|results>             │
 │  Target:    <filepath, folderpath, or URL>    │
 │───────────────────────────────────────────────│
 │  Total violations from scan:          X       │
 │  ✅ Fixed:                            Y       │
 │  ⏭️  Skipped (outside scope):         Z       │
 │  ⛔ Excluded (contrast):              C       │
 │  ❓ Could not fix (manual review):    W       │
 └───────────────────────────────────────────────┘
```

**Unfixed Issues** (if any):

Log: `[Verify] The following issues need manual review:`
List each with rule ID, WCAG criterion, and reason it was not fixed.

**Next Step:**

Log: `[Verify] To confirm all fixes, re-run the BrowserStack accessibility scan and compare results.`

Handle Phase 4 outcomes as follows:

- If all fixes are summarized successfully, continue to Phase 5.
- If any issues remain unfixed, list them explicitly as manual review items and still continue to Phase 5.
- Do not claim the accessibility work is fully verified until the user re-runs the BrowserStack scan.

**Required Phase 4 consistency check:**

- For every issue marked `Fixed`, verify there is a corresponding successful knowledge capture in custom-fixes.md.
- If a mismatch is found (fixed in code but not captured), downgrade that issue to `Manual Review`,
  log `[Verify] Downgraded issue to Manual Review because custom-fixes.md capture is missing.`,
  and include it in the manual-review list.

**CSV/Excel Update (file and folder modes only):**

Log: `[Excel] Writing "Fix" column to CSV/Excel file(s)...`

After generating the Fix Summary, open each CSV/Excel file used as input and update it
in-place following the rules in the CSV/Excel Fix Tracking section:

1. Ensure the "Fix" column exists (add if missing).
2. Match each row to its processed violation.
3. Write the applicable status value (`Fixed` / `Skipped` / `Excluded (contrast)` / `Manual Review` / `Not Found`).
4. Save the file.

- ✅ `[Excel] "Fix" column written to <filepath>. X rows updated.`
- ❌ `[Excel] Failed to write "Fix" column to <filepath>: <error>.`

Log: `[Excel] CSV/Excel fix tracking complete.`

---

### Knowledge Base Validation & Consolidation

> **🔄 MANDATORY — Review and consolidate fixes captured during Phase 3.**
> This step is NOT optional. It runs automatically at the end of Phase 4.
> This ensures custom-fixes.md remains clean and organized.

Log: `[Verify] Validating and consolidating knowledge base...`

**Step 1 — Review New Entries**

List all WCAG criterion sections added or updated in custom-fixes.md during this run:

```
[Verify] Captured X WCAG criterion section updates during this run:
  • <criterion-1> — <brief description>
  • <criterion-2> — <brief description>
  ...
```

If no criterion sections were added or changed: `[Verify] No WCAG criterion sections required changes during this run.`

**Step 2 — Consolidate Duplicates**

Check for duplicate WCAG Criterion sections in custom-fixes.md:

- If same WCAG Criterion appears multiple times, merge them into one criterion section
- Preserve different elements, rule IDs, contexts, and solution variants inside that single section
- Only keep multiple sections when the WCAG Criterion is genuinely different

Log consolidation results: `[Verify] Consolidated X duplicate WCAG criterion sections.`

**Step 3 — Update Statistics**

Update the "📊 Statistics" section in custom-fixes.md:

```
Last Updated: 2026-04-17
Total WCAG Criteria: 23
```

Log: `[Verify] Knowledge base statistics updated.`

**Step 4 — Confirm Completion**

Log: `[Verify] Knowledge base updated successfully. Custom-fixes.md now contains X WCAG criterion sections.`

Handle Knowledge Base Validation outcomes:

- Knowledge base validation MUST complete before continuing to Phase 5
- If file write fails, log the error, mark affected issues as `Manual Review`, and continue
- All changes are saved to custom-fixes.md before Phase 5

---

## 👁️ Phase 5 — Review & Approve

> **🚨 Do NOT proceed to Phase 6 unless the user explicitly approves.**

Log: `[Review] All fixes have been applied. Please review the changes before proceeding to GitFlow.`

Phase 5 is required even when GitFlow will later be skipped.
Approval means the user approves the code changes themselves, not necessarily GitFlow actions.

### Step 1 — Show Change Summary

Run `git diff --stat` to list every modified file with lines added/removed.

> **🌿 Worktree mode:** Run `git -C <worktree-path> diff --stat` for **each** worktree
> and display a combined summary, clearly labelled by worktree index and branch name.

If Git is unavailable for this step, provide a manual change summary instead of failing the review phase.

**Required review checkpoint:**

- If one or more issues were fixed in Phase 3, verify that each fixed issue has a successful knowledge-capture checkpoint.
- `custom-fixes.md` does **not** need to appear in the changed file list when all captures were `skipped-no-change` against existing WCAG criterion sections.
- `custom-fixes.md` **must** appear in the changed file list when any captures resulted in `updated-existing-criterion` or `appended-new-criterion`.
- Block approval only when a fixed issue is missing a successful capture result or when a criterion section should have changed but did not.
- If blocked, log:
  `[Review] Blocked: knowledge capture is incomplete for one or more fixed issues. Re-run knowledge capture before approval.`
- Do not offer Approve until this checkpoint passes.

```
[Review] Changed files:
  src/App.tsx                                      | +12 -3
  src/Login.tsx                                    | +8  -2
  styles/main.css                                  | +5  -1
  .github/skills/browserstack-ada-fix/custom-fixes.md | +15 -0
  ────────────────────────────────────────────────────────────
  Total: 4 files changed, 40 insertions(+), 6 deletions(-)
```

### Step 2 — Ask for Approval

Present the user with a choice:

```
[Review] Please review the changes above.

  1. ✅ Approve   ──▶  Proceed to commit, push, and open a PR.
  2. ❌ Reject    ──▶  Discard all changes (git checkout .).
  3. 🔍 Inspect   ──▶  Show full diff for a specific file before deciding.
```

Handle Phase 5 outcomes as follows:

- **Approve** → `[Review] Changes approved. Proceeding to GitFlow.` Continue to Phase 6.
- **Reject** → Only discard changes when the user explicitly selects Reject. Revert only the files changed by this workflow, not unrelated user changes. `[Review] Workflow changes reverted.` **Stop.**
- **Inspect** → Ask which file. Run `git diff <file>` and display. Loop back to Step 2.

---

## 🚀 Phase 6 — GitFlow

> **📝 Skip this phase** if: the working directory is not a Git repo,
> no Git remote is configured,
> or the user did not explicitly approve changes in Phase 5.

All branches were created in Setup Step 6 — before any fixes were applied.
Phase 6 only stages, commits, pushes, and opens PRs.

```
 ┌────────────┐    ┌────────────┐    ┌────────────┐    ┌────────────┐    ┌────────────┐
 │  Stage &   │───▶│  Push to   │───▶│  Create    │───▶│ (Remove    │───▶│  Summary   │
 │  Commit    │    │  Remote    │    │  PR        │    │  Worktrees)│    │            │
 └────────────┘    └────────────┘    └────────────┘    └────────────┘    └────────────┘
```

> **🌿 Worktree mode:** Steps 1–3 loop over each worktree (1 → N). Step 4 removes worktrees after all PRs are created.

Enter Phase 6 only when the user explicitly approved the changes in Phase 5.

If Bitbucket MCP is not available, re-run the full auto-configure and start procedure from
**[bitbucket-setup.md](bitbucket-setup.md)** only if automatic PR creation is still desired.
Otherwise, continue with the documented manual-PR fallback.

Bitbucket MCP unavailability alone does not block commit or push.

---

### Step 1 — Stage & Commit

Before staging, enforce this guard:

```
[GitFlow] Pre-commit check:
  - Fixed issues in this run: <N>
  - knowledge capture complete: <yes|no>
  - custom-fixes.md changed: <yes|no>
```

- If `<N> > 0` and `knowledge capture complete = no`, STOP and return to Phase 3 Step 4.
- If `<N> > 0`, a commit is allowed when `custom-fixes.md changed = no` only if every fixed issue has a successful `skipped-no-change` checkpoint against an existing WCAG criterion section.
- Do not create a commit that contains accessibility fixes when knowledge capture is missing or failed.

Repeat for each worktree using `git -C <worktree-path>`:

```bash
# Stage all changes
git -C <worktree-path> add -A

# Explicitly stage custom-fixes.md if it exists and was modified
if (Test-Path "<worktree-path>/.github/skills/browserstack-ada-fix/custom-fixes.md") {
  git -C <worktree-path> add .github/skills/browserstack-ada-fix/custom-fixes.md
}

# Verify what will be committed
git -C <worktree-path> diff --staged --name-only
```

Verify custom-fixes.md is staged when applicable:

```
[GitFlow] Worktree <N> staged files:
  - src/App.tsx
  - src/Login.tsx
  - .github/skills/browserstack-ada-fix/custom-fixes.md
  ...
```

- ✅ If fixes were applied (`<N> > 0`) and knowledge capture succeeded, custom-fixes.md MUST appear in the staged files list.
- ⚠️ If fixes were applied but custom-fixes.md is NOT staged, log: `[GitFlow] ERROR: custom-fixes.md not staged but fixes were applied. Stopping commit.` **Stop immediately.**
- ✅ If no fixes were applied or all captures were `skipped-no-change`, custom-fixes.md may be absent.

Proceed with commit:

```bash
git -C <worktree-path> commit -m "fix(a11y): resolve <N> WCAG violations [wt<N>]

Fixed <N> accessibility issues (<critical> critical, <major> major,
<moderate> moderate, <minor> minor).

Worktree: <N> of <total>
Mode: <file|folder|results>
Target: <path or URL>
Scan source: BrowserStack Accessibility Scanner"
```

Log per worktree: `[GitFlow] Worktree <N> committed: <short-hash> — <branch-name>.`

- ✅ `[GitFlow] Changes committed: <short-hash> -- <commit-message-first-line>.`
- ❌ `[GitFlow] Commit failed: <error>. Please resolve and retry.` **Stop.**

---

### Step 2 — Push to Remote

Repeat for each worktree:

```bash
git -C <worktree-path> push -u origin <branch-name>
```

Log per worktree: `[GitFlow] Worktree <N> pushed: <branch-name>.`

- ✅ `[GitFlow] Pushed branch <branch-name> to remote.`
- ❌ Auth fail → `[GitFlow] Push failed (authentication). Check credentials.` **Stop.**
- ❌ Other → `[GitFlow] Push failed: <error>.` **Stop.**

---

### Step 3 — Create Pull Request via Bitbucket MCP

> **🚨 Current-Branch Mode with Remote Tracking:** Skip PR creation entirely if
> `branch_strategy = current-branch` and `has_remote_tracking = true`.
> Log: `[GitFlow] PR creation skipped (working in existing branch with remote tracking)`
> and continue to Step 4 or final summary.

Determine the repository slug from `git remote get-url origin`.
Determine the destination branch: default to `master`, fallback to `main`.
Run `git ls-remote --heads origin master main` to verify.

Use `create_pull_request` (or fallback `pr_create`):

If Bitbucket MCP is unavailable at this point, skip automated PR creation, log that a manual PR is required, and continue to the final summary.

**Worktree mode:** Create one PR per worktree. Loop through each and run this step independently.

**PR fields:**

```
 Field                Value
 ────────────────     ──────────────────────────────────────────────────
 title                fix(a11y): resolve <N> WCAG violations in <target>
 source_branch        <branch-name>   (per-worktree branch in worktree mode)
 destination_branch   master (or main)
```

**PR description template:**

```markdown
## Accessibility Fixes

Resolved **<N>** WCAG violations identified by
BrowserStack Accessibility Scanner.

### Summary
| Severity | Count |
|:---------|:------|
| 🔴 Critical | X |
| 🟠 Major | X |
| 🟡 Moderate | X |
| 🟢 Minor | X |

### Mode
<file|folder|results> — `<path or URL>`

### Changes
<list of changed files from git diff --stat>

### How to Verify
Re-run the BrowserStack accessibility scan on the affected pages
and confirm the violation count has decreased.
```

- ✅ `[GitFlow] PR created! PR URL: <url> — PR #<id>: <title>`
- ❌ `[GitFlow] Failed to create PR: <error>. Push is done on branch <branch-name>.`

---

### Step 4 — Remove Worktrees

After all worktrees have been committed, pushed, and PRs created, remove each worktree:

```bash
git worktree remove <worktree-path>
```

- ✅ `[Worktree] Removed worktree <N>: <path>.`
- ⚠️ Fails (uncommitted changes) → `[Worktree] Could not remove worktree at <path>. Run manually: git worktree remove <path> --force`

After all removals: `[Worktree] All worktrees removed. Branches remain on remote for PR review.`

Run `git worktree list` to confirm the main worktree is the only one listed.

---

### Step 5 — Final Summary

```
 ╔═══════════════════════════════════════════════════════════╗
 ║           ✅  GITFLOW COMPLETE  (Worktree Mode)           ║
 ╠═══════════════════════════════════════════════════════════╣
 ║  Worktree 1 │ Branch: <wt1-branch> │ PR: <url or manual> ║
 ║  Worktree 2 │ Branch: <wt2-branch> │ PR: <url or manual> ║
 ║  Worktree 3 │ Branch: <wt3-branch> │ PR: <url or manual> ║
 ║  Worktree 4 │ Branch: <wt4-branch> │ PR: <url or manual> ║
 ╠═══════════════════════════════════════════════════════════╣
 ║  Total fixed: <N> issues across <X> branches              ║
 ║  Worktrees:   removed                                     ║
 ╚═══════════════════════════════════════════════════════════╝
```

---

## 💡 Using the BrowserStack A11y Expert

For complex or ambiguous issues, consult the `accessibilityExpert` tool:

```
 Scenario             Example Question
 ──────────────────   ─────────────────────────────────────────────────────────
 Custom widgets       "What is the correct ARIA pattern for a custom dropdown?"
 Dynamic content      "How should I handle accessibility for a modal dialog?"
 Media rules          "What WCAG criteria apply to auto-playing video content?"
```

> **💡 Tip:** Use this when the correct fix is not obvious from the rule ID alone,
> when custom components need specific ARIA patterns, or when you need
> to confirm a fix meets the right WCAG success criterion.

---

## 📚 Additional Resources

```
 Resource                          Link
 ────────────────────────────────  ──────────────────────────────────────────────────
 Custom fixes (highest priority)   custom-fixes.md
 Fix patterns by violation type    wcag-fix-patterns.md
 Bitbucket MCP setup               bitbucket-setup.md
 Git worktree overview video       https://www.youtube.com/watch?v=s4BTvj1ZVLM
 WCAG 2.2 Specification            https://www.w3.org/TR/WCAG22/
 WAI-ARIA Authoring Practices      https://www.w3.org/WAI/ARIA/apg/
 axe-core Rule Descriptions        https://github.com/dequelabs/axe-core
```
