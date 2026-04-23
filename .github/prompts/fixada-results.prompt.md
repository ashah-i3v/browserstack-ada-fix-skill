---
name: fixada-results
description: "Fix ADA/WCAG accessibility violations from BrowserStack scan result URL"
argument-hint: "BrowserStack scan result URL"
agent: "fixada"
---

# Fix ADA Issues from BrowserStack Scan URL

Fix WCAG accessibility violations directly from a BrowserStack accessibility scan result URL.

## Scan Result URL

**Required:** BrowserStack accessibility scan result URL

${1:https://accessibility.browserstack.com/workflow-analyzer/report?ids=161485}

## What This Does

1. **Fetches** scan results from BrowserStack via MCP
2. **Triages** by severity (Critical → Major → Moderate → Minor)
3. **Applies fixes** using custom-fixes.md, BrowserStack suggestions, and WCAG patterns
4. **Creates branch** (git worktree or single branch - you'll be asked)
5. **Commits & pushes** changes
6. **Opens PR** via Bitbucket MCP

## Prerequisites

This mode requires BrowserStack credentials as environment variables:
- `BROWSERSTACK_USERNAME`
- `BROWSERSTACK_ACCESS_KEY`

## Important Notes

- Contrast issues are automatically excluded (not fixable via code changes)
- Violations are READ from BrowserStack; fixes are APPLIED across the entire codebase
- You'll review and approve changes before they're committed
- No CSV/Excel file is created (use file/folder modes if you need tracking files)
