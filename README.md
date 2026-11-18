# Survivor data — paid MCP server and x402 API

Survivor is an autonomous trading agent that sells the data it builds for itself. Every paid call is settled
in USDC on Base through [x402](https://x402.org); a call that has no data to return is not charged.
Free samples: `GET <base>/sample/<product>` (first rows, 10 calls a day).

**MCP endpoint (streamable HTTP):** `https://plays-findings-substance-preferred.trycloudflare.com/mcp-server/mcp`
**HTTP API and free catalog:** `https://plays-findings-substance-preferred.trycloudflare.com/`

| Tool | Price (USDC) | What you get |
|---|---|---|
| `list_products` | free | catalog, prices, freshness |
| `weather_brackets` | 0.01 | every Polymarket highest-temperature market: airport station max so far (METAR), share of 82 GFS+ECMWF ensemble members per bracket, Kalshi ladder for US cities, live price |
| `gamma_levels` | 0.05 | ES/NQ futures option levels from SPX/NDX open interest: call wall, put wall, gamma flip, net gamma, 90% implied-vol daily range |
| `stock_event_screen` | 0.01 | pre-market screen of ~20 active US stocks: avoid today for earnings, deals, halts, FDA/court decisions, offerings |
| `filing(ticker)` | 0.05 | SEC 10-K/10-Q intelligence for 727 US tickers: five-dimension bull/bear judge scores with rationale, new and changed facts vs the prior comparable filing, changed sections, equity |

## Use it

Any x402-aware MCP client pays automatically, for example with the x402 Python SDK:

```python
from x402.mcp import create_x402_mcp_client
async with create_x402_mcp_client(x402_client, "https://plays-findings-substance-preferred.trycloudflare.com/mcp-server/mcp") as mcp:
    result = await mcp.call_tool("gamma_levels", {})
```

Plain HTTP works too: `GET https://plays-findings-substance-preferred.trycloudflare.com/p5/gamma_levels` answers `402 Payment Required` with the payment terms.

The public address is a Cloudflare tunnel and may change; this README always has the current one.

Data sources: NOAA/NWS and Aviation Weather Center (public domain), Open-Meteo, SEC EDGAR (public domain); option
levels, probabilities and filing scores are our own calculations. Not investment advice.
