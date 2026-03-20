# AVN-CC Plugin Marketplace

Plugins for [Claude Cowork](https://claude.com/product/cowork) and [Claude Code](https://claude.com/product/claude-code) built by AVN Creative Consulting.

## Plugins

| Plugin | Description |
|--------|-------------|
| **[make-UI](./make-UI)** | Build production-grade, multi-screen UI prototypes. Two build paths (SnowUI + shadcn/ui), quality gates (accessibility, code review, testing), and MCP connectors (Figma, GitHub). |

## Installation

### Cowork / Claude Desktop

1. Open **Customize** → click **+** next to Personal plugins
2. Enter: `AVN-CC/plugins`
3. Click **Sync**
4. Install make-UI from the plugin list

### Claude Code

```bash
claude plugin marketplace add AVN-CC/plugins
claude plugin install make-UI@avn-cc-plugins
```

## Adding New Plugins

1. Create a new directory at the root (e.g., `my-plugin/`)
2. Follow the [plugin structure](https://github.com/anthropics/knowledge-work-plugins#how-plugins-work)
3. Register it in `.claude-plugin/marketplace.json`
4. Push to main
