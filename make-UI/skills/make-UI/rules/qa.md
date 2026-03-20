# SnowUI Design QA

Programmatic verification gate for design prototypes. Uses MCP preview tools exclusively — **never browser screenshots** (they burn tokens and are unreliable for QA).

## Tools

- `preview_inspect` — computed styles on specific elements (dimensions, padding, colors, fonts)
- `preview_snapshot` — DOM structure, element presence, text content
- `preview_eval` — run JavaScript to check computed styles, contrast ratios, counts
- `preview_console_logs` — runtime errors (filter: "error")
- `preview_network` — failed network requests (filter: "failed")

## QA Checklist

Run these checks in order. Fix issues as found, then re-verify.

### 1. Structural Completeness
**Tool:** `preview_snapshot`

Verify all expected sections are present:
- [ ] Page container at correct dimensions (e.g., 1440x1024 for desktop dashboard)
- [ ] All major sections rendered (sidebar, header, content, right bar — whatever the design calls for)
- [ ] All content blocks present (KPI cards, chart blocks, tables, etc.)
- [ ] No empty containers or missing content
- [ ] All text nodes have actual content (no empty strings, no "undefined")

For multi-screen flows:
- [ ] All screens present and accessible
- [ ] Navigation between screens works (click links, verify destination)

### 2. Layout Dimensions
**Tool:** `preview_inspect` on container elements

Check against SnowUI specs:
- [ ] Sidebar width = 212px (if 3-column layout)
- [ ] Header height = 68px (if header present)
- [ ] Right bar width = 280px (if 3-column layout)
- [ ] Content area fills remaining width
- [ ] Grid gaps = 28px (or spec-appropriate value)
- [ ] Card dimensions match spec (e.g., KPI cards at expected width/height)
- [ ] Chart blocks at expected dimensions

### 3. Typography
**Tool:** `preview_eval` with computed styles

```javascript
// Check all heading elements use Inter
const headings = document.querySelectorAll('h1, h2, h3, [class*="title"], [class*="heading"]');
const results = Array.from(headings).map(el => ({
  text: el.textContent.substring(0, 30),
  fontFamily: getComputedStyle(el).fontFamily,
  fontSize: getComputedStyle(el).fontSize,
  fontWeight: getComputedStyle(el).fontWeight
}));
JSON.stringify(results, null, 2);
```

Check:
- [ ] All text uses Inter font family
- [ ] Value text = 24px/600 (KPI values)
- [ ] Label text = 14px/400
- [ ] Secondary text = 12px/400
- [ ] Caption text = 12px/400 at 40% opacity
- [ ] No text smaller than 12px

### 4. Colors & Opacity
**Tool:** `preview_eval` with computed styles

```javascript
// Check text color tiers — ONLY 3 valid Black opacities for text: 1.0, 0.4, 0.2
const textEls = document.querySelectorAll('p, span, h1, h2, h3, h4, h5, h6, label, td, th');
const results = [];
const badOpacities = [];
for (const el of textEls) {
  if (!el.textContent?.trim()) continue;
  const color = getComputedStyle(el).color;
  const match = color.match(/rgba?\((\d+),\s*(\d+),\s*(\d+)(?:,\s*([\d.]+))?\)/);
  if (match) {
    const alpha = match[4] ? parseFloat(match[4]) : 1.0;
    const isBlackText = match[1] === '0' && match[2] === '0' && match[3] === '0';
    if (isBlackText && ![1.0, 0.4, 0.2].some(v => Math.abs(alpha - v) < 0.05)) {
      badOpacities.push({ text: el.textContent.substring(0, 30), alpha, color });
    }
  }
}
JSON.stringify({ badOpacities: badOpacities.slice(0, 10) });
```

```javascript
// Check for hardcoded grays (should use Black/* opacity tokens instead)
const allEls = document.querySelectorAll('*');
const hardcodedGrays = [];
const grayRgbs = ['rgb(102, 102, 102)', 'rgb(153, 153, 153)', 'rgb(204, 204, 204)',
                   'rgb(51, 51, 51)', 'rgb(170, 170, 170)', 'rgb(128, 128, 128)'];
for (const el of allEls) {
  const s = getComputedStyle(el);
  if (grayRgbs.includes(s.color) || grayRgbs.includes(s.backgroundColor)) {
    hardcodedGrays.push({ tag: el.tagName, class: el.className?.toString().substring(0, 40),
      color: s.color, bg: s.backgroundColor });
  }
}
JSON.stringify({ count: hardcodedGrays.length, samples: hardcodedGrays.slice(0, 5) });
```

Check:
- [ ] Primary text: opacity 1.0 (or rgba with alpha 1)
- [ ] Secondary text: opacity 0.4 (or rgba with alpha 0.4) — labels, captions, chart axes
- [ ] Placeholder text: opacity 0.2 — input hints only
- [ ] NO text at opacity 0.8 or 0.6 (these don't exist in SnowUI)
- [ ] Background fills match token values (Background/1 = #ffffff, Background/2 = #f9f9fa)
- [ ] Borders use Black/10% (rgba(0,0,0,0.1)) — NOT hardcoded gray
- [ ] Input default stroke: Black/20% (NOT Black/40%, which is hover/focus)
- [ ] No hardcoded grays (#666, #999, #ccc) — use Black/* opacity tokens
- [ ] Status badges use same Secondary/* token for bg (10% opacity) and text (100%)
- [ ] Filled button text is white (#ffffff), not bound to White/100%
- [ ] Icon and adjacent text share the same color token

See `references/color-system-qa.md` for complete ground-truth rules.

### 5. WCAG AA Contrast
**Tool:** `preview_eval` with contrast calculation

```javascript
function luminance(r, g, b) {
  const [rs, gs, bs] = [r, g, b].map(c => {
    c = c / 255;
    return c <= 0.03928 ? c / 12.92 : Math.pow((c + 0.055) / 1.055, 2.4);
  });
  return 0.2126 * rs + 0.7152 * gs + 0.0722 * bs;
}

function contrastRatio(fg, bg) {
  const l1 = Math.max(fg, bg);
  const l2 = Math.min(fg, bg);
  return (l1 + 0.05) / (l2 + 0.05);
}

const textElements = document.querySelectorAll('p, span, h1, h2, h3, h4, h5, h6, a, button, label, td, th');
const issues = [];
for (const el of textElements) {
  if (!el.textContent.trim()) continue;
  const style = getComputedStyle(el);
  const fontSize = parseFloat(style.fontSize);
  const fontWeight = parseInt(style.fontWeight);
  const isLargeText = fontSize >= 18 || (fontSize >= 14 && fontWeight >= 700);
  const requiredRatio = isLargeText ? 3 : 4.5;
  // Parse color and backgroundColor, compute contrast, flag if below required
}
JSON.stringify(issues, null, 2);
```

Check:
- [ ] All normal text (< 18px): contrast ratio >= 4.5:1
- [ ] All large text (>= 18px or >= 14px bold): contrast ratio >= 3:1
- [ ] Interactive elements: >= 3:1 against adjacent colors

### 6. Dark Mode Parity (if applicable)
If the design includes dark mode:
- [ ] Toggle to dark mode
- [ ] Re-run checks 2-5 above
- [ ] Verify token swap happened correctly (Black/* → White/*, backgrounds inverted)
- [ ] No hard-coded colors that didn't swap

### 7. Console Clean
**Tool:** `preview_console_logs` with filter "error"

- [ ] No JavaScript runtime errors
- [ ] No failed resource loads
- [ ] No console warnings about accessibility

### 8. Network Clean
**Tool:** `preview_network` with filter "failed"

- [ ] No failed network requests (fonts, CDN resources)

## Reporting

After running all checks, summarize:
- **Pass:** checks that met spec
- **Fail:** checks that didn't, with specific values found vs expected
- **Fix:** for each fail, state what needs to change

Do not declare done until all checks pass.
