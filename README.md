# Bitquery MCP Server

By [Gaurav Agrawal](https://bitquery.io/about), Bitquery. Last updated 21 September 2026.

Official hosted [Model Context Protocol](https://modelcontextprotocol.io) server from [Bitquery](https://bitquery.io/products/bitquery-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=mcp). It gives Claude, ChatGPT, Cursor, VS Code, Codex and other MCP clients live on-chain data: DEX trades, prices, candles, trader PnL, fund tracing and address labels.

- **Endpoint:** `https://mcp.bitquery.io`
- **Transport:** Streamable HTTP
- **Sign-in:** OAuth 2.1 with your Bitquery account. No API key to paste and no config secrets.
- **Install:** nothing runs on your machine. The server is hosted.

This repository holds the install configs, example prompts and docs. The server itself is a hosted service, so there is no code to build here.

## Install in 60 seconds

**Claude Code**
```bash
claude mcp add --transport http bitquery https://mcp.bitquery.io
```
Add `--scope user` to enable it in every project. On first use run `/mcp` and finish the browser login.

**Claude Desktop and claude.ai**: Settings → Connectors → Add custom connector. Name `Bitquery`, URL `https://mcp.bitquery.io`. A browser window opens for login.

**ChatGPT** (Plus, Pro or Business): Settings → Connectors → Add connector → Custom connector. URL `https://mcp.bitquery.io`.

**Cursor**: Settings → MCP → Add new MCP server, or copy [`configs/cursor.mcp.json`](configs/cursor.mcp.json) to `.cursor/mcp.json`.

**VS Code**: copy [`configs/vscode.mcp.json`](configs/vscode.mcp.json) to `.vscode/mcp.json`.

**OpenAI Codex CLI**
```bash
codex mcp add bitquery --url https://mcp.bitquery.io
```
or use [`configs/codex.config.toml`](configs/codex.config.toml).

**Clients that only speak stdio** bridge through `mcp-remote`: see [`configs/claude-desktop-stdio-bridge.json`](configs/claude-desktop-stdio-bridge.json).

## What you can ask

### Trading and market data
DEX trades on Ethereum, Arbitrum, Base, Polygon, Optimism, BNB Chain, Tron and Solana.

| Ask | Tool the agent picks |
|---|---|
| What are the trending Solana tokens in the last 24 hours by trade volume? | `trending_tokens` |
| Show OHLCV candles for WETH/USDC on Uniswap v3 for the past 7 days. | `pair_ohlcv` |
| Who are the most profitable traders of this token this month, and what do they hold now? | `profitable_traders_by_token`, `trader_positions` |
| Which wallets are accumulating this token right now? | `accumulating_traders_by_token` |
| Decode this transaction: every swap inside it, with amounts, price and pool. | `tx_trades` |
| Run SQL over the last 30 days of per-second DEX trades. | `execute_sql` (paid plans) |

### Crypto investigations
Transfer tracing on Ethereum, Polygon, Arbitrum, Base, Optimism, Tron, Bitcoin and Solana.

| Ask | Tool or guided prompt |
|---|---|
| Trace where the funds from this wallet went and stop at the first exchange. | `money_flow` prompt |
| Screen this address for mixer, sanctioned or scam exposure and give a risk verdict. | `risk_screen` prompt |
| Build a full dossier on this address. | `address_report` prompt |
| Is this wallet an exchange deposit address, a mixer or a known scam? | `address_labels` |
| Expand this Bitcoin address into addresses that likely share an owner. | `btc_related_addresses` |

## Plans
| Plan | Price | Credits per month | Tools |
|---|---|---|---|
| Trial | free, 7 days | 100 |: |
| Trading MCP | $19 per month ($15 billed yearly) | 25,000 | 32, market data |
| AI Investigation MCP | $149 per month ($119 billed yearly) | 200,000 | 123, all data plus labels and tracing |

Paid API plans can add MCP credits without changing plan. Current pricing: [bitquery.io/products/bitquery-mcp-server](https://bitquery.io/products/bitquery-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=mcp#pricing).

## Docs
- Full guide: https://docs.bitquery.io/docs/mcp/mcp-server/
- Trading examples: https://docs.bitquery.io/docs/mcp/trading/examples/
- Tracing overview: https://docs.bitquery.io/docs/mcp/Tracing/overview/
- Build a trading agent: https://docs.bitquery.io/docs/mcp/build-a-trading-agent/

## Support
Questions and bugs: open an issue here, or write to support@bitquery.io. Account and billing questions go to support, since issues here are public.

## How this page was made
Drafted with AI assistance. Every command, price, plan and tool name was checked against the live product page and the live endpoint on 21 September 2026.
