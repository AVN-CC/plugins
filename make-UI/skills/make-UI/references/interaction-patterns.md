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

## Sidebar

- **Width:** min 212px, max 40% page width
- **Sections (top to bottom):** Profile avatar → Tab group (All, Favorite, Recent) → Nav groups → Logo
- **Active item:** fill `Black/4%`, radius 12
- **Tab states:** Active text=`Black/40%`, Inactive text=`Black/20%`
- **Drag-to-sort:** Items can be reordered within their category via drag
- **Collapse:** Sidebar can collapse to icon-only strip. State persists across sessions.
- **Directory behavior:**
  - **Simple directory:** Click = expand/collapse inline (accordion style)
  - **Complex directory:** Click = navigate to directory page, sub-items shown in content area

---

## Add Data Modal

- **Trigger:** "Add" button in table function bar
- **Layout:** Modal with form fields matching table columns
- **Required fields:** Marked with asterisk (*)
- **Validation:** Same onBlur pattern as all forms
- **Submit:** Creates row + triggers success notification ("Done")
- **Cancel:** Closes modal, no data saved

---

## Filter & Sort

- **Filter:** Dropdown menus per column. Multiple filters stack (AND logic). Active filters shown as removable tags above table.
- **Sort:** Click column header to toggle ascending/descending/none. Visual indicator: chevron up/down.
- **Keyboard:** `Cmd+C` = sort ascending, `Cmd+D` = sort descending (table context)
- **Persistence:** Active filters and sorts persist within session

---

## Block/Widget Patterns

- **Hover-to-reveal:** Action buttons (edit, delete, more) appear only on block hover
- **"Select all" checkbox:** Hidden until cursor enters the content block area
- **Reorder:** Drag handle appears on hover, drag to reposition blocks

---

## Data Formats

- Time < 7 days: relative ("5 minutes ago", "2 days ago")
- Time >= 7 days: absolute ("Mar 15" or "Mar 15, 2026")
- Numbers: comma-separated thousands (48,291)
- Currency: `$X,XXX.XX`, negative in `Secondary/Indigo`
- Truncation: ellipsis + tooltip reveal on hover
- Empty: dash "—" or hide field, never "null" or "N/A"

See `references/data-formats.md` for full rules.

---

## Layout Modes

| Mode | Panels | Content max-width |
|------|--------|------------------|
| Three columns | Sidebar + Content + Rightbar | -- |
| Two columns | Content + one panel (80px gap) | -- |
| One column | Content only | 1200px |

Dynamic scaling: use `rem`/`em`, not `px`. Text, line-height, spacing, radius, shadows all in relative units.

---

## Component States (Summary)

8 universal states: Default, Hover, Focus, Active, Disabled, Error, In Progress, Done.

- **Disabled:** Always opacity 0.4 on the entire container
- **Focus ring:** `rgba(0,0,0,0.04)` 0,0 blur-0 spread-4
- **Error:** `Secondary/Red` stroke + error text below field (pushes content down)
- **Hover-reveal:** Table actions, card actions, select-all checkbox — hidden until hover

See `references/component-states.md` for the full 18-component matrix.
