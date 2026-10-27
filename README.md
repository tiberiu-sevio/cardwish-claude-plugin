# Cardwish for Claude Code

Graded trading-card market data inside Claude: search cards, read live
listings, sold-price history and market stats, and manage price alerts across
eBay, Catawiki, MySlabs, Goldin, Fanatics and Alt.

## Install

```
/plugin marketplace add tiberiu-sevio/cardwish-claude-plugin
/plugin install cardwish@cardwish
```

On the first tool call Claude Code walks you through connecting your
Cardwish account (OAuth — sign in, click approve, done). A free account at
[cardwish.cc](https://cardwish.cc) includes 10,000 requests a month.

Prefer an API key instead? Skip the plugin and add the server directly:

```
claude mcp add --transport http cardwish https://mcp.cardwish.cc/ --header "Authorization: Bearer YOUR_KEY"
```

Keys live in your [developer portal](https://my.cardwish.cc/keys/).

## What you get

| Tool | What it does |
| --- | --- |
| `search_cards` | Typo-tolerant catalog search (names, sets, card numbers) |
| `get_card` | Catalog detail + market summary for one card |
| `get_market_stats` | Per grade × marketplace × currency: count, min/median/max, lowest ask |
| `get_price_history` | Sold prices — daily buckets or raw sales |
| `list_listings` | Live + sold marketplace listings with links |
| `create_alert` / `list_alerts` / `test_alert` / `delete_alert` | Price and new-listing alerts (email, webhook, Telegram/Discord/Slack) |
| `get_usage` | Your plan, quota and month-to-date usage |

The bundled skill teaches Claude the house rules — resolve the card id
first, never mix currencies, caveat small samples — so answers stay honest.

## Other clients

Claude Desktop / claude.ai, ChatGPT, Gemini CLI, Cursor and plain MCP
setups are covered at [cardwish.cc/connect](https://cardwish.cc/connect/).

## Docs

- API reference: https://api.cardwish.cc/docs
- MCP server: `https://mcp.cardwish.cc/` (stateless Streamable HTTP, OAuth 2.1 or `Authorization: Bearer cw_live_…`)
