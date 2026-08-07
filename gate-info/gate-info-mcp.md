# Gate Info MCP Tools

**Endpoint**: `https://api.gatemcp.ai/mcp/info`  
**Auth**: none  
**Transport**: Streamable HTTP  

Public, read-only market and research data (**32 tools**). No login, account access, or trading. Not investment advice.

Related: [News](../gate-news/gate-news-mcp.md)

---

## 1. Coin (3)

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_coin_get_coin_info` | Look up one coin by ticker, name, or contract. | **`query`** (required); `query_type` (auto / address / symbol / name / project / gate_symbol / source_id); `chain`; `scope` (basic / detailed / full); `size` (default 3, max 20); `fields` |
| `info_coin_search_coins` | Filter a multi-row asset list by sector, chain, cap, type. | `category`; `chain`; `market_cap_min` / `market_cap_max`; `asset_type` (crypto / tradefi / all); `sort_by` (market_cap / fdv / circulating_supply); `limit` (default 20, max 400); `offset` |
| `info_coin_get_coin_rankings` | Leaderboards / boards. | **`ranking_type`** (required: popular / top_gainers / top_losers / twitter_hot / airdrop / new_listing / market_pulse_hot); `time_range` (1h / 24h / 7d — gainers/losers only); `limit` (default 20, max 400); new_listing: `listing_query`, `listing_from`, `listing_tickers` |

---

## 2. Market snapshot (4)

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_marketsnapshot_get_market_snapshot` | One symbol: price, kline clip, project context. | **`symbol`** (required); `timeframe` or `indicator_timeframe` (15m / 1h / 4h / 1d, default 1h); `source` (spot / futures / alpha / fx, default spot); `quote` (default USDT); `scope` (basic / detailed / full) |
| `info_marketsnapshot_batch_market_snapshot` | Up to 20 symbols; missing symbols still allow batch success. | **`symbols`** (required, max 20); `timeframe`; `source`; `quote`; `scope` |
| `info_marketsnapshot_get_market_overview` | Market-wide cap, volume, dominance, sentiment. | — |
| `info_marketsnapshot_get_institutional_metrics` | BTC/ETH institutional daily series (ETF / CME / CFTC). | `asset` (BTC / ETH / all, default BTC); `channel` (all / etf / cme / cftc); `start_date` / `end_date` (YYYY-MM-DD); `limit` (1–366, default 30) |

---

## 3. Market trend (3)

Descriptive research only — not trading signals or forecasts.

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_markettrend_get_kline` | Trend-index OHLCV; optional indicator columns. Fine exchange intervals → `info_marketdetail_get_kline`. | **`symbol`**, **`timeframe`** (required: 1m / 5m / 15m / 1h / 4h / 1d); `period` (1h / 4h / 24h / 7d / …); `size` / `limit` (default 100, max 400); `start_time` / `end_time`; `with_indicators` |
| `info_markettrend_get_indicator_history` | Historical indicator columns (RSI, MACD, MAs, …). | **`symbol`**, **`indicators`**, **`timeframe`** (required: 15m / 1h / 4h / 1d); `start_time` / `end_time`; `limit` (default 50, max 400) |
| `info_markettrend_get_technical_analysis` | Multi-timeframe chart labels (e.g. bullish / bearish / neutral style). Not a price forecast. | **`symbol`** (required); `period` (default 3d); `start_time` / `end_time` |

---

## 4. On-chain (4)

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_onchain_get_address_info` | Address profile: balances, labels, risk-style signals. | **`address`** (required); `chain` (eth / trx / bsc / btc / sol / base / arb / …); `scope`; `min_value_usd` |
| `info_onchain_get_address_transactions` | Paginated transfer / history list. | **`address`** (required); `chain`; `tx_type` (transfer / contract_call / token_transfer / all); `time_range` or `start_time` / `end_time`; `min_value_usd`; `limit` (default 20, max 400); `from_address`; `to_address`; `nonzero_value` |
| `info_onchain_get_transaction` | One transaction by hash. | **`tx_hash`** (required); `chain` |
| `info_onchain_get_token_onchain` | Token holders / activity / transfers / smart_money. | **`token`** (required); `chain`; `scope` (holders / activity / transfers / smart_money / full) |

---

## 5. Compliance (1)

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_compliance_check_token_security` | Heuristic contract risk screen (provider signals). **Not a security guarantee.** | **`chain`** (required); `token` **or** `address` (one required); `scope` (basic / full); `lang` (en / cn / tw / ja / kr) |

---

## 6. Platform metrics (11)

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_platformmetrics_get_platform_info` | One protocol / venue profile. | **`platform_name`** (required); `scope`; `include_oi_symbol_detail`; `oi_symbol_limit` |
| `info_platformmetrics_search_platforms` | Ranked multi-protocol search. | `platform_type`; `chain`; `sort_by`; `sort_order`; `limit` |
| `info_platformmetrics_get_defi_overview` | Cross-sector DeFi / spot / perp / stablecoin / bridge rollup. | `category` |
| `info_platformmetrics_get_stablecoin_info` | Stablecoin ranking or detail; optional sections at full scope. | `symbol`; `chain`; `limit`; `scope`; `sections` (issuance_flow / usage_structure / depeg_events); `start_date` / `end_date`; `min_deviation`; `review_status` |
| `info_platformmetrics_get_bridge_metrics` | Bridge ranking or one-bridge breakdown. | `bridge_name`; `chain`; `sort_by` (volume_24h / volume_7d / deposit_txs_24h); `limit` |
| `info_platformmetrics_get_yield_pools` | Lending / LP pools by APY or TVL. | `project`; `chain`; `symbol`; `pool_type`; `sort_by` (apy / tvl_usd); `limit`; `min_tvl_usd`; `scope` |
| `info_platformmetrics_get_platform_history` | Daily TVL / volume / fees (or volume_perps) series. | `platform_name` and/or `exchange_slug`; `metrics`; `granularity`; `start_date` / `end_date` |
| `info_platformmetrics_get_exchange_reserves` | Exchange reserve snapshots; full scope can add PoR / flows / events. | `exchange`; `asset` (BTC / ETH / USDT / USDC); `scope`; `include_history`; `history_window`; `include_flows`; `include_events`; `start_date` / `end_date`; `event_type`; `limit` |
| `info_platformmetrics_get_liquidation_heatmap` | Liquidation density by symbol / price band. | **`symbol`** (required); `exchange`; `range` |
| `info_platformmetrics_get_cex_orderbook_depth` | Cross-venue CEX ±1% depth (spot / perp) for benchmarking. Gate native ladder → `info_marketdetail_get_orderbook`. | **`symbol`** (required); `market_type` (spot / perp); `exchange`; `data_scope`; `limit` (default 20, max 100) |
| `info_platformmetrics_get_chain_activity` | Chain activity: staking / L2 / BTC L2 groups. | **`metric_group`** (required: staking / l2 / btc_l2); `chain`; `project`; `start_date` / `end_date`; `lookback` (30d / 90d / 1y); `granularity`; `limit` |

---

## 7. Macro (3)

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_macro_get_macro_indicator` | Official macro latest value or series (CPI, rates, jobs, …). | **`indicator`** (required); `mode` (latest / timeseries); `country` or `country_code`; `start_time` / `end_time` (or `start_date` / `end_date`); `size` (default 20, max 400) |
| `info_macro_get_economic_calendar` | Economic calendar in a date window. | `start_date` / `end_date`; `event_type`; `importance`; `size` |
| `info_macro_get_macro_summary` | Macro dashboard: key snapshots + upcoming releases. | — |

---

## 8. Market detail (3)

Exchange pair / contract books, trades, and candles (e.g. `BTC_USDT`). Times in responses are UTC.

| Tool | Description | Parameters |
|------|-------------|------------|
| `info_marketdetail_get_orderbook` | Bids / asks depth ladder. | **`symbol`** (required); `market_type` (spot / futures / delivery / options, default spot); `depth` (default 20, max 100); `settle` (futures/delivery, default usdt) |
| `info_marketdetail_get_recent_trades` | Recent public trades. | **`symbol`** (required); `market_type`; `limit` (default 50, max 400); `settle` |
| `info_marketdetail_get_kline` | Exchange OHLCV, including fine intervals (e.g. 1m). | **`symbol`**, **`timeframe`** (required); `market_type`; `start_time` / `end_time`; `limit`; `settle` |
