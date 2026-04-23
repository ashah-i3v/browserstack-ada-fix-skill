# BrowserStack ADA Fix — Example Prompts

This file contains example prompts that invoke the `browserstack-ada-fix` skill to automatically fix accessibility violations identified by BrowserStack's scanner.

---

## 🎓 Understanding Modes

**This skill has three scan-result modes plus one preventive mode:**

1. **`file` mode** — Load violations from a single BrowserStack CSV/Excel report  
   ✅ Use when: You exported a BrowserStack scan to a CSV or Excel file  
   ❌ Do NOT use for: Source code files (.cshtml, .tsx, .css, etc.)

2. **`folder` mode** — Load violations from multiple BrowserStack CSV/Excel reports in a folder  
   ✅ Use when: You have multiple BrowserStack report files in one folder  
   ❌ Do NOT use for: Source code directories

3. **`results` mode** — Load violations directly from a BrowserStack scan URL  
   ✅ Use when: You have a BrowserStack scan URL and haven't exported to CSV/Excel  
   ⚠️ Requires: BROWSERSTACK_USERNAME and BROWSERSTACK_ACCESS_KEY environment variables

**Important:** Violations are READ from BrowserStack reports. Fixes are APPLIED to the actual source code files across your entire codebase.

---

## 📋 Quick Reference

```
fixadaissues file   <csv-or-excel-filepath>      Fix from a single BrowserStack scan result (exported CSV/Excel)
fixadaissues folder <reports-folderpath>         Fix from all BrowserStack scan results in folder (exported CSV/Excel)
fixadaissues results <url>                       Fix from BrowserStack scan result URL
fixadaissues preventive <path>                   Check code against prevention patterns (no scan required)
```

> **Important:** Scan-result modes load BrowserStack accessibility results. `file` and `folder` modes require exported report files (CSV/Excel), NOT source code files.
> **Preventive mode:** Scans code against known patterns BEFORE running BrowserStack scan.

---

## 🛡️ Preventive Mode Prompts

> **Check code against known patterns BEFORE running BrowserStack scan.**
> No BrowserStack credentials or scan results needed. Uses patterns from custom-fixes.md.

### Command Style

```
fixadaissues preventive .
```

```
fixadaissues preventive src/views/
```

```
fixadaissues preventive Enterprise/EntMvcApp/Views/Customer/Details.cshtml
```

### Natural Language

```
check uncommitted code for accessibility issues
```

```
scan my changes for ada violations before I commit
```

```
check src/views/ against accessibility patterns
```

```
I'm about to commit - scan for potential accessibility issues first
```

### Use Cases

```
# Before committing
fixadaissues preventive .
```

```
# Check a new feature branch
fixadaissues preventive src/views/NewFeature/
```

```
# Check a specific file you're working on
fixadaissues preventive Enterprise/EntMvcApp/Views/Permit/Application.cshtml
```

```
# Check all uncommitted changes (omit path)
fixadaissues preventive
```

### Why Use Preventive Mode?

- ✅ Catch issues during development, not after deployment
- ✅ Faster feedback - no BrowserStack scan needed
- ✅ Learn accessibility patterns while coding
- ✅ Reduce scan/re-scan cycles
- ✅ Works offline - no credentials required

---

## 🌐 Results Mode Prompts

### Command Style

```
fixadaissues results https://app-automate.browserstack.com/dashboard/v2/accessibility/results/abc123
```

```
fixadaissues results https://accessibility.browserstack.com/scan/xyz789
```

### Natural Language

```
fix ada issues from BrowserStack results https://app-automate.browserstack.com/dashboard/v2/accessibility/results/abc123
```

```
I have BrowserStack scan results at https://accessibility.browserstack.com/scan/xyz789 - please fix the violations
```

```
can you fix the accessibility issues from this scan: https://app-automate.browserstack.com/accessibility/report/def456
```

---

## 📊 CSV/Excel Mode Prompts

> **Important:** CSV/Excel files are BrowserStack exported reports that contain violations.
> They are NOT the source code files to fix. Fixes are applied to the actual codebase.

### File Mode (Single BrowserStack Report)

**Required:** Target must be a CSV or Excel file (.csv, .xlsx, .xls) exported from BrowserStack.

```
fixadaissues file C:\BrowserStackReport\TNTRIPS-User Profile_09-04-2026.csv
```

```
fixadaissues file reports/accessibility-scan-2026-04-10.xlsx
```

```
fix ada issues in file C:\Reports\browserstack-violations.csv
```

```
fixadaissues file ada-scan-results.xls
```

### Folder Mode (Multiple BrowserStack Reports)

**Required:** Target folder must contain ONLY CSV/Excel files (.csv, .xlsx, .xls) exported from BrowserStack.

```
fixadaissues folder C:\BrowserStackReport
```

```
fixadaissues folder reports/accessibility
```

```
fix accessibility issues in folder C:\Reports\BrowserStack
```

```
fixadaissues folder scans/ada-reports
```

**Note:** The folder should contain only BrowserStack report files. All CSV/Excel files found will be treated as violation sources, and fixes will be applied across the entire codebase based on the violations listed in those reports.

---

## 🔄 Workflow Variations

### Specify Branch Strategy Up Front

```
fixadaissues file C:\BrowserStackReport\TNTRIPS-UserProfile.csv - use git worktree with 2 worktrees
```

```
fixadaissues folder C:\BrowserStackReport - use single branch strategy
```

```
fixadaissues preventive . - use current branch strategy
```

### Multi-Step Requests

```
fix ada issues in file C:\BrowserStackReport\TNTRIPS-Critical.csv, then create PRs for critical and major issues
```

```
fixadaissues results https://browserstack.com/scan/abc123 and prioritize critical violations first
```

---

## 🎨 Domain-Specific Examples

### Working with Multiple Module Reports

```
fixadaissues file C:\BrowserStackReport\Enterprise-Module.csv
```

```
fixadaissues file C:\BrowserStackReport\Permit-Module.csv
```

```
fixadaissues folder C:\BrowserStackReport\AllModules
```

### Batch Processing

```
fixadaissues folder C:\BrowserStackReport\WeeklyScan-2026-04-10
```

```
fix all accessibility issues from reports in folder C:\Reports\BrowserStack\April
```

---

## 🚨 Error Recovery Prompts

### After Prerequisites Fail

```
I installed pwsh, now retry fixadaissues file C:\BrowserStackReport\TNTRIPS-UserProfile.csv
```

```
environment variables are set - continue with fixadaissues results https://browserstack.com/scan/abc
```

### After Review

```
approve the changes and continue to GitFlow
```

```
reject - discard all accessibility fixes
```

```
inspect Enterprise/EntMvcApp/Views/Shared/_Header.cshtml before deciding
```

---

## 💡 Advanced Prompts

### With Specific WCAG Focus

```
fixadaissues file C:\BrowserStackReport\TNTRIPS-Critical.csv and focus on level A violations only
```

```
fix critical accessibility issues from file C:\BrowserStackReport\Enterprise-Scan.csv
```

### Multi-Target

```
fixadaissues file C:\BrowserStackReport\Enterprise-Module.csv
fixadaissues file C:\BrowserStackReport\Permit-Module.csv
fixadaissues file C:\BrowserStackReport\Global-Components.csv
```

### With Validation Request

```
fixadaissues file C:\BrowserStackReport\Modal-Issues.csv and validate all fixes with accessibilityExpert
```

---

## 📚 Contextual Variations

### When BrowserStack Scan Just Completed

```
I just ran a BrowserStack accessibility scan - the results are at [URL]. Please fix all violations.
```

```
I exported the BrowserStack scan to C:\BrowserStackReport\Latest-Scan.csv - please fix all violations.
```

### When Working on Specific Module

```
I'm working on the Enterprise module and BrowserStack found 12 violations. I exported them to C:\BrowserStackReport\Enterprise-Login.csv
```

### When Multiple Reports Need Processing

```
BrowserStack found violations across multiple modules. I have all the reports in C:\BrowserStackReport\AllModules
```

### After Initial Scan Review

```
The scan found 47 violations. I exported them to CSV - fixadaissues file C:\BrowserStackReport\Weekly-Scan.csv
```

---

## ⚡ One-Line Quick Prompts

```
fixadaissues file C:\BrowserStackReport\Latest.csv
```

```
fixadaissues folder C:\BrowserStackReport
```

```
fixadaissues results [paste URL]
```

```
fix ada violations from C:\BrowserStackReport\scan.csv
```

```
fix accessibility issues from the latest scan
```

---

## 🔍 Troubleshooting Prompts

```
why are contrast issues being skipped?
```

```
show me what changes were made to Enterprise/EntMvcApp/Views/Shared/_Header.cshtml
```

```
what accessibility issues still need manual review?
```

```
re-run the fix for issues that failed
```

```
show the git diff before I approve
```

---

## Notes

- **Auto-start:** All prompts with `fixadaissues` trigger the workflow immediately
- **Case-insensitive:** Keywords work in any case (FixAdaIssues, fixadaissues, FIXADAISSUES)
- **Natural phrasing:** Skill accepts conversational language, not just command syntax
- **Branch strategy:** Asked automatically if not specified in the prompt
- **File/Folder modes:** ONLY for BrowserStack report files (CSV/Excel) - NOT for source code files
- **Results mode:** For BrowserStack scan URLs when you haven't exported to CSV/Excel
- **Fixes location:** Violations are read from reports; fixes are applied across the entire codebase
