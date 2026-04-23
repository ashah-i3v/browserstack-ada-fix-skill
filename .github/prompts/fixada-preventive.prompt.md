---
name: fixada-preventive
description: "Check code against accessibility prevention patterns BEFORE running BrowserStack scan"
argument-hint: "Path to check (file, folder, or . for current directory)"
agent: "fixada"
---

# Check Code for Accessibility Issues (Preventive)

Scan code against known accessibility patterns from custom-fixes.md BEFORE running a BrowserStack scan.

## Path to Check

**Optional:** Path to file, folder, or `.` for uncommitted changes

${1:.}

## What This Does

1. **Loads** prevention patterns from custom-fixes.md knowledge base
2. **Scans** your code (uncommitted changes or specified path)
3. **Builds** an ordered issue queue using matches from custom-fixes.md
4. **Fixes** issues one-by-one by default (apply -> review -> approve)
5. **Asks** whether to continue to next issue or commit/push/create PR after each approved issue
6. **Generates** a full report only when requested (preview/report-only mode)

## Benefits

- ✅ **Catch issues early** - during development, not after deployment
- ✅ **Faster feedback** - no BrowserStack scan needed
- ✅ **Smaller reviews** - approve one issue at a time instead of reviewing a large batch
- ✅ **Incremental delivery** - create PRs issue-by-issue when needed
- ✅ **Learn patterns** - see accessibility best practices while coding
- ✅ **Reduce costs** - fewer BrowserStack scan minutes and re-scan cycles
- ✅ **Works offline** - no credentials or scan results required

## Important Notes

- No BrowserStack credentials needed - uses your local knowledge base
- Scans against patterns in custom-fixes.md (builds over time as you fix issues)
- Best used after you've run the standard fix workflow a few times to build patterns
- Contrast issues are excluded (same as standard workflow)
- Default behavior is one-by-one issue fixing with review gates
- Full report is optional and should be used only when you explicitly want preview/triage
- Knowledge capture is idempotent: update existing fix if changed, skip if no changes needed, add new entry if missing

## Common Usage

```bash
# Check uncommitted changes before commit
fixadaissues preventive .

# Check a specific folder
fixadaissues preventive src/views/NewFeature/

# Check a single file
fixadaissues preventive Enterprise/EntMvcApp/Views/Customer/Details.cshtml

# Check all uncommitted files (omit path)
fixadaissues preventive

# One-by-one review flow for EntMvcApp
fixadaissues preventive EntMvcApp
```
