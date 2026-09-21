# Bitquery MCP Server

By [Gaurav Agrawal](https://bitquery.io/about), Bitquery. Last updated 21 September 2026.

Official hosted [Model Context Protocol](https://modelcontextprotocol.io) server from [Bitquery](https://bitquery.io/products/bitquery-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=mcp). It gives Claude, ChatGPT, Cursor, VS Code, Codex and other MCP clients live on-chain data: DEX trades, prices, candles, trader PnL, fund tracing and address labels.

- **Endpoint:** `https://mcp.bitquery.io`
- **Transport:** Streamable HTTP
- **Sign-in:** OAuth 2.1 with your Bitquery account. No API key to paste and no config secrets.
- **Install:** nothing runs on your machine. The server is hosted.

This repository holds the install configs, example prompts and docs. The server itself is a hosted service, so there is no code to build here.

**Watch the 38-second demo:** [Polymarket and Hyperliquid data in ChatGPT and Claude](https://www.youtube.com/watch?v=IwuIlcKsANk).

[![Video thumbnail: Polymarket and Hyperliquid data in ChatGPT and Claude with Bitquery MCP](https://img.youtube.com/vi/IwuIlcKsANk/hqdefault.jpg)](https://www.youtube.com/watch?v=IwuIlcKsANk)

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

### Polymarket prediction markets
14 read-only tools over on-chain Polymarket data on Polygon. They cannot place or cancel orders.

| Ask | Tool the agent picks |
|---|---|
| What are the five most traded Polymarket markets in the last 24 hours? | `polymarket_top_markets` |
| Find the open markets about the Fed rate. | `polymarket_find_markets` |
| What are the odds on this market right now? | `polymarket_market_odds` |
| Show the daily close for Yes over the last week. | `polymarket_market_price_history` |
| Show the latest fills above $10,000 in this market. | `polymarket_market_trades` |
| Who traded the most in this market? | `polymarket_top_traders` |
| Is this wallet a market maker or a directional bettor? | `polymarket_trader_summary` |
| Which outcomes were disputed this week? | `polymarket_market_disputes` |

Sample answer to the first question, production server, 21 September 2026 at 09:08 UTC, data 14 minutes behind the clock:

| Market | 24h volume | Orders | Wallets |
|---|---:|---:|---:|
| Colts vs. Chiefs | $4,114,943 | 9,124 | 292 |
| Will United Russia (ER) gain the most seats in the next Russian parliamentary election? | $4,065,436 | 8,532 | 565 |
| Will Manchester City FC win on 2026-09-20? | $1,918,415 | 5,523 | 386 |

`polymarket_top_traders` ranks wallets by dollars traded and carries no profit figure. The top wallet in the second market above had 48,736 of its 51,391 lifetime orders resting on the book, which is what a market maker looks like. Six worked questions with full output are on the [Polymarket MCP page](https://docs.bitquery.io/docs/mcp/polymarket/). To follow large traders, see [Track Polymarket whales and top traders](https://docs.bitquery.io/docs/mcp/polymarket-whale-tracker/). For building in code, see the [Polymarket GraphQL API docs](https://docs.bitquery.io/docs/examples/polymarket-api/).

### Solana and pump.fun
Ten `solana_` tools cover transfers, signatures and program instructions, and the market tools cover Solana trades, prices and launches. Read-only: nothing here signs or sends a transaction.

| Ask | Tool the agent picks |
|---|---|
| What are the most traded tokens on Solana in the last 24 hours? | `trending_tokens` |
| Show new pump.fun tokens from the last 6 hours. | `new_tokens` |
| How did this launch go in its first 60 seconds? | `token_launch_stats` |
| Who were the first buyers? | `early_buyers` |
| Where does this token trade, and how many wallets trade it? | `token_dex_venues` |
| Where did this wallet send its SOL? | `solana_trace_next_hop` |

Check the token address before you trust a row. On 21 September 2026 the top "USDT" in Solana's 24-hour volume ranking was a counterfeit: `token_dex_venues` showed 5 trades between 2 wallets behind a reported $986,660,608. Six worked questions with full output are on the [Solana MCP page](https://docs.bitquery.io/docs/mcp/solana/).

### Hyperliquid
18 read-only tools over the exchange's own blocks: perpetuals, spot pairs, third-party markets and outcome markets. They cannot place or cancel orders.

| Ask | Tool the agent picks |
|---|---|
| What are the five most traded Hyperliquid markets today? | `hyperliquid_markets` |
| Show open interest, the long and short split, and funding. | `hyperliquid_open_interest` |
| How has BTC funding moved over the last 24 hours? | `hyperliquid_funding_rates` |
| Which markets had the most value liquidated, and on which side? | `hyperliquid_liquidations` |
| Who made the most trading BTC in the last 24 hours? | `hyperliquid_top_traders` |
| What positions does this wallet hold? | `hyperliquid_trader_positions` |

Sample, production server, 21 September 2026 between 10:21 and 10:25 UTC, after Bitcoin rose 5.4% in a day: the 9,334 BTC liquidations in 24 hours were worth $92,691,944 and 9,265 of them were shorts, and BTC funding went from 10.95% to 78.64% per year within two hours. Profit rankings leave out open positions, so read a wallet's positions before calling it a winner. Six worked questions are on the [Hyperliquid MCP page](https://docs.bitquery.io/docs/mcp/hyperliquid/).

## Plans
| Plan | Price | Credits per month | Tools |
|---|---|---|---|
| Trial | free, 7 days | 100 | see the pricing page |
| Trading MCP | $19 per month ($15 billed yearly) | 25,000 | 32, market data |
| AI Investigation MCP | $149 per month ($119 billed yearly) | 200,000 | 123, all data plus labels and tracing |

Paid API plans can add MCP credits without changing plan. Current pricing: [bitquery.io/products/bitquery-mcp-server](https://bitquery.io/products/bitquery-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=mcp#pricing).

## Docs
- Full guide: https://docs.bitquery.io/docs/mcp/mcp-server/
- Trading examples: https://docs.bitquery.io/docs/mcp/trading/examples/
- Tracing overview: https://docs.bitquery.io/docs/mcp/Tracing/overview/
- Build a trading agent: https://docs.bitquery.io/docs/mcp/build-a-trading-agent/
- Polymarket: https://docs.bitquery.io/docs/mcp/polymarket/
- Solana: https://docs.bitquery.io/docs/mcp/solana/
- Hyperliquid: https://docs.bitquery.io/docs/mcp/hyperliquid/

## Support
Questions and bugs: open an issue here, or write to support@bitquery.io. Account and billing questions go to support, since issues here are public.

## How this page was made
Drafted with AI assistance. Every command, price, plan and tool name was checked against the live product page and the live endpoint on 21 September 2026.
