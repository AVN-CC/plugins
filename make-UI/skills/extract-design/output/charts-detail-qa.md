# Chart {Code-Connect} — QA Reference

Source: Figma file "Chart {Code-Connect}" (LKT5XjsHrCxjVx8YENYZPu)
Extracted: 2026-03-20
Pipeline: A (figma-console Desktop Bridge)
Scope: Full file — all 19 pages
Total raw extraction: 5.1MB across 9 JSON dump files
Cross-referenced against: chart.md, chart-graphics.md, charts-resource.md, chart-instances.md (v5 UI Kit extractions)

---

## 1. Two Chart Systems: Static vs Interactive

The Charts file contains TWO distinct chart systems that must not be confused:

### ChartMotion (Static System)

| Property | Value |
|----------|-------|
| Component set | `ChartMotion` |
| Variants | 3: `Type=Vertical`, `Type=Horizontal`, `Type=Line` |
| Size | 800x400 (all variants) |
| Layout | HORIZONTAL, gap=16 (bound) |
| Interaction | None — static display only |
| Usage | Dashboard card compositions, charts-resource page |

**Structure:** Left Text (Y-axis) + Frame containing Chart Line (grid) + Bottom Text (X-axis) + Bars/Line Graph. Bars are single rectangles per column. No tooltips. No animation segments.

### Chart Components (Interactive System)

| Property | Value |
|----------|-------|
| Location | "Chart components" frame (3500x940) on Chart motion page |
| Sub-components | 6 component sets: Bottom Text, Left Text, Horizontal Line, Vertical Line, Horizontal Bar, Vertical Bar |
| Bar variants | Number=1-12 (data count) |
| Interaction | State-aware tooltips (opacity=0 → 1 on hover) |
| Animation | 8-segment bar architecture with opacity toggles |

**Key difference:** Interactive bars have 8 RECTANGLE segments per bar (for fill animation) + embedded Tooltip instances. Static bars have 1 rectangle per bar, no tooltips.

### QA Rule 1: Never mix component systems

When building chart UIs:
- Use **ChartMotion** for static dashboard displays (no hover, no animation)
- Use **Chart components** (Vertical Bar, Horizontal Bar sub-components) for interactive charts with tooltips and animation

---

## 2. Tooltip Specification

### Tooltip Component (Dark Variant)

| Property | Value | Token |
|----------|-------|-------|
| Layout | HORIZONTAL, HUG/HUG | — |
| Corner radius | 12px | bound `12` |
| Padding | 4/8/4/8 | bound |
| Item spacing | 4px | bound |
| Default opacity | 0 (hidden) | — |
| Hover opacity | 1 (visible) | — |

### Tooltip Fill System (Glass Effect)

```
Layer 1: SOLID Black/80% (#000000 @ 0.8)    ← bound token
Layer 2: SOLID White/10% (#ffffff @ 0.1)     ← bound token
Effect: BACKGROUND_BLUR radius=40
```

This creates a frosted glass appearance. The dual-fill + blur is critical — do not simplify to a single fill.

### Tooltip Text

| Property | Value | Token |
|----------|-------|-------|
| Font | Inter Regular | bound `Font` + `Regular` |
| Size | 12px | — |
| Line height | 16px | — |
| Color | White (#ffffff @ 1.0) | — |
| Letter spacing | 0px | bound `Letter spacing` |
| Example values | "6,000", "21,256,598", "2,000f", "4,000" | — |

### Tooltip Positioning

| Chart type | Position | Dimensions |
|-----------|----------|-----------|
| Vertical bar | Centered above bar column, y=-28 | 33x16 to 62x16 |
| Horizontal bar | Centered above bar row, y=-34 | 49x26 to 62x24 |
| Scatter (Vertical 10) | Positioned at data point | 62x16 |

### Tooltip States

| State | Opacity | Trigger |
|-------|---------|---------|
| Default/Rest | 0 | — |
| Hover | 1 | Mouse over bar/data point |

### QA Rule 2: Tooltip glass effect requires dual fills + blur

Never implement tooltip as a single solid background. The correct implementation is:
```css
background: rgba(255,255,255,0.1); /* White/10% */
background: rgba(0,0,0,0.8);      /* Black/80% layered on top */
backdrop-filter: blur(40px);
border-radius: 12px;
```

---

## 3. Bar Animation Architecture (8-Segment System)

### Segment Structure

Each interactive bar (Vertical Bar / Horizontal Bar from Chart components) contains exactly 8 RECTANGLE segments:

```
Bar instance (VERTICAL or HORIZONTAL auto-layout)
├── Rectangle 1 (top/left segment)    — corner radius on outer edge
├── Rectangle 2
├── Rectangle 3
├── Rectangle 4
├── Rectangle 5
├── Rectangle 6
├── Rectangle 7
├── Rectangle 8 (bottom/right segment) — corner radius on outer edge
└── Tooltip (INSTANCE) — opacity: 0
```

### Segment Properties

| Property | Value | Token |
|----------|-------|-------|
| Sizing H | FILL | — |
| Sizing V | FILL | — |
| Max width | 28px | bound |
| Fill | Same color token per bar | bound (Primary, Secondary/*) |
| Opacity | 0 or 1 (controls animation) | — |

### Animation Keyframe Progression

The Vertical 01→12 and Horizontal 01→04 components represent animation keyframes. Each uses a different bar sub-component showing a different stage:

| Keyframe | Bar component | Segment pattern | Visual effect |
|----------|--------------|-----------------|---------------|
| Vertical 01 | VerticalBar 01 A | 1 rect, opacity=1 | Single solid bar (simple) |
| Vertical 02 | VerticalBar 02 A | 2 rects: bg@0.2 + fg@1 | Background + foreground proportional |
| Vertical 03 | VerticalBar 03 | 16 rects: 8@0 + 8 with gradient opacity (0.2→1.0) | Gradient fade-in animation |
| Vertical 04 | VerticalBar 04 | 8 rects: 4@0, 4@1 | Half-filled (fill from bottom) |
| Vertical 05 | VerticalBar 05 | 8 rects: 4@0, 4@1 | Half-filled variant |
| Vertical 06 | VerticalBar 06 | 5 rects: 1@0, 4@1 | Mostly filled |
| Vertical 07 | VerticalBar 07 | 2 rects: bg@0.5 + fg@1 | Dimmed background + full foreground |
| Vertical 08 | VerticalBar 08 | 2 rects: both@1 | Full height, two-tone |
| Vertical 09 | Component set | Property=A/B variants | Two visual states |
| Vertical 10 | Scatter/proportion | Vector lines + visible tooltip | Data point with tooltip shown |
| Vertical 11 | VerticalBar 9 | 1 rect, single color (Primary) | Monochrome bar |
| Vertical 12 | VerticalBar 10 | 2 rects: bg@0.1 + fg@1 | Subtle bg + solid foreground |

### Horizontal Bar Animation

Horizontal bars use the same 8-segment system but in HORIZONTAL layout:

| Bar | Segment pattern (left to right) |
|-----|--------------------------------|
| HorizontalBar 01 | 8 segments with varying opacity (4 on, 4 off pattern varies per bar) |
| HorizontalBar 02 | 16 segments: two stacked rows of 8 (foreground + background) |

### Color Assignment per Bar Position

Each bar in a chart uses a different color token:

| Position | Token | Hex (light) |
|----------|-------|-------------|
| 1 | Secondary/Cyan | #a0bce8 |
| 2 | Secondary/Mint | #6be6d3 |
| 3 | Primary | #007aff |
| 4 | Secondary/Blue | #7dbbff |
| 5 | Secondary/Purple | #b899eb |
| 6 | Secondary/Green | #71dd8c |

### QA Rule 3: Bar fill animation uses segment opacity, not height

Do NOT animate bars by changing height. The Figma design uses 8 fixed-height segments that toggle opacity (0→1) to create the fill animation effect. This allows smooth per-segment transitions.

---

## 4. Line Chart Motion

### Line Components (Chart motion page)

| Component | Type | Children | Purpose |
|-----------|------|----------|---------|
| Line 01 | Component set | State=Default, State=Hover | Interactive line with hover state |
| Line 02 | Component | Frame (line path) + Frame (area) | Line with area fill |
| Line 03 | Component | Background + Vertical Line grid + Line A + Dot | Line with dots and grid |
| Line 04 | Component | Background + Vertical Line grid + Line A | Line with grid (no dots) |
| Line 05 | Component | Extended width (1136x280) | Wide line variant |

### Line Sub-Components (in "Line chart components" frame)

| Component set | Variants | Purpose |
|---------------|----------|---------|
| Line A | Property=Default, 2, 3 | Primary line (3 animation states) |
| Line B | Property=Default, 2, 3 | Secondary line (3 animation states) |
| ChartDot | Property=A, B, C | Data point indicators |

### ChartDot Variants

| Variant | Size | Fill token | Detail |
|---------|------|-----------|--------|
| Property=A | 32x32 container, 16x16 ellipse | Secondary/Cyan | Teal dot |
| Property=B | 32x32 container, 16x16 ellipse | Primary | Blue dot |
| Property=C | 16x16 container | Black/100% outer + White/100% inner (6x6) | Ring dot |

### Line 01 Hover Behavior

| State | Children | Difference |
|-------|----------|-----------|
| Default | Line B + Line A | Two overlapping lines, no interaction indicator |
| Hover | Line B + Line A + Number frame (106x185) | Adds vertical indicator with data value |

### QA Rule 4: Line hover shows vertical indicator + value

On line chart hover, a vertical `Number` frame (106x185) appears at the hover position showing the data value. This is NOT a tooltip — it's a taller vertical indicator anchored to the line.

---

## 5. Donut/Semicircle Motion

### DonutGraph Component Set

Each donut segment is its own component with 4 animation states:

| State | Purpose |
|-------|---------|
| Default | Initial state before animation |
| Appear | Entrance animation (segment grows in) |
| Normal | Resting state after appear animation |
| Hover | Hover highlight state |

Variant naming: `Property=[count][letter]` where count=2-6 (segments) and letter=A-F (segment index within that count).

Examples: `2A` (first segment of 2-count donut), `6F` (sixth segment of 6-count donut).

### DonutChart Motion Component Set

| Variant | Segments |
|---------|----------|
| Data Count=2 | 2 children (120x120) |
| Data Count=3 | 3 children |
| Data Count=4 | 4 children |
| Data Count=5 | 5 children |
| Data Count=6 | 6 children |

### Donut SVG Construction (from Charts detail file)

Each segment is a VECTOR created by boolean Subtract operation:

```
Subtract (VECTOR) — arc shape
  Fill 1: SOLID [segment color token]
  Fill 2: GRADIENT_LINEAR white→white (5%→40% opacity)
  cornerRadius: 4 (bound)
```

The gradient overlay creates a 3D glass/sheen effect on every segment.

### Segment Color Sequence

| Segment | Token | Hex (light) |
|---------|-------|-------------|
| 1 | Black/20% + Black/100% | #000000 (dual fill) |
| 2 | Primary | #007aff |
| 3 | Secondary/Indigo | #5856d6 |
| 4 | Secondary/Red | #ff3b30 |
| 5 | Secondary/Orange | #ff9500 |
| 6 | Secondary/Yellow | #ffcc00 |

### QA Rule 5: Donut segments use dual fills for glass effect

Every donut/semicircle segment has TWO fills: solid color + white gradient overlay (5%→40%). Never render segments with a single flat color.

---

## 6. Bar Style Architecture (6 Styles)

The Charts detail file defines 6 bar styles, each with distinct structural patterns:

### Style 1 — Single Solid Bar
```
Structure: 1 RECTANGLE
Fill: Primary (#007AFF)
Corner radius: 20 (bound)
Variants: 78 (13 widths × 6 heights)
```

### Style 2 — Background + Foreground (proportional bg)
```
Structure: 2 RECTANGLEs (stacked)
Rectangle 1 (bg): Primary @ opacity 0.1, height = data level height
Rectangle 2 (fg): Primary @ full opacity, height = ~70% of bg height
Corner radius: 20 (bound)
Variants: 66 (11 widths × 6 heights)
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
  Corner radius: 20 (all corners)
Rectangle 2 (mid): Secondary/Indigo (#5856D6)
  Corner radius: 20 (bottom only)
Rectangle 3 (bot): Primary (#007AFF)
  Corner radius: 20 (bottom only)
Variants: 66
```

### Style 5 — Side-by-Side Bars
```
Structure: 2 RECTANGLEs (side by side)
Container width: 3× bar width (e.g., Width=4 → container=12px)
Rectangle 1: Primary
Rectangle 2: Secondary/Indigo, positioned adjacent
Corner radius: 20 (bound)
Variants: 66
```

### Style 6 — Pill Shape (small widths only)
```
Structure: 3 RECTANGLEs (stacked, separate pills)
Rectangle 1: Primary
Rectangle 2: Secondary/Indigo
Rectangle 3: Black/20% + Black/100% + gradient
Corner radius: 80 (pill shape, NOT 20)
Widths: Only 1, 2, 4, 8 (narrower set)
Variants: 24
```

### Bar Width Options

13 width values: 1, 2, 4, 8, 16, 24, 32, 40, 48, 56, 64, 72, 80 (px)

### Bar Height Levels

6 height values: 1-6 (data value level, maps to bar length)

### QA Rule 6: Corner radius differs between styles

- Styles 1-5: `cornerRadius: 20` (bound)
- Style 6 (pill): `cornerRadius: 80` (pill shape)
- Never use `cornerRadius: 8` for standalone bar styles (8 is for interactive segment ends only)

---

## 7. Dashboard Composition Layouts

### Universal Dashboard Properties

| Property | Value | Token |
|----------|-------|-------|
| Size | 1600x960 | — |
| Corner radius | 32 | — |
| Background | GRADIENT_LINEAR | — |
| Layout mode | NONE (absolute positioned cards) | — |

### Dashboard 02 — Tall Colored Cards
```
1600x960, 4 children:
├── Chart (400x594) — Secondary/Orange, vertical bar, pad=24, gap=24, r=24
├── Chart (400x594) — Secondary/Green, vertical bar
├── Chart (400x592) — Dark (Black/20%+100%), donut
└── Text "Variableization..." (marketing label)
```

### Dashboard 03 — Mixed Chart Types (6 cards)
```
1600x960, 7 children:
├── Chart (480x380) — White, vertical bar + green trend badge
├── Chart (400x380) — Black, vertical bar
├── Chart (480x380) — White, line chart + trend badge
├── Chart (400x380) — Secondary/Indigo, donut
├── Chart (480x380) — White, semicircle with value
├── Chart (480x380) — Black, semicircle with legend
└── Text "Various types of charts"
```

### Dashboard 04 — Auto Layout Demo
```
1600x960, 2 children:
├── Frame (1440x800) — HORIZONTAL layout, gap=40
│   ├── Frame (700x800) — VERTICAL layout, gap=40
│   │   ├── Chart (700x380) — Indigo donut
│   │   └── Chart (700x380) — Dark bar chart
│   └── Chart (700x800) — White, full-height chart (pad=24, gap=24)
│       ├── Icon & Text (652x32)
│       ├── Text (652x80)
│       └── Chart (652x592) — FILL/FILL
└── Text "Auto layout"
```

**This is the only dashboard using auto-layout.** Gap=40 between columns and rows.

### Dashboard 05 — Three Equal Cards
```
1600x960, 4 children:
├── Chart (453x480) — Black, dark card
├── Chart (453x480) — Primary blue card
├── Chart (453x480) — Black, dark card
└── Text "Diverse design"
```

### Dashboard 06 — Semicircle + Donut + Wide Line
```
1600x960, 4 children:
├── Chart (480x364) — Secondary/Indigo, semicircle (no legend)
├── Chart (480x404) — Black, semicircle with legend
├── Chart (932x406) — Dark card, wide chart area
└── Chart (932x366) — Black/100%, donut horizontal
```

### Dashboard 07 — Tall Donut Cards
```
1600x960, 3 children:
├── Chart (400x632) — White, tall donut vertical
├── Chart (400x656) — Dark, tall donut vertical
└── Chart (400x600) — Black, tall donut vertical
```

### Dashboard 08 — Compact Mixed
```
1600x960, 4 children:
├── Chart (480x368) — White, horizontal donut
├── Chart (480x366) — White, semicircle
├── Chart (480x364) — Black, semicircle
└── Chart (480x370) — Secondary/Orange, donut
```

### QA Rule 7: Dashboard cards use absolute positioning except Dashboard 04

6 of 7 dashboards use `layoutMode: NONE` with manually positioned cards. Only Dashboard 04 demonstrates the auto-layout capability (HORIZONTAL with gap=40, nested VERTICAL with gap=40).

---

## 8. Chart Card Anatomy (Universal)

Every chart card follows this exact structure:

```
Card FRAME (VERTICAL, gap=24, padding=24 all sides, r=24, clipsContent=true)
├── Icon & Text (INSTANCE) — HORIZONTAL, gap=8, FILL/HUG
│   ├── Icon (32x32, r=80 circular clip)
│   └── Label (TEXT) — 24px Semibold
├── Text (INSTANCE) — value block
│   ├── Value (TEXT) — 48px Semibold
│   ├── Subtitle (TEXT) — 14px Regular, 40% opacity
│   └── [Optional] Trend badge — pill (r=80), ArrowRise/ArrowFall icon
└── Chart area (INSTANCE or FRAME) — FILL/FILL
```

### Card Size Catalog

| Card type | Dimensions | Chart type |
|-----------|-----------|------------|
| Standard square | 480x480 | Bar (vertical/horizontal) |
| Wide | 1040x480 | Line chart |
| Tall donut (vertical legend) | 400x504 to 400x656 | Donut with legend below |
| Compact donut (horizontal legend) | 480x368 | Donut with legend beside |
| Semicircle with legend | 480x404 | Semicircle Style 1 |
| Semicircle without legend | 480x364 | Semicircle Style 2 |
| Semicircle with text | 480x428 | Semicircle with center value |
| Compact | 480x344 | Donut compact |

### Card Fill Patterns

**Colored cards (accent):**
```
Fill 1: SOLID [accent color] @ 1.0     ← bound token (e.g., Secondary/Green)
Fill 2: GRADIENT_LINEAR @ 0.4
  Stop 0: #ffffff @ 0.05 (position 0)
  Stop 1: #ffffff @ 0.40 (position 1)
```

**Dark cards:**
```
Fill 1: SOLID #000000 @ 1.0             ← bound Black/100%
Fill 2: SOLID #000000 @ 0.2             ← bound Black/20%
Fill 3: GRADIENT_LINEAR @ 1.0
  Stop 0: #ffffff @ 0.05 (position 0)
  Stop 1: #ffffff @ 0.40 (position 1)
```

**Light cards:**
```
Fill 1: SOLID #ffffff @ 1.0             ← bound White/100%
Shadow: DROP_SHADOW offset=0/0.5 blur=0.5 spread=0 rgba(0,0,0,0.1)
```

### QA Rule 8: Card padding and gap are always 24px

Every chart card uses exactly `padding: 24px` all sides and `itemSpacing: 24px` between children. No exceptions found across 117 cards.

---

## 9. Color Token Rules for Charts

### Text on Colored Backgrounds

| Card background | Text token | Subtitle token | Axis text token |
|----------------|-----------|---------------|-----------------|
| Any Secondary/* color | White/100% | White/40% | White/100% |
| Black/100% or dark | White/100% | White/40% | White/100% |
| White/100% | Black/100% | Black/40% | Black/100% |
| Primary | White/100% | White/40% | White/100% |

### Grid Lines on Different Backgrounds

| Card background | Grid token | Baseline token |
|----------------|-----------|----------------|
| White/100% | Black/4% | Black/20% |
| Colored/dark | White/20% | White/40% |

### Bar Colors on Different Backgrounds

| Card background | Active bar token | Inactive bar token |
|----------------|-----------------|-------------------|
| White/100% | Semantic color (Primary, Secondary/*) | — |
| Colored | White/100% | White/20% |
| Dark | Semantic color | — |

### QA Rule 9: Bars on colored cards use White tokens, not semantic colors

On colored card backgrounds (e.g., Secondary/Green), bars use `White/100%` for active and `White/20%` for inactive. The semantic color tokens (Primary, Secondary/Indigo, etc.) are only used on white or dark card backgrounds.

---

## 10. Adaptive Sizing Rules

### Bar Charts

| Property | Range | Constraint |
|----------|-------|-----------|
| Width | 800-2200px | Left Text stays fixed, chart body FILL |
| Height | 400-2200px | Bars stretch proportionally, paddingBottom=28 constant |
| Bar max width | 28px | Bound variable, cannot exceed |
| Data points | 1-12 | More possible by adding component variants |
| Bar widths | 11 options | Via maxWidth variable |
| Bar styles | 5 (in instances) / 6 (in Charts detail file) | Style 6 is pill-only |

### Donut Charts

| Property | Range |
|----------|-------|
| Size | 160x160 (compact) or 240x240 (standard) |
| Segments | 2-6 |
| Thicknesses | 11 (1-11 scale) |
| Construction | VECTOR Subtract shapes (boolean operations) |

### Semicircle Charts

| Property | Range |
|----------|-------|
| Size | 360x180 |
| Segments | 2-6 (Style 1) or 4 fixed (Style 2) |
| Thicknesses | 11 |
| Styles | Style 1 (multi-color) + Style 2 (single-color progress) |

### Line Charts

| Property | Range |
|----------|-------|
| Width | 800-2000px+ |
| Height | 400px base |
| Lines | 1-2 (Line 2 toggleable via Show Line 2) |
| Data points | 1-12 |

### QA Rule 10: Bar max-width is 28px — do not override

The bar `maxWidth: 28` is a bound variable constraint. When the chart container is very wide, bars stay at 28px max and redistribute spacing. This prevents bars from becoming disproportionately wide.

---

## 11. Local Variables (Charts File Only)

These variables are NOT in the SnowUI/iOS library — they exist only in the Charts {Code-Connect} file:

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

### QA Rule 11: Use SnowUI shared tokens unless chart-specific

Only `Line corner radius` and `Dot size` are local. All other tokens (colors, spacing, radius, typography) come from the shared SnowUI/iOS library.

---

## 12. Cross-Reference: Charts Detail vs UI Kit

### Already documented in UI Kit extraction (chart.md, chart-graphics.md, charts-resource.md):
- ChartMotion variant axes (Type, Show Left Text)
- Chart component layout (800x400, HORIZONTAL, gap=16)
- Y-axis and X-axis label specs
- Grid line specification (frame bottom borders, 0.5px, 10% black)
- Basic bar specifications (maxWidth=28, corner radius=8)
- 5 bar color styles
- Donut/semicircle construction (boolean Subtract)
- Chart card composition (3-row structure, multi-layer fills)
- Dashboard layouts (7 at 1600x960)
- Full token reference (fills, strokes, spacing, radius, typography)
- Adaptive sizing showcase (110 bar + 55 donut + 59 semicircle instances)

### NET NEW from Charts detail file:
1. **6 bar style architectures** (UI Kit only mentions bar colors, not structural variations)
2. **8-segment animation architecture** (UI Kit bars are single rectangles)
3. **Local variables** (Line corner radius, Dot size) — not in SnowUI token table
4. **Donut SVG dual fills** (solid + white gradient overlay for 3D glass effect)
5. **Tooltip glass effect** (Black/80% + White/10% + BACKGROUND_BLUR 40px)
6. **Proportion Statistics component** (proportional line visualization)
7. **Motion/animation keyframes** (4 motion pages with component variants)
8. **DonutGraph 4-state animation** (Default → Appear → Normal → Hover)
9. **Line chart hover behavior** (vertical Number indicator, not tooltip)
10. **ChartDot variants** (A/B/C with different fills and sizes)

### Enriches existing docs:
- chart-graphics.md: Bar segment fills now fully characterized per style
- charts-resource.md: Dashboard layouts confirmed with exact card positions
- chart-instances.md: Adaptive sizing confirmed, Style 6 (pill) discovered

---

## 13. Implementation Checklist

### Bar Chart
- [ ] Support 6 bar styles (not just color variants — structurally different)
- [ ] Implement 8-segment opacity animation for interactive bars
- [ ] Bar maxWidth=28px constraint
- [ ] Corner radius: 20 for styles 1-5, 80 for style 6 (pill)
- [ ] 13 width options: 1, 2, 4, 8, 16, 24, 32, 40, 48, 56, 64, 72, 80

### Tooltip
- [ ] Glass effect: Black/80% + White/10% dual fill + backdrop-filter blur(40px)
- [ ] Corner radius: 12px
- [ ] Padding: 4/8/4/8
- [ ] Hidden by default (opacity: 0), shown on hover (opacity: 1)
- [ ] Text: 12px Inter Regular, white

### Donut/Semicircle
- [ ] Boolean Subtract VECTOR shapes (not arc strokes)
- [ ] Dual fills per segment: solid color + white gradient overlay (5%→40%)
- [ ] cornerRadius: 4 on segments
- [ ] 4-state animation: Default → Appear → Normal → Hover
- [ ] Support 2-6 segments, 11 thicknesses

### Line Chart
- [ ] 1-2 lines (Line 2 toggleable)
- [ ] Line A/B sub-components with 3 animation states
- [ ] ChartDot variants (A=teal, B=blue, C=ring)
- [ ] Hover shows vertical Number indicator (106x185), NOT a tooltip
- [ ] Radial gradient area fills

### Dashboard
- [ ] 1600x960 canvas
- [ ] Gradient background, cornerRadius: 32
- [ ] Cards at absolute positions (except Dashboard 04 auto-layout: gap=40)
- [ ] Card padding/gap always 24px, cornerRadius: 24
- [ ] Multi-layer card fills (solid + gradient overlay)

### Color Pairing
- [ ] White text on colored/dark cards
- [ ] Black text on white cards
- [ ] White/20% for inactive bars on colored cards
- [ ] Grid: Black/4% on white, White/20% on colored/dark
