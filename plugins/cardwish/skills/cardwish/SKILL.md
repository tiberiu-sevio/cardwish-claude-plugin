---
name: cardwish
description: Use when the user asks about trading-card prices, values, listings, sales history or price alerts — graded Pokémon, Magic, Yu-Gi-Oh!, One Piece and other TCG cards (PSA/BGS/CGC slabs). Covers "what is X worth", "find me a deal on X", "watch X for me" and market-trend questions via the Cardwish MCP tools.
---

# Cardwish: graded-card market data

Cardwish aggregates live listings and sold prices for graded trading cards
across eBay, Catawiki, MySlabs, Goldin, Fanatics and Alt. The MCP tools are
thin views over that data. Prices are real marketplace data, never estimates.

## Tool flow

1. **Always resolve the card first**: `search_cards` with a short query
   ("charizard base set", "LOB-001"). It returns card ids — every other tool
   needs one. If several plausible matches come back, ask the user which
   printing/variant they mean instead of guessing.
2. **"What is it worth?"** → `get_market_stats` (default 90d window). It
   returns sale count, min/median/max and last sale per grade label ×
   marketplace × currency, plus the live lowest ask. Lead with the median
   for the grade the user asked about; mention the sample size.
3. **Trends and charts** → `get_price_history` (interval "day" for
   chart-ready buckets, "raw" for individual sales).
4. **"Anything for sale?"** → `list_listings` (status "active"). Each
   listing has a `url` — always include it so the user can click through.
5. **"Watch it for me"** → `create_alert` (needs the alerts scope).

## Rules that matter

- **Never mix or convert currencies.** Stats and history are bucketed per
  currency; report each currency separately.
- **Grades are not interchangeable.** A PSA 9 median says little about a
  PSA 10. Filter by `grading_company` + `grade` when the user names one.
- **Small samples deserve a caveat.** One sale in 90 days is an anecdote,
  not a market price — say so.
- **Alert conditions**: `price_below` / `price_above` need `threshold` +
  `currency` (must match the marketplace currency); `new_listing` fires on
  any new match. Optional filters: grading_company, grade, marketplace.
  Suggest a cooldown if the card trades often.
- Quota errors (429) mean the account's monthly request allowance is used
  up — point the user at https://my.cardwish.cc/usage/ rather than retrying.

## Good phrasings to offer users

- "What's a PSA 10 Base Set Charizard selling for lately?"
- "Chart the last 6 months of CGC 9.5 Umbreon VMAX Alt Art sales."
- "Any Goldin or Fanatics listings for a BGS 9.5 LOB Blue-Eyes under $2,000?"
- "Alert me if a PSA 9 Pikachu Illustrator look-alike lists below $500 on eBay."
