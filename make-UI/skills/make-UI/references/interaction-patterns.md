# SnowUI Interaction Patterns

Complete behavioral reference for all interaction patterns. Source: `docs/patterns/interactive-guidance.md` (1302 lines, 20 sections).

---

## Core UX Principles

### Speed First
> "Efficiency is the first core of SnowUI user experience."

### Preloading
Create a preloading strategy per page/data. Prioritize first meaningful paint. Every page should have a preloading plan.

### Remember Everything
On re-login, restore:
- Sidebar collapsed/expanded state
- Currently opened page
- Partially entered input content
- Selected data in tables/forms

### Animation Symmetry
Entry animation = exact reverse of exit animation. If appear = opacity 0→100%, disappear = 100%→0%.

### Dynamic Scaling
Use `rem`/`em`, not `px` for text size, line height, spacing, rounded corners, shadow size.

### Scroll Bar
- Hidden by default
- Appears when cursor enters scrollable area AND user scrolls
- Overlay mode (not push) — can overlap content
- 4px inset from container edge

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
| Card content | Stacked rectangles |

---

## Hover

- Action buttons HIDDEN by default, revealed ONLY on hover (tables, blocks, rightbar)
- "Select all" checkbox: hidden until cursor enters content block
- Sidebar items: tooltip on hover (1.5s delay)
- Image: inner shadow overlay (`#000000` 0,20 blur-20)

---

## Focus

- Focus ring: `rgba(0,0,0,0.04)` 0,0 blur-0 spread-4
- Can change shadow color to brand color
- Floating label pattern: label shrinks and moves up on focus

---

## Form Interactions

### Input Field
- **Focus:** Floating label shrinks and moves up
- **Validation:** `onBlur` — validate when field loses focus, NOT on keystroke
- **Error:** Error text below field, pushes content down (never overlays)

### Textarea
- Character count: `0/200` format in bottom-right

### Select Component
- Default: placeholder "Select"
- Selected: shows chosen value
- Error: error message below
- Focus: dropdown open with options list (text items + optional checkmarks)

### Form Buttons
- Two-button pattern: `Cancel` | `Save Changes`
- After successful save: button transitions to "Done" state
- After form submission: Global Notification appears

### Social Media Login
- "Sign in with Apple" → opens Apple account login window
- Social media buttons are separate from form buttons

---

## Table A (Full Data Table)

### Data Selection
- Click blank area in column to select that column
- `Cmd+Click` (macOS) / `Ctrl+Click` (Windows) for multi-select
- Click outside table to deselect all
- Row actions: open data in popup window or new tab

### Select All
- Checkbox hidden by default, revealed only when cursor hovers the content block

### Function Bar
- **Default state:** Search, Filter, Sort, Add data buttons
- **Selected state:** Swaps to bulk action bar (separate component)
- BUILD RULE: Implement as two separate components that swap on selection

### Bulk Actions

**Duplicate:**
- Duplicated rows appear immediately below the last selected row
- After duplication, new rows are auto-selected

**Delete:**
- Triggers Global Notification with "Deleted    Undo" (undo action available)

### Pagination
- Options: **20, 50, 100** (default: 20)
- When using dynamic scroll loading, pagination is not needed

### Dynamic Loading
- Table data loads incrementally as user scrolls down
- Replaces traditional pagination when enabled

### Column Headers
- Hover state (underline or highlight)
- Clicking column title can trigger sort or filter

### Status Badges in Rows
Values: `In Progress`, `Complete`, `Pending`, `Approved`, `Rejected`

### Table Loading State
- Dedicated skeleton/loading state for data fetch

---

## Table B (Block/Card Table)

### Opening Data
- Open data page in a popup window
- "Open as full page" button: visible ONLY when cursor is within the block
- Can also open in a new browser tab

### Action Buttons
- Hidden by default, revealed on hover
- Quick Action menu available for extended options

### Avatar Click
- Clicking avatar/user name navigates to user profile page

### Time Filter Options
- 1 hour, 3 hours, 1 day, 1 week

---

## Table Search

### Search-in-Table Behavior
1. Search results replace original table data in-place
2. Clearing search input restores original table state
3. If table is already filtered, search scope = filtered subset
4. If user searches first, filters apply to search results (and vice versa for sorting)
5. Live search: results generate as you type
6. Results include auto-completed matches

### No Results State
- Table body shows "No results" centered text
- Column headers remain visible

### Result Count
- "105 results" displayed near search field

---

## Filter & Sort

### Two Filter Plans

**Plan A: Sidebar Filter Panel**
- List all filterable items in a sidebar/panel
- Elements: filter+sort, date range picker, multi-user select, single-user select

**Plan B: Inline Filter Bar**
- Filter items appear after clicking filter button
- Shows: Project dropdown, Date range picker, Reset button, Search button
- Filter bar appears above the table

### Filter Operators

| Operator | Description |
|----------|-------------|
| Is | Exact match |
| Is not | Exclude exact match |
| Contains | Substring match |
| Does not contain | Exclude substring |
| Starts with | Prefix match |
| Ends with | Suffix match |

### Column Title Click
- Clicking column title opens filter/sort dropdown
- Filter icon appears in header when filter is active
- When both filtering and sorting available, dropdown shows both

### Combined Filter + Sort Menu
- Sort ascending (`Cmd+C`)
- Sort descending (`Cmd+D`)
- Clear filter
- Clear sort

### Active Filter Display
- Active filters shown as removable tags/pills below search bar
- Format: `Date: 10/02/2026 - 25/02/2026` | `Status` | `Reset`
- Clicking a filter tag opens edit popover for that filter
- "Reset" clears all active filters

### Date Range Filter
- Opens date picker with range selection
- Input format: `MM/DD/YYYY - MM/DD/YYYY`
- Includes time picker: `04 : 08 AM`
- Quick options: "Today", "Last selection"
- "Back" button to return from date selection

---

## Add Data Modal

### Critical Behavior (differs from search popup)

**BUILD RULE — Close vs Cancel distinction:**
- **Close button (X):** auto-saves entered data as DRAFT. Draft reappears when user clicks Add data again.
- **Cancel button:** discards all entered data. No save.
- **Cannot be closed by clicking outside** — only via close button or cancel.

This is the opposite of most modal patterns. The close button SAVES, cancel DISCARDS.

### Default Values
- Date field auto-populated with creation date

### Form Fields
- First Name, Last Name, Email, Date (auto-populated)
- Required fields marked with asterisk (*)
- Validation: same onBlur pattern as all forms

### Actions
- Cancel: discards data
- Save: persists the record, triggers success notification ("Done")

### File Upload
- Hover state on upload area
- Click opens OS file picker

---

## Search (Global)

### Activation
- Click header search or press `Cmd+/` or `/`
- Close: `ESC` or click outside

### Default Popup Structure

| Section | Content |
|---------|---------|
| **Recent search** | Previously searched terms |
| **Recently visited** | Pages user has visited |
| **Contacts** | People (avatars + names) |

### Interaction Rules
1. First result selected by default
2. `Enter` opens selected result in current page
3. `Cmd+Click` or `Cmd+Enter` opens in new browser tab
4. Live search: results generate as you type, includes auto-complete
5. Clear: removes typed text, returns to initial state

### Result Types
- **User** — People/contacts
- **Page** — Application pages/screens
- **Text on the page** — Content matches within pages
- **Data in the table** — Matches in table data

### No Results
- Shows "No results" in popup body

---

## Quick Action Menu

### When to Show Search
- More than 10 menu items
- More than 5 menu items AND a secondary menu exists

### Standard Menu Items

| Item | Shortcut | Notes |
|------|----------|-------|
| Ask AI | — | |
| Tags | — | |
| Multi-Select | — | |
| Edit Property | — | |
| Sort ascending | `Cmd+C` | |
| Sort descending | `Cmd+D` | |
| Filter | — | Opens second-level menu |
| Hide in View | — | |
| Wrap Column | — | |
| Delete Property | — | Destructive — requires confirmation |

### Destructive Actions
- Require confirmation: "Confirm Deletion?"

### Search States
- No input: shows all items
- No results: shows "No results" message
- Has results: filtered list, first result selected by default

### Second-Level Menu
- Clicking Filter opens second-level with operators: Is, Is not, Contains, Does not contain, Starts with, Ends with

### Group Selection
- Items organized into groups (Group 1, Group 2, etc.)
- **Clicking group title selects ALL content in group** (entire bar is clickable)
- Partially selected state: click to select all, click again to deselect all
- Arrow buttons expand/collapse groups
- Live filtering: as you type, list filters to matching results

---

## Notification

- **Trigger:** any write operation to the database
- **Animation:** slides up from bottom, stays 3s, auto-dismisses
- **Position:** bottom of viewport
- **Spec:** h=40, radius 16, pad 8/12, gap 8
- **Content types:**
  - "Done" — simple success
  - "Deleted    Undo" — destructive with undo action
  - "Your file is being created and will start downloading shortly." — async feedback

### Cross-Feature Usage
Notifications appear after: form save, table delete (with undo), file download, any DB write.

---

## Tooltip

### Timing Rules
| Scenario | Delay |
|----------|-------|
| First hover on any element | 1.5s |
| Consecutive hover on same-type element | 0ms (immediate) |
| Return to same element within 30s | 0.5s |

### Position
- Above target by default
- Must not block the target or important info
- Must not be blocked by other elements
- Always at highest z-index

### Content Types
- **Plain text:** single line tooltip
- **Rich:** 14px bold title + 12px description
- **Truncated text reveal:** full text on hover over ellipsized content
- **Button tips:** label + keyboard shortcut (e.g., "Favorites `Cmd+F`")
- **Data tooltips:** chart values, stat comparisons ("Compared with yesterday, up 11.01%")

---

## Date Picker

### Core Rule
**No confirm button.** Closing the picker = confirming the selection.

### Date Selection
- Default: current date selected (Today indicator)
- Click month label → enter month selection interface
- Click year → enter year selection
- Year pagination arrows for navigation

### Input Activation (Breadcrumb Pattern)
- Click different segments in top input (month, day, year, time) to activate that editing mode
- Similar to Windows folder navigation breadcrumbs
- Click outside input exits editing, preserves valid content
- **Small targets:** scale component to 1.5x or 2x

### Time Selection
- Time picker appears beside/below date grid
- Hours, minutes, seconds: selectable or inputable
- **Auto-correction:** entering "13" → auto-switch to PM, display "01"

### Date Range
- First click = start date
- Second click = end date
- Activation state (start vs end) switches automatically
- Time picker follows the active date

---

## Header

**Header is always fixed at the top of the page.**

### Elements (left to right)

| Element | Interaction |
|---------|-------------|
| Sidebar toggle | Collapses/expands sidebar |
| Breadcrumb | Shows current path (e.g., "Dashboards / Overview") |
| Search | Opens global search popup |
| Light/Dark mode | Toggles theme |
| Favorites | Adds current page to Sidebar Favorites |
| Activities | Opens in Rightbar |
| Notifications | Opens in Rightbar |
| Rightbar toggle | Collapses/expands rightbar |

---

## Sidebar

### Width
- Min: 212px
- Max: 40% of page width
- Can collapse to icon-only strip. State persists across sessions.

### Sections (top to bottom)
Profile avatar → Tab group (All, Favorite, Recent) → Nav groups → Logo

### Active Item
- Fill `Black/4%`, radius 12

### Tab States
- Active: text=`Black/40%`
- Inactive: text=`Black/20%`

### Drag-to-Sort
- Items reorderable via drag within their category
- Cannot drag items between categories

### Favorites
- Click star button to add page to favorites
- Click again to remove
- When favorited section is selected, click again to collapse

### Recently
- Shows recently visited pages
- Maximum: **10 items**

### Directory Behavior — Complex Type
- Directory itself has no selected state (only children can be selected)
- **Multiple directories can be open simultaneously**
- Must be collapsed manually by user
- Supports unlimited nesting levels (1st → 2nd → ... → Last)

### Directory Behavior — Simple Type (Accordion)
- Directory has no selected state
- **Only ONE directory can be expanded at a time**
- Directory auto-closes when selecting another menu item
- After folding, only one level selectable

### Sidebar Item Types
- Single page — direct navigation link
- Directory — expandable/collapsible group

---

## Rightbar

### Width
- Min: 212px
- Max: 40% of page width

### Action Buttons
- Hidden by default, revealed ONLY on hover over content block

### Title Sections
- Click section title to collapse that section's content

### User Contacts
Possible interactions when clicking a user:
1. Open the chat interface
2. Open the associated chat software
3. Go to the user page

### Action Menu (header)
- New conversation
- Delete all conversation
- Mark all as read
- Archive all
- Settings

### Loading More
- "Load more" button at bottom
- "See all activities" link

---

## Block / Widget

### Sizing
- Blocks are resizable, **snapped to grid**
- Block height stays consistent within the same column
- Spacing between blocks: constant
- Block width: adaptive to page width

### Action Buttons
- Depend on block content
- Common: Visit original page, Download Excel.csv
- Hidden by default, revealed on hover
- "Select all" checkbox hidden until cursor enters content block area

### Actions
- Open page → opens in new browser tab
- Download → triggers Global Notification ("Your file is being created...") + download starts

### Reorder
- Drag handle appears on hover, drag to reposition blocks

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Cmd+/` or `/` | Open global search |
| `ESC` | Close search popup |
| `Enter` | Select highlighted result |
| `Cmd+Click` | Open in new tab |
| `Cmd+C` | Sort ascending (table/quick action context) |
| `Cmd+D` | Sort descending (table/quick action context) |
| `Cmd+F` | Favorites |
| `Ctrl+Click` | Multi-select (Windows) |

---

## Layout Modes

| Mode | Panels | Specs |
|------|--------|-------|
| Three columns | Sidebar + Content + Rightbar | Full layout |
| Two columns | Content + one panel | 80px gap between panels |
| One column | Content only | max-width 1200px, centered |

Panel widths: Sidebar and Rightbar both 212px min, 40% max.

Dynamic scaling: `rem`/`em` units, not `px`.

---

## Component States (Summary)

8 universal states: Default, Hover, Focus, Active, Disabled, Error, In Progress, Done.

- **Disabled:** Always opacity 0.4 on the entire container
- **Focus ring:** `rgba(0,0,0,0.04)` 0,0 blur-0 spread-4
- **Error:** `Secondary/Red` stroke + error text below field (pushes content down)
- **Hover-reveal:** Table actions, card actions, select-all checkbox — hidden until hover

Not all 8 states have explicit Figma designs. Developers should implement all states programmatically even if only Default, Hover, Focus, and Disabled have visual designs.

See `references/component-states.md` for the full 18-component matrix.
