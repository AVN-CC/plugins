# SnowUI Color System — Ground Truth QA Reference

Complete color rules extracted from 41 pattern docs + 56 raw JSONs. Use this as the authoritative reference for verifying color correctness in generated UI.

---

## 1. Text Color Rules (ONLY 3 Black Opacities for Text)

| Role | Token | Light Value | Dark Value |
|------|-------|-------------|------------|
| Primary (headings, values, body) | `Black/100%` | `#000000` | `#ffffff` |
| Secondary (labels, captions, descriptions, chart axes) | `Black/40%` | `rgba(0,0,0,0.4)` | `rgba(255,255,255,0.4)` |
| Placeholder (input hints, search hints) | `Black/20%` | `rgba(0,0,0,0.2)` | `rgba(255,255,255,0.2)` |
| Inverted (on dark surfaces) | `White/100%` | `#ffffff` | `#000000` |

**NEVER use Black/80% or Black/60% for text.** Black/80% is exclusively a frame/surface background (tooltips, overlays).

### Special Text Colors

| Token | Hex | When |
|-------|-----|------|
| `Secondary/Indigo` | `#adadfb` | Links, email addresses, accent values ("$-2.60") |
| `Secondary/Red` | `#ff4747` | Destructive action text ("Delete Property") |
| `Secondary/Purple` | `#b899eb` | Status: "In Progress" |
| `Secondary/Green` | `#71dd8c` | Status: "Completed" |
| `Secondary/Blue` | `#7dbbff` | Status: "In Transit" |
| `Secondary/Yellow` | `#ffcc00` | Status: "Pending" |
| `Primary` | alias (mode-dependent) | Active/selected state text |

### Table Text

- Headers: `Black/40%` at 12px/400
- Cell data: `Black/100%` at 12px/400
- Search placeholder: `Black/20%` at 14px/400

### Chart Text

- ALL chart text (axes, legends): `Black/40%` at 12px/400 — uniformly. No chart text uses Black/100%.

---

## 2. Icon Color Rules

| Role | Token | Notes |
|------|-------|-------|
| Standard | `Black/100%` | Navigation icons, action icons |
| Phosphor duotone inner | `Black/100%` fill + **node opacity 0.08** | NOT fill opacity — node-level |
| Inactive/disabled | `Black/40%` | Grayed out icons |
| Navigation affordance | `Black/20%` | Chevrons, carets in breadcrumbs |
| On dark surface | `White/100%` | Sidebar active, tooltip icons |
| Active/selected | `Primary` | Checkbox, toggle, star, selected tab |
| Accent indicator | `Secondary/Indigo` | Notification dot, badge dot |
| Destructive | `Secondary/Red` | Trash icon |

**RULE: Icon and adjacent text ALWAYS share the same variable binding.** If text is `Secondary/Red`, the icon is `Secondary/Red`. No exceptions.

---

## 3. Background Color Rules

| Token | Light | Dark | Usage |
|-------|-------|------|-------|
| `Background/1` | `#ffffff` | `#333333` | Page-level |
| `Background/2` | `#f9f9fa` | `rgba(255,255,255,0.04)` | Cards, content insets, code blocks |
| `Background/3` | `rgba(255,255,255,0.8)` | `rgba(0,0,0,0.1)` | Input fields |
| `Background/4` | `#edeefc` | `#edeefc` (STATIC in SnowUI) | Indigo accent icon containers |
| `Background/5` | `#e6f1fd` | `#e6f1fd` (STATIC in SnowUI) | Blue accent icon containers |
| `Background/6` | `rgba(255,255,255,0.9)` | `rgba(64,64,64,0.9)` | Sidebar, date picker, overlay panels |
| `Black/4%` | `rgba(0,0,0,0.04)` | `rgba(255,255,255,0.1)` | Active sidebar item, tag fill, checkbox bg |
| `Black/80%` | `rgba(0,0,0,0.8)` | `rgba(255,255,255,0.8)` | Tooltip bg (with backdrop-blur) |
| `White/100%` | `#ffffff` | `#000000` | Card surfaces |

### Text-on-Background Pairing Rules

| Background | Text Tokens Allowed | Icon Tokens Allowed |
|------------|--------------------|--------------------|
| `White/100%` | `Black/100%`, `Black/40%`, `Black/20%` | `Black/100%`, `Black/40%` |
| `Background/2` | `Black/100%`, `Black/40%` | `Black/100%` |
| `Black/80%` (tooltip) | `White/100%` | `White/100%` |
| `Black/4%` (subtle) | `Black/100%`, `Black/40%` | `Black/100%` |
| `Primary` (accent) | `White/100%` | `White/100%` |
| `Secondary/Indigo` (badge) | hardcoded `#ffffff` | — |
| `Background/4` (STATIC `#edeefc`) | StaticBlack/100%, StaticBlack/40% | StaticBlack/100% |
| `Background/5` (STATIC `#e6f1fd`) | StaticBlack/100%, StaticBlack/40% | StaticBlack/100% |

**BUILD RULE — STATIC BACKGROUNDS:** When placing text on Background/4 or Background/5, you MUST use StaticBlack/* tokens (or hardcoded dark values) — NOT Black/*. Black/* inverts to white in dark mode, making text invisible on the still-light surface. Same applies to any hardcoded/brand-color background: always pair with non-inverting text tokens.

---

## 4. Stroke/Border Rules

| Token | Light | Dark | Usage |
|-------|-------|------|-------|
| `Black/10%` | `rgba(0,0,0,0.1)` | `rgba(255,255,255,0.15)` | Section dividers, card borders, table separators |
| `Black/4%` | `rgba(0,0,0,0.04)` | `rgba(255,255,255,0.1)` | Very subtle table cell borders |
| `Black/20%` | `rgba(0,0,0,0.2)` | `rgba(255,255,255,0.2)` | Input default border (0.5px) |
| `Black/40%` | `rgba(0,0,0,0.4)` | `rgba(255,255,255,0.4)` | Input hover/focus border |
| `Black/80%` | `rgba(0,0,0,0.8)` | `rgba(255,255,255,0.8)` | Checkbox/radio borders |
| `Primary` | mode-dependent | mode-dependent | Active/selected indicator strokes |

### Section Dividers

Always `strokeBottomWeight: 1` with all other sides at 0 (bottom-only border). Uses `Black/10%`.

### Input Stroke States

```
Default:  Black/20% @ 0.5px
Hover:    Black/40% @ 0.5px
Focus:    Black/40% @ 0.5px + Focus ring (rgba(0,0,0,0.04) spread-4)
```

---

## 5. Primary Token — Mode-Dependent Alias

| Mode | Resolves To | Hex |
|------|-------------|-----|
| SnowUI-Light | `Black/100%` | `#000000` |
| SnowUI-Dark | `Secondary/Indigo` | `#adadfb` |
| iOS-Light | `Secondary/Blue` | `#007aff` |
| iOS-Dark | `Secondary/Blue` | `#0a84ff` |

### Components Bound to Primary

Filled button bg, active tab text + underline, toggle active track, checkbox checked fill, selected card stroke, chart primary series, active sidebar text, pagination active indicator, progress bar fill, search focus icon.

---

## 6. Status Badge Pattern

Same token for bg AND text, differentiated by node opacity:

```
Background frame: fills="Secondary/Purple", node opacity=0.1
Text label:       fills="Secondary/Purple", node opacity=1.0
Dot indicator:    fills="Secondary/Purple", node opacity=1.0
```

Variants: Purple, Green, Blue, Yellow — all follow this exact pattern.

In CSS:
```css
.badge-bg { background: rgba(184, 153, 235, 0.1); }  /* Secondary/Purple @ 10% */
.badge-text { color: #b899eb; }                        /* Secondary/Purple @ 100% */
```

---

## 7. Filled Button Text — Hardcoded White

Filled buttons use `Primary` fill with **hardcoded `#ffffff`** for text and icons — NOT `White/100%`.

`White/100%` would invert to black in dark mode, making text invisible on the (now indigo) Primary background. The hardcoded white ensures text is always white on the colored button.

---

## 8. Asymmetric Dark Mode Opacity (CRITICAL)

`Black/10%` and `Black/4%` do NOT simply swap to White at the same opacity:

| Token | Light | Dark | Boost |
|-------|-------|------|-------|
| `Black/10%` | 10% black | **15%** white | 1.5x |
| `Black/4%` | 4% black | **10%** white | 2.5x |

This prevents subtle elements from vanishing on dark backgrounds. All other Black/* tokens swap symmetrically.

---

## 9. Static Colors (Never Change with Theme)

StaticBlack/* and StaticWhite/* are Figma **styles** (not variables). They do NOT appear in `boundVariables`. They're identical to their Black/White counterparts in light mode but do NOT invert in dark mode.

Use cases: elements that must remain the same color regardless of theme (rare in SnowUI — the design system prefers mode-aware tokens).

---

## 10. Chart Dual-Palette

The same variable bindings resolve to different palettes per mode:

| Token | SnowUI (pastel) | iOS (saturated) |
|-------|-----------------|-----------------|
| `Secondary/Indigo` | `#adadfb` | `#5856d6` / `#5e5ce6` |
| `Secondary/Blue` | `#7dbbff` | `#007aff` / `#0a84ff` |
| `Secondary/Purple` | `#b899eb` | `#af52de` / `#bf5af2` |
| `Secondary/Green` | `#71dd8c` | `#34c759` / `#30d158` |

SnowUI secondaries are STATIC (same in light and dark). iOS secondaries shift slightly between light/dark (dark is brighter).

---

## 11. Dark Mode Token Collision Rules

These are cases where naively using inverting tokens creates invisible text. Verified against raw Figma JSONs.

### Hardcoded White Pattern (Designer-Solved)
The SnowUI designer uses **hardcoded `#ffffff`** (not `White/100%`) in these places to prevent dark mode inversion:
- Filled button text/icon
- Toggle knob fill
- Checkbox checkmark fill
- Badge text on Secondary/* backgrounds

**BUILD RULE:** When placing text/icons on a `Primary` or `Secondary/*` surface, ALWAYS use hardcoded `#ffffff` — NEVER `White/100%`. `White/100%` inverts to black, making content invisible on the colored surface.

### Tooltip Dark Mode Collision
- **Tooltip bg**: Hardcoded `#000000` @ 0.8 opacity — does NOT invert (stays black in dark mode)
- **Tooltip text**: If bound to `White/100%`, it inverts to BLACK → invisible on black bg

**BUILD RULE:** Tooltip text must be hardcoded `#ffffff` (or StaticWhite), NOT `White/100%`. The tooltip bg is hardcoded black and does not invert, so the text must also be non-inverting. Secondary tooltip text: hardcoded `#ffffff` at 40% opacity.

### Notification Dark Mode Collision
- **Notification bg**: Bound to `Black/80%` + `White/10%` — these DO invert (bg becomes light/white-tinted in dark mode)
- **Notification text**: If bound to `White/100%`, it inverts to BLACK on the now-light bg — correct! Text becomes dark on light surface.

**BUILD RULE:** Notification text should use `White/100%` (inverting) because the notification bg ALSO inverts. This is a case where both inverting is correct — the bg and text invert together. However, the success icon (`Secondary/Green`) is static and stays green in both modes.

### Summary: When to Use What

| Surface | Text Token | Why |
|---------|-----------|-----|
| `Primary` fill (button, toggle, checkbox) | Hardcoded `#ffffff` | Primary changes color per mode but is always "colored" — white text is always readable |
| `Secondary/*` fill (badge bg) | Hardcoded `#ffffff` | Same reason — secondary colors are always colored |
| Hardcoded black bg (tooltip) | Hardcoded `#ffffff` or StaticWhite | bg doesn't invert, so text can't invert either |
| Inverting bg (`Black/80%`, `Background/*`) | `White/100%` or `Black/100%` | Both invert together — stays readable |
| STATIC bg (`Background/4`, `/5`) | StaticBlack/* | bg stays light, text must stay dark |

---

## 12. Anomalies and Exceptions

1. **Avatar abbreviation text**: Hardcoded `#1c1c1c` — no variable binding. Only text with a non-standard hardcoded color.
2. **Badge text on Secondary/Indigo bg**: Hardcoded `#ffffff` — no `White/100%` binding.
3. **Filled button text/icon**: Hardcoded `#ffffff` — NOT `White/100%`.
4. **Tooltip secondary text**: Raw `#ffffff` at 40% opacity — no token binding.
5. **Settings backdrop gradient**: Hardcoded `#d7d0ff @ 0.2` → `#cbddff @ 0.5` — not token-bound.

---

## QA Validation Script

Use with `preview_eval` to validate color correctness:

```javascript
// Check for hardcoded grays (should be Black/* opacity instead)
const allElements = document.querySelectorAll('*');
const hardcodedGrays = [];
for (const el of allElements) {
  const style = getComputedStyle(el);
  const color = style.color;
  const bg = style.backgroundColor;
  // Flag common mistakes: #666, #999, #ccc, #333, #aaa
  const grayHexes = ['rgb(102, 102, 102)', 'rgb(153, 153, 153)', 'rgb(204, 204, 204)',
                      'rgb(51, 51, 51)', 'rgb(170, 170, 170)', 'rgb(128, 128, 128)'];
  if (grayHexes.includes(color) || grayHexes.includes(bg)) {
    hardcodedGrays.push({
      tag: el.tagName,
      class: el.className?.toString().substring(0, 40),
      color, bg,
      text: el.textContent?.substring(0, 20)
    });
  }
}
JSON.stringify({ count: hardcodedGrays.length, samples: hardcodedGrays.slice(0, 10) });
```

```javascript
// Verify text opacity tiers (should only be 1.0, 0.4, or 0.2 for text)
const textEls = document.querySelectorAll('p, span, h1, h2, h3, h4, h5, h6, label, td, th');
const badOpacities = [];
for (const el of textEls) {
  if (!el.textContent?.trim()) continue;
  const color = getComputedStyle(el).color;
  const match = color.match(/rgba?\((\d+),\s*(\d+),\s*(\d+)(?:,\s*([\d.]+))?\)/);
  if (match) {
    const alpha = match[4] ? parseFloat(match[4]) : 1.0;
    const validAlphas = [1.0, 0.4, 0.2];
    const isValid = validAlphas.some(v => Math.abs(alpha - v) < 0.05);
    if (!isValid && alpha < 1.0) {
      badOpacities.push({
        text: el.textContent.substring(0, 30),
        alpha: alpha.toFixed(2),
        color
      });
    }
  }
}
JSON.stringify({ count: badOpacities.length, issues: badOpacities.slice(0, 10) });
```
