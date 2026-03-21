# Components Index — SnowUI Design System

> Compact lookup table for LLM UI assembly. Components are built from primitives and carry interactive state.

---

## Card

- **Variant axes**: Count(1-4) × State(Default/Hover/Selected/Static)
- **Layout**: VERTICAL auto-layout; 5 padding tiers: Base(12h/16v), Compact(16/16), Standard(16h/24v), Content(24/24), Spacious(40/40)
- **Sizing**: width FILL, height HUG; radius 16px default
- **Slots**: Each Count adds one Text slot (INSTANCE_SWAP) — swap to Frame, Tag, Strip, Button, Group
- **State signals**: Hover → stroke opacity increase; Selected → checkmark Icon ABSOLUTELY POSITIONED top-right; Static → no interaction
- **Key tokens**: fill White/100%, stroke Black/8%, selected indicator Black/100%
- **Design rules**: _"The Card component makes it easier to design cards."_ Count=1 for stat cards. Count=2 for title+content. Count=3 for title+content+actions. Count=4 for full detail cards. Selection indicator is absolutely positioned — never place it in auto-layout flow.
- **Source**: [docs/patterns/card.md](../../docs/patterns/card.md)

---

## Button

- **Variant axes**: Size(S/M/L) × Style(Borderless/Gray/Outline/Filled) × State(Default/Hover/Disabled) × Left Icon(T/F) × Text(T/F) × Right Icon(T/F)
- **Layout**: HORIZONTAL auto-layout; 4px increment scaling: S=h28/pad8/gap4, M=h36/pad12/gap6, L=h44/pad16/gap8
- **Sizing**: height fixed per size, width HUG; radius 10(S)/12(M)/14(L)
- **Slots**: 3-slot composition — Icon (left), Text (center), Icon 2 (right); each toggleable via boolean
- **State signals**: Hover → fill/opacity shift; Disabled → child opacity 0.3-0.4
- **Key tokens**: Filled=Black/100% fill + White/100% text; Gray=Black/4% fill; Outline=Black/8% stroke; Borderless=transparent; entire system uses only #000/#fff with opacity
- **Design rules**: _"The definition of a button component refers more to its interactive behavior than to the shape and elements of the component."_ Style priority: Filled for primary CTA, Outline for secondary, Gray for tertiary, Borderless for toolbar/inline. Icon-only buttons: set Text=false. Never mix sizes in the same action group.
- **Source**: [docs/patterns/button.md](../../docs/patterns/button.md)

---

## Input / Form

- **Variant axes**: Type(1-row/2-row-v/2-row-h/Textarea) × State(Default/Hover/Focus/Static)
- **Layout**: VERTICAL auto-layout; height 44px (1-row), variable (textarea); padding 12h/10v; radius 12px
- **Sizing**: width FILL, height HUG
- **Slots**: Label (Text), Placeholder/Value (Text), optional trailing Icon, optional error message (Text below, pushes content down)
- **State signals**: Focus → box-shadow 0 0 0 4px rgba(0,0,0,0.04) ring; Hover → stroke Black/12%; 7 interactive states total; floating label on focus
- **Key tokens**: fill White/100%, stroke Black/8% default, label Black/40%, value Black/100%
- **Design rules**: _"The Input component is used to create various inputs."_ 1-row for simple fields. 2-row-v for label-above. 2-row-h for inline label-left. Textarea for multi-line. Error messages are outside the input frame, never inside. Focus ring is 4px spread, not border.
- **Source**: [docs/patterns/input-form.md](../../docs/patterns/input-form.md)

---

## Search

- **Variant axes**: Type(Gray/Glass/Outline/Typing) × State(Default/Hover/Focus/Static)
- **Layout**: HORIZONTAL auto-layout; compact 28px height; padding 4h/8v/4h/8v; radius 10px
- **Sizing**: width FILL, height 28px fixed
- **Slots**: Leading Icon (search), Text (placeholder/query), trailing "/" shortcut badge
- **State signals**: Focus → all types converge to same outline appearance; Typing → clear button appears
- **Key tokens**: Gray fill=Black/4%, Glass=BACKGROUND_BLUR, Outline=Black/8% stroke
- **Popup**: 600px wide, 32px radius, glassmorphism (fill White/90%, blur 40, shadow 0 8px 28px rgba(0,0,0,0.12)); results as Quick Action rows
- **Design rules**: _"Search is a relatively common interactive component."_ All 4 types look identical on focus — type only affects resting state. "/" badge indicates keyboard shortcut. Popup is a separate layer — use Quick Action pattern for result rows.
- **Source**: [docs/patterns/search.md](../../docs/patterns/search.md)

---

## Tag / Badge

- **Tag variant axes**: Type(Default/Left arrow/Right arrow) × State(Default/Hover) × Left Icon(T/F) × Right Icon(T/F)
- **Badge variants**: Dot / Number
- **Layout**: Tag HORIZONTAL, fixed 20px height, padding 6h/0v, gap 2px, radius 6px; Badge is overlay on Icon
- **Sizing**: Tag width HUG; Badge: Dot=6×6px, Number=16px+ min
- **Slots**: Tag: optional left Icon + Text label + optional right Icon (INSTANCE_SWAP); Badge: label text or dot
- **State signals**: Tag Hover → fill opacity 0.04→0.1; Arrow types use -8px negative spacing for overlap effect
- **Key tokens**: Tag fill rgba(0,0,0,0.04), text Black/100%; Badge default #adadfb (Secondary/Indigo)
- **Design rules**: 18 Tag variants total. Arrow types create a "breadcrumb" overlap — use negative margin -8px. Tags are always compact (20px). Badges attach to Icon components only. Badge colors: 5 semantic options (Purple/Green/Blue/Yellow/Grey for status).
- **Source**: [docs/patterns/tag-badge.md](../../docs/patterns/tag-badge.md)

---

## Quick Action

- **Variant axes**: Count(1-8)
- **Layout**: VERTICAL auto-layout; padding 8px; gap 0px; radius 16px
- **Sizing**: width 240px default (expandable), height HUG
- **Slots**: 1-8 rows, each INSTANCE_SWAP to 7 sub-component types: Text, Frame, Frame/Text, Group, Search, Input, Button
- **State signals**: Row hover → fill Black/4%
- **Key tokens**: Glassmorphism container: fill White/90%, BACKGROUND_BLUR 40px, DROP_SHADOW 0 8px 28px rgba(0,0,0,0.12)
- **Design rules**: _"Quick Action supports 1-8 rows of menu items."_ Use for context menus, dropdown menus, command palettes, action sheets. Each row is independently swappable. Search row goes at top. Separator = Line primitive between rows. Glass effect is mandatory for floating panels.
- **Source**: [docs/patterns/quick-action.md](../../docs/patterns/quick-action.md)

---

## Tab / Segmented

- **Tap variant axes**: State(Active/Default/Hover) per tab item
- **Segmented**: Composed via Group + Button instances (no dedicated component)
- **Layout**: Tap: HORIZONTAL, gap 0, each item pad 8h; Segmented: Group with Black/4% tray fill, radius 12px
- **Sizing**: Tap items HUG width, FILL height; Segmented buttons FILL equally
- **Slots**: Tap: Text label + Line underline (active state); Segmented: Button instances inside Group
- **State signals**: Tap Active → text Primary + 2px Line underline; Inactive → Black/40% text, no line; Hover → Black/60% text. Segmented Active → White/100% fill button; Inactive → Borderless button
- **Key tokens**: Tap active text=Primary, underline=Primary 2px; Segmented tray=Black/4%, active=White/100% fill
- **Design rules**: _"The function of the Tap component is to switch the displayed content."_ Tap for top-level navigation/content switching. Segmented for mode toggles (2-5 options). Segmented is NOT a component — it's Group(fill) containing Buttons. Active Line underline width matches text width, not tab width.
- **Source**: [docs/patterns/tab-segmented.md](../../docs/patterns/tab-segmented.md)

---

## Notification

- **Variant axes**: State(Successful/Failure) × Big(T/F) × optional Glass(T/F)
- **Layout**: HORIZONTAL auto-layout; padding 12h/16v; gap 8px; radius 16px
- **Sizing**: width HUG (auto-sizes to content), max-width constrained
- **Slots**: Status dot (Icon, 8px), message Text, optional action Button (Big=true adds details/dismiss)
- **State signals**: N/A — notification is transient, not interactive beyond dismiss
- **Key tokens**: Non-glass: dual fill Black/80% + White/10%, text White/100%; Glass: BACKGROUND_BLUR, text Black/100%; Success=#71dd8c (green dot); Failure=#ffcc00 (yellow dot, NOT red)
- **Design rules**: _"Notification components are used to give users short, timely and relevant information."_ Slides up from bottom. Stays 3 seconds, auto-dismisses. Failure is yellow NOT red. Non-glass is dark theme, glass is light theme. Big variant adds description text + dismiss button.
- **Source**: [docs/patterns/notification.md](../../docs/patterns/notification.md)

---

## Chart (Two Systems)

SnowUI has **two distinct chart systems**. They must not be mixed.

### System 1: ChartMotion (Animated/Interactive)
"Motion" = animation. The primary chart component.
- **Variant axes**: Type(Vertical/Horizontal/Line) × Show Left Text(T/F)
- **Layout**: HORIZONTAL auto-layout; Left Text + Frame(grid + bars + x-axis labels)
- **Sizing**: 800×400 default; breakpoints: 400×200 (compact), 400×320 (horizontal compact); bar max-width 28px; 1-12 data points
- **Slots**: 3 INSTANCE_SWAP slots — Vertical Bar, Horizontal Bar, Line — swap to change chart type or bar style
- **Bar architecture**: 8 RECTANGLE segments per bar column, opacity 0→1 fill animation bottom-to-top; Height property (1-6) controls visible segments
- **Tooltip**: #1c1c1c bg, cr=8, padding 4/8, gap=4, 12px Inter white text, opacity 0→1 on hover
- **Animation**: Bars fill with staggered reveal (segments bottom-to-top). Tooltips fade in on hover.
- **Line styles**: Line A (solid gradient with area fill), Line B (dashed, dashPattern [2,4]), Line 01 (composite A+B overlay)
- **Usage**: Interactive dashboards, data exploration views, animated reports

### System 2: Static Bar Styles (Swappable Collections)
Simpler bar collections, no animation, no tooltips. Used standalone or swapped into ChartMotion.
- **Vertical styles (01-08)**: Single rounded, stacked double, segmented 16-seg, multi-color stacked, thin segmented, waterfall, thin-line G, grouped H
- **Horizontal styles (01-04)**: Standard, dense multi-bar, pill, dot-matrix
- **Key characteristics**: Single rectangle per data point, NO tooltips, NO animation, NO hover states
- **Usage**: Dashboard card compositions, static reports, chart-resource blocks, compact sparklines

### Shared Components
- **DonutChart**: Data Count=2-6 variants, 120×120, gradient arc segments via Subtract boolean ops, hidden tooltips
- **SemicircleChart**: 250×126, 3 concentric arcs, percentage label inside
- **ChartDot**: Property=A/B, 32×32, for line chart data points

### Key tokens
- Colors: Two themes (Bright Light / Pastel Light) via Themes variable; series use Secondary/* palette
- Grid lines: Black/4% (thin), Black/20% (baseline)
- Axis text: 12px/400, Black/40%, Inter
- CHART tokens (local): Line Corner Radius (2/16/28), Dot Size (4-24px across 3 vars × 3 modes)
- **Design rules**: _"The chart component can be used completely independent of SnowUI."_ Bar width max 28px (removable). Always include axis labels. Max 12 data points. Horizontal bars read left-to-right. Donut center is empty — place summary Text inside.
- **Source**: [docs/patterns/chart.md](../../docs/patterns/chart.md), [docs/patterns/chart-motion.md](../../docs/patterns/chart-motion.md)

---

## Date Picker

- **Variant axes**: Type(Date only/Date and time/Date range/Date range and time)
- **Layout**: VERTICAL auto-layout; 360×360px container; radius 20px; padding 16px
- **Sizing**: fixed 360×360px; calendar grid 7 columns × 6 rows; cell 40×40px
- **Slots**: Header (breadcrumb nav as Tag chain), weekday labels (Group of Text), day cells (Group of Buttons), optional time picker (scroll columns)
- **State signals**: Selected → fill #adadfb (Secondary/Indigo); range end → fill #000000; today → outline; hover → Black/4%
- **Key tokens**: container glassmorphism (White/90%, blur 40, shadow); selected=#adadfb; range-end=#000000
- **Design rules**: _"Used to select a date and time. Just like a Windows folder path."_ Breadcrumb navigation: Year→Month→Day (click to zoom out). No confirm button — closing confirms. Range selection highlights intermediate days at Black/4%. Time picker is scroll-based columns for hours/minutes.
- **Source**: [docs/patterns/date-picker.md](../../docs/patterns/date-picker.md)

---

## Tooltip

- **Variant axes**: None formal — adapts to context
- **Layout**: HORIZONTAL auto-layout; padding 8h/4v; radius 12px (standalone) or 8px (button context)
- **Sizing**: width HUG, height HUG; max-width 240px
- **Slots**: Inner Text instance (single slot)
- **State signals**: N/A — tooltip is display-only, triggered by parent hover
- **Key tokens**: fill rgba(0,0,0,0.8), text White/100%
- **Design rules**: _"Tooltips are used to describe or identify an element."_ Timing: 1.5s delay on first hover; 0ms for same-type elements; 500ms re-hover within 30s window. Standalone=12px radius, attached to Button=8px radius. Always single-line unless content exceeds max-width. Place above target by default, flip if clipped.
- **Source**: [docs/patterns/tooltip.md](../../docs/patterns/tooltip.md)

---

## Table

- **Variant axes**: Table A (full-featured, interactive) / Table B (preview-only, read-only)
- **Layout**: VERTICAL stack of columns (column-based, NOT row-based); uniform 40px row height; radius 16px outer
- **Sizing**: width FILL, height HUG; columns FILL equally or fixed
- **Slots**: Toolbar (Frame: Add Button + Filter/Sort/Search), Column headers (Text), Column cells (Text, Tag, Icon, Button — INSTANCE_SWAP), Pagination (Group: page buttons + per-page selector)
- **State signals**: Row hover → fill Black/2%; selected row → Black/4% + leading checkbox; header sort → Icon arrow indicator
- **Key tokens**: header fill Black/2%, dividers Black/4% stroke, status badges 5 colors (Purple/Green/Blue/Yellow/Grey)
- **Design rules**: _"Table is not a complete component, but consists of many instances."_ Column-based architecture — each column is a vertical Group. Row height always 40px. Toolbar is optional but standard for Table A. Pagination options: 20/50/100 per page. Status badges reuse Tag/Badge color system. Table B omits toolbar and pagination — use for dashboard previews.
- **Source**: [docs/patterns/table.md](../../docs/patterns/table.md)
