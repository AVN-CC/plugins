# Primitives Index — SnowUI Design System

> Compact lookup table for LLM UI assembly. Each primitive is a composable building block used inside components.

---

## Text

- **Role**: Typography primitive — all readable content flows through Text
- **Variant axes**: Count(1-7) × Vertical(true/false) × State(Default/Hover/Selected/Disabled)
- **Layout**: Vertical=true → VERTICAL stack; Vertical=false → HORIZONTAL inline
- **Sizing**: width FILL, height HUG; gap 0-8px
- **Slot pattern**: Each Count unit = one text layer; Count=2 Vertical=true is the workhorse "heading + description" pair
- **Key tokens**: heading Black/100% Semibold, description Black/40% Regular, Font family variable
- **Composition**: Consumed by Card (each Count slot), Button (center label), Input (label + placeholder), Tag (label), Tooltip (inner content)
- **Design rules**: _"The purpose of making text components is to make it more extensible."_ Count=1 for labels/values. Count=2V for title+subtitle. Count=3+ for multi-line content blocks. Never set explicit font-size on Text — size comes from the parent component context.
- **Source**: [docs/patterns/text.md](../../docs/patterns/text.md)

---

## Icon

- **Role**: Size-control container wrapping icons, avatars, images, logos
- **Variant axes**: 9 base sizes (16/20/24/28/32/40/48/64/80px)
- **Layout**: fixed square frame, child centered
- **Sizing**: fixed W=H per size; padding scale: 16-32→4px, 40-48→8px, 80→12px
- **Slot pattern**: DefaultIcon (INSTANCE_SWAP) — swap to any icon/avatar/image; optional Badge overlay (Dot or Count variant, top-right); optional Background fill
- **Key tokens**: Black/100% default fill, Black/40% disabled, badge uses Secondary/Indigo (#adadfb)
- **Composition**: Consumed by Icon & Text (left/right slot), Button (Left Icon / Right Icon slots), Card (via Frame), Search (leading icon), Input (trailing icon), Quick Action (row icons)
- **Design rules**: _"Icon component is used to set the size of the Icon within the component."_ Always wrap raw SVGs in Icon — never place bare SVGs. Badge inherits position from Icon size. Size 24px is the default for inline use; 40-48px for standalone/avatar contexts.
- **Source**: [docs/patterns/icon.md](../../docs/patterns/icon.md)

---

## Icon & Text

- **Role**: Composition primitive pairing exactly one Icon + one Text
- **Variant axes**: None formal — 4 layout permutations via direction × order
- **Layout**: H or V auto-layout; icon-first or text-first; gap 8px; radius 12px
- **Sizing**: HUG both axes
- **Slot pattern**: Slot 1 = Icon instance, Slot 2 = Text instance (both INSTANCE_SWAP)
- **Key tokens**: Inherits from Icon + Text children
- **Composition**: Used inside Frame, Card rows, navigation items, list items. Upgrade to Frame when you need 3+ children.
- **Design rules**: _"A combination of Icon and Text. You can also change the Icon to an avatar, logos, picture, etc."_ Use H+icon-first for nav items and list rows. Use V+icon-first for feature cards and menu tiles. If you need more than 2 children, switch to Frame.
- **Source**: [docs/patterns/icon-text.md](../../docs/patterns/icon-text.md)

---

## Frame

- **Role**: Most flexible layout container — wraps 2+ children of any primitive type
- **Variant axes**: None formal — 82 documented instances across 6 patterns
- **Layout**: HORIZONTAL or VERTICAL auto-layout; gap 8-20px; radius 8-12px; padding varies by pattern
- **Sizing**: width typically FILL, height HUG
- **Slot pattern**: N children (INSTANCE_SWAP each), no fixed count — compose freely from any primitive
- **Key tokens**: fills Black/4% to Background/5; strokes Black/4-8%
- **Composition**: The universal intermediate container. Sits between primitives (Icon, Text, Icon & Text) and components (Card, Quick Action). 6 key patterns: list row, content block, settings row, header bar, toolbar, compact card.
- **Design rules**: _"The Frame component will be more extensible than Icon & Text."_ Use Frame when Icon & Text is too limited (3+ slots needed). Frame is NOT a component — it has no variant states. Nest Frames for complex layouts but prefer flat structures. Always set child alignment (center-vertical for rows, stretch for columns).
- **Source**: [docs/patterns/frame.md](../../docs/patterns/frame.md)

---

## Group

- **Role**: Collection primitive — repeats homogeneous elements in numbered slots (1-8)
- **Variant axes**: Count(1-8)
- **Layout**: HORIZONTAL or VERTICAL auto-layout; gap varies: 4px tight, 8px standard, 16px loose, 28px extra-loose
- **Sizing**: width FILL or HUG by context; height HUG
- **Slot pattern**: Slots named 1-8, each INSTANCE_SWAP — swap to Button, Tag, Icon, Frame, etc. Hidden slots via visibility toggle.
- **Key tokens**: Optional tray fill Black/4% (for segmented controls), no tokens on Group itself
- **Composition**: Used for nav bars, tag groups, pagination, action pairs, feature bars, side nav, OTP input, calendar rows. Segmented control = Group(fill) + Button children. 41 documented instances across 8 patterns.
- **Design rules**: _"The function of the Group component is to group any instances."_ Group is for repeating similar items — use Frame for heterogeneous layouts. Always set Count to match actual visible children. Gap communicates relationship density: tighter gap = stronger grouping.
- **Source**: [docs/patterns/group.md](../../docs/patterns/group.md)

---

## Strip & Line

- **Role**: Strip = segmented rectangular bar; Line = stroke-based divider
- **Variant axes**: Both have Count(1-8)
- **Layout**: HORIZONTAL auto-layout; gap 2-4px (Strip), 0px (Line)
- **Sizing**: Strip segments fill equally; Line stretches to parent width
- **Slot pattern**: Each Count = one rectangle (Strip) or one stroke segment (Line)
- **Key tokens**: Strip fills use chart palette or Black/8-20%; Line stroke Black/8% default, Primary for active
- **Strip uses**: Progress bars, strength indicators, chart bar segments, step indicators
- **Line uses**: Section dividers, underline indicators (Tab active state), horizontal rules, form separators
- **Design rules**: Strip — _"Strip component is used to draw strips."_ Line — _"Line component is used to draw lines."_ Strip Count encodes discrete levels (e.g., password strength = 4 strips). Line Count=1 for simple dividers; Count>1 for multi-segment tab underlines. Line height is always 1-2px. Strip height 4-8px.
- **Source**: [docs/patterns/strip-line.md](../../docs/patterns/strip-line.md)
