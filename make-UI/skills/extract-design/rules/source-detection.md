# Source Detection Rules

Classify the user's input to select the correct extraction pipeline.

---

## Detection Logic

```
Input matches Figma URL pattern?
  → "figma.com/design/" or "figma.com/file/" → Pipeline B (Web MCP)
  → "figma.com/proto/" → Pipeline B (prototypes still have design data)

User says "current file" / "open file" / "what I have open"?
  → Check if figma-console MCP tools are available
  → If yes → Pipeline A (highest fidelity)
  → If no → Ask user to open file, or provide URL for Pipeline B

Input is a file path ending in .png, .jpg, .jpeg, .webp, .svg, .pdf?
  → Pipeline C (Screenshot/Image)

Input is a GitHub URL or local directory path?
  → Check for token files (see below) → Pipeline D

Input is an HTTP(S) URL (not Figma, not GitHub)?
  → Pipeline E (Documentation)

User says "more SnowUI pages"?
  → Pipeline A + pre-load existing SnowUI token table for cross-referencing
```

---

## Pipeline Selection Confirmation

After detecting the source, confirm with the user:

```
Detected: [source type]
Pipeline: [A/B/C/D/E] — [fidelity level]
Scope: [full system / page / component / tokens]

Proceed? (or adjust scope)
```

---

## Source-Specific Setup

### Pipeline A: figma-console
- Verify `figma_get_status` returns connected
- Call `figma_search_components` at session start (IDs are session-specific)
- Call `figma_list_open_files` to confirm which file is active

### Pipeline B: Figma Web MCP
- Extract file key from URL: `figma.com/design/[FILE_KEY]/...`
- Pass file key to `get_metadata` first

### Pipeline C: Screenshot
- Verify file exists and is readable
- Check if an existing design system reference is loaded (for token matching)
- If no reference: output will be approximate-only

### Pipeline D: GitHub/Code
- Look for these directories/files:
  - `tokens/`, `design-tokens/`, `variables/`
  - `tailwind.config.js`, `tailwind.config.ts`
  - `theme.ts`, `theme.js`, `theme.json`
  - `*.tokens.json` (Style Dictionary format)
  - `src/styles/`, `src/theme/`, `styles/`
  - `package.json` → check for `@tokens-studio/*`, `style-dictionary`, `theo`

### Pipeline E: Documentation
- Fetch the URL
- Look for: token tables, color swatches, component specs, API docs
- Check for structured data (JSON-LD, OpenAPI) in the page

---

## Incremental vs Full Extraction

If make-UI reference files already exist (check `skills/make-UI/references/`):
- Default to **incremental** mode (merge into existing)
- Ask: "I see existing references. Should I add to them (incremental) or start fresh?"

If no reference files exist:
- Default to **full system** extraction
