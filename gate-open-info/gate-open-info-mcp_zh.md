# Gate Open Info MCP 工具

**接入地址**：`https://api.gatemcp.ai/mcp/open/info`  
**认证**：无  
**传输**：Streamable HTTP  
**Server**：Gate Info / `0.1.0`  
**工具数量**：恰好 **7**（ChatGPT / OpenAI Apps 送审裁剪面）

公开、只读的加密货币研究数据。无需登录，不访问账户/余额，不下单、不转账。仅供信息研究，不构成投资建议。

本端点为**送审 allowlist**。全量工具见 [gate-info-mcp](../gate-info/gate-info-mcp_zh.md)（`/mcp/info`）。News 见 [gate-news-mcp](../gate-news/gate-news-mcp_zh.md)。

**Annotations（全部工具）：** `readOnlyHint=true`，`destructiveHint=false`，`openWorldHint=true`，`idempotentHint=true`。

省略可选字段时使用默认值；显式非法值会被**拒绝**（不静默截断）。清单外工具名返回 `tool not found`。

---

## 工具清单

| # | 工具 | Title | 用途 |
|---|------|-------|------|
| 1 | `info_coin_get_coin_info` | Get Coin Information | 币种 / 项目公开资料 |
| 2 | `info_marketsnapshot_get_market_overview` | Get Market Overview | 全市场概览 |
| 3 | `info_marketsnapshot_get_market_snapshot` | Get Market Snapshot | 单资产 Gate 现货快照 |
| 4 | `info_marketsnapshot_batch_market_snapshot` | Get Multiple Market Snapshots | 批量现货快照（≤20） |
| 5 | `info_marketdetail_get_kline` | Get Kline Data | K 线（现货 / 合约 / 交割 / 期权） |
| 6 | `info_marketdetail_get_orderbook` | Get Order Book | 公开买卖盘深度 |
| 7 | `info_marketdetail_get_recent_trades` | Get Recent Trades | 近期公开成交 |

---

## 1. 币种

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_coin_get_coin_info` | 按 ticker / 名称查询公开币种资料。返回 `symbol` / `name` / `category`，以及 `chain`（已知时为字符串数组，未知为 `null`）。 | **`query`**（必填，非空）；`query_type`（`auto` 默认 \| `symbol` \| `name`）；`size`（1–20，默认 3） |

本面不接受：`address` / `project` / `gate_symbol` / `source_id`、`fields`、`scope`、入参 `chain`。

**示例：** `{"query":"BTC","query_type":"symbol","size":1}`

---

## 2. 行情快照

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_marketsnapshot_get_market_overview` | 全市场概览（市值、成交量、主导率、情绪类指标等）。上游分片失败时字段可各自为 null。 | —（空参数） |
| `info_marketsnapshot_get_market_snapshot` | 单币种 Gate 现货快照：实时摘要、短 K 线、基础项目信息。 | **`symbol`**（必填，如 `BTC`）；`timeframe`（`15m` \| `1h` 默认 \| `4h` \| `1d`） |
| `info_marketsnapshot_batch_market_snapshot` | 批量 Gate 现货快照。未命中进入 `not_found[]`，批次仍可成功。 | **`symbols`**（必填数组，原始长度 1–20，先校验再 trim/dedupe）；`timeframe`（同单币） |

服务器固定 `source=spot`、`quote=USDT`、`scope=basic`。传入 `source` / `scope` / `quote` / `indicator_timeframe` 会被**拒绝**。

**示例：**
- overview：`{}`
- snapshot：`{"symbol":"BTC","timeframe":"1h"}`
- batch：`{"symbols":["BTC","ETH","SOL"],"timeframe":"1h"}`

---

## 3. 盘口细节

交易所交易对 / 合约盘口、成交与 K 线（如 `BTC_USDT`）。时间为 UTC。

| 工具 | 说明 | 参数 |
|------|------|------|
| `info_marketdetail_get_kline` | 公开 Gate K 线（OHLC；上游提供时含 volume 等）。不下单。 | **`symbol`**（必填）；**`timeframe`**（必填；按 `market_type` 校验；delivery 排除 `1s` / `3d`）；`market_type`（`spot` 默认 \| `futures` \| `delivery` \| `options`）；`start_time` / `end_time`（Unix 秒）；`limit`（1–400，默认 100）；`settle`（futures：`usdt` \| `btc`，默认 `usdt`；delivery：`usdt`；spot/options 勿传） |
| `info_marketdetail_get_orderbook` | 实时公开深度（买卖盘）。不下单、不改单、不撤单。 | **`symbol`**（必填）；`market_type`（同 K 线）；`depth`（1–100，默认 20）；`settle`（同 K 线规则） |
| `info_marketdetail_get_recent_trades` | 近期公开成交。不执行、不结算交易。 | **`symbol`**（必填）；`market_type`（同 K 线）；`limit`（1–400，默认 50）；`settle`（同 K 线规则） |

**示例：**
- kline：`{"symbol":"BTC_USDT","market_type":"spot","timeframe":"1h","limit":5}`
- orderbook：`{"symbol":"BTC_USDT","market_type":"spot","depth":10}`
- trades：`{"symbol":"BTC_USDT","market_type":"spot","limit":10}`

---

## 响应约定

- 完整业务数据在 **`structuredContent`**；`content[0].text` 仅一两句英文摘要。
- open 面响应**不含** `duration_ms`、`trace_id`、`cex_tool` 等内部字段。
- 非法入参不得伪装成成功空结果（例如 `limit=401` → `INVALID_ARGUMENT`，不静默截断为 400）。

---

## 本端点不包含

不在 `tools/list`（直调 → `tool not found`）：币种搜索/榜单、技术指标、链上、合规、平台指标、宏观、机构指标、News、预测市场、Docs，以及任何交易 / 钱包工具。
