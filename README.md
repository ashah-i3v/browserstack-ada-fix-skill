# 🚀 BrowserStack ADA Fix — Quick Start Guide

Automatically fix WCAG accessibility violations from BrowserStack scan results — directly in your codebase.

---

## ✨ What Does This Do?

This skill takes BrowserStack accessibility scan results and automatically:

1. ✅ **Loads** violations from your BrowserStack reports
2. 🔍 **Finds** the affected code in your codebase
3. 🔧 **Applies** fixes using proven patterns
4. 🌿 **Creates** git branches with organized changes
5. 📤 **Commits & pushes** to Bitbucket
6. 🔗 **Opens** pull requests for review

**All you need:** Either a BrowserStack accessibility scan result (CSV file or URL) or a code path for preventive mode.

**🚀 Killer Feature:** Use **git worktree mode** to run ADA fixes in the background while you continue your current work without interruption!

---

## 🎯 Quick Start — Four Ways to Use

### 1. From a CSV/Excel Report File

**Best for:** When you've exported a BrowserStack scan to a file

```
/fixada-file C:\BrowserStackReport\TNTRIPS-User Profile_09-04-2026.csv
```

or natural language:

```
fixadaissues file C:\BrowserStackReport\my-scan-results.csv
```

### 2. From a Folder of Reports

**Best for:** When you have multiple BrowserStack scan results to process together

```
/fixada-folder C:\BrowserStackReport
```

or natural language:

```
fixadaissues folder C:\BrowserStackReport
```

### 3. From a BrowserStack URL

**Best for:** When you have a scan URL and haven't exported to CSV

```
/fixada-results https://accessibility.browserstack.com/workflow-analyzer/report?ids=161485
```

or natural language:

```
fixadaissues results https://accessibility.browserstack.com/scan/abc123
```

### 4. Preventive Scan (Before BrowserStack)

**Best for:** Catching likely accessibility issues in your code before running a BrowserStack scan

```
fixadaissues preventive .
```

or natural language:

```
check uncommitted code for accessibility issues
```

---

## 📋 Prerequisites

### Required for All Modes

- ✅ **PowerShell 7** (auto-installs if missing)
- ✅ **Git** with user.name and user.email configured
- ✅ **Node.js 18+**

### Required for URL Mode Only

- ✅ **Environment Variables:**
  ```powershell
  $env:BROWSERSTACK_USERNAME="your_username"
  $env:BROWSERSTACK_ACCESS_KEY="your_access_key"
  ```

Don't worry — the skill checks all prerequisites automatically and guides you through any missing setup.

### Preventive Mode Note

- ✅ No BrowserStack credentials required
- ✅ No exported scan file required
- ✅ Uses known prevention patterns from custom-fixes.md

---

## 🎓 How It Works

### Step-by-Step Workflow

```
1. Parse your command → Detect mode (file/folder/results/preventive)
2. Ask branch strategy → Git worktree, single branch, or current branch?
3. Check prerequisites → Auto-setup missing requirements
4. Load violations → Read from CSV/Excel or fetch from URL
5. Triage issues → Organize by severity (Critical → Minor)
6. Apply fixes → Use custom fixes, BrowserStack suggestions, WCAG patterns
7. Review changes → You approve/reject/inspect
8. GitFlow → Commit, push, create PR
```

### What Gets Fixed?

- 🖼️ **Images** — Missing or meaningless alt text
- 📝 **Forms** — Missing labels, ARIA attributes
- ⌨️ **Keyboard** — Focus issues, keyboard navigation
- 🏷️ **ARIA** — Incorrect or missing roles
- 📑 **Structure** — Heading hierarchy, landmarks
- 🔗 **Links** — Descriptive text for screen readers

### What Doesn't Get Fixed?

- ⛔ **Contrast issues** — Automatically excluded (design decisions, not code bugs)

---

## 🧠 How Fixes Are Decided

The workflow uses a **3-tier priority system** to determine the best fix for each violation.

### Priority Order (Highest to Lowest)

```
┌────────────────────────────────────────────────────────────┐
│ 1️⃣ CUSTOM FIXES (Highest Priority)                       │
│ Source: custom-fixes.md                                   │
│                                                            │
│ Why first?                                                 │
│ • Project-specific fixes verified to work                 │
│ • Proven to resolve violations on re-scan                 │
│ • More reliable than generic patterns                     │
│                                                            │
│ Example: Font Awesome icon accessibility patterns         │
│          specific to this codebase                        │
└────────────────────────────────────────────────────────────┘
                          ⬇️ If no custom fix found
                          
┌────────────────────────────────────────────────────────────┐
│ 2️⃣ BROWSERSTACK SUGGESTIONS (Medium Priority)            │
│ Source: Scan result "How to fix" field                   │
│                                                            │
│ Why second?                                                │
│ • Violation-specific guidance from BrowserStack           │
│ • Tailored to the exact element and context               │
│ • Usually accurate for common patterns                    │
│                                                            │
│ Example: "Add aria-label='Collapse section' to the       │
│           chevron icon for screen reader users"           │
└────────────────────────────────────────────────────────────┘
                          ⬇️ If no suggestion provided
                          
┌────────────────────────────────────────────────────────────┐
│ 3️⃣ GENERIC WCAG PATTERNS (Fallback)                      │
│ Source: wcag-fix-patterns.md                              │
│                                                            │
│ Why last?                                                  │
│ • General WCAG compliance patterns                        │
│ • Works for most cases but may need adjustment            │
│ • Safe, standards-based approach                          │
│                                                            │
│ Example: For missing alt text → add alt attribute         │
│          with descriptive text based on context           │
└────────────────────────────────────────────────────────────┘
                          ⬇️ If no safe fix can be determined
                          
┌────────────────────────────────────────────────────────────┐
│ ❓ MANUAL REVIEW REQUIRED                                 │
│                                                            │
│ When this happens:                                         │
│ • Custom component with unclear ARIA pattern              │
│ • Element not found in codebase                           │
│ • Fix requires design decision                            │
│                                                            │
│ Action: Flagged in summary for human review               │
└────────────────────────────────────────────────────────────┘
```

### Decision Matrix

| Violation Type | Custom Fix | BrowserStack | Generic | Result |
|:---------------|:-----------|:-------------|:--------|:-------|
| Font Awesome icon missing aria-label | ✅ Found in custom-fixes.md | N/A | N/A | **Use custom fix** (project-specific pattern) |
| Form input missing label | ❌ Not in custom-fixes | ✅ "Add `<label for='email'>`" | N/A | **Use BrowserStack suggestion** |
| Image missing alt | ❌ Not in custom-fixes | ❌ No suggestion | ✅ Alt text pattern | **Use generic WCAG pattern** |
| Custom dropdown widget | ❌ Not in custom-fixes | ❌ No suggestion | ❌ No safe pattern | **Manual review required** |

### Quality Assurance

**Every fix is validated by:**

1. **Code search** — Element must be found in codebase
2. **Context analysis** — Fix must match the element's purpose
3. **WCAG compliance** — Fix must satisfy the specific success criterion
4. **Optional validation** — `accessibilityExpert` tool for complex cases

### Learning System

When a fix is applied and later verified to work:

```
1. Re-scan shows violation is resolved
2. Working fix is documented in custom-fixes.md
3. Future runs automatically use this proven fix
4. Team knowledge accumulates over time
```

This creates a **feedback loop** where the workflow gets smarter with each successful fix!

---

## 📂 Understanding Modes

### File Mode (CSV/Excel Report)

**What it is:** Single BrowserStack accessibility scan result exported as CSV or Excel

**When to use:** You have one specific scan result file

**What happens:**
- Violations are READ from the CSV/Excel file (row by row)
- Fixes are APPLIED across your entire codebase
- A "Fix" column is added/updated in the CSV/Excel file showing what was fixed

**Example:**
```
/fixada-file C:\BrowserStackReport\Enterprise-Login-Scan.csv
```

### Folder Mode (Multiple Reports)

**What it is:** Folder containing multiple BrowserStack CSV/Excel reports

**When to use:** You have several scan results to process together

**What happens:**
- All CSV/Excel files in the folder are treated as BrowserStack reports
- Violations from all files are combined and processed
- Each file gets its "Fix" column updated

**Example:**
```
/fixada-folder C:\BrowserStackReport\WeeklyScan-2026-04-10
```

### Results Mode (BrowserStack URL)

**What it is:** Direct BrowserStack accessibility scan URL

**When to use:** You want to fix directly from BrowserStack without exporting

**What happens:**
- Scan results are fetched from BrowserStack via API
- No CSV/Excel file is created (tracking happens in-memory)

**Requires:** BROWSERSTACK_USERNAME and BROWSERSTACK_ACCESS_KEY environment variables

**Example:**
```
/fixada-results https://accessibility.browserstack.com/workflow-analyzer/report?ids=161485
```

### Preventive Mode (Pre-Scan)

**What it is:** A proactive code scan using known accessibility prevention patterns

**When to use:** Before committing code or before running BrowserStack scans

**What happens:**
- The workflow scans your target path (or uncommitted changes) against prevention patterns
- It groups findings by WCAG criteria
- You can choose report-only mode or apply fixes

**Example:**
```
fixadaissues preventive Enterprise/EntMvcApp/Views/
```

**Detailed flow:** See [preventive-workflow.md](preventive-workflow.md)

---

## 🌿 Branch Strategy

Before starting, you'll be asked to choose:

### Option 1: Git Worktree (Recommended)

**What it does:** Creates separate working directory(ies) with dedicated branches

**Two key benefits:**

#### 1️⃣ Work in Parallel (Main Benefit!)
**The killer feature:** Continue your current work while ADA fixes run in a separate worktree

**Visual example:**
```
┌───────────────────────────────────────────────────────────────┐
│ C:\Repo\cmcs-net-tennessee\          (Your main workspace)  │
│ Branch: feature/PARS-5000                                     │
│ Status: ✅ You're actively coding here                        │
│                                                               │
│ What you're doing:                                            │
│ • Writing new features                                        │
│ • Running tests                                               │
│ • Making commits                                              │
└───────────────────────────────────────────────────────────────┘
                           ⬆️
                    You work here
                    
                           ⬇️
              Meanwhile, in parallel...
              
┌───────────────────────────────────────────────────────────────┐
│ C:\Repo\cmcs-net-tennessee-ada-wt1\   (Worktree)            │
│ Branch: alokshahceltic/ada-fix-violations                    │
│ Status: 🔧 ADA fixes running automatically                    │
│                                                               │
│ What's happening:                                             │
│ • Loading BrowserStack violations                             │
│ • Finding affected code                                       │
│ • Applying fixes                                              │
│ • Creating commits                                            │
│ • Pushing to remote                                           │
│ • Opening PR                                                  │
└───────────────────────────────────────────────────────────────┘

Result: Both work streams complete independently!
         No conflicts. No interruptions. Maximum productivity.
```

**Example:**
```
Your main repo: Working on feature PARS-5000
Worktree 1:     ADA fixes running (can take time)
                ↓
You switch back to main repo immediately
Continue coding on PARS-5000 without interruption
```

**Why this matters:**
- Don't block your current work
- Run multiple fix sessions simultaneously
- No conflicts between your feature branch and ADA fixes
- Keep your working directory clean

#### 2️⃣ Organize by Severity (Bonus Feature)
Split fixes across multiple worktrees by severity level

**You'll be asked:** "How many worktrees? (1-4)"
- **1 worktree** = All fixes in one branch (parallel work benefit only)
- **2-4 worktrees** = Fixes split by severity + parallel work

**Examples:**
- 1 worktree = All ADA fixes isolated, you can keep working
- 2 worktrees = Critical in one PR, Major in another + parallel work
- 4 worktrees = Critical, Major, Moderate, Minor in separate PRs + parallel work

### Option 2: Single Branch (Simpler)

**What it does:** Creates one branch in your current repository

**Benefits:**
- Simpler workflow
- All fixes in one PR
- Familiar git flow

**Trade-off:** Your working directory changes to the ADA fix branch. You must wait for the workflow to complete before resuming other work.

**Use when:** 
- Small number of violations
- You're not in the middle of other work
- You prefer one consolidated PR

### Option 3: Current Branch

**What it does:** Applies fixes directly on your current checked-out branch

**Benefits:**
- Fastest path when you're intentionally fixing accessibility within current feature work
- No branch creation overhead

**Trade-off:** Changes mix with your in-progress feature work, so review scope can grow.

**Use when:**
- Accessibility fixes are part of your current branch deliverable
- You intentionally want one combined change set

---

## 📊 CSV/Excel Fix Tracking

When using **file** or **folder** mode, the workflow automatically adds a "Fix" column to your CSV/Excel files:

| Status | Meaning |
|:-------|:--------|
| ✅ **Fixed** | Issue found and fixed in the codebase |
| ⛔ **Excluded (contrast)** | Contrast issue (not fixable via code) |
| ⏭️ **Skipped** | Outside scope or already fixed |
| ❓ **Manual Review** | No safe automatic fix available |
| 🔍 **Not Found** | Element not located in codebase |

**Why this matters:** Run the workflow again later and it will skip already-fixed issues!

---

## 🔧 Example Workflows

### Scenario 1: Fix Critical Issues First

```
1. Run BrowserStack accessibility scan
2. Export to CSV: "Enterprise-Critical-Issues.csv"
3. Run: /fixada-file C:\BrowserStackReport\Enterprise-Critical-Issues.csv
4. Choose: git worktree, 1 worktree
5. Review changes
6. Approve → PR created automatically
```

### Scenario 2: Process Multiple Modules

```
1. Export scans for each module:
   - Enterprise-Module.csv
   - Permit-Module.csv
   - Global-Components.csv
2. Put all in: C:\BrowserStackReport\AllModules\
3. Run: /fixada-folder C:\BrowserStackReport\AllModules
4. Choose: git worktree, 3 worktrees (split by severity)
5. Review → 3 PRs created (Critical, Major, Moderate)
```

### Scenario 3: Quick Fix from URL

```
1. Complete BrowserStack scan
2. Copy the results URL
3. Run: /fixada-results https://accessibility.browserstack.com/scan/xyz
4. Choose: single branch
5. Review → 1 PR created with all fixes
```

### Scenario 4: Preventive Check Before Commit

```
1. You finish a UI change and want early accessibility feedback
2. Run: fixadaissues preventive .
3. Choose: current branch (or single branch/worktree)
4. Review grouped findings by WCAG criterion
5. Apply fixes now or keep report-only output
```

### Scenario 5: Parallel Work (Don't Block Your Dev Work!) 🚀

**The Problem:** You're working on feature PARS-5000, but need to fix ADA violations. You don't want to stop your current work.

**The Solution:** Use worktree mode!

```
Current situation:
- Main repo: On branch "feature/PARS-5000"
- You're in the middle of coding
- BrowserStack found 25 violations to fix

Workflow:
1. Run: /fixada-file C:\BrowserStackReport\violations.csv
2. Choose: git worktree, 1 worktree
3. Workflow creates: C:\Repo\cmcs-net-tennessee-ada-wt1\
4. ADA fixes start running in the worktree
5. IMMEDIATELY: You switch back to your main repo
6. Continue working on feature/PARS-5000
7. No conflicts, no interruptions!

Result:
- Worktree 1: ADA fixes complete, PR created
- Main repo: Your feature work continues uninterrupted
- Both branches exist independently on remote
```

**Why this is powerful:**
- Fix accessibility issues WITHOUT stopping your current work
- Run multiple ADA fix sessions (different reports) simultaneously in different worktrees
- Keep your main working directory clean and focused
- No branch switching headaches

**Pro tip:** Start the ADA fix, go back to your feature work, get the PR notification when done!

---

## 🛠️ Troubleshooting

### "File not found" Error

**Problem:** The CSV/Excel file path is incorrect

**Solution:**
- Use absolute paths: `C:\BrowserStackReport\file.csv`
- Check file extension: Must be `.csv`, `.xlsx`, or `.xls`
- Verify the file exists

### "No BrowserStack report files found in folder"

**Problem:** The folder doesn't contain CSV/Excel files

**Solution:**
- Make sure you exported BrowserStack scan results
- Check the folder path is correct
- Folder should contain ONLY BrowserStack report files

### "Environment variables not set" (Results mode)

**Problem:** BROWSERSTACK_USERNAME or BROWSERSTACK_ACCESS_KEY missing

**Solution:**
```powershell
$env:BROWSERSTACK_USERNAME="your_email@company.com"
$env:BROWSERSTACK_ACCESS_KEY="your_access_key"
```

Get your access key from: [BrowserStack Account Settings](https://www.browserstack.com/accounts/settings)

### "PowerShell 7 not found"

**Problem:** pwsh is not installed

**Solution:** The workflow will attempt automatic installation via winget. If it fails, [download manually](https://aka.ms/powershell-release).

### "All issues already fixed"

**Not an error!** The CSV/Excel file shows all issues were previously processed. This means:
- Your fixes from a previous run are still in place
- Re-run BrowserStack scan to verify violations are actually resolved
- If new violations appear, export a fresh report

---

## 📚 Detailed Documentation

| Document | Purpose |
|:---------|:--------|
| [SKILL.md](SKILL.md) | Complete technical workflow specification |
| [prompts.md](prompts.md) | Example commands and invocation patterns |
| [custom-fixes.md](custom-fixes.md) | Project-specific verified fixes |
| [wcag-fix-patterns.md](wcag-fix-patterns.md) | Generic WCAG fix patterns by violation type |
| [bitbucket-setup.md](bitbucket-setup.md) | Bitbucket MCP configuration guide |

---

## 🎯 Best Practices

### ✅ Do This

- **Use worktree mode for parallel work** — Don't block your current feature development!
- **Export fresh reports** after each BrowserStack scan
- **Keep CSV/Excel files** for tracking (the "Fix" column shows history)
- **Review changes** before approving (always inspect code changes)
- **Re-scan after merging** to verify violations are actually resolved
- **Use multiple worktrees** for large projects with many violations
- **Commit often** when working through multiple reports
- **Start ADA fix, switch back to your work** — let it run in parallel

### ❌ Avoid This

- **Don't** use source code files (.cshtml, .tsx) as file/folder targets
- **Don't** delete CSV/Excel files — they track what's been fixed
- **Don't** skip the review phase — always verify changes
- **Don't** expect 100% automatic fixes — some issues need manual review
- **Don't** fix contrast issues via this workflow (design decisions, not code)

---

## 🆘 Getting Help

### During the Workflow

The workflow provides detailed logging at every step:

```
[Setup]   Prerequisites and configuration
[Scan]    Loading scan results
[Triage]  Issue analysis
[Fix]     Code changes
[Verify]  Summary report
[Review]  Change approval
[GitFlow] Commit, push, PR
```

Watch for these prefixes — they tell you exactly what's happening.

### Common Questions

**Q: Can I continue working on my feature while ADA fixes run?**  
A: **YES!** Use git worktree mode. The fixes run in a separate directory, and you can immediately switch back to your current work. This is the #1 reason to use worktrees.

**Q: Can I run this multiple times on the same report?**  
A: Yes! Already-fixed issues are skipped automatically.

**Q: What if I don't like the changes?**  
A: Choose "Reject" during review — all changes are discarded safely.

**Q: Can I run multiple ADA fix sessions at the same time?**  
A: Yes! Create multiple worktrees (up to 4). Each runs independently. You can process different reports simultaneously.

**Q: Why are contrast issues excluded?**  
A: Contrast issues require design decisions (color changes), not code fixes. They're better handled by designers.

**Q: Can I fix only specific severities?**  
A: Not directly, but use worktrees to separate by severity, then merge only the PRs you want.

**Q: What if a fix doesn't work?**  
A: Add the working fix to [custom-fixes.md](custom-fixes.md) — it will be used in future runs.

---

## 🎓 Learning Path

### 1. First Time (30 minutes)

- Export a small BrowserStack scan (5-10 violations)
- Run: `/fixada-file <your-file>.csv`
- Choose: single branch
- Review changes carefully
- Approve and merge the PR
- Re-scan to verify fixes worked

### 2. Getting Comfortable (Next few runs)

- Try folder mode with multiple reports
- Experiment with worktrees (start with 2)
- Inspect code changes before approving
- Add custom fixes when you find better solutions

### 3. Advanced Usage

- Process large scans (50+ violations)
- Use 4 worktrees for severity-based PRs
- Contribute patterns to [wcag-fix-patterns.md](wcag-fix-patterns.md)
- Help teammates get started

---

## 🎉 Success Metrics

After running this workflow, you should see:

- ✅ BrowserStack violation count decreased
- ✅ PRs created with clear fix descriptions
- ✅ CSV/Excel files updated with "Fix" status
- ✅ Code changes follow WCAG patterns
- ✅ Accessibility improvements merged to master

**Remember:** Re-run BrowserStack scans after merging to confirm violations are actually resolved!

---

## 📞 Support

- **Skill Issues:** Check [SKILL.md](SKILL.md) for complete workflow details
- **WCAG Questions:** See [wcag-fix-patterns.md](wcag-fix-patterns.md)
- **Git/Worktree Help:** [Git Worktree Tutorial](https://www.youtube.com/watch?v=s4BTvj1ZVLM)
- **BrowserStack API:** [BrowserStack Accessibility Documentation](https://www.browserstack.com/docs/accessibility)

---

## ✨ Quick Reference Card

```
┌───────────────────────────────────────────────────────────────┐
│  BROWSERSTACK ADA FIX — QUICK REFERENCE                       │
├───────────────────────────────────────────────────────────────┤
│  FILE MODE                                                    │
│  /fixada-file <csv-or-excel-path>                            │
│  → Fix from single BrowserStack export                       │
│                                                               │
│  FOLDER MODE                                                  │
│  /fixada-folder <folder-with-reports>                        │
│  → Fix from multiple BrowserStack exports                    │
│                                                               │
│  URL MODE                                                     │
│  /fixada-results <browserstack-url>                          │
│  → Fix directly from BrowserStack scan                       │
│                                                               │
│  PREVENTIVE MODE                                              │
│  fixadaissues preventive <path>                               │
│  → Check code against prevention patterns pre-scan            │
│                                                               │
│  NATURAL LANGUAGE                                             │
│  fixadaissues file <path>                                    │
│  fixadaissues folder <path>                                  │
│  fixadaissues results <url>                                  │
│  fixadaissues preventive <path>                              │
├───────────────────────────────────────────────────────────────┤
│  YOU WILL BE ASKED                                            │
│  1. Branch strategy? (worktree, single, or current branch)   │
│     💡 Worktree = Continue your work without interruption    │
│  2. If worktree: How many? (1-4)                             │
│     1 = Parallel work only                                   │
│     2-4 = Parallel work + severity-based PRs                 │
│  3. Review changes? (approve/reject/inspect)                 │
├─────────────────────────────────────────────────────────────┤
│  WHAT GETS FIXED                                            │
│  ✅ Images, Forms, Keyboard, ARIA, Structure, Links        │
│  ⛔ Contrast (excluded — requires design decisions)        │
├─────────────────────────────────────────────────────────────┤
│  🚀 PRO TIP: Use Worktree Mode!                            │
│  • Start ADA fix in worktree                               │
│  • Immediately return to your feature branch               │
│  • Keep coding without interruption                        │
│  • Get PR notification when fixes complete                 │
└─────────────────────────────────────────────────────────────┘
```

---

**Ready to start?** Try this now:

```
/fixada-file C:\BrowserStackReport\TNTRIPS-User Profile_09-04-2026.csv
```

Happy fixing! 🎉
