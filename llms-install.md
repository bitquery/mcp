# Installing the Bitquery MCP server

By [Gaurav Agrawal](https://bitquery.io/about), Bitquery. Last updated 13 November 2026.

This file is for AI coding agents that install MCP servers on a user's behalf. Follow the section for the client you are running in.

## Facts for every client

- **Server URL:** `https://mcp.bitquery.io/mcp`
- **Transport:** Streamable HTTP. The server is hosted. Nothing is downloaded, built or run on the user's machine.
- **Sign-in:** OAuth 2.1 in the browser with the user's Bitquery account. The client discovers it from the server.
- **Do not ask the user for an API key or token**, and do not write an `Authorization` header or any other secret into a config file. If a client has no OAuth support, use the stdio bridge at the end of this file.
- **Account:** the user needs a Bitquery account. A free 7-day trial is created at https://account.bitquery.io.
- **Access:** every tool is read-only. Nothing here signs, sends or changes anything on-chain or in the user's account.

After adding the server, the first tool call opens a browser window for sign-in. Tell the user to expect it.

## Claude Code

```bash
claude mcp add --transport http bitquery https://mcp.bitquery.io/mcp
```

Add `--scope user` to enable it in every project. Then the user runs `claude mcp login bitquery`, which opens the browser login.

## Claude Desktop and claude.ai

This is done in the app, not in a file.

1. Customize → Connectors → + Add → Add custom connector.
2. Name: `Bitquery`. URL: `https://mcp.bitquery.io/mcp`.
3. Click Continue, then Add, and sign in.

On Team and Enterprise plans, an owner first adds the connector under Organization settings → Connectors; members then click Connect.

## Cursor

Write `.cursor/mcp.json` in the project, or `~/.cursor/mcp.json` for all projects:

```json
{ "mcpServers": { "bitquery": { "url": "https://mcp.bitquery.io/mcp" } } }
```

Cursor shows a "Needs login" button under Settings → MCP. The user clicks it to sign in.

## VS Code

Write `.vscode/mcp.json` in the workspace:

```json
{ "servers": { "bitquery": { "type": "http", "url": "https://mcp.bitquery.io/mcp" } } }
```

VS Code prompts for sign-in when the server starts.

## Cline

Add this entry to `cline_mcp_settings.json` (MCP Servers → Configure → Configure MCP Servers):

```json
{
  "mcpServers": {
    "bitquery": {
      "type": "streamableHttp",
      "url": "https://mcp.bitquery.io/mcp"
    }
  }
}
```

Or use MCP Servers → Remote Servers: name `bitquery`, URL `https://mcp.bitquery.io/mcp`, transport Streamable HTTP. Cline opens the browser for sign-in.

## OpenAI Codex CLI

```bash
codex mcp add bitquery --url https://mcp.bitquery.io/mcp
codex mcp login bitquery
```

Or add this to `~/.codex/config.toml`, then run the login command:

```toml
[mcp_servers.bitquery]
url = "https://mcp.bitquery.io/mcp"
```

## Gemini CLI

```bash
gemini extensions install https://github.com/bitquery/mcp
```

This reads `gemini-extension.json` from this repository. Gemini CLI starts the OAuth sign-in on first use; `/mcp auth bitquery` starts it by hand.

## OpenClaw

```bash
openclaw mcp set bitquery '{"url":"https://mcp.bitquery.io/mcp","transport":"streamable-http","auth":"oauth"}'
openclaw mcp login bitquery
```

`openclaw mcp status --verbose` confirms the sign-in worked.

## Hermes

Add this to `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  bitquery:
    url: https://mcp.bitquery.io/mcp
    auth: oauth
```

Then:

```bash
hermes mcp login bitquery
```

Start a new session, or run `/reload-mcp` in the open one.

## ChatGPT

Done in the app, on Plus, Pro or Business: Settings → Connectors → Add connector → Custom connector, URL `https://mcp.bitquery.io/mcp`.

## Clients that only speak stdio

Use this only when the client cannot connect to a remote server. It runs the third-party `mcp-remote` package from npm on the user's machine, so ask the user before adding it.

```json
{ "mcpServers": { "bitquery": { "command": "npx", "args": ["-y", "mcp-remote", "https://mcp.bitquery.io/mcp"] } } }
```

## Check that it works

Ask the client: "What are the trending Solana tokens in the last 24 hours by trade volume?" A working install calls the `trending_tokens` tool and returns a table.

If the client reports 401 Unauthorized, the sign-in was not finished. Run the client's login step again. Do not work around it with a token.

## More help

- Full guide: https://docs.bitquery.io/docs/mcp/mcp-server/
- Support: support@bitquery.io
