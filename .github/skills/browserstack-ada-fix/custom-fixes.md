# Custom Accessibility Fixes — Knowledge Base

> **Highest-priority fix source** — These fixes override BrowserStack suggestions
> and generic WCAG patterns because they have been verified to resolve the violation
> on re-scan in this project.
>
> **🔄 Living Document** — Updated during Phase 3 after **every** fix, without exception.
> This file serves as both a fix override source and a cross-product knowledge base.

---

## 📖 How to Use This File

### During Workflow Execution

**Phase 3 (Fix Issues):** The workflow checks this file **first** before falling
back to BrowserStack suggestions or `wcag-fix-patterns.md`. Match by **WCAG Criterion**
first, then narrow by **Rule ID** and pattern scope.

**Preventive Mode:** Use `fixadaissues preventive <path>` to check uncommitted code
against Prevention Patterns below before issues are created.

### For Cross-Product Sharing

- **General Fixes:** Copy "Cross-Product Fixes" section to other products
- **Context-Specific:** Review "Product Tags" to identify which fixes apply to your context
- **Prevention:** Use "Prevention Patterns" as code review checklist before committing

---

## 🏷️ Product Tags (Optional)

You can tag fixes with product/module context for reference:
`[Enterprise]` `[Permit]` `[AutoRouting]` `[CSHTML]` `[JS-Component]` `[CSS-Theme]` `[ArcGIS]`

---

## 🛡️ Prevention Patterns

> **Use these patterns BEFORE writing code to prevent accessibility issues.**
> Run `fixadaissues preventive <path>` to scan uncommitted code against these patterns.

### Images & Media

```csharp
// ❌ AVOID
<img src="..." />
<img src="..." alt="" />  // Empty alt when image is meaningful

// ✅ CORRECT
<img src="..." alt="Descriptive text" />
<img src="..." alt="" role="presentation" />  // Decorative images only
```

### Form Controls

```csharp
// ❌ AVOID
<input type="text" />
<input type="text" placeholder="Name" />  // Placeholder is NOT a label

// ✅ CORRECT
<label for="nameInput">Name</label>
<input type="text" id="nameInput" />

// OR (when visible label isn't possible)
<input type="text" aria-label="Name" />
```

### Buttons & Links

```csharp
// ❌ AVOID
<a href="#">Click here</a>
<button>Submit</button>  // Too generic in context
<div onclick="...">Action</div>  // Not keyboard accessible

// ✅ CORRECT
<a href="...">View customer details</a>
<button type="submit" aria-label="Submit permit application">Submit</button>
<button type="button" onclick="...">Action</button>
```

### Heading Hierarchy

```html
// ❌ AVOID — skipping levels
<h1>Page Title</h1>
<h3>Section</h3>  <!-- Skipped h2 -->

// ✅ CORRECT
<h1>Page Title</h1>
<h2>Main Section</h2>
<h3>Subsection</h3>
```

### Color & Contrast

```css
/* ⚠️ REMINDER — Contrast issues are excluded from this workflow */
/* Design/UX must address these before development */

/* Ensure text contrast ratio: */
/* - Regular text: 4.5:1 minimum (WCAG AA) */
/* - Large text (18pt+): 3:1 minimum (WCAG AA) */
/* - UI components: 3:1 minimum (WCAG AA) */
```

### ARIA Usage

```html
<!-- ❌ AVOID — redundant ARIA -->
<button role="button">Click</button>  <!-- Native semantics already exist -->

<!-- ❌ AVOID — incorrect ARIA -->
<div role="button">Click</div>  <!-- Missing tabindex and keyboard handler -->

<!-- ✅ CORRECT — only when native HTML isn't enough -->
<div role="button" tabindex="0" onkeypress="..." onclick="...">Custom Action</div>

<!-- ✅ BEST — use native HTML when possible -->
<button type="button">Custom Action</button>
```

---

## 📝 Entry Template

> **Used by the workflow to auto-add fixes during Phase 3.**
> Manual entries should follow this format for consistency.

```markdown
### WCAG <X.X.X> - <short criterion label>

| Field             | Value                                          |
|:------------------|:-----------------------------------------------|
| WCAG Criterion    | `<X.X.X>` (Level A / AA / AAA)                 |
| Rule ID(s)        | `<rule-id-1>`, `<rule-id-2>`                   |
| Pattern Scope     | `<reusable issue class or UI pattern>`         |
| Product Tags      | `[Enterprise]` `[CSHTML]`                      |
| Date Added        | YYYY-MM-DD                                     |
| Last Updated      | YYYY-MM-DD                                     |

**Solution Variants:**

\```html
<!-- Before -->
<element ...>...</element>

<!-- After -->
<element ... fix-applied>...</element>
\```

**Why Standard Fix Failed:** Brief explanation of what didn't work.

**Prevention:** Any reusable prevention guidance for this criterion.

**Notes:** Keep multiple elements, contexts, and proven solutions inside the same WCAG criterion section.
---
