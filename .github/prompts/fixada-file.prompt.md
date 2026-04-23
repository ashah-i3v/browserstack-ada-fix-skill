---
name: fixada-file
description: "Fix ADA/WCAG accessibility violations from a single BrowserStack scan result (CSV/Excel file)"
argument-hint: "Path to BrowserStack CSV/Excel report"
agent: "fixada"
---

# Fix ADA Issues from BrowserStack Report File

Fix WCAG accessibility violations from a BrowserStack scan result exported as CSV or Excel.

## Report File

**Required:** Path to BrowserStack accessibility scan result (CSV/Excel format)

${1:C:\\BrowserStackReport\\scan-result.csv}

## What This Does

1. **Loads** violations from the BrowserStack CSV/Excel report
2. **Triages** by severity (Critical → Major → Moderate → Minor)
3. **Applies fixes** using custom-fixes.md, BrowserStack suggestions, and WCAG patterns
4. **Creates branch** (git worktree or single branch - you'll be asked)
5. **Commits & pushes** changes
6. **Opens PR** via Bitbucket MCP
7. **Updates** "Fix" column in the CSV/Excel file with fix status

## Common Usage

```bash
# Fix violations from a CSV export
fixadaissues file C:\Downloads\browserstack-accessibility-scan-2024-04-17.csv

# Fix violations from an Excel file
fixadaissues file C:\Reports\ADA\enterprise-module-scan.xlsx

# Process a report from Downloads folder
fixadaissues file C:\Users\AlokShah\Downloads\accessibility-report.csv

# Process a report from project folder
fixadaissues file C:\Repo\cmcs-net-tennessee\Reports\ADA\permit-scan-results.csv
```

## Important Notes

- Contrast issues are automatically excluded (not fixable via code changes)
- Violations are READ from the report; fixes are APPLIED across the entire codebase
- You'll review and approve changes before they're committed
- The CSV/Excel file must be a BrowserStack exported report, NOT a source code file
