# Gate News MCP 工具

**接入地址**：`https://api.gatemcp.ai/mcp/news`  
**认证**：无  
**传输**：Streamable HTTP  

公开、只读的资讯、社交、事件与预测市场研究数据（**18 个工具**）。无需登录，不访问账户，不能交易。不构成投资建议或价格预测。

相关：[Info](../gate-info/gate-info-mcp_zh.md)

---

## 1. 资讯快讯（8）

| 工具 | 说明 | 参数 |
|------|------|------|
| `news_feed_search_news` | 检索平台资讯/标题。空 `query`≈热度模式（可带 `coin`）；非空≈相似度模式。 | `query`；`coin`；`platform` / `platform_type`；`lang`（本地过滤）；`time_range`（1h / 24h / 7d / 30d）或 `start_time` / `end_time`；`sort_by`；`top_total_score`；`limit`（默认 10，最大 100）；`page`；`similarity_score` |
| `news_feed_web_search` | 全网检索，返回合成摘要与引用外链。 | `query`；`coin`；`mode`（analysis / brief）；`time_range`（1h / 24h / 7d / 30d）；`lang`（zh / en / auto）；`limit`（默认 5，最大 10） |
| `news_feed_search_x` | X/Twitter 话题检索（帖级证据与引用）。 | `query`；`time_range`（1h / 24h / 7d）或 `days`；`allowed_handles` / `excluded_handles`（各最多 10，互斥）；`model`；图片/视频理解开关；`coin`；`platform` / `platform_type`；`lang`；时间过滤；`sort_by`；`top_total_score`；`limit`；`page`；`similarity_score` |
| `news_feed_search_ugc` | 多平台 UGC（Reddit / Discord / Telegram / YouTube 等）。`query` 与/或 `coin` 至少一个非空。 | `query`；`coin`；`platform`（reddit / discord / telegram / youtube / all）；`domain`（crypto / defi / finance / macro / ai_agent / web3_dev / all）；`channel`；`quality_tier`（A / B / all）；`time_range`（1h / 24h / 7d / 30d / all）；`sort_by`（relevance / upvotes / recent）；`limit`（默认 10，最大 50） |
| `news_feed_get_exchange_announcements` | 交易所官方公告（上新、下架、维护）。媒体标题请用 `news_feed_search_news`。 | `exchange` / `platform`；`query`；`coin`；`announcement_type`（listing / delisting / maintenance / all）；`from` / `to`（Unix 秒）；`limit`（设置时最大 100） |
| `news_feed_get_social_sentiment` | 币种聚合舆情（分数、正负比、提及量、样本帖）。默认窗口 24h。 | `coin`；`time_range`（1h / 24h / 7d） |
| `news_feed_get_mention_burst` | 24h 多平台提及爆发/增长/方向。 | `coin`（实务必填）；`window`（24h）；`platforms`（all / gate_square / binance_square / twitter / telegram / youtube / reddit / discord） |
| `news_feed_get_hot_topics` | 单币近期热门话题（2–4 条，默认近 4h）。 | `coin`（实务必填）；`window`（4h）；`limit`（2–4，默认 4）；`platforms` |

---

## 2. 事件与行情归因（5）

仅研究用途，非交易执行，也不保证归因完整。

| 工具 | 说明 | 参数 |
|------|------|------|
| `news_events_get_latest_events` | 过滤事件列表/时间线（含 `event_id`、impact 方向字段）。 | `event_type`；`coin`；`time_range`（1h / 24h / 7d，与绝对时间互斥）；`direction`（positive / negative / neutral / mixed / unknown / all）；`limit`（默认 20，最大 100）；`start_time` / `end_time`；`cursor` |
| `news_events_get_event_detail` | 按 `event_id` 查事件详情。 | **`event_id`**（必填） |
| `news_events_explain_market_move` | 解释窗口内币价波动可能驱动因素。 | `query`；`coin`；`time_range`（30m / 1h / 2h / 4h / 24h，默认 2h）；`mode`（auto / price_move / event_impact）；`lang`（zh / en） |
| `news_events_get_market_move_report` | 读取已落库行情归因报告。优先级：`report_id` > `event_id` > 该 symbol 最新。 | `symbol`；`report_id`；`event_id` |
| `news_events_list_market_move_reports` | 按 symbol 与更新时间窗列出历史报告。 | `symbol`；`start_time`；`end_time`；`limit`（默认 20，最大 100） |

---

## 3. 预测市场（5）

| 工具 | 说明 | 参数 |
|------|------|------|
| `news_prediction_get_volume_delta_ranking` | 按成交量变化日排行（UTC 日期）。 | `date_utc`（YYYY-MM-DD）；`limit`（默认 20，最大 100）；`venue`（polymarket / predict_fun）；`category`；`status`（active / closed / resolved / all，默认 active） |
| `news_prediction_get_fastest_rising_ranking` | 按概率上升日排行。 | 参数同成交量变化排行 |
| `news_prediction_get_market_orderbook` | 当前盘口（仅 `mode=current`）。 | `venue`（polymarket / predict_fun）；`market_id`；`depth`（1–20，默认 20）；`mode`（current） |
| `news_prediction_search_events` | 搜索预测事件。 | `query`；`coin`（不能单独无 query/category）；`category`；`status`（默认 active）；`venue`；`sort_by`（attention / volume / liquidity / recently_listed / probability_change / volume_delta_today）；`limit`（默认 20，最大 100）；`page_token`；`with_markets` |
| `news_prediction_get_event_signal` | 按 `event_ref`（`venue:venue_event_id`）取单事件信号；含结果概率、量能等。 | `event_ref`；`window`（1h / 24h / 7d，默认 24h）；`venue`；`include_markets`（默认 true） |
