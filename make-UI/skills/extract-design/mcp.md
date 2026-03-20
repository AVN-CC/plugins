# MCP Tool Catalog for Design Extraction

This skill uses different MCP tools depending on the source type. Check available tools at session start and select the highest-fidelity pipeline.

---

## Tool Selection Matrix

| Task | figma-console (desktop) | Figma Web MCP (URL) | No MCP |
|------|------------------------|---------------------|--------|
| Token extraction | `figma_execute` script 1 | `get_variable_defs` | Parse from code/docs |
| BoundVariable map | `figma_execute` script 2 | Infer from `get_design_context` | Not available |
| Component properties | `figma_execute` script 3 | `get_design_context` per component | Parse from docs/code |
| Style catalog | `figma_execute` script 4 | Partial from `get_variable_defs` | Parse from CSS |
| Dark mode diff | `figma_execute` script 5 | Compare two `get_screenshot` outputs | Visual comparison |
| State matrix | `figma_execute` script 6 | Infer from screenshots | Manual from docs |
| Visual reference | `figma_take_screenshot` | `get_screenshot` | Read image file |
| Component search | `figma_search_components` | N/A | N/A |
| System overview | `figma_get_design_system_summary` | `get_metadata` | N/A |
| Navigate to page | `figma_navigate` | N/A | N/A |

---

## Pipeline A Tools: figma-console (Desktop App)

Highest fidelity. File must be open in Figma desktop.

### Overview & Navigation

| Tool | Purpose |
|------|---------|
| `figma_get_design_system_summary` | Collection counts, component counts, style counts |
| `figma_get_design_system_kit` | Bulk dump: tokens + components + styles |
| `figma_get_variables` | All variables with mode values |
| `figma_get_styles` | Text styles, color styles, effect styles |
| `figma_navigate` | Jump to specific page/frame |
| `figma_list_open_files` | Check what files are open |

### Extraction via figma_execute

The 6 scripts in `references/figma-execute-scripts.md` run via `figma_execute`. This is the v6 superpower — automates what was manual in v5.

| Script | Tool Call |
|--------|----------|
| extract-all-variables | `figma_execute` with script 1 code |
| extract-bound-variables-per-page | `figma_execute` with script 2 code |
| extract-component-properties | `figma_execute` with script 3 code |
| extract-styles-catalog | `figma_execute` with script 4 code |
| diff-light-dark-frames | `figma_execute` with script 5 code |
| extract-state-variants | `figma_execute` with script 6 code |

### Component Deep-Dive

| Tool | Purpose |
|------|---------|
| `figma_search_components` | Find components by name (call at session start — IDs are session-specific) |
| `figma_get_component` | Basic component info |
| `figma_get_component_details` | Full details: variant axes, slots, constraints |
| `figma_get_component_for_development` | Dev-ready specs |

### Visual Capture

| Tool | Purpose |
|------|---------|
| `figma_take_screenshot` | Capture current view for behavioral analysis |
| `figma_capture_screenshot` | Alternative capture method |

### Specialized

| Tool | Purpose |
|------|---------|
| `figma_get_token_values` | Resolve specific token to CSS value |
| `figma_browse_tokens` | Interactive token browser |
| `figma_audit_design_system` | Run design system audit |
| `figma_lint_design` | Check for design issues |

---

## Pipeline B Tools: Figma Web MCP (URL only)

Works without desktop app. Lower fidelity — no `figma_execute`.

| Tool | Purpose |
|------|---------|
| `get_metadata` | File structure, pages, component counts |
| `get_variable_defs` | Variable definitions and mode values |
| `get_design_context` | Component tree, properties for a frame |
| `get_screenshot` | Visual capture |

**Limitation**: Cannot run custom scripts. Must infer variable bindings from color values matched against known token tables.

---

## Pipeline C-E Tools: Non-Figma Sources

### Screenshots/Images
- Use Claude's native multimodal capability (Read tool on image files)
- No MCP needed

### GitHub Repos
| Tool | Purpose |
|------|---------|
| GitHub MCP tools | Read files, search code |
| `Bash` (git clone) | Local repo access |
| `Glob`/`Grep` | Find token files, component definitions |

### Documentation URLs
| Tool | Purpose |
|------|---------|
| `WebFetch` | Pull documentation pages |
| `WebSearch` | Find design system docs |

---

## MCP Detection at Session Start

```
1. Check system reminder for available MCP tools
2. If figma-console tools present → Pipeline A available
3. If Figma Web MCP present → Pipeline B available
4. If neither → Pipelines C/D/E only
5. Report available pipelines to user
```

The skill must always work without any MCPs — they enhance, never gate. Screenshots and documentation can always be processed.
