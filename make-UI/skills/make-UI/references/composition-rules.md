# SnowUI Composition Rules

10 agent-optimized rules for building SnowUI-compliant interfaces. Each rule includes a designer quote and structural example.

---

## 1. Opacity is your hierarchy (1.0 / 0.4 / 0.2 / 0.04)

State and emphasis are controlled entirely through opacity on black, never through color changes.

> "The fill color from dark to light represents the frequency of use from high to low."

```
Primary text:    Black/100% (opacity 1.0)
Section headers: Black/40%  (opacity 0.4)
Placeholders:    Black/20%  (opacity 0.2)
Active fill:     Black/4%   (opacity 0.04)
Borders:         Black/10%  (opacity 0.1)
```

---

## 2. Text is a component, not a string

Never embed raw text nodes. Wrap text in the Text component (Count 1-7, Vertical true/false). This eliminates variant explosion in parent components.

> "The purpose of making text components is to make it more extensible."

> "Now, if we replace the text with a text component, we won't need to add more variants to the component."

```
// WRONG: raw text in a card
<div className="card"><p>Title</p><p>Description</p></div>

// RIGHT: Text component with Count=2, Vertical=true
<Text vertical count={2}>
  <span className="text-heading-s">Title</span>
  <span className="text-body-s text-secondary">Description</span>
</Text>
```

---

## 3. Slots are swappable (INSTANCE_SWAP)

Card slots accept any component instance except the Card itself. Swap Text for Frame, Tag, Strip, Button, Icon & Text, or Group.

> "Instances inside the Card can be replaced with any instance except itself."

> "Replace Icon with avatar. Double-click to select the Icon, and replace the property with an avatar."

```
Card (Count=4, VERTICAL)
  Slot 1: Tag (label)           -- swapped from Text
  Slot 2: Text (title + desc)   -- default
  Slot 3: Strip (progress bar)  -- swapped from Text
  Slot 4: Frame (avatars + date)-- swapped from Text
```

---

## 4. Padding responds to content (asymmetric when icons present)

Five padding tiers scale with card width. Padding is always token-bound and increases as content grows.

> "Components can be added, modified and deleted. Modifying them will affect all components and pages, please proceed with caution."

```
Component base:  12/16/12/16  gap=4   (120px cards)
Compact:         16/16/16/16  gap=4   (260px, address)
Standard:        16/24/16/24  gap=8   (260px, credit)
Content:         24/24/24/24  gap=8   (280px, stat/project)
Spacious:        40/40/40/40  gap=24  (372px, pricing)
```

---

## 5. State through opacity, not color

Component states are signaled through stroke opacity and weight. Fill color never changes between states.

> "I didn't put all the state of the component into the component, because most of the design is not needed, just let the development know."

```
Default:  no stroke, no icon
Hover:    stroke Black/40% @ 0.5px, ghost ellipse @ 0.2
Selected: stroke Primary @ 1px, solid ellipse @ 1.0 + 4x inner-shadow
Static:   no stroke, no icon (non-interactive)
Active:   fill Black/4% (sidebar items)
```

---

## 6. 4px grid always

All spacing, size, and corner radius values are multiples of 4. No exceptions in the token system.

> "Their values are multiples of 4."

```
Spacing scale: 0, 4, 8, 12, 16, 20, 24, 28, 32, 40, 48
Size scale:    16, 20, 24, 28, 32, 40, 48, 64, 80, 100, 124
Radius scale:  0, 4, 8, 12, 16, 20, 24, 28, 32, 40, 48, 80
```

---

## 7. Chart composition is card-first

All chart blocks use the same container spec. The chart graphic is a design resource inside the card, not a standalone element.

> "I found that the data graphics of charts are difficult to create rich chart styles with a few simple components, so I put the data graphics into design resources."

```
Chart block = Card container (pad 24, gap 16, radius 20, fill Background/2)
  + Header row (title + action)
  + Chart graphic (design resource, variable size)

Grid: 28px gap between all chart blocks
Fill: always Background/2 (#f9f9fa), never Background/4 or /5
```

---

## 8. Dark mode is token swap, not redesign

Structure, spacing, typography, and radius stay identical between modes. Only fills, strokes, and text colors change via theme tokens.

> "In SnowUI, we use variables to control color, spacing, icon size, and corner radius. These are key parts of the design system."

```
Light -> Dark changes:
  Black/100% (#000000) -> White (#ffffff)
  Background/1 (#ffffff) -> #333333
  Background/2 (#f9f9fa) -> rgba(255,255,255,0.04)
  Primary button (#000000) -> Secondary/Indigo (#adadfb)

Structure stays identical: same padding, gap, radius, font sizes.
```

---

## 9. The 90% principle (components, colors, text styles)

If something is used in fewer than 10% of cases, it does not belong in the design system. Keep counts minimal.

> "In SnowUI we follow 90% of the design from the design system."

> "Doing so avoids bloating the design system. It can keep the rules in the design system more applicable. The remaining 10% (colors, spacing, fonts, components, etc.) can be exempt from design system rules."

```
Max limits:
  Text styles:    < 20  (currently 16)
  Colors:         < 32  (currently ~31)
  Effect styles:  < 8   (currently 10, slightly over)
  Spacing/Radius: < 16  (currently 12)
  Components:     minimize (fewer parts, more breadth)
```

---

## 10. Fewer parts, more breadth

Build the most frequently used combinations as base components. Every other design is composed from base components. Extensibility over coverage.

> "The design logic of SnowUI components is to make the most frequently used combinations into base components, and then make other components and page elements from the base components."

> "The extensibility of components is better. For example, you can replace the Icon in the button component with the avatar, and you don't need to add new components for this design."

```
Hierarchy:
  Base Components (building blocks, minimal set)
    -> Common Components (composed from base)
    -> Chart Components (base + design resources)
    -> Page Components (page-specific, conform to rules but outside system)

Key: a Button with an avatar swap IS the avatar-button.
     No new component needed.
```
