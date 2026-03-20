# SnowUI Interaction Patterns

Compact reference for all interaction behaviors. Source: `docs/patterns/interactive-guidance.md`

---

## Loading

Replace content with simple geometric skeleton shapes matching target dimensions. No spinners. Transition directly from skeleton to loaded state.

| Content Type | Skeleton Shape |
|-------------|---------------|
| Text line | Rounded rectangle |
| Avatar | Circle |
| Stat number | Rectangle block |
| Chart area | Rounded rectangle |
| Nav items | Horizontal lines |

---

## Hover

- Action buttons are HIDDEN by default, revealed ONLY on hover (tables, blocks, rightbar)
- "Select all" checkbox: hidden until cursor enters content block
- Sidebar items: tooltip on hover (1.5s delay)
- Image: inner shadow overlay (`#000000` 0,20 blur-20)

---

## Focus

- Focus ring effect style: `rgba(0,0,0,0.04)` 0,0 blur-0 spread-4
- Can change shadow color to brand color
- Floating label pattern: label shrinks and moves up on focus

---

## Validation

- **Trigger:** `onBlur` -- validate when field loses focus, NOT on keystroke
- **Error position:** below the form field, pushes content down (not overlay)
- **Textarea:** character count `0/200` in bottom-right
- **Password strength:** 4 bars, each 72x4, gap 8, fill `#000000`

---

## Scroll

- Hidden by default
- Appears when cursor enters scrollable area AND user scrolls
- Overlay mode (not push) -- can overlap content
- 4px inset from container edge

---

## Animation

- Entry animation = exact reverse of exit animation
- If appear is opacity 0%->100%, disappear is 100%->0%
- Symmetrical transitions always

---

## Table

- **Function bar:** Default state (Search, Filter, Sort, Add) swaps to Selected state (bulk action bar) when rows are chosen
- **Selection:** Click blank area in column to select. Ctrl/Cmd+Click for multi-select. Click outside to deselect
- **Duplicate:** appears below last selected row, auto-selects duplicated rows
- **Delete:** triggers Global Notification with "Undo" action
- **Pagination:** 20, 50, 100 (default 20). Not needed when using dynamic scroll loading
- **Search in table:** replaces data in-place, live as you type, includes auto-complete. Clearing restores original. Scope respects existing filters

---

## Notification

- **Trigger:** any write operation to the database
- **Animation:** slides up from bottom, stays 3s, auto-dismisses
- **Position:** bottom of viewport
- **Spec:** h=40, radius 16, pad 8/12, gap 8, bg `#000000`, text 14px white
- **Success icon:** CheckCircle `#71dd8c`
- **Failure/warning:** Yellow (implied from status token `#ffcc00`)
- **Content types:** "Done", "Deleted  Undo", "Your file is being created..."

---

## Remember Everything

On re-login, restore:
- Sidebar collapsed/expanded state
- Currently opened page
- Partially entered input content
- Selected data in tables/forms

---

## Tooltip

- **First hover:** 1.5s delay
- **Same-type consecutive:** immediate (0ms)
- **Return within 30s:** 0.5s delay
- **Position:** above target by default, must not block target or important info, always top z-index
- **Content:** plain text, or rich (14px bold title + 12px description), or truncated text reveal
- **Button tooltips:** label + keyboard shortcut (e.g., "Favorites `Cmd+F`")

---

## Date Picker

- **No confirm button** -- closing the picker confirms the selection
- **Default:** current date selected (Today indicator)
- **Month/Year nav:** click month label to enter month selection, click year to enter year selection
- **Input activation:** click segment in top input to edit that part (breadcrumb-style, like Windows folder nav)
- **Range:** first click = start, second click = end. Activation toggles automatically
- **Time:** selectable or inputable. Auto-correction: entering "13" switches to PM and corrects to "01"
- **Small targets:** scale component to 1.5x or 2x

---

## Search (Global)

- **Open:** click header search or press `Cmd+/` or `/`
- **Close:** `ESC` or click outside
- **Default:** first result selected
- **Enter:** opens selected result in current page
- **Cmd+Click / Cmd+Enter:** opens in new browser tab
- **Live search:** results generate as you type, includes auto-complete
- **Result types:** User, Page, Text on page, Data in table
- **Clear:** returns to initial state (Recent search, Recently visited, Contacts)

---

## Quick Action Menu

- Show search when: >10 items, or >5 items with submenu
- First result selected by default
- Destructive actions require confirmation: "Confirm Deletion?"
- Group selection: clicking group title selects all in group

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Cmd+/` or `/` | Open global search |
| `ESC` | Close search popup |
| `Enter` | Select highlighted result |
| `Cmd+Click` | Open in new tab |
| `Cmd+C` | Sort ascending (table context) |
| `Cmd+D` | Sort descending (table context) |
| `Cmd+F` | Favorites |

---

## Layout Modes

| Mode | Panels | Content max-width |
|------|--------|------------------|
| Three columns | Sidebar + Content + Rightbar | -- |
| Two columns | Content + one panel (80px gap) | -- |
| One column | Content only | 1200px |

Dynamic scaling: use `rem`/`em`, not `px`. Text, line-height, spacing, radius, shadows all in relative units.
