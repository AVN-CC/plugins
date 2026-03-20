# Design Corpus Access Guide

The SnowUI design system data lives in a Git LFS repository. This guide explains how to access it at each level of detail.

## Repository

- **URL**: https://github.com/AVN-CC/Design
- **Structure**: `docs/patterns/*.md` (40 pattern docs) + `docs/patterns/raw/*.json` (56 raw JSONs via LFS)
- **Total size**: ~496MB (mostly raw JSONs; pattern docs are ~1MB total)

## Access Priority (progressive disclosure)

### Level 1: Reference Indexes (always loaded)
**Location**: `references/` directory in this skill
**Size**: ~30KB total (6 files)
**When to use**: Always. These are loaded with the skill and contain distilled component specs.

Files:
- `primitives-index.md` — 6 primitives with variant axes, layout specs, key tokens
- `components-index.md` — 12 components with variant axes, slot maps, sizing
- `blocks-index.md` — 10 page-level blocks with layout specs
- `tokens.md` — Complete 131-variable token system across 20 collections, all modal values
- `composition-rules.md` — 10 rules with verbatim designer quotes
- `interaction-patterns.md` — 14 interaction categories with timing/behavior specs

### Level 2: Pattern Docs (read on demand)
**Location**: `docs/patterns/*.md` in the Design repo (AVN-CC/Design)
**Size**: ~21K lines across 40 files
**When to use**: When reference indexes don't have enough detail for a specific component.

How to access:
1. **Local clone exists**: Read directly from the cloned repo path
2. **GitHub MCP available**: Fetch from `AVN-CC/Design` repo via MCP
3. **Neither**: Use reference indexes only (sufficient for most prototypes)

Key pattern docs for common tasks:
| Task | Read These |
|------|-----------|
| Dashboard | `dashboard-layout.md`, `card.md`, `chart.md`, `table.md` |
| Auth flow | `authentication-flows.md`, `input-form.md`, `button.md` |
| Settings | `settings-flows.md`, `input-form.md`, `tab-segmented.md` |
| Any page | `design-system-foundations.md`, `interactive-guidance.md` |

### Level 3: Raw JSON (pixel-perfect only)
**Location**: `docs/patterns/raw/*-v5-raw.json` in the Design repo (Git LFS)
**Size**: ~496MB across 56 files
**When to use**: Only when you need exact values not in pattern docs:
- Precise fill colors or gradients
- Exact constraint values (MIN, MAX, STRETCH, etc.)
- Full component tree structure for complex compositions
- SVG paths for icons
- `boundVariables` references for token resolution

**Requires**: `git lfs pull` after cloning the repo. Without LFS, these files are small pointer stubs.

## SVG Icon Access

Icons are stored as flat JSON objects in the Design repo:

| File | Contents |
|------|----------|
| `svgs-phosphor-icons.json` | Phosphor icon set — `icons["ShoppingBagOpen"]` returns SVG string |
| `svgs-icons.json` | SnowUI custom icons |
| `svgs-logos.json` | Brand logos (Google, Apple, Microsoft, etc.) |
| `svgs-chart-graphics.json` | Chart decorative elements |
| `svgs-chart-shapes.json` | Chart shape primitives (bars, dots, lines) |
| `svgs-emoji.json` | Emoji set |
| `svgs-illustrations.json` | Illustration set |
| `svgs-cursors.json` | Custom cursor set |

**Retrieval pattern**: Read the JSON file, access `icons["IconName"]` or `items["ItemName"]` to get the SVG string. Embed inline in HTML.

## Token Resolution

### In HTML
Use CSS custom properties at the `:root` level:
```css
:root {
  /* Colors — SnowUI-Light mode */
  --bg-1: #ffffff;
  --bg-2: #f9f9fa;
  --bg-4: #edeefc;
  --bg-5: #e6f1fd;
  --black-100: #000000;
  --black-40: rgba(0,0,0,0.4);
  --black-10: rgba(0,0,0,0.1);
  --black-4: rgba(0,0,0,0.04);

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 12px;
  --space-lg: 16px;
  --space-xl: 20px;
  --space-2xl: 24px;
  --space-3xl: 28px;
  --space-4xl: 40px;
  --space-5xl: 48px;

  /* Typography */
  --font-family: 'Inter', -apple-system, sans-serif;
  --font-regular: 400;
  --font-semibold: 600;

  /* Corner radius */
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-xl: 20px;
  --radius-2xl: 24px;
  --radius-pill: 80px;
}

/* Dark mode token swap */
[data-theme="dark"] {
  --bg-1: #000000;
  --bg-2: #1a1a1a;
  --black-100: #ffffff;
  --black-40: rgba(255,255,255,0.4);
  --black-10: rgba(255,255,255,0.1);
  --black-4: rgba(255,255,255,0.04);
}
```

### In React
Import tokens as a CSS file or use a tokens module:
```tsx
// tokens.css imported globally
// Components use var(--bg-1), var(--black-100), etc.
```

The full token system (131 variables, 20 collections, 4 color modes) is documented in `references/tokens.md`.

## Checking if Design Repo is Available

At the start of a session:
1. Check if a local clone exists (common parent directory or user's home)
2. Check if GitHub MCP is available
3. Fall back to reference indexes only

The skill works at all three levels — reference indexes alone are sufficient for most prototypes. Pattern docs and raw JSON provide progressively deeper fidelity.
