# Gate News MCP Tools

**Endpoint**: `https://api.gatemcp.ai/mcp/news`  
**Auth**: none  
**Transport**: Streamable HTTP  

Public, read-only news, social, events, and prediction-market research (**18 tools**). No login, account access, or trading. Not investment advice or price forecasts.

Related: [Info](../gate-info/gate-info-mcp.md)

---

## 1. News feed (8)

| Tool | Description | Parameters |
|------|-------------|------------|
| `news_feed_search_news` | Search platform news / headlines. Empty `query` ≈ heat mode (optional `coin`); non-empty ≈ similarity mode. | `query`; `coin`; `platform` / `platform_type`; `lang` (local filter); `time_range` (1h / 24h / 7d / 30d) or `start_time` / `end_time`; `sort_by`; `top_total_score`; `limit` (default 10, max 100); `page`; `similarity_score` |
| `news_feed_web_search` | Open-web search with a synthesized answer and cited pages. | `query`; `coin`; `mode` (analysis / brief); `time_range` (1h / 24h / 7d / 30d); `lang` (zh / en / auto); `limit` (default 5, max 10) |
| `news_feed_search_x` | X/Twitter topic search with tweet-level evidence and citations. | `query`; `time_range` (1h / 24h / 7d) or `days`; `allowed_handles` / `excluded_handles` (max 10 each, exclusive); `model`; `enable_image_understanding` / `enable_video_understanding`; `coin`; `platform` / `platform_type`; `lang`; time filters; `sort_by`; `top_total_score`; `limit`; `page`; `similarity_score` |
| `news_feed_search_ugc` | Multi-platform UGC (Reddit / Discord / Telegram / YouTube style). `query` and/or `coin` required (both empty invalid). | `query`; `coin`; `platform` (reddit / discord / telegram / youtube / all); `domain` (crypto / defi / finance / macro / ai_agent / web3_dev / all); `channel`; `quality_tier` (A / B / all); `time_range` (1h / 24h / 7d / 30d / all); `sort_by` (relevance / upvotes / recent); `limit` (default 10, max 50) |
| `news_feed_get_exchange_announcements` | Official exchange notices (listings, delistings, maintenance). Media headlines → `news_feed_search_news`. | `exchange` / `platform`; `query`; `coin`; `announcement_type` (listing / delisting / maintenance / all); `from` / `to` (Unix s); `limit` (max 100 when set) |
| `news_feed_get_social_sentiment` | Per-coin aggregate sentiment (score, split, mentions, sample posts). Default window 24h; coin optional (defaults apply server-side, often BTC). | `coin`; `time_range` (1h / 24h / 7d) |
| `news_feed_get_mention_burst` | 24h multi-platform mention burst / growth / direction. | `coin` (required in practice); `window` (24h); `platforms` (all / gate_square / binance_square / twitter / telegram / youtube / reddit / discord) |
| `news_feed_get_hot_topics` | Top 2–4 social themes for a coin (latest 4h window). | `coin` (required in practice); `window` (4h); `limit` (2–4, default 4); `platforms` |

---

## 2. Events & market moves (5)

Research only — not trade execution or guaranteed attribution.

| Tool | Description | Parameters |
|------|-------------|------------|
| `news_events_get_latest_events` | Filtered event list / timeline (`event_id`, impact direction fields). | `event_type`; `coin`; `time_range` (1h / 24h / 7d; exclusive with absolute times); `direction` (positive / negative / neutral / mixed / unknown / all); `limit` (default 20, max 100); `start_time` / `end_time`; `cursor` |
| `news_events_get_event_detail` | Full detail for one digest `event_id`. | **`event_id`** (required) |
| `news_events_explain_market_move` | Explain drivers of a recent coin price move in a window. | `query`; `coin`; `time_range` (30m / 1h / 2h / 4h / 24h, default 2h); `mode` (auto / price_move / event_impact); `lang` (zh / en) |
| `news_events_get_market_move_report` | Fetch a stored market-move report. Lookup order: `report_id` > `event_id` > latest for `symbol`. | `symbol`; `report_id`; `event_id` |
| `news_events_list_market_move_reports` | List stored reports for a symbol by `updated_at` window. | `symbol`; `start_time`; `end_time`; `limit` (default 20, max 100) |

---

## 3. Prediction markets (5)

| Tool | Description | Parameters |
|------|-------------|------------|
| `news_prediction_get_volume_delta_ranking` | Daily ranking by volume change (UTC date). | `date_utc` (YYYY-MM-DD); `limit` (default 20, max 100); `venue` (polymarket / predict_fun); `category`; `status` (active / closed / resolved / all, default active) |
| `news_prediction_get_fastest_rising_ranking` | Daily ranking by probability rise. | same as volume-delta ranking |
| `news_prediction_get_market_orderbook` | Live current order book (`mode=current` only). | `venue` (polymarket / predict_fun); `market_id`; `depth` (1–20, default 20); `mode` (current) |
| `news_prediction_search_events` | Search prediction events. | `query`; `coin` (not alone without query/category); `category`; `status` (default active); `venue`; `sort_by` (attention / volume / liquidity / recently_listed / probability_change / volume_delta_today); `limit` (default 20, max 100); `page_token`; `with_markets` |
| `news_prediction_get_event_signal` | One event signal by `event_ref` (`venue:venue_event_id`). Outcomes, volume flow; optional markets. | `event_ref`; `window` (1h / 24h / 7d, default 24h); `venue`; `include_markets` (default true) |
