# SnowUI Atomic Composition

Assemble UI from SnowUI's atomic design system. Composition knowledge for building layouts from primitives → components → blocks → pages.

## The Atomic Hierarchy

```
PRIMITIVES (5)
├── Text      — Content wrapper. Count(1-7) × Vertical × State. width=FILL, height=HUG.
├── Icon      — Icon container. 9 sizes (16-80px). DefaultIcon slot + Badge overlay.
├── Icon&Text — Icon + Text pairing. VERTICAL layout, 8px gap, radius 12.
├── Frame     — Flexible layout container. Direction × Order × Fill variants.
├── Group     — Collection container. HORIZONTAL or VERTICAL. Wraps Button/Tag/Dot sets.
└── Line/Strip — Dividers (Line: stroke-based, 1px) and progress bars (Strip: filled rectangles).

COMPONENTS (12)
├── Card        — 16 variants. Count(1-4) × State(4). Slots accept ANY instance. Padding tiers: 16/24/40.
├── Button      — 216 variants. 6 axes. 3 slots: LeftIcon + Text + RightIcon. Sizes: 24/36/48px.
├── Input       — 16 variants. Type(4) × State(4). Heights: 44/64/44/56px.
├── Search      — 16 variants. Type(4) × State(4). 160×28px default. Popup: 600×768.
├── Tag         — 3 types. Asymmetric padding (4px icon side, 8px no-icon). Fill: Black/4%.
├── Badge       — Dot (6×6) or Number (pill). Overlays on Icon component.
├── Quick Action — 8 INSTANCE_SWAP slots. Accepts: Text, Frame, Input, Search, Group, Button.
├── Tab/Seg     — Tap(Text+Line underline) in Group. Segmented: Group(Black/4% tray) + Buttons.
├── Notification — Regular(4) + Glass(8). Success=#71dd8c, Failure=#ffcc00. Toast: 3s auto-dismiss.
├── Chart       — Type(H/V/Line) × ShowLeftText. 800×400 default. Grid=frame borders, Bars=28px pill.
├── Date Picker — Calendar grid 328px wide. 7 columns. No confirm button.
├── Tooltip     — Arrow position(4). 1.5s hover delay. Dark bg, light text.
└── Table       — Header + data rows + function bar + pagination. Hover-only actions.
```

## Page Templates

### 3-Column Dashboard (1440×1024)
```
┌─────────┬────────────────────────────┬──────────┐
│ Sidebar  │         Header (68px)      │          │
│ (212px)  ├────────────────────────────┤ Right Bar│
│          │     Content Area           │ (280px)  │
│          │     padding: 28px          │          │
│          │     gap: 28px              │          │
│          │                            │          │
└─────────┴────────────────────────────┴──────────┘
```
- Sidebar: 212px, pad 16, gap 8. Sections: Profile → Tabs → Nav groups → Logo.
- Header: 68px height, pad 20/28. Left: toggle + breadcrumb. Right: search + icons.
- Content: CSS Grid, gap 28px. KPI row (4 cards) → Chart row → Table → Bottom row.
- Right Bar: 280px, pad 16, gap 16. Notifications → Activities → Contacts.
- Dividers: 0.5px borders, Black/10%.

### 2-Column (Sidebar + Content)
Same sidebar. Content fills remaining width. No right bar. For settings, detail views.

### Single Column
Auth pages, mobile layouts. Centered content card. Max-width 480px typically.

### Mobile (375×812)
Bottom tab bar (56px). Top status bar. Content scrolls. Cards stack vertically.

## Composition Patterns

### KPI Card
```
Card(Count=2, padding=24, radius=20, fill=Background/4 or Background/5)
├── Text(Count=1) → "Total Revenue" [14px/400, Black/40%]
└── Text(Count=2, Vertical=false) → "48,291" [24px/600] + "+11.01%" [12px/400, Secondary/Green]
```

### Chart Block
```
Container(padding=24, gap=24, radius=24, fill=Background/1)
├── Frame(HORIZONTAL) → Icon(20px) + "Revenue" [14px/600]
├── Frame(HORIZONTAL, SPACE_BETWEEN)
│   ├── Text(Count=2) → "$58,211" [32px/600] + "Current Week" [12px/400, Black/40%]
│   └── Legend(HORIZONTAL, gap=16) → [Dot(6px, Black/100%) + "Current" 12px] [Dot(6px, Secondary/Cyan) + "Previous" 12px]
└── ChartArea(HORIZONTAL, gap=16)
    ├── YAxis(width=23, 12px/400, Black/40%, LEFT-aligned)
    ├── GridFrame(FILL) → N rows at 31px, border-bottom: 0.5px Black/10%
    └── XAxis(12px/400, Black/100%, CENTER-aligned)
```

### Table Block
```
Container(padding=0, radius=20, fill=Background/1, stroke=Black/10% 0.5px)
├── FunctionBar(HORIZONTAL, pad=12/16, gap=8) → Search + Filter + Sort + Add buttons
├── HeaderRow(HORIZONTAL, pad=8/16, fill=Background/2) → Column headers [12px/400, Black/40%]
├── DataRows × N(HORIZONTAL, pad=8/16, height=40) → Cell content [12px/400]
│   Cell types: text, icon+text(user), badge(status), avatar group, action buttons(hover-only)
└── Pagination(HORIZONTAL, pad=12/16) → "Showing 1-10 of 50" + page buttons
```

### Sidebar Nav Item
```
Frame(HORIZONTAL, pad=8/12, gap=8, height=36, radius=12)
├── Icon(20×20, fills=Black/100%)
└── Text(Count=1) → "eCommerce" [14px/400]
Active state: fill=Black/4%
```

### Auth Card
```
Container(480px, pad=40, gap=24, radius=24, fill=Background/1)
├── Text(Count=2, Vertical=true)
│   ├── "Sign In" [24px/600]
│   └── "Enter your credentials" [14px/400, Black/40%]
├── Form(VERTICAL, gap=16)
│   ├── Input(type=2-row-vertical) → Label + Email
│   ├── Input(type=2-row-vertical) → Label + Password
│   └── Button(size=Large, style=Filled, width=FILL) → "Sign In"
└── Text(Count=1) → "Or sign in with" [12px/400, Black/40%, CENTER]
    └── Group(HORIZONTAL, gap=8)
        └── Button × 3 (icon-only, style=Outline) → Google, Apple, Microsoft
```

## Token Quick Reference

### Colors (SnowUI-Light)
- Background/1: #ffffff (page)
- Background/2: #f9f9fa (cards, sections)
- Background/4: #edeefc (accent card purple)
- Background/5: #e6f1fd (accent card blue)
- Black/100%: #000000 (primary text)
- Black/40%: rgba(0,0,0,0.4) (secondary text)
- Black/10%: rgba(0,0,0,0.1) (borders)
- Black/4%: rgba(0,0,0,0.04) (hover, active backgrounds)
- Primary: → Black/100% (SnowUI-Light), → Secondary/Indigo (SnowUI-Dark)

### Spacing
4, 8, 12, 16, 20, 24, 28, 40, 48px. Master grid gap: 28px.

### Typography
Font: Inter. Weight: 400 (Regular), 600 (Semibold).
Scale: 12/16, 14/20, 16/24, 18/28, 20/28, 24/32, 32/40, 48/56, 64/72 (size/line-height).

### Corner Radius
4, 8, 12, 16, 20, 24, 28, 32, 40, 48px + 80px (pill).
