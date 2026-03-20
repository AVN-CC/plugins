# MCP Integration Catalog

This skill works standalone but enhances significantly with MCP connectors. Check what's available and use accordingly.

## Always Available

### Preview MCP (built-in)
Used for programmatic QA (see `rules/qa.md`).

| Tool | Purpose |
|------|---------|
| `preview_start` | Start preview server for HTML prototypes |
| `preview_snapshot` | DOM structure, element presence, text content |
| `preview_inspect` | Computed styles on specific elements |
| `preview_eval` | Run JavaScript (contrast checks, typography audit) |
| `preview_console_logs` | Runtime errors |
| `preview_network` | Failed network requests |
| `preview_screenshot` | Visual screenshot (use sparingly — prefer snapshot/inspect) |
| `preview_click` / `preview_fill` | Test interactions, form fills |
| `preview_resize` | Test responsive / dark mode |

---

## If `~~figma-console` is available

The figma-console MCP provides 59+ tools for live Figma interaction. Key tools for make-UI:

| Tool | How make-UI Uses It |
|------|---------------------|
| `figma_search_components` | Find exact component specs by name (e.g., "Card", "Button") |
| `figma_get_component_details` | Get variant axes, slot properties, constraints |
| `figma_get_variables` | Look up token values across all modes |
| `figma_get_token_values` | Resolve specific tokens to CSS values |
| `figma_take_screenshot` | Capture Figma component for visual reference |
| `figma_get_styles` | Get text styles, color styles, effect styles |
| `figma_get_design_system_summary` | Overview of the full design system |

**Workflow enhancement**: Before composing, search Figma for the exact component to cross-reference pattern docs with live design data. Particularly useful when pattern docs don't have exact values.

**Session note**: Always call `figma_search_components` at session start — nodeIds are session-specific.

---

## If `~~Figma` (Web MCP) is available

The standard Figma MCP (not figma-console) provides:

| Tool | How make-UI Uses It |
|------|---------------------|
| `get_design_context` | Pull design context for a specific file/frame |
| `get_screenshot` | Capture designs for visual reference |
| `get_metadata` | File structure, pages, components |
| `get_variable_defs` | Variable definitions and modes |

**When to use**: When you need a quick visual reference of the original Figma design, or when figma-console isn't available.

---

## If `~~GitHub` is available

Access the Design corpus (AVN-CC/Design) directly without a local clone.

| Capability | How make-UI Uses It |
|-----------|---------------------|
| Read files from repos | Read pattern docs: `docs/patterns/button.md` |
| Read raw JSON (LFS) | Access raw extraction data for pixel-perfect specs |
| Search code | Find specific component definitions across pattern docs |

**Workflow enhancement**: When `assets.md` says "read the full pattern doc", use GitHub MCP to fetch it directly from AVN-CC/Design instead of requiring a local clone.

---

## If `~~Google Drive` is available

| Capability | How make-UI Uses It |
|-----------|---------------------|
| Search files | Find brand assets, logos, existing design docs |
| Read documents | Pull requirements docs, PRDs, design briefs |
| Read spreadsheets | Pull data for realistic table content |

**Workflow enhancement**: When the user references "the PRD" or "the design doc", search Drive for it and pull relevant content for the prototype.

---

## If data source MCPs are available

### HubSpot, Mixpanel, Segment, etc.

| Capability | How make-UI Uses It |
|-----------|---------------------|
| Pull real metrics | Use actual KPI values instead of generated ones |
| Pull real user data | Use anonymized real names, emails, timestamps |
| Pull real events | Activity feeds with real event data |
| Pull real analytics | Chart data from actual usage metrics |

**Workflow enhancement**: Instead of generating fake data (rules/copy.md), pull real data from connected sources. Anonymize PII (replace real emails with user@example.com patterns, use first name + last initial only).

---

## MCP Detection Pattern

At the start of any make-UI session, check available tools:

1. List available MCP tools (the system reminder shows them)
2. Note which categories are available
3. Adjust the workflow:
   - **No MCPs**: Use pattern docs + references only. Generate all copy.
   - **Figma MCPs**: Cross-reference pattern docs with live Figma data
   - **GitHub MCP**: Read full pattern docs on-demand from Design repo
   - **Data MCPs**: Pull real data for realistic prototypes

The skill must always work without any MCPs — they enhance, never gate.
