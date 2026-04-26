# conduitAI plugin for Claude Code

Search, browse, and install MCP servers, agents, skills, prompts, and plugins from the [conduitAI](https://conduitai.app) catalog — directly from Claude Code.

## Install

```bash
# Add the conduitAI marketplace (once)
/plugin marketplace add conduit-technologies/conduitai-claude-plugins

# Install the plugin
/plugin install conduitai@conduitai-plugins
```

Or test locally from this repo:

```bash
claude --plugin-dir ./plugins/conduitai
```

## First-time authentication

The plugin bundles a hosted MCP server at `https://mcp.conduitai.app/mcp`. On first use, Claude Code will prompt you to authenticate:

1. Run `/mcp` inside Claude Code.
2. Select **conduitai → Authenticate**.
3. Your browser opens `https://conduitai.app/oauth/authorize`; sign in with your conduitAI account and click **Authorize**.
4. Tokens are stored securely by Claude Code and refreshed automatically.

You can also trigger the browser flow directly by asking Claude to call the bundled `authenticate` tool, e.g. *"Authenticate with conduitAI."*

Tokens can be revoked from the `/mcp` menu (**Clear authentication**) or from your conduitAI account settings.

## Tools exposed by the MCP server

Auth requirements mirror the conduitai.app web UI — public for catalog discovery, sign-in for AI features and per-user operations.

| Tool | Purpose | Auth |
| :--- | :--- | :---: |
| `authenticate` | Open browser to sign in with conduitAI | — |
| `auth_status` | Check current sign-in state | — |
| `search_catalog` | Keyword search MCP servers, agents, skills, prompts, plugins | — |
| `semantic_search_catalog` | **v0.2.0** — Natural-language search (vector + FTS hybrid) | ✅ |
| `get_asset_details` | Fetch a single asset by slug | — |
| `browse_catalog` | List assets with filters + sort | — |
| `ask_conduitai` | **v0.2.0** — RAG-powered Q&A grounded in the conduitAI knowledge base | ✅ |
| `list_installed` | Show your installed assets | ✅ |
| `install_asset` | Install an asset into the current project | ✅ |
| `uninstall_asset` | Remove an installed asset | ✅ |

## Included skills

- `/conduitai:search-conduitai <query>` — wrapper that asks Claude to run `search_catalog` and summarize results.
- `/conduitai:install-from-conduitai <asset>` — guided install flow that confirms the target asset before calling `install_asset`.

## OAuth details for self-hosters

Discovery metadata is served from the MCP host:

- `GET https://mcp.conduitai.app/.well-known/oauth-authorization-server` (RFC 8414)
- `POST https://mcp.conduitai.app/oauth/register` (Dynamic Client Registration)
- `POST https://mcp.conduitai.app/oauth/token`
- Authorization UI: `https://conduitai.app/oauth/authorize`

Claude Code's default `http://localhost:PORT/callback` redirect URI is accepted via DCR; no pre-registration is required.

## License

MIT — see [LICENSE](https://github.com/conduitai-app/mcp-server/blob/main/LICENSE).
