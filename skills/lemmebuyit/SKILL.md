---
name: lemmebuyit
description: Use LemMeBuyIt's hosted MCP to search live US retailer catalogs, compare prices, find promotions, rank resale flips, and pull price history. Trigger when the user asks to search a retailer, look up a GTIN/UPC/SKU, check stock, find coupons, scan for flips, or get Amazon ASIN / retailer price history.
---

# LemMeBuyIt product search

Use the `lemmebuyit` MCP server at `https://mcp.lemmebuyit.com/mcp`. Do not scrape retailer sites and do not call `api.lemmebuyit.com` unless the user asked for the REST API.

The live retailer set changes. Call `list_retailers` instead of assuming a fixed count.

## Auth

The server is OAuth-protected. The client handles sign-in. If tools are unauthorized, tell the user to connect LemMeBuyIt (OAuth) or paste an API key in the client MCP settings. Never put keys in repo files.

## Tools

Eleven tools. Search, lookup, promotions, and history reads do not change retailer data. `check_skill_run` reserves one skill run against the plan limit.

| Tool | When to use |
|------|-------------|
| `get_status` | First call after connect. Confirms the connection and plan. |
| `list_retailers` | Discover retailer ids the connection can search. |
| `get_retailer_categories` | Resolve a retailer-specific category path before a filtered search. |
| `search_retailer_products` | Search one retailer by text, GTIN/UPC/EAN, or category. Supports price/stock filters and cursor pagination (`after`). Sampling, not a full-catalog export. |
| `get_retailer_product` | Full detail for one `retailer_id` + `product_id`. |
| `search_promotions` | Current coupons, sales, and deals. Filter by retailer, category, or text. Active promotions by default. |
| `find_flips` | Rank resale candidates by ROI after fees. Paper estimates. Verify live before buying. |
| `get_product_price_history` | Weekly retailer price history (paid). Free plans return a structured upgrade prompt. Show that. Do not invent history. |
| `get_amazon_asin_price_history` | Weekly Amazon price / Buy Box history by ASIN (paid). Same upgrade rule. |
| `check_listing_eligibility` | Paid. Whether the user's connected Amazon seller account can list each ASIN. |
| `check_skill_run` | Once, at the start of a packaged LemMeBuyIt skill, when that skill says to. Not a general search tool. |

## How to search

1. `get_status` if the connection is unproven.
2. `list_retailers` when the retailer id is unknown.
3. `search_retailer_products` with the retailer id. Prefer GTIN when the user has a barcode.
4. `search_promotions` when the user wants coupons, promo codes, or current deals.
5. `get_retailer_product` for a specific SKU.
6. Price-history tools only after a product id or ASIN is known.

On `429 rate_limited`, wait `retry_after` seconds and retry. Do not fabricate prices, stock, ASINs, or history.

## Resale flips

`find_flips` returns product families ranked by ROI after marketplace fees, priced against the better of the Amazon buy box and the Walmart cross-match. Rows are BUY or THIN. These are paper estimates from catalog matches, not a live buy signal.

- Pass a retailer id, or `retailer_id: "all"` to scan across stores.
- `window: "today"` is the daily-deals report.
- A merchandising vertical (`shoes_apparel`, `electronics`, and the other server enums) is cross-store: set `retailer_id: "all"` and pass `vertical`.
- `exhaustive: true` with one retailer id pages that store's ROI-screened catalog. It does not apply to `all`, to a vertical, to `window: "today"`, or to Walmart.
- Free plans get a top-10 taste and cannot paginate. Paid plans page with `meta.next_cursor` until `meta.has_more` is false.
- Shortlist winners, then confirm live prices and listing eligibility before telling the user to buy.

## Amazon listing eligibility

`check_listing_eligibility` is paid. Pass the ASINs to check. If the result says the seller account is not connected, tell the user to connect it at https://www.lemmebuyit.com/dashboard/integrations and stop. Do not guess ELIGIBLE, GATED, or RESTRICTED.

Docs: https://www.lemmebuyit.com/docs
