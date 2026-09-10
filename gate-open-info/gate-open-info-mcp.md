# Gate Open Info MCP Tools

**Endpoint**: `https://api.gatemcp.ai/mcp/open/info`  
**Auth**: none  
**Transport**: Streamable HTTP  
**Server**: Gate Info / `0.1.0`  
**Tools**: exactly **7** (ChatGPT / OpenAI Apps submission surface)

Public, read-only cryptocurrency research data. No login, no account balances, no order placement or fund transfers. Informational only — not investment advice.

This endpoint is the **submission allowlist** only. Full catalog: [gate-info-mcp](../gate-info/gate-info-mcp.md) (`/mcp/info`). News: [gate-news-mcp](../gate-news/gate-news-mcp.md).

**Annotations (all tools):** `readOnlyHint=true`, `destructiveHint=false`, `openWorldHint=true`, `idempotentHint=true`.

Omitted optional fields use defaults. Explicit invalid values are **rejected** (never silently clamped). Any name outside this list returns `tool not found`.

---

## Tools

| # | Tool | Title | Purpose |
|---|------|-------|---------|
| 1 | `info_coin_get_coin_info` | Get Coin Information | Public coin / project profile |
| 2 | `info_marketsnapshot_get_market_overview` | Get Market Overview | Market-wide overview metrics |
| 3 | `info_marketsnapshot_get_market_snapshot` | Get Market Snapshot | Single-asset Gate spot snapshot |
| 4 | `info_marketsnapshot_batch_market_snapshot` | Get Multiple Market Snapshots | Batch spot snapshots (≤20) |
| 5 | `info_marketdetail_get_kline` | Get Kline Data | Candlesticks (spot / futures / delivery / options) |
| 6 | `info_marketdetail_get_orderbook` | Get Order Book | Public order book depth |
| 7 | `info_marketdetail_get_recent_trades` | Get Recent Trades | Recent public trades |

---

## 1. Coin

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_coin_get_coin_info` | Look up public coin profiles by ticker or name. Returns `symbol` / `name` / `category` and `chain` as a string list when known (`null` if unknown). | **`query`** (required, non-empty); `query_type` (`auto` default \| `symbol` \| `name`); `size` (1–20, default 3) |

Not accepted on this surface: `address` / `project` / `gate_symbol` / `source_id`, `fields`, `scope`, input `chain`.

**Example:** `{"query":"BTC","query_type":"symbol","size":1}`

---

## 2. Market snapshot

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_marketsnapshot_get_market_overview` | Market breadth dashboard (cap, volume, dominance, sentiment-style metrics). Fields may be null independently when an upstream slice fails. | — (empty arguments) |
| `info_marketsnapshot_get_market_snapshot` | One-symbol Gate spot snapshot: realtime clip, short kline, basic project summary. | **`symbol`** (required, e.g. `BTC`); `timeframe` (`15m` \| `1h` default \| `4h` \| `1d`) |
| `info_marketsnapshot_batch_market_snapshot` | Batch Gate spot snapshots. Missing symbols go to `not_found[]`; batch can still succeed. | **`symbols`** (required array, raw length 1–20 before trim/dedupe); `timeframe` (same as single) |

Server fixes `source=spot`, `quote=USDT`, `scope=basic`. Passing `source` / `scope` / `quote` / `indicator_timeframe` is **rejected**.

**Examples:**
- overview: `{}`
- snapshot: `{"symbol":"BTC","timeframe":"1h"}`
- batch: `{"symbols":["BTC","ETH","SOL"],"timeframe":"1h"}`

---

## 3. Market detail

Exchange pair / contract books, trades, and candles (e.g. `BTC_USDT`). Times in responses are UTC.

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_marketdetail_get_kline` | Public Gate candlesticks (OHLC; optional volume fields when upstream provides them). Never places trades. | **`symbol`** (required); **`timeframe`** (required; validated per `market_type`; delivery excludes `1s` / `3d`); `market_type` (`spot` default \| `futures` \| `delivery` \| `options`); `start_time` / `end_time` (Unix seconds); `limit` (1–400, default 100); `settle` (futures: `usdt` \| `btc`, default `usdt`; delivery: `usdt`; omit for spot/options) |
| `info_marketdetail_get_orderbook` | Live public depth snapshot (bids/asks). Never places, amends, or cancels orders. | **`symbol`** (required); `market_type` (same as kline); `depth` (1–100, default 20); `settle` (same rules as kline) |
| `info_marketdetail_get_recent_trades` | Recent public trades. Does not execute or settle trades. | **`symbol`** (required); `market_type` (same as kline); `limit` (1–400, default 50); `settle` (same rules as kline) |

**Examples:**
- kline: `{"symbol":"BTC_USDT","market_type":"spot","timeframe":"1h","limit":5}`
- orderbook: `{"symbol":"BTC_USDT","market_type":"spot","depth":10}`
- trades: `{"symbol":"BTC_USDT","market_type":"spot","limit":10}`

---

## Responses

- Full business payload is in **`structuredContent`**. `content[0].text` is a short English summary only.
- Open-surface responses do **not** include internal fields such as `duration_ms`, `trace_id`, or `cex_tool`.
- Invalid input never returns a successful empty result (e.g. `limit=401` → `INVALID_ARGUMENT`, not silent truncate).

---

## Out of scope on this endpoint

Not listed in `tools/list` (direct call → `tool not found`): coin search/rankings, technical/indicator tools, on-chain, compliance, platform metrics, macro, institutional, news, prediction markets, docs, and any trading / wallet tools.
