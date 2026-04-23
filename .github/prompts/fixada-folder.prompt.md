---
name: fixada-folder
description: "Fix ADA/WCAG accessibility violations from multiple BrowserStack scan results in a folder (CSV/Excel files)"
argument-hint: "Path to folder containing BrowserStack reports"
agent: "fixada"
---

# Fix ADA Issues from BrowserStack Reports Folder

Fix WCAG accessibility violations from multiple BrowserStack scan results (CSV/Excel files in a folder).

## Reports Folder

**Required:** Path to folder containing ONLY BrowserStack accessibility scan results (CSV/Excel format)

${1:C:\\BrowserStackReport\\}

## What This Does

1. **Scans folder** for all CSV/Excel files (treats each as a BrowserStack report)
2. **Loads** violations from all reports in the folder
3. **Triages** by severity (Critical → Major → Moderate → Minor)
4. **Applies fixes** using custom-fixes.md, BrowserStack suggestions, and WCAG patterns
5. **Creates branch** (git worktree or single branch - you'll be asked)
6. **Commits & pushs** changes
7. **Opens PR** via Bitbucket MCP
8. **Updates** "Fix" column in each CSV/Excel file with fix status

## Common Usage

```bash
# Fix all violations from a sprint's accessibility scans
fixadaissues folder C:\Reports\ADA\Sprint-24\

# Process multiple module scans from Downloads
fixadaissues folder C:\Users\AlokShah\Downloads\BrowserStackReports\

# Batch process weekly accessibility reports
fixadaissues folder C:\Repo\cmcs-net-tennessee\Reports\ADA\Week-16-2024\

# Process comprehensive release audit
fixadaissues folder C:\ADA-Scans\Release-2.5.0\
```

## Important Notes

- The folder should contain ONLY BrowserStack report files (CSV/Excel)
- All CSV/Excel files found will be treated as violation sources
- Contrast issues are automatically excluded (not fixable via code changes)
- Violations are READ from reports; fixes are APPLIED across the entire codebase
- You'll review and approve changes before they're committed
- One fix run processes all reports in the folder together
