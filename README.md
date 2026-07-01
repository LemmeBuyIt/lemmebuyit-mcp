# LemMeBuyIt MCP

Real-time shopping intelligence for AI agents. LemMeBuyIt is a hosted [Model Context Protocol](https://modelcontextprotocol.io) server that lets any MCP-compatible client (Grok, Claude, Cursor, and others) search 86 US retailers, compare live prices, pull price history, and surface genuine deals.

- **MCP endpoint:** `https://mcp.lemmebuyit.com/mcp` (Streamable HTTP)
- **Auth:** `x-api-key` header. Get a free key at [lemmebuyit.com](https://www.lemmebuyit.com).
- **Official MCP registry:** `com.lemmebuyit/lemmebuyit-mcp`
- **API docs:** https://api.lemmebuyit.com/docs

## What it does

- Search product catalogs across 86 US retailers
- Live price comparison with on-sale and discount filtering
- Price history (paid tier)
- Genuine-deal detection that filters out fake or inflated discounts

## Tools

| Tool | Description |
|------|-------------|
| `get_status` | Health and status of the API |
| `list_retailers` | List retailers available to your key |
| `get_retailer_categories` | List categories for a retailer |
| `search_retailer_products` | Search a retailer's catalog (text or barcode) with price and discount filters |
| `get_retailer_product` | Full details for a specific product |
| `get_product_price_history` | Weekly price history for a product (paid) |
| `get_amazon_asin_price_history` | Weekly Amazon price history by ASIN (paid) |

## Connect it to Grok

1. Go to [grok.com/connectors](https://grok.com/connectors), then New Connector and Custom
2. Server URL: `https://mcp.lemmebuyit.com/mcp`
3. Auth header: `x-api-key: <your free-tier key>`

Grok discovers the tools automatically. The same endpoint works in any MCP client that supports remote Streamable HTTP servers.

## Links

- Website: https://www.lemmebuyit.com
- Docs: https://api.lemmebuyit.com/docs
- Pricing: https://www.lemmebuyit.com/pricing

---

This repository is the public landing page and documentation for the hosted LemMeBuyIt MCP service. The server is operated at `mcp.lemmebuyit.com`.
