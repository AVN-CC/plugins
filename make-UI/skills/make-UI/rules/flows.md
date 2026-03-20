# Multi-Screen Flow Patterns

When building product experiences with multiple screens, follow these patterns.

## Flow Types

### Auth Flow (3-6 screens)
```
Login → [Forgot Password → Reset Password → Check Email] → Dashboard
  ↕
Register → [Verify Email → Complete Profile] → Dashboard
```
Screens: Login, Register, Forgot Password, Reset Password, Email Verification, Profile Completion
Shared: Auth card layout (480px centered, radius 24), logo, background

### Onboarding Flow (4-6 screens)
```
Welcome → Profile Setup → Preferences → [Plan Selection] → [Team Invite] → Dashboard
```
Screens: Welcome, Profile, Preferences, Plan Selection, Team Invite, Success
Shared: Progress indicator (step N of M), skip/back navigation, consistent card width

### CRUD Flow (4-5 screens)
```
List View → Detail View → Edit Modal/Page → [Confirm Dialog] → Updated List
```
Screens: List (table + filters), Detail (read-only), Edit (form), Confirmation
Shared: Header with breadcrumbs, sidebar nav, action buttons

### Settings Flow (1 shell + N panels)
```
Settings Shell
├── Profile Panel
├── Account Panel
├── Notifications Panel
├── Security Panel
├── Billing Panel
└── Team Panel
```
Screens: Layout shell with sidebar nav + swappable content panels
Shared: Settings sidebar (fixed), save/cancel footer, section headers

### Dashboard Flow (1 complex screen + drill-downs)
```
Dashboard (3-column) → [Chart Detail] → [Table Full View] → [Report Export]
```
Screens: Main dashboard, expanded chart view, full table view, report builder
Shared: Sidebar, header, navigation context (breadcrumbs)

### Admin Portal (full product)
```
Dashboard
├── Users → User List → User Detail → Edit User
├── Content → Content List → Content Editor → Preview
├── Analytics → Overview → Custom Report Builder
└── Settings → [Settings Flow above]
```
This is a combination of multiple flow types. Build the shell first, then each section.

## Screen Inventory Template

Before building ANY multi-screen flow, create this inventory:

```markdown
| # | Screen Name      | Type       | Key Components                | Nav From         | Nav To            | Shared? |
|---|------------------|------------|-------------------------------|------------------|-------------------|---------|
| 1 | Login            | Auth       | Auth Card, Input×2, Button    | —                | Register, Forgot  | Shell   |
| 2 | Register         | Auth       | Auth Card, Input×4, Button    | Login            | Verify Email      | Shell   |
| 3 | Forgot Password  | Auth       | Auth Card, Input×1, Button    | Login            | Check Email       | Shell   |
| 4 | Dashboard        | Dashboard  | KPI×4, Chart×2, Table, RBar   | Login/Register   | Settings, Detail  | Nav     |
| 5 | Settings         | Settings   | Sidebar Nav, Form panels      | Dashboard        | Dashboard         | Nav     |
```

Present this to the user for approval before building.

## Shared Components Strategy

### Build Order
1. **Layout shell** — the outer frame that persists across screens
   - Sidebar navigation (if present)
   - Header bar (if present)
   - Content area wrapper
2. **Shared UI components** — reused across 2+ screens
   - Form layouts, button groups, card patterns
   - Modal/dialog templates
   - Toast/notification containers
3. **Individual screens** — unique content for each screen
   - Fill the content area with screen-specific blocks

### HTML Implementation
For HTML prototypes, two approaches:

**Multi-file** (cleaner, better for large flows):
```
src/previews/my-app/
├── index.html          ← Login (entry point)
├── register.html
├── dashboard.html
├── settings.html
└── shared.css          ← Shared styles + tokens
```
Each file includes the shared shell. Navigation via `<a href="dashboard.html">`.

**Single-file** (faster iteration, better for small flows):
```html
<!-- All screens in one HTML file, show/hide with hash routing -->
<div id="login" class="screen active">...</div>
<div id="register" class="screen">...</div>
<div id="dashboard" class="screen">...</div>
<script>
  window.addEventListener('hashchange', () => {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    const target = document.querySelector(location.hash || '#login');
    if (target) target.classList.add('active');
  });
</script>
```

### React Implementation
```
src/
├── components/
│   ├── Layout.tsx          ← Shared shell (sidebar + header + outlet)
│   ├── Sidebar.tsx
│   ├── Header.tsx
│   └── ui/                 ← SnowUI component implementations
├── pages/
│   ├── Login.tsx
│   ├── Register.tsx
│   ├── Dashboard.tsx
│   └── Settings.tsx
├── router.tsx              ← React Router config
└── tokens.css              ← SnowUI CSS custom properties
```

## State Across Screens

### HTML (minimal state)
- URL hash for current screen
- `localStorage` for persisted choices (theme, form data)
- CSS classes for active states

### React (full state)
- React Router for navigation
- Context or Zustand for shared state (user, theme, sidebar open/closed)
- Form state local to each page unless explicitly shared

## Navigation Patterns

### Sidebar Navigation
- Always visible on desktop (212px)
- Active item: `fill=Black/4%`, text bold
- Section groups with collapsible headers
- Bottom: settings/logout

### Tab Navigation
- Within a page section (e.g., settings tabs)
- SnowUI Tap component: Text + Line underline
- Active tab: Line visible, text Black/100%
- Inactive: no line, text Black/40%

### Breadcrumbs
- Show hierarchy: "Dashboard / Settings / Security"
- Each segment clickable except current
- 14px/400, Black/40% for ancestors, Black/100% for current
