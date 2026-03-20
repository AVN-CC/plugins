---
name: extract-design
description: Extract design system tokens, components, states, interactions, and behavioral rules from Figma files, screenshots, GitHub repos, or documentation URLs. Produces normalized reference files compatible with the make-UI skill. Triggers on "extract design", "extract tokens", "pull design system", "ingest design", "extract from Figma", "design extraction", "add new components", "consume this design", or requests to add new pages/patterns to the make-UI references.
---

# Extract Design — Design-to-Data Pipeline

You extract design system knowledge from any source and produce structured reference files that the make-UI skill consumes. This turns multi-session manual extraction into a repeatable pipeline.

## Workflow Overview

```
Detect Source → Classify Scope → Extract → Normalize → Validate → Merge → Verify
```

---

## Step 1: Detect Source

Ask the user what they want to extract from. Accept any of these:

| Source | Pipeline | Fidelity |
|--------|----------|----------|
| "Current Figma file" (desktop app open) | A: figma-console | Pixel-perfect |
| Figma URL (`figma.com/design/...`) | B: Figma Web MCP | Structural |
| Screenshot or image path | C: Multimodal | Approximate |
| GitHub repo URL or local path | D: Code parsing | Token-level |
| Documentation URL | E: Web fetch | Reference-level |
| "More SnowUI pages" | A + SnowUI optimizations | Pixel-perfect |

Read `rules/source-detection.md` to classify the input and select the right pipeline.

**Key question to ask:** "What do you want to extract from, and do you have it open in Figma desktop?"

---

## Step 2: Classify Scope

Determine what to extract based on user intent:

| Scope | What's Extracted | When to Use |
|-------|-----------------|-------------|
| **Full system** | Tokens, colors, typography, spacing, effects, components, states, interactions | New design system, first extraction |
| **Page/screen** | Component usage, layout patterns, behavioral rules from a specific page | Incremental — adding a new page |
| **Component** | Variant axes, slots, states, token bindings for one component | Deep-dive on a specific component |
| **Tokens only** | Variables, styles, mode values | Quick token sync |

Present the detected scope to the user for confirmation before extracting.

---

## Step 3: Extract

Run the source-specific extraction pipeline. Read `mcp.md` for tool selection per source type.

### Pipeline A: Figma via figma-console (Highest Fidelity)

This is the primary pipeline. Requires the Figma file to be open in the desktop app.

**Phase 1: System Overview**
1. `figma_get_design_system_summary` — collection counts, component counts, style counts
2. Present overview to user, confirm extraction scope

**Phase 2: Token Extraction**
3. Run `extract-all-variables` script (see `references/figma-execute-scripts.md`)
4. Run `extract-styles-catalog` script — text styles, color styles, effect styles

**Phase 3: Component Extraction**
5. Run `extract-component-properties` script — variant axes, slots, instance swaps
6. Run `extract-state-variants` script — state deltas per component set

**Phase 4: Page-Level Extraction** (per page of interest)
7. Navigate to page: `figma_navigate`
8. Run `extract-bound-variables-per-page` script — all variable bindings with node context
9. `figma_take_screenshot` — visual reference

**Phase 5: Dark Mode Analysis** (if applicable)
10. Run `diff-light-dark-frames` script on light/dark frame pairs
11. Flag asymmetric inversions, hardcoded overrides, structural differences

**Phase 6: Behavioral Analysis**
12. Claude analyzes screenshots + binding data to infer:
    - Interaction patterns (hover-reveal, state transitions)
    - Composition rules (padding tiers, slot patterns)
    - Layout specs (column counts, gap values, responsive behavior)

### Pipeline B: Figma Web MCP (URL only)

Lower fidelity — no `figma_execute` available.

1. `get_metadata` — file structure, pages
2. `get_variable_defs` — variable definitions with mode values
3. `get_design_context` per page/frame — component tree, properties
4. `get_screenshot` — visual capture for Claude analysis
5. Claude infers bindings by matching color values to known token tables

### Pipeline C: Screenshot/Image

1. Read the image (Claude multimodal)
2. Identify: layout grid, color palette, typography scale, component types
3. Map colors to nearest known design system tokens (if a reference exists)
4. Extract approximate spacing values (assume 4px grid)
5. All uncertain values marked `[approximate]`

### Pipeline D: GitHub/Code

1. Scan repo structure for token files (`tokens/`, `theme/`, `variables/`, `tailwind.config.*`)
2. Parse token format (JSON, YAML, CSS custom properties, Style Dictionary, Tailwind)
3. Parse component files for variant definitions, prop types, state handling
4. Map to reference file format

### Pipeline E: Documentation URL

1. `WebFetch` the documentation pages
2. Extract structured content: token tables, component specs, usage guidelines
3. Parse code blocks for token values and component APIs
4. Map to reference file format with source attribution

---

## Step 4: Normalize

Transform raw extraction data into make-UI reference file format.

Read `rules/normalize.md` for transformation rules.
Read `references/output-schemas.md` for target file templates.

Output files produced (as applicable):
- `tokens.md` — token tables with N-mode values
- `color-system-qa.md` — pairing rules, build rules, collision detection
- `component-states.md` — state matrix per component
- `composition-rules.md` — numbered rules with examples
- `interaction-patterns.md` — behavioral rules per category
- `data-formats.md` — display format tables

Write normalized files to `output/` first (working directory), NOT directly to make-UI references.

---

## Step 5: Validate

Run quality checks on extracted data:

1. **Token completeness**: Every token has values for all detected modes
2. **Color validity**: All values are valid hex or rgba
3. **Spacing grid**: All spacing values are multiples of 4 (or the detected grid unit)
4. **State coverage**: Every component has at least Default + one interactive state
5. **Cross-reference**: Token names used in component specs actually exist in token table
6. **Dark mode**: Check for potential collision patterns (text on colored surfaces)

Flag issues for user review. See `evals/evals.json` for assertion templates.

---

## Step 6: Merge

If existing make-UI reference files are present, merge new data incrementally.

Read `rules/merge.md` for the merge strategy.

**Core principle: Never overwrite manually verified data.**

- **Tokens**: Append new tokens to existing tables. Same name + different value = `[CONFLICT]` marker.
- **Color QA**: Append new rules. Existing rules stay untouched.
- **Components**: Add new component sections. Merge new states into existing components.
- **Interactions**: Append new sections. Merge into existing categories if they match.
- **Composition rules**: Append as new numbered rules (increment from last).

Present merge plan to user before writing.

---

## Step 7: Verify

Present extraction summary:

```
Extraction Summary
==================
Source: [source description]
Scope: [full system / page / component / tokens]
Pipeline: [A / B / C / D / E]

Tokens extracted:    [count] ([new] new, [existing] already known)
Components:          [count] ([new] new)
State matrices:      [count]
Interaction rules:   [count]
Composition rules:   [count]
Conflicts:           [count] (require manual resolution)
Approximate values:  [count] (from screenshot/inference)

Files written to output/:
  - tokens.md ([lines] lines)
  - component-states.md ([lines] lines)
  - ...

Ready to merge into make-UI references? (Y/n)
```

After user approval, copy from `output/` to the make-UI `references/` directory.

---

## Integration with Other Skills

| Skill | Relationship |
|-------|-------------|
| **make-UI** | Consumes the reference files this skill produces |
| **code-review** | Can verify implementations match extracted specs |
| **accessibility** | Can audit extracted color pairings for WCAG contrast |

This skill works standalone but enhances with MCPs — same pattern as make-UI. Read `mcp.md` for the full tool catalog.

---

## Important Rules

1. **Two-stage output**: Always write to `output/` first. User reviews before publish to `references/`.
2. **Never overwrite**: Existing reference data was manually verified. Append, never replace.
3. **Flag uncertainty**: Mark approximate values with `[approximate]`, conflicts with `[CONFLICT]`.
4. **Generic first**: Don't assume SnowUI patterns. Detect the design system's conventions first.
5. **Source attribution**: Every extracted rule includes its source (page name, frame ID, screenshot).
