# conduitAI plugin marketplace

Official Claude Code marketplace for plugins published by [conduitAI](https://conduitai.app).

## Install

Inside Claude Code:

```
/plugin marketplace add conduit-technologies/conduitai-claude-plugins
/plugin install conduitai@conduitai-plugins
```

Then run `/mcp`, select **conduitai → Authenticate**, sign in with your conduitAI account in the browser, and you're connected.

## What's included

| Plugin | Description |
| :--- | :--- |
| **`conduitai`** | Search, browse, and install MCP servers, agents, skills, prompts, and plugins from the [conduitAI catalog](https://conduitai.app). Bundles the hosted MCP server at `mcp.conduitai.app` with browser-based OAuth 2.1 authentication. |

## Plugin overview

The `conduitai` plugin connects Claude Code to a remote MCP server that exposes:

- **`search_catalog`** — search the catalog by query, type, sort, verified-only filter
- **`browse_catalog`** — list assets with filters and sort order
- **`get_asset_details`** — fetch a single asset by slug
- **`install_asset`** — install an MCP server, agent, skill, prompt, or plugin into the current project
- **`uninstall_asset`** — remove an installed asset
- **`list_installed`** — show your currently installed assets
- **`authenticate` / `auth_status`** — sign-in helpers (browser flow)

See [`plugins/conduitai/README.md`](./plugins/conduitai/README.md) for full plugin docs.

## Authentication

The first tool call (or running `/mcp` → Authenticate) opens your browser to `https://conduitai.app/oauth/authorize`. Sign in with your conduitAI account, click **Authorize**, and Claude Code stores a refreshing OAuth token. No API keys to manage; tokens can be revoked from the `/mcp` menu or your conduitAI account settings.

OAuth metadata is published at:

- `GET https://mcp.conduitai.app/.well-known/oauth-authorization-server` (RFC 8414)
- `POST https://mcp.conduitai.app/oauth/register` (Dynamic Client Registration, RFC 7591)
- `POST https://mcp.conduitai.app/oauth/token`

Claude Code's default `http://localhost:PORT/callback` redirect URI is supported via DCR; no client pre-registration required.

## Updating

```
/plugin marketplace update conduitai-plugins
```

Plugin versions are pinned via the `version` field in [`marketplace.json`](./.claude-plugin/marketplace.json). New releases are published by bumping that field.

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

## Reporting issues

- Plugin / MCP server issues: <https://github.com/conduitai-app/mcp-server/issues>
- Catalog / website issues: <https://conduitai.app/help>

## License

MIT — see [`plugins/conduitai/LICENSE`](https://github.com/conduitai-app/mcp-server/blob/main/LICENSE).
