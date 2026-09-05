---
name: lemmebuyit
description: Use LemMeBuyIt's hosted MCP to search live US retailer catalogs, look up a product, compare prices, and pull price history. Trigger when the user asks to search Walmart, Target, Home Depot, or other LemMeBuyIt retailers, look up a GTIN/UPC/SKU, check stock, compare prices, or get Amazon ASIN / retailer price history.
---

# LemMeBuyIt product search

Use the `lemmebuyit` MCP server at `https://mcp.lemmebuyit.com/mcp`. Do not scrape retailer sites and do not call `api.lemmebuyit.com` unless the user asked for the REST API.

## Auth

The server is OAuth-protected. The client handles sign-in. If tools are unauthorized, tell the user to connect LemMeBuyIt (OAuth) or paste an API key in the client MCP settings. Never put keys in repo files.

## Tools

All seven are read-only. Each call counts as one request.

| Tool | When to use |
|------|-------------|
| `get_status` | First call after connect. Confirms the connection and plan. |
| `list_retailers` | Discover retailer ids the connection can search. |
| `get_retailer_categories` | Resolve a retailer-specific category path before a filtered search. |
| `search_retailer_products` | Search one retailer by text, GTIN/UPC/EAN, or category. Supports price/stock filters and cursor pagination (`after`). |
| `get_retailer_product` | Full detail for one `retailer` + `product_id`. |
| `get_product_price_history` | Weekly retailer price history (paid). Free plans return a structured upgrade prompt — show that, do not invent history. |
| `get_amazon_asin_price_history` | Weekly Amazon price / Buy Box history by ASIN (paid). Same upgrade rule. |

## How to search

1. `get_status` if the connection is unproven.
2. `list_retailers` when the retailer id is unknown.
3. `search_retailer_products` with the retailer id. Prefer GTIN when the user has a barcode.
4. `get_retailer_product` for a specific SKU.
5. Price-history tools only after a product id or ASIN is known.

On `429 rate_limited`, wait `retry_after` seconds and retry. Do not fabricate prices, stock, ASINs, or history.

Docs: https://www.lemmebuyit.com/docs
