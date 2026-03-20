# SnowUI Component State Matrix

8 states across 18 component types. Source: `docs/patterns/interactive-guidance.md` (1302 lines) + raw JSON verification.

---

## The 8 States

| State | Visual Change | Trigger |
|-------|--------------|---------|
| **Default** | Base appearance, no adornment | Initial render |
| **Hover** | Stroke appears or bg shifts | Cursor enters |
| **Focus** | Focus ring: `rgba(0,0,0,0.04)` spread-4 | Tab/click into |
| **Active/Pressed** | Fill: `Black/4%` or `Primary` | Mousedown / selected |
| **Disabled** | Opacity 0.4 on entire element | Programmatic |
| **Error** | `Secondary/Red` stroke + error text below | Validation failure (onBlur) |
| **In Progress** | Loading skeleton or spinner | Async operation |
| **Done/Success** | `Secondary/Green` icon + notification toast | Operation complete |

---

## State Matrix by Component

### Input Fields

```
Default:     bg=Background/3, stroke=Black/20% @ 0.5px, text=Black/20% (placeholder)
Hover:       stroke=Black/40% @ 0.5px
Focus:       stroke=Black/40% @ 0.5px + Focus ring, floating label animates up
Active:      text=Black/100% (value entered)
Disabled:    opacity 0.4 on container
Error:       stroke=Secondary/Red, error text below (pushes content down)
```

### Buttons

```
Filled:
  Default:   bg=Primary, text=#ffffff (hardcoded)
  Hover:     bg=Primary with subtle overlay
  Focus:     Focus ring around button
  Disabled:  opacity 0.4

Outline:
  Default:   bg=White/100%, stroke=Black/10%
  Hover:     bg=Black/4%
  Focus:     Focus ring
  Disabled:  opacity 0.4

Gray:
  Default:   bg=Black/4%, text=Black/40%
  Hover:     bg=Black/10%
  Focus:     Focus ring
  Disabled:  opacity 0.4
```

### Cards

```
Default:     no stroke, no adornment
Hover:       stroke=Black/40% @ 0.5px, ghost action icon appears (opacity 0.2)
Selected:    stroke=Primary @ 1px, solid action icon (opacity 1.0) + 4x inner-shadow
Static:      no stroke, no interaction (display-only card)
```

### Sidebar Nav Items

```
Default:     transparent bg, icon=Black/100%, text=Black/100%
Hover:       bg=Black/4%, radius=12
Active:      bg=Black/4%, radius=12 (persists)
```

### Table Rows

```
Default:     transparent bg
Hover:       bg=Black/4%, action buttons APPEAR (hidden by default)
Selected:    bg=Black/4%, checkbox visible + checked
```

### Tabs

```
Inactive:    text=Black/40%, no underline
Hover:       text=Black/100%
Active:      text=Primary, 2px bottom underline stroke=Primary
```

### Segmented Control

```
Inactive:    Button(style=Borderless), text=Black/40%
Active:      Button(style=Filled or White/100%), text=Black/100%
Container:   Black/4% tray behind all segments
```

### Toggle/Switch

```
Off:         track=Background/3, knob=HARDCODED #ffffff
On:          track=Primary, knob=HARDCODED #ffffff
Disabled:    opacity 0.4
BUILD RULE:  Knob is hardcoded white, NOT White/100%. Safe on Primary in all modes.
```

### Checkbox

```
Unchecked:   bg=Black/4%, stroke=Black/80%
Checked:     bg=Primary, checkmark=HARDCODED #ffffff
Disabled:    opacity 0.4
BUILD RULE:  Checkmark is hardcoded white, NOT White/100%. Same pattern as filled button text.
```

### Search

```
Default:     icon=Black/20%
Hover:       icon=Black/40%, bg shifts
Focus:       icon=Primary, focus ring
Active:      icon=Primary, search popup appears (600x768)
```

### Badge (Status)

```
Dot style:       6x6 circle, fill=Secondary/* (matching status color)
Filled style:    bg=Secondary/* @ 10% node opacity, text=Secondary/* @ 100%
Number style:    bg=Secondary/Indigo, text=hardcoded #ffffff
```

### Notification Toast

```
Default:     bg=Black/80%+White/10% (BOUND — inverts in dark), text=White/100% (BOUND — inverts too)
             Both invert together: light bg + dark text in dark mode = readable
Success:     icon=CheckCircle fill=Secondary/Green (#71dd8c) — static, stays green
Warning:     icon fill=Secondary/Yellow (#ffcc00) — static
With action: "Deleted  Undo" — undo link in toast
Animation:   slide up from bottom, stay 3s, auto-dismiss
```

### Tooltip

```
Container:   bg=HARDCODED #000000 @ 0.8 (does NOT invert in dark mode)
Text:        HARDCODED #ffffff (NOT White/100% — would invert to black on black bg)
Secondary:   HARDCODED #ffffff @ 40% opacity
Arrow:       4 positions (top, bottom, left, right)
Timing:      1.5s first hover, 0ms consecutive same-type, 0.5s return within 30s
BUILD RULE:  Tooltip bg is hardcoded, so text MUST also be hardcoded white.
```

### Date Picker

```
Container:   360x360, radius=16, fill=Background/6, Glass2 + backdrop blur 40px
Header:      bottom-only stroke Black/10% @ 1px
Today:       highlighted cell
Selected:    Primary fill on date cell
Range:       start/end cells = Primary, between = Black/4%
```

### Image/Avatar

```
Placeholder: circle, fill=Black/4%
Hover:       inner shadow overlay (#000000, 0,20 blur-20)
Loaded:      image fill replaces placeholder
```

---

## Universal Rules

1. **Disabled = opacity 0.4** on the entire element container. Never gray out individual children.
2. **Focus ring** is always `rgba(0,0,0,0.04)` 0,0 blur-0 spread-4. Can swap to brand color.
3. **Hover-reveal actions**: Table row actions, card actions, "select all" checkbox — all hidden until hover.
4. **Error pushes content down** — never overlays. Error text appears BELOW the field.
5. **Validation triggers on blur**, not on keystroke.
6. **Animation symmetry**: Entry = exact reverse of exit. Always.

---

## Mobile-Specific States

### Mobile Tab Bar (Floating Pill)
```
Active:   icon=Black/100% (solid), no secondary fill
Inactive: icon=Black/100%, secondary fill at node opacity 0.08
Container: radius 80px, fill White/4% (near-transparent)
```

### Mobile Account Tabs (Horizontal Scroll)
```
Active:   fill=Colors/Blue (#007AFF), text=White
Inactive: fill=Black/4%, text=Black/100%
Container: horizontal scroll, total tabs > viewport width
```

### AI Chat Input
```
Empty:    bg=White @ 0.8, stroke=Black/100% @ 0.5px, placeholder=Black/20%, send button opacity 0.1
Typing:   placeholder hidden, send button full opacity
Glass:    backdrop-blur 40px, drop-shadow 0 8px 28px rgba(0,0,0,0.1)
```

### Mobile Auth Input (Splash/Dark Background)
```
Default:  fill=White/10%, stroke=White @ 0.4 @ 0.5px, backdrop-blur 40px
Focused:  stroke opacity unchanged (already 0.4 on dark bg)
```

### OTP Input Cells
```
Empty:    fill=White @ 0.8, stroke=Black @ 0.2 @ 0.5px
Focused:  stroke opacity 0.4
Filled:   text=Black/100%
```

### Password Strength Indicator
```
4 segments: 72.3x4px each, gap 8px, total 313px
None:   all segments fill=Black/10%
Weak:   1/4 filled
Fair:   2/4 filled
Good:   3/4 filled
Strong: 4/4 filled
```

### Copilot Suggestion Chips
```
Default:  height 36, radius 16, fill=Black/4%, text=14px
Hover:    fill shifts darker
Cycling:  ArrowClockwise icon rotates to refresh suggestions
```

---

## Glassmorphism Recipe

Used on AI Chat header/input, auth splash buttons, date picker, sidebar overlay:

```
fill: [base-color] @ 0.8 opacity
backdrop-filter: blur(40px)
stroke: [contrast-color] @ 0.5px
border-radius: context-appropriate (16px input, 20px button, 80px pill)
```

Social login buttons on splash screens: each has brand-color fill @ 0.8 + blur 40px.
