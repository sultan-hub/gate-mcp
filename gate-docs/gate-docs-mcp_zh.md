# Gate Docs MCP 工具

**接入地址**：`https://api.gatemcp.ai/mcp/docs`  
**认证**：无  
**传输**：Streamable HTTP  

相关：[Info](../gate-info/gate-info-mcp_zh.md) · [News](../gate-news/gate-news-mcp_zh.md)

---

## 工具列表

| 工具 | 说明 | 参数 |
|------|------|------|
| `docs_research_search_research` | 跨库语义检索：研报、论文、指南、教程等。 | `query`（必填）；`content_type`；`domain`；`source`；`limit` |
| `docs_research_get_coin_research` | 按币种获取相关研报。 | `coin`（必填，如 `BTC` 或 `BTC,ETH`）；`limit` |
| `docs_research_search_papers` | 检索学术论文与白皮书。 | `query`（必填）；`content_type`；`domain`；`min_citations`；`recency`；`limit` |
