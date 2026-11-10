# Amazon Product, Price, and Stock Tracker MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-amazon-product-price-stock-tracker/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-amazon-product-price-stock-tracker) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-amazon-product-price-stock-tracker%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-amazon-product-price-stock-tracker&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-amazon-product-price-stock-tracker)](https://www.npmjs.com/package/@mambalabsdev/mcp-amazon-product-price-stock-tracker) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-amazon-product-price-stock-tracker)](https://www.npmjs.com/package/@mambalabsdev/mcp-amazon-product-price-stock-tracker) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-amazon-product-price-stock-tracker)](https://github.com/mambalabsdev/mcp-amazon-product-price-stock-tracker/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-amazon-product-price-stock-tracker)

An MCP server for the Mamba Labs Amazon Product, Price, and Stock Tracker actor on Apify. Read Amazon prices and stock by ASIN or URL and get change rows between runs.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give ASINs or product URLs on 9 Amazon marketplaces. You get one flat row per product with the buy box price, list price, stock, seller, and rating, read for that marketplace's own delivery country, plus a free change row when a price moved or a product went out of or back into stock since the last run.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-amazon-product-price-stock-tracker": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-amazon-product-price-stock-tracker"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `track_amazon_products` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "What is the current price and stock of these Amazon ASINs?"
- "Check amazon.de for this product and tell me who sells it."
- "Which of my tracked Amazon products changed price since the last run?"

## Tool

`track_amazon_products`. One tool. Example arguments:

```json
{
  "products": [
    "B00MNV8E0C"
  ],
  "marketplace": "amazon.com"
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `products` | array | no | ASINs or Amazon product URLs, up to 1,000 per run. A URL uses the marketplace in the URL; a bare ASIN uses the Marketplace setting. |
| `marketplace` | string | no | The store a bare ASIN is read from. |
| `track_changes` | boolean | no | true (default) or false. With true, the actor keeps each product's last reading in a key value store in your account, and a later run writes a change row when the price moves or the product goes out of or back into stock. |
| `state_key` | string | no | Optional name for the set of last readings, when you keep several separate tracking lists. Letters, numbers, dots, dashes, underscores. |
| `previous_snapshot` | array | no | Optional. Rows from an earlier run of this actor, as a JSON list. When given, changes are computed against these rows, not the stored readings. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |
| `proxy_mode` | string | no | auto (default): amazon.com and amazon.ca are read through the Apify proxy pool with residential as the fallback; other marketplaces use residential addresses in that country, because Amazon prices for the delivery country. residential: every product uses residential addresses in its marketplace country and is billed as product-row-local. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `product-row`, one product from amazon.com or amazon.ca | $0.0056 | $0.00532 | $0.00504 | $0.00476 |
| `product-row-local`, one product from any other marketplace, or with Proxy mode residential | $0.015 | $0.01425 | $0.0135 | $0.01275 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

Change rows, blocked products, missing products, unreadable pages, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/amazon-product-price-stock-tracker](https://apify.com/mambalabs/amazon-product-price-stock-tracker)

## License

MIT. See [LICENSE](LICENSE).
