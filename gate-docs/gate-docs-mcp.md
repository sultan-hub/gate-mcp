# Gate Docs MCP Tools

**Endpoint**: `https://api.gatemcp.ai/mcp/docs`  
**Auth**: none  
**Transport**: Streamable HTTP  

Related: [Info](../gate-info/gate-info-mcp.md) · [News](../gate-news/gate-news-mcp.md)

---

## Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `docs_research_search_research` | Semantic search across research reports, papers, guides, and tutorials. | `query` (required); `content_type`; `domain`; `source`; `limit` |
| `docs_research_get_coin_research` | Research reports for one or more coins. | `coin` (required, e.g. `BTC` or `BTC,ETH`); `limit` |
| `docs_research_search_papers` | Search academic papers and whitepapers. | `query` (required); `content_type`; `domain`; `min_citations`; `recency`; `limit` |
