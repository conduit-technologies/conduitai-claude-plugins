# conduitAI for Claude Code

> The trusted marketplace for AI tooling — search, browse, and install MCP servers, agents, skills, prompts, and plugins directly from Claude Code.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Plugin: conduitai](https://img.shields.io/badge/plugin-conduitai-14b8a6)](./plugins/conduitai)
[![Catalog: conduitai.app](https://img.shields.io/badge/catalog-conduitai.app-22d3ee)](https://conduitai.app)

## Install

In Claude Code:

```text
/plugin marketplace add conduit-technologies/conduitai-claude-plugins
/plugin install conduitai@conduitai-plugins
/reload-plugins
```

Then run `/mcp`, select **conduitai → Authenticate**, sign in with your conduitAI account in the browser, and the plugin is connected.

## Why conduitAI

The AI tool ecosystem is fragmented: MCP servers, agents, and plugins live across GitHub, npm, blog posts, and Discord. Finding the right tool means hours of searching, and even when you find one you don't know if it's safe, maintained, or production-ready.

**conduitAI** is the curated marketplace that scores, verifies, and indexes them so you can find the right tool, see if it's trustworthy, and install it without manual config hunting.

- **Quality scores** — multi-factor health/maintenance/security signals across every asset
- **Verified publishers** — official + community-vetted
- **One-click install** — auto-generates the right config for Claude Code, Cursor, Claude Desktop, and more
- **Free to browse, sign-in for install** — full catalog readable without an account

## Example use cases

### 1. Discover tools by problem statement

> *"I want to add Stripe payments to my Next.js app. What MCP servers can help?"*

The plugin searches the conduitAI catalog and returns quality-ranked results with descriptions, install methods, and links.

### 2. Compare top options

> *"Give me the top 3 filesystem MCP servers ranked by quality."*

The plugin browses the catalog by type, sorts by quality score, and surfaces a side-by-side comparison.

### 3. Install with one command

> *"Install the GitHub MCP server in this project."*

The plugin authenticates you (once), picks the right install method for your MCP client, and writes the config — no copy-pasting JSON.

### 4. Audit your library

> *"What conduitAI assets do I have installed across all my projects?"*

The plugin lists every installed asset (MCP servers, agents, skills, prompts, plugins) so you can see what's active and clean up what isn't.

## Tools exposed by the plugin

The bundled MCP server (`https://mcp.conduitai.app/mcp`) provides 14 tools, all callable by Claude in response to natural-language requests:

| Tool | Purpose | Auth required |
| :--- | :--- | :---: |
| `search_catalog` | Find assets by query, type, sort, verified-only filter (keyword) | Optional |
| `semantic_search_catalog` | **v0.2.0** — Natural-language search combining vector similarity + full-text rank. Best for "find me a tool that does X" queries | Optional |
| `browse_catalog` | List assets with filters and sort order | Optional |
| `get_asset_details` | Full details for a single asset by slug | Optional |
| `ask_conduitai` | **v0.2.0** — RAG-powered Q&A grounded in the conduitAI knowledge base, with cited sources | Optional |
| `analyze_repo` | **v0.3.0** — Analyze any GitHub repo for safety + maintenance signals | Optional |
| `bookmark_asset` | **v0.3.0** — Save a catalog asset to your bookmarks (capacity: 1000 per user) | ✅ |
| `list_bookmarks` | **v0.3.0** — List your bookmarked assets with filters + pagination | ✅ |
| `unbookmark_asset` | **v0.3.0** — Remove a bookmark by id or (slug, asset_type) | ✅ |
| `install_asset` | Install an MCP server, agent, skill, prompt, or plugin into the current project | ✅ |
| `uninstall_asset` | Remove an installed asset | ✅ |
| `list_installed` | Show your currently installed assets | ✅ |
| `authenticate` / `auth_status` | Sign-in helpers (in HTTP mode, defers to Claude Code's `/mcp` OAuth) | — |

Two skills are also bundled and discoverable as `/conduitai:search-conduitai` and `/conduitai:install-from-conduitai` in `/help`.

## Authentication

The first tool call (or running `/mcp` → Authenticate) opens your browser to `https://conduitai.app/oauth/authorize`. Sign in with your conduitAI account, click **Authorize**, and Claude Code stores a refreshing OAuth token. No API keys to manage; tokens can be revoked from the `/mcp` menu (**Clear authentication**) or your conduitAI account settings.

OAuth metadata is published at:

- `GET https://mcp.conduitai.app/.well-known/oauth-authorization-server` (RFC 8414)
- `POST https://mcp.conduitai.app/oauth/register` (Dynamic Client Registration, RFC 7591)
- `POST https://mcp.conduitai.app/oauth/token`

Claude Code's default `http://localhost:PORT/callback` redirect URI is supported via DCR; no client pre-registration required.

## Tested with

- **Claude Code** 2.1.x and later
- Other MCP-compatible clients are supported via the same OAuth 2.1 flow but have not been formally tested for the marketplace install flow

## Updating

By default, third-party Claude Code marketplaces (this is one) have **auto-update disabled**. To get new versions automatically:

1. In Claude Code, run `/plugin`
2. Navigate to the **Marketplaces** tab
3. Select **conduitai-plugins**
4. Choose **Enable auto-update**

When auto-update is on, Claude Code refreshes the marketplace catalog at startup, updates the plugin to the latest version, and prompts you to run `/reload-plugins`.

If you prefer to update manually:

```text
/plugin marketplace update conduitai-plugins
/reload-plugins
```

Plugin versions are pinned via the `version` field in [`marketplace.json`](./.claude-plugin/marketplace.json). New releases are published by bumping that field, so you only get updates when we ship them — not on every commit.

## Test locally before pushing

If you have this repo checked out:

```bash
# From the marketplace repo root:
claude
/plugin marketplace add ./
/plugin install conduitai@conduitai-plugins
```

Or skip the marketplace and load the plugin directly:

```bash
claude --plugin-dir ./plugins/conduitai
```

## Roadmap

**v0.3.0** *(current)* — Asset evaluation + library management:

- `analyze_repo` — analyze any GitHub URL for safety + maintenance signals
- `bookmark_asset` / `list_bookmarks` / `unbookmark_asset` — save catalog assets for later (capacity: 1000 per user)

**v0.2.0** — AI-augmented discovery:

- `semantic_search_catalog` — natural-language search beyond keywords (vector + FTS)
- `ask_conduitai` — RAG-powered Q&A grounded in the conduitAI knowledge base

**v0.4.0+** *(planned, requires backend work)*:

- `recommend_assets` — given a project description, recommend tools (no need to know what to search for)
- `find_project_gaps` — "what's missing from my AI agent setup?"

## Reporting issues

- **Plugin / MCP server issues** — <https://github.com/conduit-technologies/conduitai-claude-plugins/issues>
- **Catalog / website issues** — <https://conduitai.app/help>
- **Security disclosures** — <https://conduitai.app/legal/responsible-disclosure>

## Legal

- **License** — MIT, see [LICENSE](./LICENSE)
- **Privacy Policy** — <https://conduitai.app/privacy>
- **Terms of Service** — <https://conduitai.app/terms>
- **Cookie Policy** — <https://conduitai.app/legal/cookie-policy>

---

Built by [Conduit Technologies, LLC](https://conduitai.app). Plugin source: this repo. MCP server source: <https://github.com/conduit-technologies/conduitai-mcp-server> (planned).
