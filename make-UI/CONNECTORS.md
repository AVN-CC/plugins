# Connectors

## How tool references work

Plugin files use `~~category` as a placeholder for whatever tool the user connects in that category. For example, `~~design tool` might mean Figma or any other design tool with an MCP server.

Plugins are **tool-agnostic** — they describe workflows in terms of categories rather than specific products. The `.mcp.json` pre-configures specific MCP servers, but any MCP server in that category works.

## Connectors for this plugin

| Category | Placeholder | Included servers | Other options |
|----------|-------------|-----------------|---------------|
| Design tool | `~~design tool` | Figma | Sketch, Adobe XD, Framer |
| Source control | `~~source control` | GitHub | GitLab, Bitbucket |
| Calendar | `~~calendar` | Google Calendar | Microsoft 365 |
| Email | `~~email` | Gmail | Microsoft 365 |
| Design corpus | `~~design corpus` | — (local clone or GitHub MCP) | AVN-CC/Design repo via GitHub MCP |
| Live Figma inspection | `~~figma console` | — (figma-console MCP, local) | Requires figma-console MCP server running locally |
| Data sources | `~~data source` | — | HubSpot, Mixpanel, Segment (for realistic prototype data) |

## Optional: figma-console MCP

For live Figma inspection (component search, variable lookup, screenshots), install the figma-console MCP server locally. This is not included in `.mcp.json` because it requires a local process. See the make-UI skill's `mcp.md` for details.

## Optional: Design corpus access

The SnowUI design system data lives in [AVN-CC/Design](https://github.com/AVN-CC/Design). Three access levels:

1. **Reference indexes** (always available) — bundled in `skills/make-UI/references/`
2. **Pattern docs** (on demand) — read from local clone or via `~~source control` (GitHub MCP)
3. **Raw JSON** (pixel-perfect only) — requires `git lfs pull` on a local clone
