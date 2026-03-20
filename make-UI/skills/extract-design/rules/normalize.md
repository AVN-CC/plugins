# Normalization Rules

Transform raw extraction data into make-UI reference file format.

---

## Color Value Conversion

### Figma RGBA to Hex

Figma returns colors as `{ r: 0-1, g: 0-1, b: 0-1 }` with separate opacity.

```
r, g, b in 0-1 range → multiply by 255, round, convert to hex
opacity → if < 1, append as 2-digit hex (opacity * 255)

Examples:
  { r: 0, g: 0, b: 0 }, opacity: 1.0     → #000000
  { r: 0, g: 0, b: 0 }, opacity: 0.4      → #00000066
  { r: 1, g: 1, b: 1 }, opacity: 0.04     → #ffffff0a
  { r: 0.678, g: 0.678, b: 0.984 }        → #adadfb
```

### Node Opacity vs Fill Opacity

Figma has TWO opacity systems:
- **Fill opacity**: `paint.opacity` — the color's alpha channel
- **Node opacity**: `node.opacity` — the entire element's opacity

These are different! A node with `opacity: 0.1` and a fill of `Secondary/Purple` at `opacity: 1.0` means the FRAME is 10% opaque, not the color. In CSS:

```css
/* Fill opacity = color alpha */
background: rgba(184, 153, 235, 0.1);  /* Secondary/Purple at 10% fill opacity */

/* Node opacity = element opacity */
opacity: 0.1;  /* The entire element is 10% opaque */
```

The badge pattern uses node opacity (frame at 0.1, text at 1.0). **Always check which one the binding uses.**

---

## Token Name Mapping

### Standard Categories

Map Figma variable collection names to reference file sections:

| Figma Collection | Reference Section |
|-----------------|------------------|
| Colors, Color | Colors section in tokens.md |
| Spacing, Space, Gap | Spacing section |
| Radius, Corner Radius, Border Radius | Corner Radius section |
| Size, Sizing | Size section |
| Font, Typography, Type | Font section + Typography Scale |
| Font Weight, Weight | Font Weight section |
| Paragraph, Paragraph Spacing | Paragraph Spacing section |

### Token Name Hierarchy

Preserve the `/` separator from Figma variable names:
- `Background/1` stays `Background/1`
- `Black/100%` stays `Black/100%`
- `Secondary/Indigo` stays `Secondary/Indigo`

For non-Figma sources, normalize to `Category/Name` format:
- `--color-primary` → `Primary`
- `colors.background.default` → `Background/Default`
- `$spacing-4` → `Spacing/4`

---

## Component State Normalization

### Detecting State Types from Variant Deltas

| Delta Pattern | Likely State |
|--------------|-------------|
| Opacity → 0.4 on container | Disabled |
| Shadow with spread appears | Focus |
| Stroke appears or darkens | Hover |
| Fill changes to Primary | Active / Selected |
| Fill changes to Secondary/Red | Error |
| Stroke changes to Secondary/Red | Error |
| Spinner or skeleton replaces content | In Progress |
| Checkmark or success icon appears | Done / Success |

### Building the State Matrix

For each component set with state variants (Script 6 output):

1. Find the "Default" variant (usually named `Default`, `Rest`, `Normal`, or the first variant)
2. For each other variant, compute the delta from Default:
   - Which properties changed?
   - What are the new values?
3. Express the delta as: `[property]=[value]`
4. Group by component name

---

## Behavioral Rule Extraction

### From Text Annotations

Figma annotation frames contain TEXT nodes with behavioral rules. Look for:
- Frames with names like "Guidance", "Notes", "Annotation", "Documentation"
- TEXT nodes inside these frames
- Content describing: triggers, timing, animations, state transitions

### From Component Structure

Infer behavioral rules from structure:
- Boolean properties named `Show*` or `Visible*` → hover-reveal pattern
- Instance swap slots → composition/swappable content
- Nested component instances → slot architecture

### From Visual Analysis (Screenshots)

When analyzing screenshots, look for:
- Multiple states shown side-by-side (state machine documentation)
- Arrow annotations (flow/transition indicators)
- Numbered annotations (step sequences)
- Highlighted regions (interactive areas)

---

## Dark Mode Analysis

### From diff-light-dark-frames (Script 5)

Classify each diff entry:

| Pattern | Classification | Action |
|---------|---------------|--------|
| `hasBoundVariables: true`, fills differ | Correct token binding | Document the token's light/dark values |
| `hasBoundVariables: false`, fills differ | Hardcoded override | Flag for review — may be intentional (tooltip bg, brand colors) |
| `STRUCTURAL_DIFF` | Layout change | Flag — dark mode should not change structure |
| Same fills in both modes | Static color | Note as non-inverting (static background, brand color) |

### Collision Detection

After extraction, check every text-on-background pairing:

```
For each TEXT node:
  1. Find its parent FRAME's fill token
  2. Find the TEXT node's fill token
  3. Resolve both tokens in dark mode
  4. Check: does text remain readable on the dark-mode background?

Red flags:
  - Text uses White/100% (inverts to black) on a hardcoded dark bg (stays dark) → invisible
  - Text uses Black/* (inverts to white) on a static light bg (stays light) → invisible
```

---

## Output File Writing

### Always write to `output/` first

```
skills/extract-design/output/
├── tokens.md
├── color-system-qa.md
├── component-states.md
├── composition-rules.md
├── interaction-patterns.md
└── data-formats.md
```

### Include extraction metadata at the top of each file

```markdown
# [Design System] [File Type]

Source: [Figma file name / URL / screenshot path / repo]
Extracted: [YYYY-MM-DD]
Pipeline: [A/B/C/D/E]
Scope: [full/page/component/tokens]

---
```

### Mark uncertain values

- `[approximate]` — value inferred from screenshot, not from token data
- `[CONFLICT]` — conflicts with existing reference data
- `[UNRESOLVED]` — variable reference couldn't be resolved
- `[verify]` — value extracted but should be manually confirmed
