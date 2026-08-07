# Gate Info MCP 工具

**接入地址**：`https://api.gatemcp.ai/mcp/info`  
**认证**：无  
**传输**：Streamable HTTP  

公开、只读的行情与研究数据（**32 个工具**）。无需登录，不访问账户，不能交易。不构成投资建议。

相关：[News](../gate-news/gate-news-mcp_zh.md)

---

## 1. 币种（3）

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_coin_get_coin_info` | 按 ticker / 名称 / 合约查询单币。 | **`query`**（必填）；`query_type`（auto / address / symbol / name / project / gate_symbol / source_id）；`chain`；`scope`（basic / detailed / full）；`size`（默认 3，最大 20）；`fields` |
| `info_coin_search_coins` | 按分类、链、市值、类型筛选资产列表。 | `category`；`chain`；`market_cap_min` / `market_cap_max`；`asset_type`（crypto / tradefi / all）；`sort_by`（market_cap / fdv / circulating_supply）；`limit`（默认 20，最大 400）；`offset` |
| `info_coin_get_coin_rankings` | 各类榜单。 | **`ranking_type`**（必填：popular / top_gainers / top_losers / twitter_hot / airdrop / new_listing / market_pulse_hot）；`time_range`（仅涨跌榜：1h / 24h / 7d）；`limit`（默认 20，最大 400）；new_listing：`listing_query`、`listing_from`、`listing_tickers` |

---

## 2. 行情快照（4）

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_marketsnapshot_get_market_snapshot` | 单币种价格、K 线摘要、项目上下文。 | **`symbol`**（必填）；`timeframe` 或 `indicator_timeframe`（15m / 1h / 4h / 1d，默认 1h）；`source`（spot / futures / alpha / fx，默认 spot）；`quote`（默认 USDT）；`scope` |
| `info_marketsnapshot_batch_market_snapshot` | 批量快照，最多 20 个；部分未命中仍可成功。 | **`symbols`**（必填，≤20）；`timeframe`；`source`；`quote`；`scope` |
| `info_marketsnapshot_get_market_overview` | 全市场市值、成交量、主导率、情绪等。 | — |
| `info_marketsnapshot_get_institutional_metrics` | BTC/ETH 机构日度指标（ETF / CME / CFTC）。 | `asset`（BTC / ETH / all，默认 BTC）；`channel`（all / etf / cme / cftc）；`start_date` / `end_date`；`limit`（1–366，默认 30） |

---

## 3. 行情趋势（3）

仅描述性研究，**非**交易信号或价格预测。

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_markettrend_get_kline` | 趋势索引 OHLCV，可选指标列。交易所细粒度 K 线见 `info_marketdetail_get_kline`。 | **`symbol`**、**`timeframe`**（必填：1m / 5m / 15m / 1h / 4h / 1d）；`period`；`size` / `limit`（默认 100，最大 400）；`start_time` / `end_time`；`with_indicators` |
| `info_markettrend_get_indicator_history` | 指标历史列（RSI、MACD、均线等）。 | **`symbol`**、**`indicators`**、**`timeframe`**（必填：15m / 1h / 4h / 1d）；`start_time` / `end_time`；`limit`（默认 50，最大 400） |
| `info_markettrend_get_technical_analysis` | 多周期图表标签（如偏多/偏空/中性风格）。非价格预测。 | **`symbol`**（必填）；`period`（默认 3d）；`start_time` / `end_time` |

---

## 4. 链上（4）

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_onchain_get_address_info` | 地址画像：余额、标签、风险类信号。 | **`address`**（必填）；`chain`（eth / trx / bsc / btc / sol / base / arb 等）；`scope`；`min_value_usd` |
| `info_onchain_get_address_transactions` | 分页转账 / 交易历史。 | **`address`**（必填）；`chain`；`tx_type`；`time_range` 或 `start_time` / `end_time`；`min_value_usd`；`limit`（默认 20，最大 400）；`from_address`；`to_address`；`nonzero_value` |
| `info_onchain_get_transaction` | 按哈希查交易。 | **`tx_hash`**（必填）；`chain` |
| `info_onchain_get_token_onchain` | 代币持仓 / 活跃 / 转账 / smart_money。 | **`token`**（必填）；`chain`；`scope`（holders / activity / transfers / smart_money / full） |

---

## 5. 合规（1）

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_compliance_check_token_security` | 合约启发式风险筛查。**非安全承诺。** | **`chain`**（必填）；`token` **或** `address`（二选一）；`scope`（basic / full）；`lang`（en / cn / tw / ja / kr） |

---

## 6. 平台指标（11）

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_platformmetrics_get_platform_info` | 单协议 / 平台画像。 | **`platform_name`**（必填）；`scope`；`include_oi_symbol_detail`；`oi_symbol_limit` |
| `info_platformmetrics_search_platforms` | 多协议搜索排序。 | `platform_type`；`chain`；`sort_by`；`sort_order`；`limit` |
| `info_platformmetrics_get_defi_overview` | DeFi / 现货 / 合约 / 稳定币 / 桥等汇总。 | `category` |
| `info_platformmetrics_get_stablecoin_info` | 稳定币排名或详情；full 下可选 sections。 | `symbol`；`chain`；`limit`；`scope`；`sections`（issuance_flow / usage_structure / depeg_events）；`start_date` / `end_date`；`min_deviation`；`review_status` |
| `info_platformmetrics_get_bridge_metrics` | 桥排名或单桥明细。 | `bridge_name`；`chain`；`sort_by`（volume_24h / volume_7d / deposit_txs_24h）；`limit` |
| `info_platformmetrics_get_yield_pools` | 借贷 / LP 收益池。 | `project`；`chain`；`symbol`；`pool_type`；`sort_by`（apy / tvl_usd）；`limit`；`min_tvl_usd`；`scope` |
| `info_platformmetrics_get_platform_history` | TVL / 成交量 / 费用（或 volume_perps）历史序列。 | `platform_name` 和/或 `exchange_slug`；`metrics`；`granularity`；`start_date` / `end_date` |
| `info_platformmetrics_get_exchange_reserves` | 交易所储备；full 可含 PoR / 资金流 / 事件。 | `exchange`；`asset`（BTC / ETH / USDT / USDC）；`scope`；`include_history`；`history_window`；`include_flows`；`include_events`；`start_date` / `end_date`；`event_type`；`limit` |
| `info_platformmetrics_get_liquidation_heatmap` | 清算热力（价格区间）。 | **`symbol`**（必填）；`exchange`；`range` |
| `info_platformmetrics_get_cex_orderbook_depth` | 竞品 CEX ±1% 盘口深度。本所档位见 `info_marketdetail_get_orderbook`。 | **`symbol`**（必填）；`market_type`（spot / perp）；`exchange`；`data_scope`；`limit`（默认 20，最大 100） |
| `info_platformmetrics_get_chain_activity` | 链级活动：staking / l2 / btc_l2。 | **`metric_group`**（必填）；`chain`；`project`；`start_date` / `end_date`；`lookback`（30d / 90d / 1y）；`granularity`；`limit` |

---

## 7. 宏观（3）

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_macro_get_macro_indicator` | 官方宏观指标最新值或序列。 | **`indicator`**（必填）；`mode`（latest / timeseries）；`country` 或 `country_code`；时间范围；`size`（默认 20，最大 400） |
| `info_macro_get_economic_calendar` | 经济日历。 | `start_date` / `end_date`；`event_type`；`importance`；`size` |
| `info_macro_get_macro_summary` | 宏观看板：关键指标 + 临近日历。 | — |

---

## 8. 盘口细节（3）

交易所交易对 / 合约盘口、成交与 K 线（如 `BTC_USDT`）。时间为 UTC。

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_marketdetail_get_orderbook` | 买卖盘深度。 | **`symbol`**（必填）；`market_type`（spot / futures / delivery / options，默认 spot）；`depth`（默认 20，最大 100）；`settle`（合约结算，默认 usdt） |
| `info_marketdetail_get_recent_trades` | 近期公开成交。 | **`symbol`**（必填）；`market_type`；`limit`（默认 50，最大 400）；`settle` |
| `info_marketdetail_get_kline` | 交易所 OHLCV（含细粒度周期）。 | **`symbol`**、**`timeframe`**（必填）；`market_type`；`start_time` / `end_time`；`limit`；`settle` |
