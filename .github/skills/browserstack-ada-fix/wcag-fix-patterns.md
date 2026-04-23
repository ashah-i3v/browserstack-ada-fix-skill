# WCAG Fix Patterns Reference

Comprehensive accessibility violation fixes aligned with **WCAG 2.2** (W3C Recommendation, December 2024),
**Section 508**, and **EN 301 549**. Organized by category with axe-core / BrowserStack rule ID mappings.

---

## Quick Reference: Rule ID to Fix Pattern

Use this table to jump from a BrowserStack / axe-core rule ID to the correct fix pattern.

| Rule ID | WCAG SC | Section | Impact |
|---------|---------|---------|--------|
| `image-alt` | 1.1.1 | [Images](#images-wcag-111) | Critical |
| `input-image-alt` | 1.1.1 | [Images](#images-wcag-111) | Critical |
| `role-img-alt` | 1.1.1 | [Images](#images-wcag-111) | Serious |
| `svg-img-alt` | 1.1.1 | [Images](#images-wcag-111) | Serious |
| `object-alt` | 1.1.1 | [Images](#images-wcag-111) | Serious |
| `area-alt` | 1.1.1 | [Images](#images-wcag-111) | Critical |
| `aria-meter-name` | 1.1.1 | [Images](#images-wcag-111) | Serious |
| `aria-progressbar-name` | 1.1.1 | [Images](#images-wcag-111) | Serious |
| `color-contrast` | 1.4.3 | [Color Contrast](#color-contrast-wcag-143--146--1411) | Serious |
| `color-contrast-enhanced` | 1.4.6 | [Color Contrast](#color-contrast-wcag-143--146--1411) | Serious |
| `link-in-text-block` | 1.4.1 | [Color Contrast](#color-contrast-wcag-143--146--1411) | Serious |
| `meta-viewport` | 1.4.4 | [Text & Typography](#text--typography-wcag-144-1410-1412) | Critical |
| `avoid-inline-spacing` | 1.4.12 | [Text & Typography](#text--typography-wcag-144-1410-1412) | Serious |
| `css-orientation-lock` | 1.3.4 | [Orientation & Input Purpose](#orientation--input-purpose-wcag-134-135) | Serious |
| `autocomplete-valid` | 1.3.5 | [Orientation & Input Purpose](#orientation--input-purpose-wcag-134-135) | Serious |
| `label` | 4.1.2 | [Forms](#forms-wcag-131-332-412) | Critical |
| `select-name` | 4.1.2 | [Forms](#forms-wcag-131-332-412) | Critical |
| `form-field-multiple-labels` | 3.3.2 | [Forms](#forms-wcag-131-332-412) | Moderate |
| `label-title-only` | 4.1.2 | [Forms](#forms-wcag-131-332-412) | Serious |
| `input-button-name` | 4.1.2 | [Forms](#forms-wcag-131-332-412) | Critical |
| `scrollable-region-focusable` | 2.1.1 | [Keyboard Accessibility](#keyboard-accessibility-wcag-211-212-247) | Moderate |
| `frame-focusable-content` | 2.1.1 | [Keyboard Accessibility](#keyboard-accessibility-wcag-211-212-247) | Serious |
| `nested-interactive` | 4.1.2 | [Keyboard Accessibility](#keyboard-accessibility-wcag-211-212-247) | Serious |
| `tabindex` | 2.4.3 | [Keyboard Accessibility](#keyboard-accessibility-wcag-211-212-247) | Serious |
| `bypass` | 2.4.1 | [Keyboard Accessibility](#keyboard-accessibility-wcag-211-212-247) | Serious |
| `skip-link` | 2.4.1 | [Keyboard Accessibility](#keyboard-accessibility-wcag-211-212-247) | Moderate |
| `heading-order` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `empty-heading` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Minor |
| `p-as-heading` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Serious |
| `page-has-heading-one` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `region` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `landmark-one-main` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `landmark-banner-is-top-level` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `landmark-contentinfo-is-top-level` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `landmark-main-is-top-level` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `landmark-no-duplicate-banner` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `landmark-no-duplicate-contentinfo` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `landmark-no-duplicate-main` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `landmark-unique` | 1.3.1 | [Headings & Structure](#headings--structure-wcag-131-246) | Moderate |
| `link-name` | 2.4.4 | [Links & Buttons](#links--buttons-wcag-244-412) | Serious |
| `button-name` | 4.1.2 | [Links & Buttons](#links--buttons-wcag-244-412) | Critical |
| `aria-command-name` | 4.1.2 | [Links & Buttons](#links--buttons-wcag-244-412) | Serious |
| `document-title` | 2.4.2 | [Links & Buttons](#links--buttons-wcag-244-412) | Serious |
| `duplicate-id` | 4.1.1 | [ARIA](#aria-wcag-412) | Minor |
| `duplicate-id-active` | 4.1.1 | [ARIA](#aria-wcag-412) | Serious |
| `duplicate-id-aria` | 4.1.1 | [ARIA](#aria-wcag-412) | Critical |
| `aria-allowed-attr` | 4.1.2 | [ARIA](#aria-wcag-412) | Critical |
| `aria-required-attr` | 4.1.2 | [ARIA](#aria-wcag-412) | Critical |
| `aria-required-children` | 1.3.1 | [ARIA](#aria-wcag-412) | Critical |
| `aria-required-parent` | 1.3.1 | [ARIA](#aria-wcag-412) | Critical |
| `aria-roles` | 4.1.2 | [ARIA](#aria-wcag-412) | Critical |
| `aria-valid-attr` | 4.1.2 | [ARIA](#aria-wcag-412) | Critical |
| `aria-valid-attr-value` | 4.1.2 | [ARIA](#aria-wcag-412) | Critical |
| `aria-hidden-body` | 4.1.2 | [ARIA](#aria-wcag-412) | Critical |
| `aria-hidden-focus` | 4.1.2 | [ARIA](#aria-wcag-412) | Serious |
| `aria-input-field-name` | 4.1.2 | [ARIA](#aria-wcag-412) | Serious |
| `aria-toggle-field-name` | 4.1.2 | [ARIA](#aria-wcag-412) | Serious |
| `aria-tooltip-name` | 4.1.2 | [ARIA](#aria-wcag-412) | Serious |
| `aria-dialog-name` | 4.1.2 | [ARIA](#aria-wcag-412) | Serious |
| `aria-treeitem-name` | 4.1.2 | [ARIA](#aria-wcag-412) | Serious |
| `aria-roledescription` | 4.1.2 | [ARIA](#aria-wcag-412) | Serious |
| `aria-allowed-role` | 4.1.2 | [ARIA](#aria-wcag-412) | Minor |
| `aria-text` | 4.1.2 | [ARIA](#aria-wcag-412) | Serious |
| `presentation-role-conflict` | 4.1.2 | [ARIA](#aria-wcag-412) | Minor |
| `definition-list` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Serious |
| `dlitem` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Serious |
| `list` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Serious |
| `listitem` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Serious |
| `td-headers-attr` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Serious |
| `th-has-data-cells` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Serious |
| `scope-attr-valid` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Critical |
| `table-duplicate-name` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Minor |
| `table-fake-caption` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Serious |
| `empty-table-header` | 1.3.1 | [Tables & Lists](#tables--lists-wcag-131) | Minor |
| `html-has-lang` | 3.1.1 | [Language](#language-wcag-311-312) | Serious |
| `html-lang-valid` | 3.1.1 | [Language](#language-wcag-311-312) | Serious |
| `html-xml-lang-mismatch` | 3.1.1 | [Language](#language-wcag-311-312) | Moderate |
| `valid-lang` | 3.1.2 | [Language](#language-wcag-311-312) | Serious |
| `video-caption` | 1.2.2 | [Media](#media-wcag-121---125) | Critical |
| `audio-caption` | 1.2.1 | [Media](#media-wcag-121---125) | Critical |
| `no-autoplay-audio` | 1.4.2 | [Media](#media-wcag-121---125) | Moderate |
| `frame-title` | 4.1.2 | [Frames & Embeds](#frames--embeds-wcag-412) | Serious |
| `frame-title-unique` | 4.1.2 | [Frames & Embeds](#frames--embeds-wcag-412) | Serious |
| `blink` | 2.2.2 | [Timing & Motion](#timing--motion-wcag-221-231) | Serious |
| `marquee` | 2.2.2 | [Timing & Motion](#timing--motion-wcag-221-231) | Serious |
| `meta-refresh` | 2.2.1 | [Timing & Motion](#timing--motion-wcag-221-231) | Critical |
| `target-size` | 2.5.8 | [Target Size & Pointer](#target-size--pointer-wcag-258-257) | Serious |
| `label-content-name-mismatch` | 2.5.3 | [Label in Name](#label-in-name-wcag-253) | Serious |
| `focus-order-semantics` | 2.4.3 | [Focus Management (WCAG 2.2)](#focus-management-wcag-22---2411-2412-2413) | Minor |

---

## Images (WCAG 1.1.1)

### Missing alt text

**Rule IDs:** `image-alt`, `input-image-alt`

```html
<!-- Before -->
<img src="logo.png">

<!-- Fix: Add descriptive alt -->
<img src="logo.png" alt="Company logo">

<!-- Decorative image: use empty alt -->
<img src="divider.png" alt="" role="presentation">
```

### Informative SVG missing accessible name

**Rule IDs:** `svg-img-alt`

```html
<!-- Before -->
<svg>...</svg>

<!-- Fix -->
<svg role="img" aria-label="Sales trend chart">...</svg>

<!-- Or with title element -->
<svg role="img" aria-labelledby="svg-title">
  <title id="svg-title">Sales trend chart</title>
  ...
</svg>
```

### Icon buttons without labels

**Rule IDs:** `button-name`

```html
<!-- Before -->
<button><img src="search.svg"></button>

<!-- Fix -->
<button aria-label="Search"><img src="search.svg" alt=""></button>
```

### Object elements missing alt

**Rule IDs:** `object-alt`

```html
<!-- Before -->
<object data="chart.swf" type="application/x-shockwave-flash"></object>

<!-- Fix -->
<object data="chart.swf" type="application/x-shockwave-flash">
  <p>Quarterly sales chart showing 15% growth</p>
</object>
```

### Image map areas

**Rule IDs:** `area-alt`

```html
<map name="infographic">
  <area shape="rect" coords="0,0,100,100" href="/sales" alt="Sales data">
  <area shape="rect" coords="100,0,200,100" href="/revenue" alt="Revenue data">
</map>
```

### Elements with role="img"

**Rule IDs:** `role-img-alt`

```html
<!-- Before -->
<div role="img" style="background-image: url(chart.png)"></div>

<!-- Fix -->
<div role="img" aria-label="Bar chart showing monthly revenue" style="background-image: url(chart.png)"></div>
```

### Meter and progressbar names

**Rule IDs:** `aria-meter-name`, `aria-progressbar-name`

```html
<div role="meter" aria-label="Storage used" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100">75%</div>
<div role="progressbar" aria-label="Upload progress" aria-valuenow="40" aria-valuemin="0" aria-valuemax="100">40%</div>
```

---

## Color Contrast (WCAG 1.4.3 / 1.4.6 / 1.4.11)

**Required ratios:**
- Normal text (< 18pt / < 14pt bold): **4.5:1** (AA) / **7:1** (AAA)
- Large text (>= 18pt / >= 14pt bold): **3:1** (AA) / **4.5:1** (AAA)
- UI components & graphical objects (1.4.11): **3:1**

### Fixing low contrast text

**Rule IDs:** `color-contrast`, `color-contrast-enhanced`

```css
/* Before - fails (e.g., #999 on #fff = 2.85:1) */
.label { color: #999; }

/* Fix - meets 4.5:1 (e.g., #595959 on #fff = 7.08:1) */
.label { color: #595959; }
```

### Contrast-safe color pairs

| Background | Text Color | Ratio | Use For |
|-----------|------------|-------|---------|
| `#ffffff` | `#595959`  | 7.0:1 | Body text |
| `#ffffff` | `#0056b3`  | 7.5:1 | Links |
| `#ffffff` | `#d32f2f`  | 5.6:1 | Error text |
| `#ffffff` | `#2e7d32`  | 5.1:1 | Success text |
| `#ffffff` | `#6d4c00`  | 7.1:1 | Warning text |
| `#1a1a1a` | `#ffffff`  | 16.0:1| Dark mode body |
| `#1a1a1a` | `#6db3f2`  | 6.5:1 | Dark mode links |
| `#1a1a1a` | `#ff8a80`  | 5.0:1 | Dark mode error |
| `#1a1a1a` | `#69f0ae`  | 8.4:1 | Dark mode success |

### Non-text contrast (WCAG 1.4.11)

UI components and meaningful graphics must have at least **3:1** contrast against adjacent colors.

```css
/* Before - border too faint */
.input-field { border: 1px solid #ddd; }

/* Fix - meets 3:1 against white background */
.input-field { border: 1px solid #767676; }

/* Before - icon too low contrast */
.icon { color: #ccc; }

/* Fix */
.icon { color: #767676; }
```

### Links distinguished from surrounding text

**Rule IDs:** `link-in-text-block`

```css
/* Links must be distinguishable by more than color alone */
a {
  color: #0056b3;
  text-decoration: underline;
}

/* If underline removed, use another non-color cue plus 3:1 contrast with surrounding text */
a {
  color: #0056b3;
  text-decoration: none;
  border-bottom: 1px solid currentColor;
}
```

---

## Text & Typography (WCAG 1.4.4, 1.4.10, 1.4.12)

### Resize text (1.4.4) — do not disable zoom

**Rule IDs:** `meta-viewport`

```html
<!-- BAD — disables zoom -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">

<!-- Fix — allow zoom -->
<meta name="viewport" content="width=device-width, initial-scale=1">
```

### Reflow (1.4.10) — no horizontal scrolling at 320 CSS px

Content must reflow to 320 CSS px width (equivalent to 400% zoom at 1280 px) without requiring horizontal scrolling.

```css
/* Use fluid layout */
.container {
  max-width: 100%;
  padding: 0 1rem;
}

/* Avoid fixed widths */
.card {
  width: 100%;
  max-width: 40rem;
}

/* Use responsive images */
img {
  max-width: 100%;
  height: auto;
}
```

### Text spacing (1.4.12) — do not break when overridden

**Rule IDs:** `avoid-inline-spacing`

Content must remain readable when users override text spacing to:
- Line height >= 1.5x font size
- Letter spacing >= 0.12x font size
- Word spacing >= 0.16x font size
- Paragraph spacing >= 2x font size

```css
/* BAD — inline styles that override user preferences */
/* style="line-height: 1.1 !important" */

/* Fix — use relative units and avoid !important on spacing */
.content {
  line-height: 1.5;
  letter-spacing: normal;
  word-spacing: normal;
}

/* Never set fixed height on text containers */
/* BAD */
.text-box { height: 80px; overflow: hidden; }

/* Fix */
.text-box { min-height: 80px; }
```

---

## Content on Hover or Focus (WCAG 1.4.13)

Popup content triggered by hover or focus must be **dismissible**, **hoverable**, and **persistent**.

```css
/* Tooltip pattern that meets 1.4.13 */
[data-tooltip] {
  position: relative;
}

[data-tooltip]::after {
  content: attr(data-tooltip);
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  padding: 0.5rem 0.75rem;
  background: #1a1a1a;
  color: #fff;
  border-radius: 4px;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.15s;
}

/* Show on hover AND focus */
[data-tooltip]:hover::after,
[data-tooltip]:focus::after {
  opacity: 1;
  pointer-events: auto; /* Allow hovering over the tooltip itself */
}
```

```javascript
// Dismiss on Escape
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') {
    document.querySelectorAll('[data-tooltip-visible]').forEach(el => {
      el.removeAttribute('data-tooltip-visible');
    });
  }
});
```

---

## Orientation & Input Purpose (WCAG 1.3.4, 1.3.5)

### Orientation (1.3.4) — do not lock orientation

**Rule IDs:** `css-orientation-lock`

```css
/* BAD — locks to portrait */
@media (orientation: portrait) {
  .app { display: block; }
}
@media (orientation: landscape) {
  .app { display: none; }
}

/* Fix — adapt layout, never hide */
@media (orientation: portrait) {
  .grid { grid-template-columns: 1fr; }
}
@media (orientation: landscape) {
  .grid { grid-template-columns: 1fr 1fr; }
}
```

### Input purpose / autocomplete (1.3.5)

**Rule IDs:** `autocomplete-valid`

Use standard `autocomplete` values for identity-related fields so browsers and assistive tech can auto-fill.

```html
<label for="fname">First Name</label>
<input type="text" id="fname" autocomplete="given-name">

<label for="lname">Last Name</label>
<input type="text" id="lname" autocomplete="family-name">

<label for="email">Email</label>
<input type="email" id="email" autocomplete="email">

<label for="tel">Phone</label>
<input type="tel" id="tel" autocomplete="tel">

<label for="address">Street Address</label>
<input type="text" id="address" autocomplete="street-address">

<label for="cc">Credit Card Number</label>
<input type="text" id="cc" autocomplete="cc-number">

<label for="bday">Birthday</label>
<input type="date" id="bday" autocomplete="bday">

<label for="newpw">New Password</label>
<input type="password" id="newpw" autocomplete="new-password">
```

**Common autocomplete tokens:** `name`, `given-name`, `family-name`, `email`, `tel`, `street-address`,
`address-line1`, `address-line2`, `address-level2` (city), `address-level1` (state), `postal-code`,
`country`, `cc-name`, `cc-number`, `cc-exp`, `cc-csc`, `bday`, `sex`, `username`, `new-password`,
`current-password`, `organization`, `url`.

---

## Forms (WCAG 1.3.1, 3.3.2, 4.1.2)

### Input without label

**Rule IDs:** `label`, `label-title-only`

```html
<!-- Before -->
<input type="email" placeholder="Email">

<!-- Fix option 1: Visible label (preferred) -->
<label for="email">Email</label>
<input type="email" id="email" placeholder="e.g. user@example.com">

<!-- Fix option 2: aria-label (when visible label not feasible) -->
<input type="email" aria-label="Email address" placeholder="Email">

<!-- Fix option 3: aria-labelledby (reference existing text) -->
<span id="email-label">Email</span>
<input type="email" aria-labelledby="email-label">
```

### Select without label

**Rule IDs:** `select-name`

```html
<!-- Before -->
<select>
  <option>Choose country</option>
</select>

<!-- Fix -->
<label for="country">Country</label>
<select id="country">
  <option value="">Choose country</option>
</select>
```

### Input button without name

**Rule IDs:** `input-button-name`

```html
<!-- Before -->
<input type="submit">

<!-- Fix -->
<input type="submit" value="Submit Application">
```

### Multiple labels on a field

**Rule IDs:** `form-field-multiple-labels`

```html
<!-- Before — two <label> elements for one input -->
<label for="pw">Password</label>
<label for="pw">Must be 8+ characters</label>
<input type="password" id="pw">

<!-- Fix — single label, use aria-describedby for help text -->
<label for="pw">Password</label>
<input type="password" id="pw" aria-describedby="pw-hint">
<span id="pw-hint">Must be 8+ characters</span>
```

### Missing error identification (WCAG 3.3.1)

```html
<label for="email">Email</label>
<input type="email" id="email" aria-describedby="email-error" aria-invalid="true">
<span id="email-error" role="alert">Please enter a valid email address.</span>
```

### Required fields (WCAG 3.3.2)

```html
<label for="name">Name <span aria-hidden="true">*</span></label>
<input type="text" id="name" required aria-required="true">
```

### Grouped controls (WCAG 1.3.1)

```html
<fieldset>
  <legend>Shipping Method</legend>
  <input type="radio" id="standard" name="shipping" value="standard">
  <label for="standard">Standard (5-7 days)</label>
  <input type="radio" id="express" name="shipping" value="express">
  <label for="express">Express (2-3 days)</label>
</fieldset>
```

### Error suggestions (WCAG 3.3.3)

```html
<label for="date">Date of Birth</label>
<input type="text" id="date" aria-describedby="date-error" aria-invalid="true" value="13/32/1990">
<span id="date-error" role="alert">
  Invalid date. Please use MM/DD/YYYY format (e.g., 01/15/1990).
</span>
```

### Error prevention on legal/financial submissions (WCAG 3.3.4)

```html
<!-- Provide a review step before final submit -->
<section aria-label="Review your order">
  <h2>Review Your Order</h2>
  <dl>
    <dt>Item</dt><dd>Widget Pro</dd>
    <dt>Total</dt><dd>$49.99</dd>
  </dl>
  <button type="button" onclick="goBack()">Edit Order</button>
  <button type="submit">Confirm Purchase</button>
</section>
```

---

## Keyboard Accessibility (WCAG 2.1.1, 2.1.2, 2.4.7)

### Non-interactive element used as button

```html
<!-- Before -->
<div onclick="doAction()">Click me</div>

<!-- Fix -->
<button type="button" onclick="doAction()">Click me</button>

<!-- Or if div must stay -->
<div role="button" tabindex="0"
     onclick="doAction()"
     onkeydown="if(event.key==='Enter'||event.key===' ')doAction()">
  Click me
</div>
```

### Missing focus indicator (WCAG 2.4.7)

```css
/* Never do this without a replacement */
/* BAD: *:focus { outline: none; } */

/* Fix: visible focus style */
:focus-visible {
  outline: 2px solid #0056b3;
  outline-offset: 2px;
}
```

### Focus trap in modal (WCAG 2.1.2)

```javascript
function trapFocus(modal) {
  const focusable = modal.querySelectorAll(
    'a[href], button:not([disabled]), input:not([disabled]), select:not([disabled]), textarea:not([disabled]), [tabindex]:not([tabindex="-1"])'
  );
  const first = focusable[0];
  const last = focusable[focusable.length - 1];

  modal.addEventListener('keydown', (e) => {
    if (e.key !== 'Tab') return;
    if (e.shiftKey && document.activeElement === first) {
      e.preventDefault();
      last.focus();
    } else if (!e.shiftKey && document.activeElement === last) {
      e.preventDefault();
      first.focus();
    }
  });
  first.focus();
}
```

### Skip navigation link (WCAG 2.4.1)

**Rule IDs:** `bypass`, `skip-link`

```html
<body>
  <a href="#main-content" class="skip-link">Skip to main content</a>
  <nav>...</nav>
  <main id="main-content">...</main>
</body>
```

```css
.skip-link {
  position: absolute;
  left: -9999px;
  top: auto;
  width: 1px;
  height: 1px;
  overflow: hidden;
}
.skip-link:focus {
  position: static;
  width: auto;
  height: auto;
  overflow: visible;
  padding: 8px 16px;
  background: #0056b3;
  color: #fff;
  z-index: 9999;
}
```

### Scrollable region keyboard accessible

**Rule IDs:** `scrollable-region-focusable`

```html
<!-- Before — scrollable but not keyboard accessible -->
<div style="overflow: auto; height: 200px;">
  <p>Long content...</p>
</div>

<!-- Fix — add tabindex and accessible label -->
<div tabindex="0" role="region" aria-label="Scrollable content" style="overflow: auto; height: 200px;">
  <p>Long content...</p>
</div>
```

### No nested interactive elements

**Rule IDs:** `nested-interactive`

```html
<!-- BAD — button inside a link -->
<a href="/page"><button>Click</button></a>

<!-- Fix — choose one interactive element -->
<a href="/page" class="btn-styled">Click</a>
```

### Positive tabindex

**Rule IDs:** `tabindex`

```html
<!-- BAD — manually ordering focus -->
<input tabindex="3"> <input tabindex="1"> <input tabindex="2">

<!-- Fix — use tabindex="0" and rely on DOM order -->
<input tabindex="0"> <input tabindex="0"> <input tabindex="0">
```

---

## Focus Management (WCAG 2.2 — 2.4.11, 2.4.12, 2.4.13)

### Focus Not Obscured — Minimum (2.4.11 — Level AA)

The focused element must not be fully hidden by author-created content such as sticky headers,
footers, banners, or overlapping panels.

```css
/* Ensure focused elements aren't hidden behind sticky headers */
:focus-visible {
  scroll-margin-top: 80px; /* Height of sticky header */
  scroll-margin-bottom: 60px; /* Height of sticky footer */
}

.sticky-header {
  position: sticky;
  top: 0;
  z-index: 100;
}

/* Account for sticky regions when focus moves */
html {
  scroll-padding-top: 80px;
  scroll-padding-bottom: 60px;
}
```

```javascript
// When opening a panel that might overlap content, manage focus
function openSidePanel() {
  const panel = document.getElementById('side-panel');
  panel.hidden = false;
  const mainContent = document.getElementById('main');
  mainContent.style.marginRight = `${panel.offsetWidth}px`;
}
```

### Focus Not Obscured — Enhanced (2.4.12 — Level AAA)

No part of the focused element can be hidden. Same techniques as above, applied more strictly.

### Focus Appearance (2.4.13 — Level AAA)

Focus indicators must have:
- At least **2 CSS px** thick outline or equivalent area
- At least **3:1** contrast between focused and unfocused states

```css
/* Compliant focus indicator */
:focus-visible {
  outline: 3px solid #0056b3;
  outline-offset: 2px;
}

/* High-contrast focus for dark backgrounds */
.dark-theme :focus-visible {
  outline: 3px solid #ffdd57;
  outline-offset: 2px;
}

/* Alternative: box-shadow (counts toward the area requirement) */
:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px #0056b3;
}
```

---

## Target Size & Pointer (WCAG 2.5.8, 2.5.7)

### Target size minimum (2.5.8 — Level AA)

**Rule IDs:** `target-size`

All interactive targets must be at least **24 x 24 CSS px**, or have sufficient spacing so that
a 24 px circle centered on each target does not overlap another target.

**Exceptions:** inline links in text, user-agent-controlled elements, essential presentations.

```css
/* Ensure buttons meet minimum target size */
button, a.btn, [role="button"] {
  min-width: 24px;
  min-height: 24px;
  padding: 6px 12px;
}

/* Icon-only buttons need explicit sizing */
.icon-btn {
  min-width: 44px;  /* 44px recommended for touch */
  min-height: 44px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

/* Ensure spacing between small targets */
.button-group button {
  margin: 4px; /* Ensures 24px non-overlap when buttons are small */
}

/* Touch-friendly form controls */
input[type="checkbox"],
input[type="radio"] {
  min-width: 24px;
  min-height: 24px;
}
```

### Dragging movements (2.5.7 — Level AA)

Any operation that uses dragging must also have a single-pointer alternative (no path-based gestures required).

```html
<!-- Sortable list: provide up/down buttons alongside drag handles -->
<ul role="list">
  <li>
    <span class="drag-handle" aria-hidden="true">&#x2630;</span>
    <span>Item 1</span>
    <button aria-label="Move Item 1 up" onclick="moveUp(0)">&#x25B2;</button>
    <button aria-label="Move Item 1 down" onclick="moveDown(0)">&#x25BC;</button>
  </li>
</ul>
```

```html
<!-- Slider: draggable thumb must also respond to click-to-set and arrow keys -->
<input type="range" min="0" max="100" value="50" aria-label="Volume">
```

```html
<!-- Map pan: provide arrow buttons alongside drag -->
<div class="map-controls">
  <button aria-label="Pan left" onclick="panMap('left')">&#x2190;</button>
  <button aria-label="Pan right" onclick="panMap('right')">&#x2192;</button>
  <button aria-label="Pan up" onclick="panMap('up')">&#x2191;</button>
  <button aria-label="Pan down" onclick="panMap('down')">&#x2193;</button>
</div>
```

---

## Label in Name (WCAG 2.5.3)

**Rule IDs:** `label-content-name-mismatch`

The accessible name must contain the visible label text.

```html
<!-- BAD — visible text says "Search" but aria-label says something different -->
<button aria-label="Find products">Search</button>

<!-- Fix — accessible name includes the visible label -->
<button aria-label="Search products">Search</button>

<!-- Or just use the visible text -->
<button>Search</button>
```

---

## Consistent Help (WCAG 3.2.6 — Level A, WCAG 2.2)

Help mechanisms must appear in the same relative order across pages within a set.

```html
<!-- Place help in a consistent location on every page (e.g. footer or header) -->
<footer>
  <nav aria-label="Help">
    <ul>
      <li><a href="/contact">Contact Us</a></li>
      <li><a href="/faq">FAQ</a></li>
      <li><a href="/chat">Live Chat</a></li>
    </ul>
  </nav>
</footer>
```

---

## Redundant Entry (WCAG 3.3.7 — Level A, WCAG 2.2)

Do not require users to re-enter information already provided in the same session,
unless re-entry is essential (e.g. for security confirmation).

```html
<!-- Auto-populate shipping from billing -->
<fieldset>
  <legend>Shipping Address</legend>
  <label>
    <input type="checkbox" id="same-as-billing" onchange="copyBilling()">
    Same as billing address
  </label>
  <label for="ship-street">Street</label>
  <input type="text" id="ship-street" autocomplete="shipping street-address">
</fieldset>
```

```javascript
function copyBilling() {
  if (document.getElementById('same-as-billing').checked) {
    document.getElementById('ship-street').value =
      document.getElementById('bill-street').value;
    // Copy other fields...
  }
}
```

---

## Accessible Authentication (WCAG 3.3.8 / 3.3.9 — WCAG 2.2)

### Minimum (3.3.8 — Level AA)

Do not require cognitive function tests (memorizing passwords, solving puzzles, transcribing text)
unless an alternative is provided.

```html
<!-- Compliant approaches: -->

<!-- 1. Allow password managers (don't block paste) -->
<input type="password" id="password" autocomplete="current-password">

<!-- 2. Allow copy-paste on verification codes -->
<label for="otp">Enter the code sent to your email</label>
<input type="text" id="otp" autocomplete="one-time-code" inputmode="numeric">

<!-- 3. Provide a passwordless alternative -->
<button type="button" onclick="sendMagicLink()">Sign in with email link</button>

<!-- 4. If CAPTCHA is used, provide an audio or other alternative -->
<div class="captcha">
  <img src="captcha.png" alt="Type the characters you see">
  <button type="button" onclick="playAudioCaptcha()">Listen to audio version</button>
</div>
```

### Enhanced (3.3.9 — Level AAA)

No cognitive test at all — only object recognition or personal content identification are allowed.

---

## Headings & Structure (WCAG 1.3.1, 2.4.6)

### Skipped heading levels

**Rule IDs:** `heading-order`

```html
<!-- Before (skips h2) -->
<h1>Page Title</h1>
<h3>Section Title</h3>

<!-- Fix -->
<h1>Page Title</h1>
<h2>Section Title</h2>
```

### Empty heading

**Rule IDs:** `empty-heading`

```html
<!-- Before -->
<h2></h2>

<!-- Fix: add text or remove the empty heading -->
<h2>Section Title</h2>
```

### Styled paragraphs used as headings

**Rule IDs:** `p-as-heading`

```html
<!-- Before -->
<p style="font-size: 24px; font-weight: bold;">Important Section</p>

<!-- Fix -->
<h2>Important Section</h2>
```

### Missing page landmarks

**Rule IDs:** `region`, `landmark-one-main`, `landmark-banner-is-top-level`,
`landmark-contentinfo-is-top-level`, `landmark-main-is-top-level`,
`landmark-no-duplicate-banner`, `landmark-no-duplicate-contentinfo`, `landmark-no-duplicate-main`,
`landmark-unique`

```html
<body>
  <header role="banner">...</header>
  <nav role="navigation" aria-label="Main">...</nav>
  <main role="main">...</main>
  <aside role="complementary" aria-label="Related">...</aside>
  <footer role="contentinfo">...</footer>
</body>
```

### Missing page title (WCAG 2.4.2)

**Rule IDs:** `document-title`

```html
<head>
  <title>Page Name - Site Name</title>
</head>
```

### Multiple navigations need distinct labels

```html
<nav aria-label="Main">...</nav>
<nav aria-label="Footer">...</nav>
```

---

## Links & Buttons (WCAG 2.4.4, 4.1.2)

### Non-descriptive link text

**Rule IDs:** `link-name`

```html
<!-- Before -->
<a href="/report">Click here</a>

<!-- Fix -->
<a href="/report">View quarterly report</a>

<!-- Or with context via aria-label -->
<a href="/report" aria-label="View quarterly report">View report</a>
```

### Empty link

```html
<!-- Before -->
<a href="/home"><img src="logo.png"></a>

<!-- Fix -->
<a href="/home"><img src="logo.png" alt="Home"></a>
```

### Link opens in new window without warning

```html
<a href="/terms" target="_blank" rel="noopener">
  Terms of Service <span class="sr-only">(opens in new tab)</span>
</a>
```

### Button without accessible name

**Rule IDs:** `button-name`, `aria-command-name`

```html
<!-- Before -->
<button><i class="icon-close"></i></button>

<!-- Fix -->
<button aria-label="Close dialog"><i class="icon-close"></i></button>
```

---

## ARIA (WCAG 4.1.2)

### Valid ARIA usage rules

**Rule IDs:** `aria-allowed-attr`, `aria-required-attr`, `aria-valid-attr`, `aria-valid-attr-value`,
`aria-roles`, `aria-roledescription`, `aria-allowed-role`

```html
<!-- Every ARIA role must be valid -->
<div role="tabpanel">...</div> <!-- OK -->
<div role="foobar">...</div>  <!-- INVALID — "foobar" is not a real role -->

<!-- Required attributes must be present -->
<div role="slider" aria-valuenow="5" aria-valuemin="0" aria-valuemax="10">...</div>

<!-- Attribute values must be valid -->
<div aria-hidden="true">...</div>  <!-- OK -->
<div aria-hidden="yes">...</div>   <!-- INVALID — must be "true" or "false" -->
```

### aria-hidden must not be on body

**Rule IDs:** `aria-hidden-body`

```html
<!-- NEVER do this -->
<body aria-hidden="true">

<!-- Only use aria-hidden on specific elements -->
<div aria-hidden="true">Decorative content</div>
```

### aria-hidden elements must not be focusable

**Rule IDs:** `aria-hidden-focus`

```html
<!-- BAD -->
<div aria-hidden="true">
  <button>Hidden but focusable</button>
</div>

<!-- Fix — add tabindex="-1" or remove from aria-hidden container -->
<div aria-hidden="true">
  <button tabindex="-1">Hidden and not focusable</button>
</div>
```

### ARIA input and toggle fields need accessible names

**Rule IDs:** `aria-input-field-name`, `aria-toggle-field-name`

```html
<div role="textbox" aria-label="Search query" contenteditable="true"></div>
<div role="switch" aria-label="Dark mode" aria-checked="false"></div>
<div role="slider" aria-label="Volume" aria-valuenow="50" aria-valuemin="0" aria-valuemax="100"></div>
```

### Duplicate IDs

**Rule IDs:** `duplicate-id`, `duplicate-id-active`, `duplicate-id-aria`

```html
<!-- BAD — same ID used twice -->
<label for="name">Name</label>
<input id="name">
<input id="name"> <!-- Duplicate! -->

<!-- Fix — unique IDs -->
<label for="first-name">First Name</label>
<input id="first-name">
<label for="last-name">Last Name</label>
<input id="last-name">
```

### Presentation role conflicts

**Rule IDs:** `presentation-role-conflict`

```html
<!-- BAD — element with role="presentation" has tabindex -->
<img role="presentation" tabindex="0" src="decorative.png">

<!-- Fix — remove tabindex from presentational elements -->
<img role="presentation" alt="" src="decorative.png">
```

### Custom dropdown/select

```html
<div role="combobox" aria-expanded="false" aria-haspopup="listbox"
     aria-label="Choose a fruit">
  <input type="text" aria-autocomplete="list" aria-controls="fruit-list">
  <ul id="fruit-list" role="listbox" hidden>
    <li role="option" id="opt-apple">Apple</li>
    <li role="option" id="opt-banana">Banana</li>
  </ul>
</div>
```

### Tab interface

```html
<div role="tablist" aria-label="Account settings">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">Profile</button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2" tabindex="-1">Security</button>
</div>
<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">...</div>
<div role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>...</div>
```

### Modal dialog

**Rule IDs:** `aria-dialog-name`

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p>Are you sure you want to delete this item?</p>
  <button type="button">Cancel</button>
  <button type="button">Delete</button>
</div>
```

### Live regions for dynamic content (WCAG 4.1.3)

```html
<!-- Status messages -->
<div role="status" aria-live="polite">3 results found</div>

<!-- Error alerts -->
<div role="alert" aria-live="assertive">Session expired. Please log in again.</div>

<!-- Log region -->
<div role="log" aria-live="polite" aria-label="Chat messages">...</div>

<!-- Timer -->
<div role="timer" aria-live="off" aria-label="Countdown">5:00</div>
```

---

## ARIA APG Widget Patterns

Patterns from the [WAI-ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/patterns/).

### Accordion

```html
<div class="accordion">
  <h3>
    <button aria-expanded="false" aria-controls="sect1" id="accordion1">
      Section 1
    </button>
  </h3>
  <div id="sect1" role="region" aria-labelledby="accordion1" hidden>
    <p>Section 1 content...</p>
  </div>
  <h3>
    <button aria-expanded="false" aria-controls="sect2" id="accordion2">
      Section 2
    </button>
  </h3>
  <div id="sect2" role="region" aria-labelledby="accordion2" hidden>
    <p>Section 2 content...</p>
  </div>
</div>
```

### Disclosure (show/hide)

```html
<button aria-expanded="false" aria-controls="details-panel">
  Show Details
</button>
<div id="details-panel" hidden>
  <p>Additional details here...</p>
</div>
```

### Menubar

```html
<nav aria-label="Main menu">
  <ul role="menubar">
    <li role="none">
      <button role="menuitem" aria-haspopup="true" aria-expanded="false">File</button>
      <ul role="menu" hidden>
        <li role="none"><button role="menuitem">New</button></li>
        <li role="none"><button role="menuitem">Open</button></li>
        <li role="separator"></li>
        <li role="none"><button role="menuitem">Save</button></li>
      </ul>
    </li>
    <li role="none">
      <button role="menuitem">Edit</button>
    </li>
  </ul>
</nav>
```

### Breadcrumb

```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/products">Products</a></li>
    <li><a href="/products/widgets" aria-current="page">Widgets</a></li>
  </ol>
</nav>
```

### Alert dialog

```html
<div role="alertdialog" aria-modal="true" aria-labelledby="alert-title" aria-describedby="alert-desc">
  <h2 id="alert-title">Unsaved Changes</h2>
  <p id="alert-desc">You have unsaved changes. Do you want to discard them?</p>
  <button type="button">Discard</button>
  <button type="button" autofocus>Keep Editing</button>
</div>
```

### Tree view

**Rule IDs:** `aria-treeitem-name`

```html
<ul role="tree" aria-label="File browser">
  <li role="treeitem" aria-expanded="true">
    <span>Documents</span>
    <ul role="group">
      <li role="treeitem" class="doc">Report.pdf</li>
      <li role="treeitem" class="doc">Notes.txt</li>
    </ul>
  </li>
  <li role="treeitem" aria-expanded="false">
    <span>Images</span>
    <ul role="group" hidden>
      <li role="treeitem">Photo.jpg</li>
    </ul>
  </li>
</ul>
```

### Carousel / Slide show

```html
<section aria-roledescription="carousel" aria-label="Product highlights">
  <div aria-live="off">
    <div role="group" aria-roledescription="slide" aria-label="1 of 3">
      <img src="slide1.jpg" alt="New widget model with ergonomic grip">
      <p>Introducing the Widget Pro</p>
    </div>
  </div>
  <button aria-label="Previous slide">&#x2190;</button>
  <button aria-label="Next slide">&#x2192;</button>
  <button aria-label="Pause auto-rotation">&#x23F8;</button>
</section>
```

### Switch (toggle)

```html
<button role="switch" aria-checked="false" onclick="toggleSwitch(this)">
  <span aria-hidden="true">Off</span>
  Dark Mode
</button>
```

```javascript
function toggleSwitch(btn) {
  const checked = btn.getAttribute('aria-checked') === 'true';
  btn.setAttribute('aria-checked', String(!checked));
  btn.querySelector('span').textContent = checked ? 'Off' : 'On';
}
```

### Toolbar

```html
<div role="toolbar" aria-label="Text formatting" aria-controls="editor">
  <button aria-pressed="false" onclick="toggleBold(this)">Bold</button>
  <button aria-pressed="false" onclick="toggleItalic(this)">Italic</button>
  <button aria-pressed="false" onclick="toggleUnderline(this)">Underline</button>
</div>
```

---

## Tables & Lists (WCAG 1.3.1)

### Data table without proper headers

**Rule IDs:** `td-headers-attr`, `th-has-data-cells`, `scope-attr-valid`, `empty-table-header`

```html
<!-- Before -->
<table>
  <tr><td>Name</td><td>Email</td></tr>
  <tr><td>Alice</td><td>alice@example.com</td></tr>
</table>

<!-- Fix -->
<table>
  <caption>User Directory</caption>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Alice</td>
      <td>alice@example.com</td>
    </tr>
  </tbody>
</table>
```

### Complex table with row and column headers

```html
<table>
  <caption>Quarterly Sales by Region</caption>
  <thead>
    <tr>
      <th scope="col">Region</th>
      <th scope="col">Q1</th>
      <th scope="col">Q2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">North</th>
      <td>$150k</td>
      <td>$175k</td>
    </tr>
    <tr>
      <th scope="row">South</th>
      <td>$120k</td>
      <td>$135k</td>
    </tr>
  </tbody>
</table>
```

### Fake caption using first row

**Rule IDs:** `table-fake-caption`

```html
<!-- BAD — first row pretending to be a caption -->
<table>
  <tr><td colspan="3"><strong>Sales Data</strong></td></tr>
  <tr><td>Name</td><td>Q1</td><td>Q2</td></tr>
</table>

<!-- Fix — use <caption> -->
<table>
  <caption>Sales Data</caption>
  <thead><tr><th>Name</th><th>Q1</th><th>Q2</th></tr></thead>
</table>
```

### List structure

**Rule IDs:** `list`, `listitem`, `definition-list`, `dlitem`

```html
<!-- Lists must contain correct children -->
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>

<!-- Definition lists -->
<dl>
  <dt>Term</dt>
  <dd>Definition</dd>
</dl>
```

---

## Language (WCAG 3.1.1, 3.1.2)

### Missing document language

**Rule IDs:** `html-has-lang`, `html-lang-valid`

```html
<html lang="en">
```

### lang and xml:lang mismatch

**Rule IDs:** `html-xml-lang-mismatch`

```html
<!-- Both must agree on base language -->
<html lang="en" xml:lang="en">
```

### Inline language changes

**Rule IDs:** `valid-lang`

```html
<p>The French word <span lang="fr">bonjour</span> means hello.</p>
```

---

## Media (WCAG 1.2.1 - 1.2.5)

### Video without captions

**Rule IDs:** `video-caption`

```html
<video controls>
  <source src="demo.mp4" type="video/mp4">
  <track kind="captions" src="demo-captions.vtt" srclang="en" label="English" default>
</video>
```

### Audio without captions/transcript

**Rule IDs:** `audio-caption`

```html
<audio controls>
  <source src="podcast.mp3" type="audio/mpeg">
</audio>
<a href="podcast-transcript.html">Read transcript</a>
```

### Video with audio description (1.2.5)

```html
<video controls>
  <source src="demo.mp4" type="video/mp4">
  <track kind="captions" src="demo-captions.vtt" srclang="en" label="English" default>
  <track kind="descriptions" src="demo-descriptions.vtt" srclang="en" label="Audio descriptions">
</video>
```

### Autoplay audio

**Rule IDs:** `no-autoplay-audio`

```html
<!-- BAD — auto-plays audio with no way to stop -->
<audio autoplay src="background.mp3"></audio>

<!-- Fix — add controls, or limit to < 3 seconds, or don't autoplay -->
<audio controls src="background.mp3"></audio>
```

---

## Frames & Embeds (WCAG 4.1.2)

### Iframe missing title

**Rule IDs:** `frame-title`, `frame-title-unique`

```html
<!-- Before -->
<iframe src="https://maps.example.com/embed"></iframe>

<!-- Fix -->
<iframe src="https://maps.example.com/embed" title="Office location map"></iframe>
```

### Multiple iframes need unique titles

```html
<iframe src="/header" title="Site header"></iframe>
<iframe src="/content" title="Main content"></iframe>
<iframe src="/footer" title="Site footer"></iframe>
```

### Iframe with focusable content

**Rule IDs:** `frame-focusable-content`

```html
<!-- BAD — iframe with focusable content has tabindex="-1" -->
<iframe src="/form" tabindex="-1"></iframe>

<!-- Fix — remove negative tabindex -->
<iframe src="/form" title="Contact form"></iframe>
```

---

## Timing & Motion (WCAG 2.2.1, 2.3.1)

### Blink and marquee

**Rule IDs:** `blink`, `marquee`

```html
<!-- NEVER use these elements -->
<!-- <blink>Bad</blink> -->
<!-- <marquee>Bad</marquee> -->

<!-- Use CSS animation with reduced-motion support instead -->
<div class="announcement" role="alert">Important update available</div>
```

### Meta refresh

**Rule IDs:** `meta-refresh`

```html
<!-- BAD — auto-redirects -->
<meta http-equiv="refresh" content="5;url=/new-page">

<!-- Fix — use server-side redirect (301/302) or provide a link -->
<p>This page has moved. <a href="/new-page">Go to the new page</a>.</p>
```

### Auto-refreshing content

```html
<button aria-pressed="true" onclick="toggleAutoRefresh()">Auto-refresh</button>
```

### Animation with no reduced-motion support

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Flashing content (2.3.1)

Content must not flash more than 3 times per second. If flashing is essential, keep it below the
general flash threshold: the area is small enough (< 21,824 sq px) or the luminance change is below
the threshold.

---

## Screen-Reader-Only Utility

Reusable CSS class for visually hidden but accessible text:

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

---

## React / JSX Specific Patterns

### Image alt in JSX

```jsx
<img src={logo} alt="Company logo" />
// Decorative:
<img src={divider} alt="" role="presentation" />
```

### onClick on non-interactive element

```jsx
// Before
<div onClick={handleClick}>Action</div>

// Fix: use a button
<button type="button" onClick={handleClick}>Action</button>
```

### Dynamic page title

```jsx
useEffect(() => {
  document.title = `${pageTitle} - My App`;
}, [pageTitle]);
```

### Form label in JSX

```jsx
<label htmlFor="email">Email</label>
<input type="email" id="email" />
```

### Route change announcements

```jsx
import { useLocation } from 'react-router-dom';

function RouteAnnouncer() {
  const location = useLocation();
  const [announcement, setAnnouncement] = useState('');

  useEffect(() => {
    setAnnouncement(`Navigated to ${document.title}`);
  }, [location]);

  return (
    <div role="status" aria-live="assertive" className="sr-only">
      {announcement}
    </div>
  );
}
```

### Focus management after navigation

```jsx
import { useEffect, useRef } from 'react';
import { useLocation } from 'react-router-dom';

function MainContent({ children }) {
  const mainRef = useRef(null);
  const location = useLocation();

  useEffect(() => {
    mainRef.current?.focus();
  }, [location.pathname]);

  return (
    <main ref={mainRef} tabIndex={-1} id="main-content">
      {children}
    </main>
  );
}
```

---

## Next.js Specific Patterns

### Head / metadata

```jsx
// app/layout.tsx (App Router)
export const metadata = {
  title: { default: 'My App', template: '%s - My App' },
};

// Per page
export const metadata = { title: 'Dashboard' };
// Renders: <title>Dashboard - My App</title>
```

### next/image alt

```jsx
import Image from 'next/image';
<Image src="/hero.jpg" alt="Team working together in office" width={800} height={400} />
```

### next/link

```jsx
import Link from 'next/link';
<Link href="/about">About Us</Link>
// Next.js renders <a>, no extra work needed for keyboard access
```

---

## Angular Specific Patterns

### Router-based page title

```typescript
{ path: 'dashboard', component: DashboardComponent, title: 'Dashboard - My App' }
```

### Live announcer for route changes

```typescript
constructor(private liveAnnouncer: LiveAnnouncer) {}

ngOnInit() {
  this.liveAnnouncer.announce('Dashboard page loaded');
}
```

### cdkTrapFocus for modals

```html
<div class="modal" cdkTrapFocus cdkTrapFocusAutoCapture>
  <h2>Dialog Title</h2>
  <button (click)="close()">Close</button>
</div>
```

### Angular Material a11y

```html
<!-- Angular Material components have built-in a11y -->
<mat-form-field>
  <mat-label>Email</mat-label>
  <input matInput type="email" required>
  <mat-error>Email is required</mat-error>
</mat-form-field>
```

---

## Vue Specific Patterns

### Dynamic page title

```javascript
// Vue 3 Composition API
import { watch } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
watch(() => route.meta.title, (title) => {
  document.title = `${title} - My App`;
}, { immediate: true });
```

```javascript
// Options API
watch: {
  '$route'(to) {
    document.title = `${to.meta.title} - My App`;
  }
}
```

### Accessible v-for lists

```html
<ul role="list">
  <li v-for="item in items" :key="item.id">{{ item.name }}</li>
</ul>
```

### Focus management on route change

```javascript
// router/index.js
router.afterEach((to) => {
  nextTick(() => {
    const main = document.getElementById('main-content');
    if (main) main.focus();
  });
});
```

### vue-announcer for screen readers

```html
<template>
  <div role="status" aria-live="assertive" class="sr-only">
    {{ announcement }}
  </div>
</template>
```

---

## Svelte Specific Patterns

### Page title

```svelte
<svelte:head>
  <title>{pageTitle} - My App</title>
</svelte:head>
```

### Accessible event handlers

```svelte
<!-- Svelte warns if on:click is used without on:keydown -->
<div role="button" tabindex="0"
     on:click={handleAction}
     on:keydown={(e) => { if (e.key === 'Enter' || e.key === ' ') handleAction(); }}>
  Action
</div>

<!-- Better: just use a button -->
<button on:click={handleAction}>Action</button>
```

### a11y linting

Svelte has built-in a11y warnings at compile time for:
- Missing alt text on images
- Non-interactive elements with event handlers
- Missing form labels
- Autofocus usage

---

## Web Components Patterns

### Shadow DOM and labels

```javascript
class MyInput extends HTMLElement {
  constructor() {
    super();
    const shadow = this.attachShadow({ mode: 'open', delegatesFocus: true });
    shadow.innerHTML = `
      <label for="inner-input"><slot name="label">Label</slot></label>
      <input id="inner-input" part="input">
    `;
  }

  static get observedAttributes() { return ['aria-label']; }

  attributeChangedCallback(name, _, newVal) {
    if (name === 'aria-label') {
      this.shadowRoot.querySelector('input').setAttribute('aria-label', newVal);
    }
  }
}
customElements.define('my-input', MyInput);
```

### Delegating focus

```javascript
this.attachShadow({ mode: 'open', delegatesFocus: true });
```

---

## Compliance Standards Quick Reference

| Standard | Scope | Based On | Key Requirement |
|----------|-------|----------|-----------------|
| **WCAG 2.2** | Global web standard | W3C Recommendation (Dec 2024) | Levels A, AA, AAA |
| **ADA Title III** | US — public accommodations | US law; references WCAG 2.1 AA | Websites of public entities |
| **Section 508** | US — federal agencies | Incorporates WCAG 2.0 AA | Federal ICT procurement |
| **EN 301 549** | EU — public sector | Incorporates WCAG 2.1 AA | European Accessibility Act |
| **AODA** | Ontario, Canada | WCAG 2.0 AA | Accessibility for Ontarians |
| **EAA** | EU — private sector (2025+) | WCAG 2.1 AA minimum | Products and services |

### WCAG 2.2 New Success Criteria Summary

| SC | Name | Level | Key Requirement |
|----|------|-------|-----------------|
| 2.4.11 | Focus Not Obscured (Min) | AA | Focused item not fully hidden by sticky elements |
| 2.4.12 | Focus Not Obscured (Enhanced) | AAA | No part of focused item hidden |
| 2.4.13 | Focus Appearance | AAA | 2px outline, 3:1 contrast on focus |
| 2.5.7 | Dragging Movements | AA | Single-pointer alternative to drag |
| 2.5.8 | Target Size (Minimum) | AA | 24x24 CSS px or adequate spacing |
| 3.2.6 | Consistent Help | A | Help in same relative location |
| 3.3.7 | Redundant Entry | A | Don't re-ask info already given |
| 3.3.8 | Accessible Auth (Min) | AA | No cognitive test without alternative |
| 3.3.9 | Accessible Auth (Enhanced) | AAA | No cognitive test at all |

**Removed in WCAG 2.2:** SC 4.1.1 Parsing (obsolete — modern browsers handle malformed HTML).

---

## References

- [WCAG 2.2 Specification](https://www.w3.org/TR/WCAG22/) — W3C Recommendation, December 2024
- [Understanding WCAG 2.2](https://www.w3.org/WAI/WCAG22/Understanding/) — explanations of each criterion
- [WAI-ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/) — widget patterns and examples
- [axe-core Rule Descriptions](https://github.com/dequelabs/axe-core/blob/develop/doc/rule-descriptions.md) — automated rule IDs
- [Deque University Rule List](https://dequeuniversity.com/rules/axe-devtools/4.5) — detailed rule documentation
- [Section 508 Standards](https://www.section508.gov/) — US federal accessibility requirements
- [EN 301 549](https://www.etsi.org/deliver/etsi_en/301500_301599/301549/) — European accessibility standard
