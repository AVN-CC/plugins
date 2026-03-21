# UI Kit (Full File) — 🔶 Chart Page Extraction

Source: Figma file "UI Kit (Full File)" (saHCjvBZyTisRjRtjuuMey)
Extracted: 2026-03-21
Pipeline: A (figma-console Desktop Bridge)
Scope: 🔶 Chart page — 108 top-level children
Cross-referenced with: Chart {Code-Connect} (LKT5XjsHrCxjVx8YENYZPu) extraction

---

## 1. ChartMotion Component — Master Architecture

**Component Set:** `ChartMotion` (parent of 3 variant components)
**Variant Axis:** `Type` = Vertical | Horizontal | Line
**Boolean Property:** `Show Left Text` (default: true)
**Layout:** HORIZONTAL (all variants)

### Instance Swap Properties

| Slot | Property Key | Default Target | Usage |
|------|-------------|---------------|-------|
| Vertical Bar | `Vertical Bar#13381:3` | Vertical Bar Number=12 (480×40) | Bar collection for vertical charts |
| Horizontal Bar | `Horizontal Bar#51227:17` | Horizontal Bar Number=12 (40×480) | Bar collection for horizontal charts |
| Line | `Line#51227:13` | Line A Property=Default (560×240) | Line graphic for line charts |

### Variant Dimensions (from Chart instances frame)

| Config | Type | Size | Notes |
|--------|------|------|-------|
| Full desktop | Vertical | 800×400 | 12-month data, full axis labels |
| Full desktop | Horizontal | 800×400 | 12-month data, horizontal bars |
| Half width | Vertical | 400×200 | 6-month data, compact |
| Half width | Horizontal | 400×320 | 6-month data, taller for labels |
| Half width | Line | 400×200 | 6-month data, line charts |

---

## 2. Vertical Bar Styles (8 Styles)

All styles share container layout: HORIZONTAL, SPACE_BETWEEN, counterAxis=MAX (bottom-aligned), 6 bars per row at 240×160 base.

### Vertical 01 — Single Rounded Bar
- **Sub-component:** `VerticalBar 01 A` (Height=2, State=Default)
- **Bar width:** 28px, **height:** variable by Height variant
- **Corner radius:** 8 (bound to `Corner Radius/8` token — Standard: 8, Expanded+4: 12, Expanded+8: 16, Condensed-4: 4)
- **Fill:** `Secondary/Cyan` rgba(160, 188, 232) — bound to Colors collection
- **Tooltip:** 33×16, cornerRadius=12, opacity=0 (hidden by default)

### Vertical 02 — Stacked Double Bar
- **Sub-component:** `VerticalBar 02 A` (Height=2, State=Default)
- **Structure:** 2 rectangles stacked
  - Top: 28×80, cr=8, fill=Secondary/Cyan
  - Bottom: 28×60, cr=0, fill=Secondary/Cyan
- **Pattern:** Two-tone stacked bar

### Vertical 03 — Segmented (16 segments)
- **Sub-component:** `VerticalBar 03` (Height=2, State=Default)
- **Structure:** 16 small rectangles, each 28×8, cr=2
- **Fill:** Each segment individually colored (Secondary/Cyan base)
- **Pattern:** Granular segmented/stacked bar — progress or multi-category

### Vertical 04 — Stacked Multi-Color (8 segments)
- **Sub-component:** `VerticalBar 04` (Height=2)
- **Structure:** 8 rectangles, each 28×20, cr=0
- **Fills vary per segment:** Black, Purple (184,153,235), Indigo (173,173,251), White
- **Pattern:** Stacked bar with distinct category colors

### Vertical 05 — Thin Segmented (8 segments)
- **Sub-component:** `VerticalBar 05` (Height=2)
- **Structure:** 8 rectangles, each 28×18, cr=2
- **Fills:** Mostly black, bottom segment Indigo (173,173,251)
- **Pattern:** Minimalist stacked with accent bottom

### Vertical 06 — Variable-Height Segments (5 segments)
- **Sub-component:** `VerticalBar 06` (Height=2)
- **Structure:** 5 rectangles with varying heights (80, 8, 8, 20, 36), all 28px wide, cr=2
- **Pattern:** Waterfall/breakdown bar

### Vertical 07 — Thin Paired Lines (G-style)
- **Sub-component:** `VerticalBar 07` (Height=2)
- **Structure:** 2 thin rectangles, each 4px wide
  - Heights: 75px and 80px
- **Fill:** Secondary/Cyan rgba(160, 188, 232)
- **Pattern:** Thin proportional dual-line (matches `Bar/Vertical/G` naming)

### Vertical 08 — Side-by-Side Thick (H-style)
- **Sub-component:** `VerticalBar 08` (Height=2)
- **Structure:** 2 rectangles, each 8px wide, 30px tall
- **Fills:** Black + Indigo (173,173,251)
- **Pattern:** Grouped comparison bars (matches `Bar/Vertical/H` naming)

---

## 3. Horizontal Bar Styles (4 Styles)

All styles: VERTICAL layout, SPACE_BETWEEN, 6 bars per column at 160×240 base.

### Horizontal 01 — Standard Rounded
- **Sub-component:** `HorizontalBar 01` (Height=2, State=Default)
- **Structure:** 8 rectangles, 20×28 each
- **Fill:** Secondary/Cyan rgba(160, 188, 232)
- **Pattern:** Standard horizontal grouped bars

### Horizontal 02 — Dense Multi-Bar
- **Sub-component:** `HorizontalBar 02` (Height=2, State=Default)
- **Structure:** 16 rectangles, 20×28 each
- **Fill:** Secondary/Cyan rgba(160, 188, 232)
- **Pattern:** Dense horizontal grouped — more data series

### Horizontal 03 — Dashed/Dotted Thin
- **Sub-component:** `HorizontalBar 03` (Height=2, State=Default)
- **Structure:** 7 rectangles, 21×8 each, cr=80 (fully rounded pill)
- **Fill:** Black (0,0,0)
- **Pattern:** Thin pill-shaped horizontal segments

### Horizontal 04 — Ultra-Thin Dots
- **Sub-component:** `HorizontalBar 04` (Height=2, State=Default)
- **Structure:** 10 rectangles, 14×2 each, cr=80 (fully rounded)
- **Fill:** Black (0,0,0)
- **Pattern:** Dot-matrix style horizontal, minimal

---

## 4. Line Chart Components

### Line A (Primary Line)
- **Component:** `Line A` → variant `Property=Default` (560×240)
- **Structure:**
  - Background rectangle: 560×240, fill=black (variable-bound)
  - Background frame: 560×216, contains 2 vector paths (area fill, one at 60% opacity)
  - Line vector: 560×157, stroke=GRADIENT_LINEAR + SOLID, strokeWeight=1, strokeCap=ROUND
- **Pattern:** Solid curved line with gradient fill area beneath

### Line B (Secondary/Dashed Line)
- **Component:** `Line B` → variant `Property=Default` (560×240)
- **Structure:**
  - Background rectangle: 560×240, fill=black
  - Line 2 vector: 560×157, stroke=Secondary/Cyan rgba(160,188,232), strokeWeight=1, strokeCap=ROUND
  - **dashPattern:** [2, 4] — dashed line
- **Pattern:** Dashed comparison line, no area fill

### Line 01 (Composite: A+B Overlay)
- **Component:** `Line 01` → variant `State=Default` (560×240)
- **Structure:**
  - Line B instance (bottom layer): dashed line
  - Line A instance (top layer): solid line with area fill
- **Pattern:** Two-line comparison chart with solid primary + dashed secondary

---

## 5. DonutChart Component

**Component Set:** `DonutChart`
**Variant Axis:** `Data Count` = 2 | 3 | 4 | 5 | 6
**Size:** 120×120 (fixed)

### Internal Structure (Data Count=2)
- 2× `DonutGraph` instances:
  - Primary: 111×120 — full-size ring, gradient fill + black segment
  - Secondary: 60×89 — partial ring, gradient + solid cyan (125,187,255)
- Each DonutGraph has a hidden Tooltip (opacity=0, cr=12)

### Arc Construction
- Built with `Subtract` boolean operations on vectors
- Gradient fills create smooth color transitions per segment
- Corner radius on subtract shapes: 3px (subtle softening)

---

## 6. SemicircleChart Component

**Component:** `SemicircleChart` (standalone, no variants)
**Size:** 250×126

### Internal Structure
- Frame container: 250×126
- 3 ellipse stroke arcs:
  - Outer: 36×55, fill=black, cr=4 — smallest arc
  - Middle: 141×79, fill=black, cr=4 — medium arc
  - Inner: 98×119, fill=Indigo (173,173,251), cr=4 — colored arc
- Text instance: 65×48, contains percentage label (e.g., "58%")

---

## 7. Sub-Component Inventory

### Axis Components (Number=1-12 variants for data point scaling)

| Component | Layout | Orientation | Size per Item |
|-----------|--------|-------------|---------------|
| Vertical Bar | HORIZONTAL, gap=8, SPACE_BETWEEN, align=MAX | Columns of bars | 480×40 (Number=12) |
| Horizontal Bar | VERTICAL, gap=12 | Rows of bars | 40×480 (Number=12) |
| Bottom Text | HORIZONTAL | Month labels | 300×16 |
| Left Text | VERTICAL, SPACE_BETWEEN | Y-axis values | 25×(18×N) |
| Vertical Line | HORIZONTAL, SPACE_BETWEEN | Grid lines (horizontal chart) | 56×80 (Number=13) |
| Horizontal Line | VERTICAL | Grid lines (vertical chart) | 40×56 (Number=13) |

### Data Point Scaling
- All axis sub-components support Number=1 through Number=12 (or 13 for grid lines)
- Grid line components include 1 extra line vs data bars (N+1 for fencepost)
- Bottom Text & Left Text child counts directly match the Number variant value

### ChartDot
- **Variants:** Property=A, Property=B
- **Size:** 32×32
- **Usage:** Data point indicators on line charts, scatter plots

### Bar Style Reference (legacy naming)
- `Bar/Vertical/G/6/Default` → 28×160, 2 thin rects (4px wide) — matches Vertical 07
- `Bar/Vertical/H/6/Default` → 28×160, 2 thick rects (8px wide) — matches Vertical 08

---

## 8. Bound Variable Tokens

### Corner Radius Collection

| Token Name | Standard | Expanded (+4) | Expanded (+8) | Condensed (-4) |
|------------|----------|---------------|---------------|----------------|
| `8` | 8 | 12 | 16 | 4 |
| `12` | 12 | 16 | 20 | 8 |

Usage: Bar rectangles use `Corner Radius/8`, tooltips use `Corner Radius/12`.

### Colors Collection

| Token | SnowUI-Light | SnowUI-Dark | iOS-Light | iOS-Dark |
|-------|-------------|-------------|-----------|---------|
| `Secondary/Cyan` | rgba(160,188,232) | rgba(160,188,232) | rgba(50,173,230) | rgba(100,210,255) |

### Font Collection

| Token | Inter | SF Pro | Bai Jamjuree | Space Grotesk |
|-------|-------|--------|--------------|---------------|
| `Font` | Inter | SF Pro | Bai Jamjuree | Space Grotesk |
| `Letter spacing` | 0 | 0 | 0 | 0 |

### Font Weight Collection

| Token | Regular / Semi Bold | Semi Bold / Regular |
|-------|--------------------|--------------------|
| `Regular` | 400 | 600 |

### Spacing Collection

| Token | Standard | Expanded (+4) | Expanded (+8) | Condensed (-4) |
|-------|----------|---------------|---------------|----------------|
| `8` | 8 | 12 | 16 | 4 |

---

## 9. Chart Documentation (267 text nodes extracted)

### Key Design Principles
1. **"Chart component is one of the most complex components. SnowUI's chart component is powerful and can be adapted to both desktop and mobile devices."**
2. **Independence:** "The chart component can be used completely independent of SnowUI. The chart graphics part can be replaced by the Chart graphics component in the design resources."
3. **Scalable data:** Default 1-12 data points, extendable via variants
4. **Bar max-width:** 28px ("the width of the Bar is limited to a maximum of 28, which can be removed by changing the Bar component")
5. **Theme switching:** "Use variables to change chart graph color" — Pastel Light and Bright Light via Themes variable

### Interactive Guidance
- **Change bar style:** Select the Bar instance layer to swap individual bar styles
- **Change chart type:** Select Vertical Bar instance layer and replace with another chart graphic instance (e.g., Line, Horizontal Bar)
- **Hide left text:** Toggle `Show Left Text` boolean
- **Responsive:** Desktop and mobile versions supported; bar width constraint (28px max) can be removed

---

## 10. Chart Instances Matrix (24 instances)

### ChartMotion Instances (18 total)

| # | Type | Size | Vertical Bar Swap | Horizontal Bar Swap | Line Swap |
|---|------|------|-------------------|--------------------|-----------|
| 1 | Vertical | 800×400 | Vertical Bar N=12 | — | — |
| 2 | Horizontal | 800×400 | — | Horizontal Bar N=12 | — |
| 3 | Vertical | 400×200 | Vertical 01 | — | — |
| 4 | Line | 400×200 | Vertical 01 | — | Line A |
| 5 | Line | 400×200 | Vertical 01 | — | Line B (dashed) |
| 6 | Line | 400×200 | Vertical 01 | — | Line 01 (composite) |
| 7 | Horizontal | 400×320 | — | Horizontal 01 | — |
| 8 | Horizontal | 400×320 | — | Horizontal 02 | — |
| 9 | Horizontal | 400×320 | — | Horizontal 04 | — |
| 10 | Horizontal | 400×320 | — | Horizontal 03 | — |
| 11 | Vertical | 400×200 | Vertical 02 | — | — |
| 12 | Vertical | 400×200 | Vertical 03 | — | — |
| 13 | Vertical | 400×200 | Vertical 04 | — | — |
| 14 | Vertical | 400×200 | Vertical 05 | — | — |
| 15 | Vertical | 400×200 | Vertical 06 | — | — |
| 16 | Vertical | 400×200 | Vertical 07 | — | — |
| 17 | Vertical | 400×200 | Vertical 08 | — | — |
| 18 | Vertical | 400×200 | Vertical 08 | — | — |

### DonutChart Instances (5 total)
| Data Count | Size |
|-----------|------|
| 2 | 120×120 |
| 3 | 120×120 |
| 4 | 120×120 |
| 5 | 120×120 |
| 6 | 120×120 |

### SemicircleChart Instance (1)
- Size: 250×126
- Percentage label: "58%"

---

## 11. Layout Architecture — ChartMotion Internal

### Type=Vertical
```
ChartMotion (HORIZONTAL)
├── Left Text (VERTICAL, SPACE_BETWEEN)
│   └── Number=12: "110K", "100K", "90K"... "0"
└── Frame (fills remaining width)
    ├── Horizontal Line (VERTICAL, SPACE_BETWEEN, padding: 16/0/28/0)
    │   └── Number=12: grid lines
    ├── Vertical Bar (HORIZONTAL, gap=8, SPACE_BETWEEN, align=MAX)
    │   └── Number=12: bar instances (bottom-aligned)
    └── Bottom Text (HORIZONTAL)
        └── Number=12: "Jan", "Feb"... "Dec"
```

### Type=Horizontal
```
ChartMotion (HORIZONTAL)
├── Left Text (VERTICAL, gap=12, SPACE_BETWEEN, padding: 10/0/24/0)
│   └── Number=12: "Dec", "Nov"... "Jan"
└── Frame (fills remaining width)
    ├── Vertical Line (HORIZONTAL, gap=8, SPACE_BETWEEN, padding: 8/0/0/0)
    │   └── Number=13: grid lines (N+1 fencepost)
    ├── Horizontal Bar (VERTICAL, gap=12, padding: 16/0/28/0)
    │   └── Number=12: bar instances
    └── Bottom Text (HORIZONTAL)
        └── Number=12: "0", "10K"... "110K"
```

### Type=Line
```
ChartMotion (HORIZONTAL)
├── Left Text (VERTICAL, SPACE_BETWEEN)
│   └── Y-axis values
└── Frame
    ├── Horizontal Line (grid)
    ├── Line [instance swap slot] (560×240)
    │   └── Line A / Line B / Line 01
    └── Bottom Text
        └── X-axis labels
```

---

## 12. Cross-Reference: UI Kit vs Code-Connect File

| Feature | UI Kit (this extraction) | Code-Connect (previous) |
|---------|------------------------|------------------------|
| Vertical bar styles | 8 (Vertical 01-08) | 6 (Style A-F) |
| Horizontal bar styles | 4 (Horizontal 01-04) | 4 (Style A-D) |
| Line variants | 3 (Line A, Line B, Line 01) | 2 (Line, Line Dashed) |
| DonutChart variants | 5 (Data Count 2-6) | 5 (Data Count 2-6) |
| SemicircleChart | 1 (no variants) | 1 (no variants) |
| Bar max-width | 28px | 28px |
| Corner radius token | Corner Radius/8 collection | Line corner radius (local) |
| Color binding | Secondary/Cyan via Colors | Primary (#007AFF) |

### Net-New from UI Kit
- **Vertical 07 & 08** — thin-line and side-by-side grouped bar styles (G/H naming) not in Code-Connect
- **Line 01 composite** — overlaid Line A + Line B (solid + dashed) as a single component
- **Horizontal 03 & 04** — pill/dot-matrix horizontal styles
- **Adaptive spacing modes** — Corner Radius, Spacing tokens with Standard/Expanded/Condensed modes
- **Font variable system** — 4 font families switchable via collection modes
- **Interactive documentation** — 267 text nodes of design guidance
