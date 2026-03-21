# Chart Rules — Form & Assembly

Rules for building SnowUI charts. Two systems exist and must not be mixed.

---

## System Selection

"Motion" = animation. ChartMotion IS the animated/interactive system.

| Need | System | Why |
|------|--------|-----|
| Dashboard with interactive chart | ChartMotion (Animated) | Full animation + tooltips + hover |
| Data exploration view | ChartMotion (Animated) | User needs to inspect values via tooltips |
| Animated data reveal on load | ChartMotion (Animated) | 8-segment bars fill with stagger |
| Static card / sparkline | Static Bar Styles | No interaction needed, simpler structure |
| Print/export view | Static Bar Styles | No JS interaction, single rectangles |
| Compact KPI indicator | Static Bar Styles | Small, no hover affordance |

**Rule: ChartMotion bars always use 8-segment architecture with animation + tooltips. Static bar styles (Vertical 01-08) use single rectangles with NO tooltips and NO animation.**

---

## FORM RULES — ChartMotion (Animated/Interactive)

### Animation
- **Bar fill animation**: On load, bars fill bottom-to-top with staggered delay (e.g., 50ms per bar column)
- **Segment reveal**: Each bar's 8 segments transition from opacity 0→1 sequentially
- **CSS implementation**: Use `@keyframes` or CSS transitions with `animation-delay` per column
- **Tooltip animation**: opacity 0→1 on hover, transition 0.15s ease
- **Line chart**: Dots enlarge on hover (r=4→r=8 using CHART/Dot token Small→Dot 2 Small)

### Structure (mandatory)
```
ChartMotion (HORIZONTAL, gap=16)
├── Left Text (VERTICAL, SPACE_BETWEEN)     ← Y-axis values
│   └── Labels: "110K"..."0" (top to bottom, 12px/400, Black/40%)
└── Frame (FILL remaining width)
    ├── Grid Lines (background layer)
    │   └── N+1 lines: border-bottom 0.5px Black/4%, baseline Black/20%
    ├── Data Area (bars or line)
    │   └── Bar collection OR Line graphic
    └── Bottom Text (HORIZONTAL)             ← X-axis labels
        └── Labels: "Jan"..."Dec" (12px/400, Black/40%)
```

### Bar Form Rules
- **Max width:** 28px per bar rectangle — NEVER exceed this
- **Corner radius:** Use `CHART/Line Corner Radius` token (Standard: 8px for bars)
- **Bottom alignment:** Bars align to MAX (bottom of container) — data grows upward
- **Spacing:** gap=8 between bar groups (SPACE_BETWEEN for even distribution)
- **Color binding:** Fill uses `Secondary/*` palette via Colors collection variable
- **Height representation:** Bar height is proportional to data value; container clips overflow

### Bar Style Selection
| Style | When to Use | Visual |
|-------|------------|--------|
| Vertical 01 (Single Rounded) | Default — one data series | Single 28px bar, cr=8 |
| Vertical 02 (Stacked Double) | Two series comparison | Two stacked bars |
| Vertical 03 (Segmented 16) | Progress/completion data | 16 small segments |
| Vertical 04 (Multi-Color) | Category breakdown | 8 colored segments |
| Vertical 05 (Thin Segmented) | Minimalist stacked | 8 thin segments, accent bottom |
| Vertical 06 (Waterfall) | Breakdown/variance | Variable-height segments |
| Vertical 07 (Thin Lines) | Dual series, minimal | 2 thin 4px lines (G-style) |
| Vertical 08 (Grouped) | Side-by-side comparison | 2 thick 8px bars (H-style) |

### Line Form Rules
- **Stroke weight:** 1px (bound to token)
- **Stroke cap:** ROUND — always
- **Solid line (A):** Gradient stroke + area fill beneath (60% opacity background vector)
- **Dashed line (B):** dashPattern [2, 4], solid stroke color Secondary/Cyan
- **Composite (Line 01):** Layer Line B below Line A — dashed is secondary/comparison

### Grid Line Rules
- **Horizontal grid (for vertical bars):** Number of lines = data point count + 1 (fencepost)
- **Vertical grid (for horizontal bars):** Same fencepost rule
- **Stroke:** 0.5px, Black/4% (thin lines), Black/20% (baseline only)
- **Layout:** SPACE_BETWEEN within grid container, with padding to align with bars

### Axis Label Rules
- **All axis text:** 12px Regular (400 weight), Black/40%, Inter
- **Y-axis (Left Text):** Right-aligned, width=25px, VERTICAL SPACE_BETWEEN
- **X-axis (Bottom Text):** Center-aligned, HORIZONTAL, matches bar count
- **Y-axis always starts at 0** at the bottom
- **Y-axis scale:** Round to clean numbers (10K, 20K, 30K... or 100, 200, 300...)

### Sizing Rules
| Context | Size | Data Points |
|---------|------|-------------|
| Full dashboard card | 800×400 | 12 (Jan-Dec) |
| Half-width card | 400×200 | 6 (Jan-Jun) |
| Horizontal compact | 400×320 | 6 (taller for labels) |
| Mobile card | 360×200 | 6 max |

---

## FORM RULES — Chart Components (Interactive)

### Bar Segment Architecture
```
VerticalBar Instance (28×160, clipsContent=true)
├── Rectangle 1 (segment) — 28×20, opacity 0|1, cr top-only
├── Rectangle 2 — 28×20, opacity 0|1
├── Rectangle 3 — 28×20, opacity 0|1
├── Rectangle 4 — 28×20, opacity 0|1
├── Rectangle 5 — 28×20, opacity 0|1
├── Rectangle 6 — 28×20, opacity 0|1
├── Rectangle 7 — 28×20, opacity 0|1
├── Rectangle 8 — 28×20, opacity 0|1, cr bottom-only
└── Tooltip (opacity=0, positioned above bar)
```

### Segment Animation Rules
- **Fill direction:** Bottom-to-top (vertical), left-to-right (horizontal)
- **Height property (1-6):** Controls how many of 8 segments are visible (opacity=1)
- **Height=1:** Only bottom segment visible. **Height=6:** All segments visible
- **Transition:** Segments reveal sequentially — never skip or show out of order
- **Color per segment:** Each rectangle gets its own fill from Secondary/* palette

### Tooltip Form Rules
- **Size:** HUG content, min 33×16
- **Corner radius:** 12px (vertical bars), 8px (horizontal bars)
- **Padding:** 4px vertical, 8px horizontal
- **Background:** Dual-layer glass — fill 1: White/10%, fill 2: Black/80%
- **Effect:** BACKGROUND_BLUR radius=40
- **Text:** 12px Regular, white fill (#ffffff hardcoded, NOT White/100%)
- **Default state:** opacity=0 (hidden)
- **Hover state:** opacity=1 (visible), positioned centered above the bar
- **Content:** Data value string (e.g., "10,256")

### Interactive Line Chart Rules
- **Data points:** ChartDot (Property=A or B), 32×32, positioned on line path
- **Dot size:** Use `CHART/Dot Size` token — Dot for default, Dot 2 for hover-enlarged
- **Line path:** SVG polyline or path, stroke-width 1-2px, ROUND line cap
- **Area fill:** Semi-transparent fill below line (opacity 0.1-0.3)
- **Tooltip on dot hover:** Same glass tooltip spec as bars

---

## FORM RULES — DonutChart

### Arc Construction
- **Container:** 120×120 fixed
- **Arcs:** Built from vector Subtract boolean operations, NOT SVG `<circle>` with stroke-dasharray
- **Gradient fills:** Each segment uses GRADIENT_LINEAR for smooth color transition
- **Gap between segments:** 3px corner radius on subtract shapes creates visual separation
- **Data Count variants:** 2-6 segments; each adds one DonutGraph child instance
- **Tooltip:** Hidden (opacity=0), same glass spec, positioned near segment

### Donut Assembly Rules
- **Center is EMPTY** — place summary Text inside (percentage, total count, label)
- **Inner ring ratio:** ~60% of outer diameter (thick ring, not thin)
- **Color order:** Follow Secondary/* palette order: Indigo → Cyan → Mint → Blue → Purple → Green
- **Max segments:** 6 — if more than 6 categories, group into "Other"

---

## FORM RULES — SemicircleChart

- **Container:** 250×126
- **3 concentric arcs:** Outer (smallest value), middle, inner (largest/accent)
- **Arc style:** Vector strokes with 4px corner radius
- **Colors:** Outer two use Black/100%, inner uses Secondary/Indigo (#adadfb)
- **Percentage label:** Centered in the arc opening, 2-line Text (label 16px + value 32px)

---

## ASSEMBLY RULES — Chart Block (Card Composition)

### Standard Chart Card
```
Container(padding=24, gap=24, radius=24, fill=Background/1)
├── Header(HORIZONTAL, SPACE_BETWEEN)
│   ├── Frame(HORIZONTAL, gap=8)
│   │   ├── Icon(20px, Black/100%)
│   │   └── Title [14px/600, Black/100%] → "Revenue"
│   └── Menu dots or period selector
├── Stats(HORIZONTAL, SPACE_BETWEEN)
│   ├── Text(Count=2) → "$58,211" [32px/600] + "Current Week" [12px/400, Black/40%]
│   └── Legend(HORIZONTAL, gap=16)
│       ├── LegendItem → Dot(6px, Black/100%) + "Current" [12px/400]
│       └── LegendItem → Dot(6px, Secondary/Cyan) + "Previous" [12px/400]
└── ChartArea
    └── ChartMotion(Type=Vertical) OR Chart Component assembly
```

### Legend Rules
- **Dot size:** 6px circle (use `CHART/Dot` Small=4, or hardcode 6px)
- **Layout:** HORIZONTAL, gap=16 between items, gap=8 between dot and label
- **Text:** 12px/400, Black/100% for series name
- **Dot color:** Matches bar/line series color from Secondary/* palette
- **Position:** Top-right of chart header area, NEVER overlapping chart area

### Card Sizing by Chart Type
| Chart Type | Card Size | Notes |
|-----------|-----------|-------|
| Vertical bar | 480×480 (standard), 920×480 (wide) | Most common |
| Horizontal bar | 480×560 (taller for y-labels) | Needs more vertical space |
| Line | 480×400, 920×400 | Can be narrower |
| Donut | 280×280 (small), 480×480 (with legend) | Square preferred |
| Semicircle | 280×200, 480×300 | Half-height |
| Statistics/KPI | 480×240 | Compact number display |

### Dashboard Grid Assembly
```
ContentArea(VERTICAL, gap=28)
├── KPI Row(HORIZONTAL, gap=28)
│   └── 3-4 KPI Cards (FILL, equal width)
├── Chart Row(HORIZONTAL, gap=28)
│   ├── Primary Chart Card (2/3 width or FILL)
│   └── Secondary Chart Card (1/3 width or FILL)
├── Table Block (FILL)
└── Bottom Row(HORIZONTAL, gap=28)
    ├── Donut Card
    ├── Activity List
    └── Statistics Card
```

---

## COMMON MISTAKES & FIXES

### Mistake: Mixing static and interactive chart bars
**Wrong:** Using 8-segment animated bars inside a ChartMotion layout
**Fix:** ChartMotion uses single-rectangle bars (Vertical 01-08 styles). Interactive bars belong in Chart Components only.

### Mistake: Hardcoded gray for chart text
**Wrong:** `color: #666` or `color: gray`
**Fix:** Always use `Black/40%` → `rgba(0,0,0,0.4)` for all chart axis text and labels.

### Mistake: Bars exceeding max width
**Wrong:** Bars wider than 28px, especially on narrow charts
**Fix:** Constrain bar width to 28px max. On narrow charts, reduce bar count or use horizontal layout.

### Mistake: Line chart clipping at edges
**Wrong:** Polyline starting at x=0 gets clipped by container
**Fix:** Add 8-16px padding to chart area (SVG viewBox or container padding) so leftmost/rightmost data points aren't cut off.

### Mistake: Wrong grid line count
**Wrong:** Same number of grid lines as data bars
**Fix:** Grid lines follow fencepost rule: N+1 lines for N data points (one above each + one below last).

### Mistake: Tooltips visible by default
**Wrong:** Showing tooltip text on all bars in static view
**Fix:** Tooltips are opacity=0 by default. Only show on hover (:hover opacity=1). In HTML: use CSS `opacity: 0; transition: opacity 0.15s;` on `.tooltip`, and `.bar:hover .tooltip { opacity: 1; }`.

### Mistake: Wrong donut construction
**Wrong:** Using `stroke-dasharray` on a `<circle>` element
**Fix:** Donut segments are Subtract boolean operations with gradient fills, not stroke tricks. In code: use SVG `<path>` with arc commands, or `conic-gradient` in CSS.

### Mistake: Chart area not filling container
**Wrong:** Fixed-width chart inside a responsive card
**Fix:** Chart area uses FILL (flex: 1) to take remaining width after y-axis labels. Only the y-axis label column has fixed width (25px).
