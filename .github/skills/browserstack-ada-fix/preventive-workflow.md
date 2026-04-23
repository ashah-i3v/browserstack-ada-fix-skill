# Preventive Accessibility Workflow

> Check code against known patterns before running a BrowserStack scan.
> Catch issues during development, not after deployment.

---

## Overview

This workflow scans code against prevention patterns and verified fixes from `custom-fixes.md` without requiring:
- BrowserStack credentials
- Existing scan exports
- Internet access

Default behavior is intentionally review-friendly:
- Display all applicable WCAG criteria with violations **before** starting
- Build an ordered queue from `custom-fixes.md` grouped by WCAG criterion
- Ask: Fix all or one-by-one?
- **One-by-one mode**: Fix one WCAG criterion (all its violations), review it, then decide next step
- **Fix all mode**: Process all criteria automatically, review at the end

**Key concept**: Each **WCAG criterion** is treated as one reviewable unit, not individual violations.

---

## Workflow Phases

```
1. Branch Selection   (pre-Phase 0)
   Choose: git worktree | single branch | current branch
          |
          v
2. Setup              (Phase 0)
   Load prevention patterns + optional branch creation
          |
          v
3. Scan               (Phase 1)
   Check code against prevention patterns
          |
          v
4. Queue              (Phase 2)
   Build ordered issue queue from custom-fixes.md
          |
          v
5. Fix                (Phase 3)
   Default: fix one issue, review it, then ask to continue
          |
          v
6. Verify             (Phase 4)
   Lightweight summary of fixes applied so far
          |
          v
7. Final Review       (Phase 5)
   Optional final pass over accumulated approved changes
          |
          v
8. GitFlow (optional) (Phase 6)
   Commit, push, and create PRs when requested
```

---

## Triggering Preventive Mode

### Command Style

```
fixadaissues preventive <path>
```

### Natural Language

```
check uncommitted code for accessibility issues
scan my changes for ada violations before I commit
check EntMvcApp against accessibility patterns
```

### Path Options

```
 Path                Description
 ──────────────────  ─────────────────────────────────────────────────────
 .                   Current directory / uncommitted changes
 src/views/          Specific folder
 Enterprise/App.cs   Specific file
 (omit path)         All uncommitted files
```

---

## Pre-Phase 0 — Branch Selection

Ask the user to choose one of these before Phase 0:

```
[Setup] Branch strategy required before Phase 0.
[Setup] Do you want to:
  1. git worktree
  2. single branch
  3. current branch
```

Response handling:
- `git worktree` -> Ask: `[Setup] How many worktrees should be created? Enter 1-4.`
- `single branch` -> Create one branch and continue.
- `current branch` -> Work in the current branch or stop after review-only steps.
- Unclear answer -> Repeat once, then stop until user chooses.

State:

```
branch_strategy = current-branch | single-branch | git-worktree
worktree_count = 0 when current-branch or single-branch, otherwise 1..4
has_remote_tracking = false (set to true if current-branch mode and current branch has remote)
```

---

## Phase 0 — Setup

Log: `[Preventive] Starting preventive accessibility check...`

### Step 1 — Load Prevention Patterns

Log: `[Preventive] Loading prevention patterns from custom-fixes.md...`

Read `custom-fixes.md` and use it as the primary source for both:
- Prevention pattern detection
- Ordered issue processing during fixes

Extract for each relevant pattern or verified fix:
- Rule ID
- WCAG criterion
- Detection heuristic or pattern
- Fix guidance
- Product tags or context if relevant

### Step 2 — Git Configuration

Only if `branch_strategy != current-branch`:
- Check `git config user.name`
- Check `git config user.email`
- Check repository and remote availability

If Git is unavailable or not configured:
- Log: `[Preventive] Git not available. Skipping branch creation and GitFlow.`
- Set `branch_strategy = current-branch`
- Continue

### Step 2a — Remote Tracking Check (Current-Branch Mode)

Only if `branch_strategy = current-branch`:
- Run `git rev-parse --abbrev-ref --symbolic-full-name @{u}` to check for remote tracking branch
- If remote tracking branch exists:
  - Set `has_remote_tracking = true`
  - Log: `[Preventive] Current branch has remote tracking branch. Commit and push will be available; PR creation will be skipped.`
- If no remote tracking branch:
  - Set `has_remote_tracking = false`

### Step 3 — Branch Creation

Only if `branch_strategy != current-branch`.

Branch names:

```
 Strategy        Branch Name Pattern
 ─────────────   ───────────────────────────────────────────────────────
 single branch   <user>/preventive-ada-<path-or-timestamp>
 git worktree    <user>/preventive-ada-<path-or-timestamp>-wt<N>
```

---

## Phase 1 — Scan Code

Log: `[Preventive] Scanning code against prevention patterns...`

### Step 1 — Determine Files to Scan

Based on the path provided:

```
 Path Type           Action
 ─────────────────   ─────────────────────────────────────────────────────
 File path           Scan that file
 Folder path         Recursively scan supported code files in folder
 .                   Use uncommitted files
 Omitted             Use all uncommitted files
```

Supported file types:
`.cshtml`, `.html`, `.htm`, `.aspx`, `.ascx`, `.razor`, `.jsx`, `.tsx`, `.vue`, `.js`, `.ts`

Log: `[Preventive] Found X files to scan.`

### Step 2 — Scan Each File

For each file:

```
[Preventive] Scanning <file>...
```

Record matches with:
- File path
- Line number
- Code snippet
- Rule ID
- WCAG criterion
- Severity
- Fix source from `custom-fixes.md`

Log progress every 10 files.

### Step 3 — Filter Results

Contrast issues are excluded.

Always skip matches whose Rule ID or description contains:
- `color-contrast`
- `color-contrast-enhanced`
- `contrast`
- `non-text-contrast`

For each excluded finding, log:

```
[Preventive] Skipping "<rule-id>" -- contrast issues are excluded from this workflow.
```

Log: `[Preventive] Scan complete. Found X potential issues (Y excluded as contrast-related).`

---

## Phase 2 — Build Issue Queue and Display Summary

Log: `[Preventive] Building issue queue from matches...`

### Step 1 — Build Ordered Queue by WCAG Criterion

Queue construction rules:
1. Match findings to verified fixes in `custom-fixes.md` by WCAG criterion first
2. Group all violations under the same WCAG criterion together as **one issue**
3. Preserve the order of WCAG criteria as they appear in `custom-fixes.md`
4. Each WCAG criterion becomes one reviewable unit

Example queue structure:

```
Issue 1: WCAG 1.1.1 - Non-text Content
  - 5 violations across 3 files
  
Issue 2: WCAG 1.3.1 - Info and Relationships
  - 12 violations across 7 files
  
Issue 3: WCAG 2.4.4 - Link Purpose
  - 8 violations across 4 files
```

### Step 2 — Display All Applicable Issues

**ALWAYS display this summary** before asking how to proceed:

```
[Preventive] ══════════════════════════════════════════════════════════════
[Preventive] Found X WCAG criteria with violations in this codebase
[Preventive] ══════════════════════════════════════════════════════════════

Issue 1 of X: WCAG 1.1.1 - Non-text Content
  Rule IDs: image-alt, input-image-alt
  Files affected: 3
  Violations: 5
  Fix source: custom-fixes.md

Issue 2 of X: WCAG 1.3.1 - Info and Relationships
  Rule IDs: form-field-multiple-labels, label, list
  Files affected: 7
  Violations: 12
  Fix source: custom-fixes.md

Issue 3 of X: WCAG 2.4.4 - Link Purpose (In Context)
  Rule IDs: link-name
  Files affected: 4
  Violations: 8
  Fix source: custom-fixes.md

[Preventive] ══════════════════════════════════════════════════════════════
[Preventive] Total: X WCAG criteria | Y files affected | Z violations
[Preventive] Excluded (contrast): C violations (skipped)
[Preventive] ══════════════════════════════════════════════════════════════
```

---

## Phase 3 — Fix Issues

After displaying the summary, ask:

```
[Fix] How would you like to proceed?
  1. Fix all issues (all WCAG criteria)
  2. Fix one by one (one WCAG criterion at a time)
  3. Stop
```

Response handling:
- `Fix all issues` -> Continue to Step 2 (Fix All Mode)
- `Fix one by one` -> Continue to Step 1 (One-by-One Mode)
- `Stop` -> End workflow with no edits

---

### Step 1 — One-by-One Mode (Recommended)

**Treats each WCAG criterion as one reviewable unit.**

Initialization:

```
[Fix] Starting ONE-BY-ONE mode
[Fix] Total WCAG criteria to process: X
[Fix] Each criterion will be fixed completely, then reviewed before moving to the next
```

For each WCAG criterion:

**1. Display criterion details**

```
╔══════════════════════════════════════════════════════════╗
║  WCAG Criterion <current>/<total>                       ║
╠══════════════════════════════════════════════════════════╣
║  Criterion:   WCAG X.X.X - <criterion name>             ║
║  Source:      custom-fixes.md                           ║
║  Violations:  Y violations across Z files               ║
║  Rule IDs:    <rule-id-1, rule-id-2, ...>               ║
╚══════════════════════════════════════════════════════════╝

[Fix] Violations to fix under this criterion:
  • <file-1>:<line> - <rule-id> - <description>
  • <file-2>:<line> - <rule-id> - <description>
  • <file-3>:<line> - <rule-id> - <description>
  ...
```

**2. Apply all fixes for this criterion**

```
[Fix] Fixing WCAG X.X.X - <criterion name>...
[Fix] Processing violation 1/Y: <file>:<line> (<rule-id>)
[Fix] Applied fix
[Fix] Processing violation 2/Y: <file>:<line> (<rule-id>)
[Fix] Applied fix
...
[Fix] All Y violations fixed for WCAG X.X.X
```

**3. Ask user to review this criterion**

```
[Review] WCAG X.X.X (<criterion name>) is ready for review.
[Review] Fixed Y violations across Z files.
[Review] Options:
  1. Approve this criterion
  2. Inspect diff for this criterion
  3. Reject and revise this criterion
  4. Stop workflow here
```

Response handling:
- `Approve this criterion` -> Continue to step 4
- `Inspect diff for this criterion` -> Show diff for all files affected by this criterion, then ask again
- `Reject and revise this criterion` -> Revert changes for this criterion only, then return to review
- `Stop workflow here` -> Go to Phase 4

**4. After approval, ask what to do next**

If `branch_strategy = current-branch` and `has_remote_tracking = true`:

```
[Fix] WCAG X.X.X approved. What's next?
  1. Next criterion
  2. Commit and next
  3. Push and next
  4. Push and stop
  5. Stop
```

Response handling:
- `Next criterion` -> Continue to next WCAG criterion without Git actions
- `Commit and next` -> Commit current approved changes, then continue
- `Push and next` -> Commit if needed, push, then continue
- `Push and stop` -> Commit if needed, push, then go to Phase 4
- `Stop` -> Go to Phase 4

Otherwise (new branch or no remote):

```
[Fix] WCAG X.X.X approved. What's next?
  1. Next criterion
  2. Commit and next
  3. Push and next
  4. Push and stop
  5. PR and next
  6. Stop
```

Response handling:
- `Next criterion` -> Continue to next WCAG criterion without Git actions
- `Commit and next` -> Commit current approved changes, then continue
- `Push and next` -> Commit if needed, push, then continue
- `Push and stop` -> Commit if needed, push, then go to Phase 4
- `PR and next` -> Commit, push, create PR, then continue
- `Stop` -> Go to Phase 4

**5. Track progress**

```
[Fix] Progress: <current> of <total> WCAG criteria fixed and approved
```

---

### Step 2 — Fix All Mode

**Processes all WCAG criteria without stopping.**

Initialization:

```
[Fix] Starting FIX ALL mode
[Fix] Total WCAG criteria: X
[Fix] Total violations: Y
[Fix] All issues will be fixed without stopping
```

For each WCAG criterion:

```
[Fix] Criterion <current>/<total>: WCAG X.X.X - <criterion name>
[Fix] Processing Y violations across Z files...
[Fix] Violation 1/Y: <file>:<line> (<rule-id>) - Applied
[Fix] Violation 2/Y: <file>:<line> (<rule-id>) - Applied
...
[Fix] Completed WCAG X.X.X (Y violations fixed)
[Fix] Progress: <current>/<total> criteria complete
```

Completion:

```
[Fix] Fix All mode complete
[Fix] Fixed X WCAG criteria (Y total violations)
[Fix] Proceeding to Phase 4 for verification
```

---

### Fix Strategy (Applies to Both Modes)

For each individual violation within a WCAG criterion, always prefer this order:
1. Verified fix from `custom-fixes.md` for that specific WCAG criterion
2. BrowserStack suggestion if applicable
3. Generic WCAG pattern only as fallback

All violations under the same WCAG criterion typically use the same fix pattern from `custom-fixes.md`.

---

## Phase 4 — Verify

Log: `[Verify] Preventive fixes complete. Generating summary...`

Summary should include:
- WCAG criteria found
- WCAG criteria fixed and approved
- WCAG criteria remaining
- Total violations processed
- Excluded contrast findings
- Commits created
- PRs created
- Stopping point if the user ended early

Example:

```
[Verify] ══════════════════════════════════════════════════════════════
[Verify] Preventive Workflow Summary
[Verify] ══════════════════════════════════════════════════════════════
  WCAG criteria found:        X
  WCAG criteria fixed:        Y
  WCAG criteria remaining:    Z
  
  Total violations fixed:     N
  Excluded (contrast):        C
  
  Commits created:            M
  PRs created:                P
[Verify] ══════════════════════════════════════════════════════════════
```

For one-by-one mode, explicitly note that the workflow can be resumed later:

```
[Verify] One-by-one mode: You stopped after WCAG X.X.X
[Verify] To resume, run the same command again - already-fixed issues will be skipped
```

---

## Phase 5 — Final Review

This phase is optional in one-by-one mode and broader in batch mode.

- In one-by-one mode, most approval already happened in Phase 3.
- In batch mode, this is the main review point.

Use this phase to:
- Show `git diff --stat`
- Show focused diffs if requested
- Let the user do one final pass before commit, push, or PR creation

If the user already approved each issue individually and is ready to proceed, this phase can be shortened or skipped.

---

## Phase 6 — GitFlow

Only if:
- Git is available and configured
- User wants Git actions

In one-by-one mode, Git actions may already have happened between issues.
Do not repeat them unnecessarily.

### Current-Branch Mode with Remote Tracking

If `branch_strategy = current-branch` and `has_remote_tracking = true`:

Supported actions after any approved issue:
- Commit current approved changes
- Push current branch

PR creation is NOT supported in this scenario because:
- User is working in an existing branch
- Branch already has remote tracking (likely has existing PR or is part of ongoing work)
- Changes should be committed and pushed to the existing branch

Log when user requests commit/push:

```
[GitFlow] Working in current-branch mode with remote tracking
[GitFlow] Committing changes to current branch...
[GitFlow] Pushing to remote tracking branch...
[GitFlow] PR creation skipped (working in existing branch)
```

### New Branch or No Remote

If `branch_strategy = single-branch` or `branch_strategy = git-worktree` or (`branch_strategy = current-branch` and `has_remote_tracking = false`):

Supported actions after any approved issue:
- Commit current approved changes
- Push current branch
- Create a PR for the current approved issue set

PR description should reflect that preventive mode may be incremental:

```markdown
## Proactive Accessibility Fixes

Mode: preventive
Processing style: one-by-one or batch
Issues included in this PR: <subset description>

This PR contains approved preventive fixes taken from custom-fixes.md.
Additional queued issues may be handled in later PRs.
```

---

## Key Differences from Standard Workflow

| Aspect | Standard Workflow | Preventive Workflow |
|:--|:--|:--|
| Input | BrowserStack scan | Code files + prevention patterns |
| Issue unit | Individual violation | WCAG criterion (all violations grouped) |
| Report display | Optional | Always shown before fixing |
| Queue behavior | N/A | Ordered by WCAG criteria from custom-fixes.md |
| Fix decision | Auto-fix all | User chooses: Fix all or one-by-one |
| Review cadence | End-of-run review | Per-criterion review in one-by-one mode |
| Git usage | Usually end of run | Can happen after each approved criterion |
| Stopping points | End only | After each WCAG criterion |

---

## Examples

### Example 1 — Quick Check Before Commit

```bash
fixadaissues preventive .

# Displays all WCAG criteria with violations
# User chooses: Fix one by one
# First criterion (e.g., WCAG 1.1.1) is fixed completely
# User reviews all changes for that criterion
# User approves it
# Workflow asks: Next criterion? Commit? Push? Stop?
```

### Example 2 — One PR Per WCAG Criterion

```bash
fixadaissues preventive EntMvcApp

# User chooses: single branch
# Displays all WCAG criteria
# User chooses: Fix one by one
# First criterion is fixed
# User approves it
# User chooses: PR and next
# A small PR is created for the first criterion
# Workflow continues with the next criterion
```

### Example 3 — Full Cleanup Run

```bash
fixadaissues preventive Enterprise/

# User chooses: git worktree
# Displays all WCAG criteria
# User chooses: Fix all issues
# All criteria are fixed automatically
# User reviews everything at the end in Phase 5
```

### Example 4 — Incremental Approach

```bash
fixadaissues preventive .

# Displays: 5 WCAG criteria found
# User chooses: Fix one by one
# Fixes and approves WCAG 1.1.1
# Fixes and approves WCAG 1.3.1
# User chooses: Stop
# 3 criteria remain for later
# Re-run the same command later to continue
```

---

## Recommendation

For preventive mode, prefer **one-by-one mode**.

That gives you:
- **Smaller units**: Each WCAG criterion is one reviewable chunk
- **Clear progress**: See exactly which criteria are done
- **Easier review**: All related violations reviewed together
- **Natural stopping points**: Stop after any criterion
- **Incremental PRs**: Create one PR per criterion if desired
- **Easier rollback**: Revert one criterion at a time if needed

**When to use Fix All mode:**
- You trust the patterns in custom-fixes.md completely
- You want to batch-process a large codebase
- You'll review everything at the end anyway
- You're doing a one-time cleanup before launch

Use the summary display to understand scope before committing to either mode.
