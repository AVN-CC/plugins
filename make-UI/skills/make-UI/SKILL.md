---
name: make-UI
description: Build production-grade, multi-screen UI prototypes and functional product experiences using the SnowUI atomic design system. Triggers on "make UI", "build a UI", "prototype", "build a dashboard", "create screens", "build a CRM", "make a page", "auth flow", "settings page", or requests for functional demos with multiple screens and navigation. NOT for generic "design" questions — this skill BUILDS working UI.
---

# SnowUI make-UI — Functional Product Experience Builder

You build production-grade, multi-screen functional product experiences using the SnowUI atomic design system. Not mockups. Not wireframes. Working HTML prototypes that can become React apps. SnowUI is an LLM-forward atomic design system — fewer parts, more breadth, composition over configuration.

## Workflow Overview

```
Understand → Scope → Load → Compose → Copy → Build → QA → Iterate → Save
```

---

## Step 1: Understand the Request

Accept input in three modes:

### Mode A: Context Dump
The user provides what they need. Parse for:
- **Domain** (CRM, eCommerce, healthcare, PM, analytics, etc.)
- **Scope** (single screen, multi-screen flow, full product experience)
- **Page types** (dashboard, list/table, detail, form/settings, auth, chat)
- **Content sections** (KPIs, charts, tables, activity feeds, forms, etc.)
- **Audience** (ops team, end customers, executives, clinical staff)
- **Mode** (light, dark, both)

If the context is sufficient, proceed directly.

### Mode B: Brainstorm
If vague, ask only what you need from this bank:
1. What domain/vertical?
2. What screens or flows? (auth, dashboard, settings, detail, etc.)
3. Layout? (3-col dashboard, 2-col, single, mobile)
4. Who uses this? (data-heavy ops vs clean consumer vs executive vs clinical)
5. Light/dark/both?

Offer: "I can build a v1 that's directionally correct — want to see something fast?"

### Mode C: "Trust Me"
Pick smart defaults and build. Show it. Iterate from there.

---

## Step 2: Scope the Experience

**This is critical.** Determine whether this is a single screen or a multi-screen product experience.

### Single Screen
One page: dashboard, settings panel, detail view. Skip to Step 3.

### Multi-Screen Flow
Read `rules/flows.md` for flow patterns. Then:

1. **Screen inventory** — List every screen with:
   - Name and purpose
   - Key components used
   - Navigation in/out (what links here, where does this go)
   - Which components are shared vs unique

2. **Navigation map** — Draw the flow:
   ```
   Login → [Forgot Password] → Reset → Verify → Dashboard
                                                    ├── Settings
                                                    ├── Detail View
                                                    └── Reports
   ```

3. **Shared components** — Identify what's reused across screens:
   - Layout shell (sidebar + header + content area)
   - Navigation (sidebar items, header actions)
   - Common patterns (modals, toasts, form layouts)

4. **Build order** — Shared shell first, then individual screens.

Present the screen inventory to the user for confirmation before building.

---

## Step 3: Load Design Knowledge

Read these reference files from this skill's `references/` directory:
1. `primitives-index.md` — atomic primitive catalog (Text, Icon, Frame, Group, Line)
2. `components-index.md` — component catalog with variant axes and slot maps
3. `blocks-index.md` — page-level composition patterns
4. `tokens.md` — complete token system (131 variables, all modes)
5. `composition-rules.md` — 10 atomic composition rules with designer quotes
6. `interaction-patterns.md` — state management and transitions

**For deeper specs**: Read the full pattern doc from the Design corpus. See `assets.md` for access instructions.

**If MCPs available**: Check `mcp.md` for enhanced capabilities (Figma live data, external data sources, etc.).

---

## Step 4: Compose

Read `rules/compose.md` for the full atomic hierarchy and composition patterns.

Assemble from SnowUI's atomic hierarchy:
```
Primitives (Text, Frame, Group, Icon, Line)
  → Components (Card, Button, Input, Search, Tag, Badge, etc.)
    → Blocks (Sidebar, Header, Content Grid, Chart Block, Table, Right Bar)
      → Screens
        → Product Experience (multi-screen with navigation)
```

**Key composition principles:**
- **Opacity is hierarchy:** Primary=1.0, Secondary=0.4, Tertiary=0.2, Background=0.04
- **Text is a component:** Use Text(Count=N, Vertical=true/false), not inline strings
- **Slots are swappable:** Card, Quick Action, Button all use INSTANCE_SWAP
- **4px grid:** Every dimension is a multiple of 4
- **Dark mode = token swap:** Same layout, same spacing, only token values change

---

## Step 5: Generate Copy

Read `rules/copy.md` for copywriting rules. All text must be:
- Domain-appropriate (CRM → contacts/deals/pipeline; healthcare → patients/encounters/vitals)
- Realistic values with proper formatting ($, %, commas)
- 5-8 rows for tables with diverse, realistic sample data
- No lorem ipsum. No "John Doe" / "Jane Smith".
- Varied actors, timestamps, action descriptions

---

## Step 6: Build

### HTML (always first)
Generate HTML files to `src/previews/`:
- **Single screen**: `src/previews/<name>.html`
- **Multi-screen**: `src/previews/<name>/index.html` + `src/previews/<name>/<screen>.html`
  - Or single HTML with show/hide sections + hash routing for fast iteration

Technical requirements:
- Self-contained: Tailwind CDN + Google Fonts Inter + inline SVGs
- CSS custom properties matching SnowUI tokens (see `rules/compose.md` token reference)
- Dark mode toggle via `[data-theme="dark"]` if both modes requested
- Accurate dimensions, spacing, typography, colors from extraction specs
- Multi-screen: working navigation between all screens

**Serve via preview server** and verify.

### React (on user request, after HTML approval)
Before building React, clarify specs not obvious from context:
- **Responsive:** "Need breakpoints? (375px → 768px → 1440px)" — expensive to retrofit
- **Animation depth:** "Light (hover states) or heavy (page transitions, skeletons)?"
- **State management:** "Local state or shared (Context/Zustand)?"
- **Routing:** "React Router for multi-screen?"

React components are built directly from SnowUI extraction specs — NOT wrapping shadcn. SnowUI IS the component system. Use CSS custom properties for tokens.

---

## Step 7: QA

Read `rules/qa.md` for the full programmatic QA checklist. **Never browser screenshots.**

Use MCP preview tools:
- `preview_inspect` — computed styles vs spec
- `preview_snapshot` — structural completeness
- `preview_console_logs` — runtime errors
- `preview_eval` — WCAG contrast, typography verification

For multi-screen: QA each screen individually, plus navigation between screens.

---

## Step 8: Iterate

Fix issues from QA. Re-verify. Present to user. Incorporate feedback. Repeat.

For multi-screen flows: iterate on individual screens AND the overall flow experience.

---

## Step 9: Save (on request)

When the user likes a design:
1. Keep HTML/React output in place
2. Create composition log at `designs/<name>/composition-log.md`:
   ```markdown
   ## [Design Name]
   Date: [date]
   Domain: [domain]
   Screens: [count and names]

   ### Components Used
   - [list of primitives, components, blocks per screen]

   ### Screen Map
   - [navigation flow between screens]

   ### Iteration Log
   - v1: [what was built]
   - User feedback: [what changed]
   - v2: [what was adjusted]
   ```
3. Add entry to `designs/catalog.md`

---

## SVG Icon Retrieval

Icons stored as flat JSON in the Design corpus:
- `docs/patterns/raw/svgs-phosphor-icons.json` → `icons["ShoppingBagOpen"]`
- `docs/patterns/raw/svgs-chart-shapes.json` → chart SVGs by name
- `docs/patterns/raw/svgs-logos.json` → logo SVGs by name

See `assets.md` for access instructions.

---

## Integration with Other Skills

During build, leverage existing skills when available:
- **`frontend-design`** — creative polish on React builds
- **`design-critique`** (in `.agents/skills/`) — self-review gate before declaring done
- **`design-accessibility-review`** (in `.agents/skills/`) — WCAG verification during QA
