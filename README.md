# LemMeBuyIt MCP

Hosted [Model Context Protocol](https://modelcontextprotocol.io) plugin for Grok, Cursor, Claude, and other MCP clients. Search live product catalogs across 103 US retailers, compare prices, look up stock, and pull price history.

This repository is the public plugin package. The server runs at `mcp.lemmebuyit.com`. There is no local server process and no secrets in this repo.

- **MCP endpoint:** `https://mcp.lemmebuyit.com/mcp` (Streamable HTTP)
- **Auth:** OAuth 2.1 + PKCE (client-managed). Optional `x-api-key` for clients that cannot do OAuth — get a free key at [lemmebuyit.com](https://www.lemmebuyit.com).
- **Docs:** https://www.lemmebuyit.com/docs
- **REST API:** https://api.lemmebuyit.com/docs
- **Official MCP registry:** `com.lemmebuyit/lemmebuyit-mcp`

## What it does

- Search product catalogs across 103 US retailers
- Look up a product by SKU, GTIN/UPC, brand, or category
- Live price, stock, and rating fields on search and product detail
- Weekly price history and Amazon ASIN / Buy Box history (paid plans)

## Tools

Seven read-only tools. No writes, no checkout, no destructive operations.

| Tool | Description |
|------|-------------|
| `get_status` | Health and plan for the connected account |
| `list_retailers` | Retailers available to this connection |
| `get_retailer_categories` | Category tree for one retailer |
| `search_retailer_products` | Search a retailer by text or barcode, with price and stock filters |
| `get_retailer_product` | Full details for a specific product |
| `get_product_price_history` | Weekly retailer price history (paid) |
| `get_amazon_asin_price_history` | Weekly Amazon price history by ASIN (paid) |

## Install

### Cursor / Grok Bot

Install from the Cursor Marketplace once listed, or load this repo as a local plugin:

```bash
mkdir -p ~/.cursor/plugins/local
ln -s /path/to/lemmebuyit-mcp ~/.cursor/plugins/local/lemmebuyit
```

The plugin points at `https://mcp.lemmebuyit.com/mcp`. On first use the client should prompt for OAuth. No API key belongs in `mcp.json`.

### Grok Build

After this plugin is in [xai-org/plugin-marketplace](https://github.com/xai-org/plugin-marketplace), install it from Grok Build's plugin catalog. The catalog entry pins a commit SHA of this repo.

### Any MCP client

```json
{
  "mcpServers": {
    "lemmebuyit": {
      "url": "https://mcp.lemmebuyit.com/mcp"
    }
  }
}
```

Claude Code:

```bash
claude mcp add --transport http lemmebuyit https://mcp.lemmebuyit.com/mcp
```

## Network and credentials

| Endpoint | Why |
|----------|-----|
| `https://mcp.lemmebuyit.com/mcp` | Hosted MCP (tools) |
| `https://mcp.lemmebuyit.com/.well-known/oauth-protected-resource/mcp` | OAuth resource metadata |
| `https://www.lemmebuyit.com/.well-known/oauth-authorization-server` | OAuth authorization server metadata |
| `https://www.lemmebuyit.com` | Sign-in / consent |

Credentials: a LemMeBuyIt account via OAuth, or an API key the *user* configures in their client. This package does not ship keys, tokens, or env vars.

Privacy: https://www.lemmebuyit.com/privacy  
Terms: https://www.lemmebuyit.com/terms  
Support: admin@lemmebuyit.com

## License

MIT. See [LICENSE](LICENSE).
