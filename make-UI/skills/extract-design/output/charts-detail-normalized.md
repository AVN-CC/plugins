# Chart {Code-Connect} — Deep Extraction Report

Source: Figma file "Chart {Code-Connect}" (LKT5XjsHrCxjVx8YENYZPu)
Extracted: 2026-03-20
Pipeline: A (figma-console Desktop Bridge)
Scope: Full file — all 19 pages
Total raw extraction: 5.1MB across 9 JSON dump files

---

## Local Variables (unique to this file)

These variables are NOT in the SnowUI/iOS library — they are local to the Charts file:

### Line Corner Radius

| Token | Small | Medium | Big |
|-------|-------|--------|-----|
| Line corner radius | 2 | 16 | 28 |

Usage: Controls corner radius on bar chart rectangles and line chart stroke caps.

### Dot Size

| Token | Dot | Dot 2 | Dot 3 |
|-------|-----|-------|-------|
| Small | 4 | 8 | 10 |
| Medium | 8 | 16 | 20 |
| Big | 10 | 20 | 24 |

Usage: Scatter chart dot sizes, legend indicator dots, line chart data points.

---

## Bar Style Architecture (6 Styles x Vertical/Horizontal)

All bar styles share the same variant axes:
- **Width**: 1, 2, 4, 8, 16, 24, 32, 40, 48, 56, 64, 72, 80 (px, controls bar thickness)
- **Height**: 1-6 (data value level, maps to bar length)

Container: FIXED width x 160px height, `clipsContent: true`, `layoutMode: NONE`

### Style 1 — Single Solid Bar
```
Structure: 1 RECTANGLE
Fill: Primary (#007AFF)
Corner radius: 20 (bound)
Variants: 78 (13 widths x 6 heights)
```

### Style 2 — Background + Foreground (proportional bg)
```
Structure: 2 RECTANGLEs (stacked)
Rectangle 1 (bg): Primary @ opacity 0.1, height = data level height
Rectangle 2 (fg): Primary @ full opacity, height = ~70% of bg height
Corner radius: 20 (bound)
Variants: 66 (11 widths x 6 heights)
```

### Style 3 — Background + Foreground (full-height bg)
```
Structure: 2 RECTANGLEs (stacked)
Rectangle 1 (bg): Primary @ opacity 0.1, height = ALWAYS 160px (full container)
Rectangle 2 (fg): Primary @ full opacity, height = proportional
Corner radius: 20 (bound)
Variants: 66
```

### Style 4 — Three-Segment Stacked Bar
```
Structure: 3 RECTANGLEs (stacked vertically)
Rectangle 1 (top): Black/20% + Black/100% + GRADIENT_LINEAR overlay
  - Corner radius: 20 (all corners)
Rectangle 2 (mid): Secondary/Indigo (#5856D6)
  - Corner radius: 20 (bottom only)
Rectangle 3 (bot): Primary (#007AFF)
  - Corner radius: 20 (bottom only)
Variants: 66
```

### Style 5 — Side-by-Side Bars
```
Structure: 2 RECTANGLEs (side by side)
Container width: 3x bar width (e.g., Width=4 → container=12px)
Rectangle 1: Primary, full bar width
Rectangle 2: Secondary/Indigo, full bar width, positioned adjacent
Corner radius: 20 (bound)
Variants: 66
```

### Style 6 — Pill Shape (small widths only)
```
Structure: 3 RECTANGLEs (stacked, separate pills)
Rectangle 1: Primary
Rectangle 2: Secondary/Indigo
Rectangle 3: Black/20% + Black/100% + gradient
Corner radius: 80 (pill shape, not 20)
Widths: Only 1, 2, 4, 8 (narrower set)
Variants: 24
```

---

## Donut Chart Component

### Donut Component Set (55 variants)
Variant axes: `Segment=2-6, Type=Small/Medium/Big/S1/S2/S3/S4/S5/S6/S7/S8`

Structure per variant:
```
COMPONENT — 160x160 (varies by Type)
  ├── Subtract (VECTOR) — segment 1, cornerRadius=4 (bound)
  │     fills: [SOLID Black/20%, SOLID Black/100%, GRADIENT_LINEAR white→white 5%→40%]
  ├── Subtract (VECTOR) — segment 2, cornerRadius=4
  │     fills: [SOLID Primary, GRADIENT_LINEAR white→white 5%→40%]
  ├── Subtract (VECTOR) — segment 3 (if Segment≥3)
  │     fills: [SOLID Secondary/Indigo, GRADIENT_LINEAR]
  ├── Subtract (VECTOR) — segment 4 (if Segment≥4)
  │     fills: [SOLID Secondary/Red, GRADIENT_LINEAR]
  ├── Subtract (VECTOR) — segment 5 (if Segment≥5)
  │     fills: [SOLID Secondary/Orange, GRADIENT_LINEAR]
  └── Subtract (VECTOR) — segment 6 (if Segment≥6)
        fills: [SOLID Secondary/Yellow, GRADIENT_LINEAR]
```

**Key pattern**: Every donut segment is a VECTOR created by boolean Subtract operation (arc shape), each with dual fills:
1. Solid color (token-bound)
2. White gradient overlay (5% → 40% opacity) for 3D glass effect

**Donut Chart standalone component** (603:3021):
```
COMPONENT — 400x184
  layoutMode: VERTICAL
  itemSpacing: 16 (bound)
  ├── Semicircle/Style 1 (INSTANCE) — 360x180
  └── Legend (INSTANCE) — tag/dot/label/value pattern
```

### Color Palette for Segments

| Segment | Token | RGB |
|---------|-------|-----|
| 1 | Black/20% + Black/100% | #000000 @ 0.2 + #000000 |
| 2 | Primary | #007AFF |
| 3 | Secondary/Indigo | #5856D6 |
| 4 | Secondary/Red | (red) |
| 5 | Secondary/Orange | (orange) |
| 6 | Secondary/Yellow | (yellow) |

---

## Semicircle Chart Component

### Semicircle/Style 1 (55 variants)
Same variant structure as Donut: Segment=2-6, Type=Small/Medium/Big/S1-S8

Structure: Same as Donut but half-circle (180px height vs 360px width)
- VECTOR Subtract shapes, each 360x180
- Same dual-fill pattern (solid + gradient overlay)
- cornerRadius=4 (bound)

### Semicircle/Style 2 (4 variants)
Simplified variant with fewer segment options.

### Semicircle/Style 3
Frame-based (not component set), 360x180 with 5 children.

### Standalone Semicircle Chart Component (603:3017)
```
COMPONENT — 360x220
  layoutMode: VERTICAL
  itemSpacing: 16 (bound)
  counterAxisAlignItems: MAX (right-aligned)
  sizingV: HUG
  ├── Semicircle/Style 1 (INSTANCE) — 360x180
  ├── Legend row 1
  └── Legend row 2
```

---

## Scatter Chart Component

```
COMPONENT — 440x138
  layoutMode: NONE (absolute positioned)
  cornerRadius: 8 (bound)
  paddingTop: 16 (bound)
  itemSpacing: 8
  counterAxisAlignItems: CENTER
  primaryAxisAlignItems: SPACE_BETWEEN
```

Contains 27 Dot2 instances positioned absolutely to form scatter pattern.
Each dot: VECTOR 12x12, fill = Secondary/Indigo (bound), inside 32x32 frame.

---

## Proportion Statistics Component

```
COMPONENT — 440x200
  layoutMode: HORIZONTAL
  itemSpacing: 7
  counterAxisAlignItems: MAX
  primaryAxisAlignItems: SPACE_BETWEEN
```

Three data series, each with proportional line lengths + labels:
- **Old users 52%** — Black/100% stroke, weight=2
- **New users 18%** — Primary stroke, weight=2
- **Visitors 30%** — Black/40% stroke, weight=2

Text hierarchy per series:
- Label: 14px Regular, fill matches stroke token
- Value: 32px Semi Bold, fill matches stroke token

---

## Motion Pages (Animation Sequences)

### Chart Motion (3 variants)
ChartMotion component set with animation keyframes.
"Chart components" documentation frame (3500x940, 6 children).

### Bar Chart Motion
18 top-level components: Vertical 01-12, Horizontal 01-04
Plus Vertical 09 component set (2 variants) and documentation frame (2700x1620).
Total: 200 text nodes, 482 instances, 247 frames.

### Line Chart Motion
Line 01 component set (2 variants) + Line 02-05 standalone components.
Line chart components documentation frame (1604x1000).

### Donut Chart Motion
DonutChart component set (5 variants) + SemicircleChart standalone.
Donut chart components documentation frame (1752x784).

---

## Instances Page (Adaptive Sizing Matrix)

224 total instances demonstrating adaptive behavior:

### Chart Instances (96 instances)
Width progression: 800 → 1000 → 1200 → 1400 → 1600 → 1800 → 2000 → 2200
Height progression: 400 → 500 → 600 → 700 → 800 → 900 → 1000 → 1100 → 1200 → 1600 → 1800 → 2000 → 2200

Types: Horizontal, Vertical, Line (captured via instance properties)

### Donut Instances (55 instances)
All 160x160, varying childCount (2-6) = number of segments visible.

### Semicircle Instances (59 instances, Styles 1-59)
All 360x180, varying childCount (2-6).

### Capability Annotations (from TEXT nodes)
- "Supports 1-12 data, adaptive width/height, 11 bar widths, 5 bar styles" (bar charts)
- "Supports 2-6 data, adaptive width/height, 11 donut thicknesses" (donuts)
- "Supports 2-6 data, adaptive width/height, 11 semicircle thicknesses, 2 styles" (semicircles)
- "Supports 1-12 data, adaptive width/height, Line 2 can be hidden" (line charts)

---

## Charts Page (Dashboard Compositions)

117 individual chart card frames + 7 dashboard layouts.

### Chart Card Sizes
- Standard: 480x480 (most common)
- Wide: 1040x480
- Tall: 400x504 to 400x656
- Short: 480x344 to 480x368
- Medium: 480x404 to 480x428

### Dashboard Layouts (7 layouts at 1600x960)
Named 02-08, each composing multiple chart cards into dashboard views.

---

## Deduplication Notes vs Existing Docs

### Already documented in chart.md:
- Variant axes (Type: Horizontal/Vertical/Line, Show Left Text)
- Chart component layout (800x400, HORIZONTAL, gap=16)
- Y-axis label structure
- Grid line specs
- Bottom text structure
- Basic bar specifications

### NET NEW from this file:
1. **6 bar style architectures** (chart.md only mentions bar thickness, not style variations)
2. **Local variables** (Line corner radius, Dot size) — not in SnowUI token table
3. **Donut SVG construction** (boolean-subtracted arcs with dual fills)
4. **Semicircle SVG construction** (same pattern as donut, half-circle)
5. **Scatter Chart component** (27 dots, absolute positioned)
6. **Proportion Statistics component** (proportional line visualization)
7. **Motion/animation keyframes** (4 motion pages with component variants)
8. **Full adaptive sizing matrix** (224 instances across width/height ranges)
9. **Dashboard composition layouts** (7 dashboard arrangements)
10. **Capability annotations** (data limits, bar widths, thicknesses)

### Enriches existing docs:
- chart-graphics.md: Bar segment fills now fully characterized per style
- charts-resource.md: Dashboard layouts can be cross-referenced
- chart-instances.md: Adaptive sizing data now complete with variant properties
