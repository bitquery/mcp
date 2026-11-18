# Bitquery MCP

This extension connects Gemini CLI to the hosted Bitquery MCP server at `https://mcp.bitquery.io/mcp`. It gives live on-chain data: DEX trades, prices, candles, trader PnL, fund tracing, address labels, Polymarket and Hyperliquid.

## Sign-in

The server uses OAuth 2.1 with a Bitquery account. There is no API key to paste. If a tool call fails with an authorization error, ask the user to run `/mcp auth bitquery` and finish the sign-in in the browser.

## How to use the tools

- All tools are read-only. Nothing here signs, sends or cancels a transaction or an order.
- Pass the blockchain whenever the user names it.
- Token symbols are not unique. Resolve a token to its contract or mint address with `find_tokens` or `find_token_by_address` before quoting a price or volume.
- Per-chain tools carry a chain prefix such as `eth_`, `base_`, `solana_`, `tron_` or `btc_`. Call `chain_capabilities` to see which tools a chain has.
- Hyperliquid has its own `hyperliquid_` tools. Resolve a ticker with `hyperliquid_markets` first.
- Some tools depend on the user's plan. If a tool is not offered, tell the user it needs a plan that includes it.

## Example prompts

1. What are the trending Solana tokens in the last 24 hours by trade volume?
2. Show OHLCV candles for WETH/USDC on Uniswap v3 for the past 7 days.
3. Trace where the funds from this wallet went and stop at the first exchange.
4. What are the five most traded Polymarket markets in the last 24 hours?
5. Show open interest, the long and short split, and funding for BTC on Hyperliquid.

## More

- Full guide: https://docs.bitquery.io/docs/mcp/mcp-server/
- Plans and pricing: https://bitquery.io/products/bitquery-mcp-server
