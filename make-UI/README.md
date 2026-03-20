# make-UI Plugin

A UI prototyping and component development plugin for [Cowork](https://claude.com/product/cowork) and [Claude Code](https://claude.com/product/claude-code). Builds production-grade, multi-screen product experiences using the SnowUI atomic design system. Designed for digital health clinics on Canvas Medical, but works for any product UI.

## Installation

### Cowork / Claude Desktop

Install from the Customize panel, or:

```bash
claude plugins add AVN-CC/Clinic-Front-Door/.agents/plugins/make-UI
```

### Claude Code

```bash
claude plugin install make-UI
```

## Commands

Explicit workflows you invoke with a slash command:

| Command | Description |
|---|---|
| `/make-UI` | Build a multi-screen UI prototype — dashboards, auth flows, settings, admin portals, enrollment flows |
| `/accessibility` | Run a WCAG 2.1 AA accessibility audit on a design or component |
| `/code-review` | Review code for security, performance, and correctness |
| `/testing` | Generate Playwright tests for a component or page |

All commands work **standalone** and get **supercharged** with MCP connectors (Figma, GitHub, data sources).

## Skills

Domain knowledge Claude uses automatically when relevant:

| Skill | Triggers on | What it does |
|---|---|---|
| **make-UI** | "build a UI", "prototype", "dashboard", "auth flow", "settings page" | Full SnowUI prototype builder — screen inventory, composition, copy, QA, multi-screen flows |
| **shadcn** | Component code generation, installing shadcn/ui blocks | shadcn/ui v4 rules for @base-ui/react, Tailwind v4, oklch theming |
| **accessibility** | Accessibility review, WCAG audit, contrast check | WCAG 2.1 AA hard gate — contrast, keyboard nav, focus states, screen readers, touch targets |
| **code-review** | Code review, security check, performance review | Security (XSS), performance (re-renders), correctness (typed props, edge cases) |
| **testing** | Test generation, Playwright, component testing | Playwright tests — interactive states, keyboard nav, dark mode, mobile, edge cases |

## Two Build Paths

Both paths produce **@base-ui/react** components — never Radix.

### Path 1: make-UI (SnowUI prototypes)

For building functional product experiences — dashboards, auth flows, enrollment, scheduling, admin portals. Uses the SnowUI atomic design system extracted from Figma.

```
User input → screen inventory → compose layout → write copy → build HTML → QA → iterate
```

Output: Interactive HTML prototypes (always). React components (on request).

### Path 2: shadcn (component assembly)

For building or modifying individual React components using shadcn/ui v4. Load its rules before writing any component code.

```
Load rules → search registry → install/adapt → code with @base-ui
```

Output: React components using shadcn/ui + @base-ui primitives.

## Quality Gates

After building with either path, every component passes through:

1. **Code review** — security, performance, correctness
2. **Accessibility** — WCAG 2.1 AA (hard gate — nothing ships without passing)
3. **Testing** — Playwright test generation

## Connectors

See [CONNECTORS.md](./CONNECTORS.md) for available tool integrations. Key connectors:

- **Figma** — Design context, screenshots, component specs
- **GitHub** — Access Design corpus (AVN-CC/Design) pattern docs
- **figma-console** (optional, local) — Live Figma inspection with 59+ tools

## Design Corpus

The SnowUI design system data lives in [AVN-CC/Design](https://github.com/AVN-CC/Design):

- **40 pattern docs** — human-readable component specs (`docs/patterns/*.md`)
- **56 raw JSONs** — pixel-level Figma extractions via Git LFS (`docs/patterns/raw/`)
- **6 reference indexes** — distilled summaries bundled in `skills/make-UI/references/`

## Customizing

- **Swap connectors** — Edit `.mcp.json` to point at your tool stack
- **Add domain context** — Modify skill files with your terminology, workflows, and design standards
- **Extend the design system** — Add pattern docs to `skills/make-UI/references/` or clone AVN-CC/Design
