# TCGplayer Price Trends & Sales Velocity Tracker

TCGplayer card and sealed product prices with 30 and 90 day price change, sales velocity and days of supply per printing.

[![Run on Apify](https://img.shields.io/badge/Run%20on-Apify-0f9f74)](https://apify.com/datagrit/tcgplayer-price-trend-tracker) [![Docs](https://img.shields.io/badge/docs-getdatagrit.github.io-0e1726)](https://getdatagrit.github.io/tcgplayer-price-trend-tracker/)

**from $7.00 per 1,000 results + $10 per run (pay per result; the rate depends on your Apify plan).** Export as JSON, CSV or Excel, call it through the API, or schedule it on Apify.

## What it does

TCGplayer Price Trends & Sales Velocity Tracker reads the public TCGplayer catalogue and returns, for every printing and language of a card or sealed product, the current market price, the 30-day and 90-day price change, the units sold in 30 and 90 days, sales per day and days of supply. Results are clean records you can export as JSON, CSV or Excel, call through the Apify API, or connect to n8n, Make and AI agents through MCP.
It is built for card resellers, collectors, investors and price-tracking tools that need to know which cards are moving, not only what they cost today. Choose a game, add search terms, set your filters and run.

## Quick start

1. Open [TCGplayer Price Trends & Sales Velocity Tracker on Apify Store](https://apify.com/datagrit/tcgplayer-price-trend-tracker) and click **Try for free**.
2. Fill in the input form (or paste the JSON below) and run it.
3. Download the dataset, or fetch it from the API.

```json
{
  "searchQueries": [
    "charizard"
  ],
  "productLine": "pokemon",
  "maxProductsToScan": 30,
  "maxItems": 50
}
```

## Input

| Field | Type | What it does |
|---|---|---|
| `searchQueries` | array | Card or product names to search on TCGplayer, for example charizard or booster box. Every term is searched separately and a product found twice is returned once. Leave empty to read a whole product line or set. |
| `productLine` | string | Limit the search to one trading card game. Choose All games to search every line, which works best together with a search term or a set. |
| `setNames` | array | Optional. Keep only products from these sets, for example Base Set or swsh-crown-zenith. Use the set name as shown on TCGplayer or the last part of the set URL. A set that does not exist in the chosen game stops the run with an error that lists similar names. |
| `rarities` | array | Optional. Keep only these rarities, for example Secret Rare or Special Illustration Rare. Names follow TCGplayer and differ by game. |
| `productType` | string | Single cards and sealed products (booster boxes, packs, decks) are both in the catalogue. Sealed products are the ones sold as Unopened. |
| `printings` | array | Optional. Keep only these printings, for example Normal, Foil, Holofoil or Reverse Holofoil. Every printing has its own price and sales history. |
| `languages` | array | Optional. Keep only these languages, for example English or Japanese. Leave empty for all languages. |
| `minMarketPrice` | number | Keep only printings whose Near Mint market price (Unopened for sealed products) is at least this amount. 0 disables the filter. |
| `maxMarketPrice` | number | Keep only printings whose market price is at most this amount. 0 disables the filter. |
| `minSold30d` | integer | Keep only printings that sold at least this many units in the last 30 days at the main condition. Use it to drop cards nobody buys. 0 disables the filter. |
| `minSold90d` | integer | Keep only printings that sold at least this many units in the last 90 days at the main condition. 0 disables the filter. |
| `minPriceChange30dPct` | number | Optional. Keep only printings whose market price rose at least this many percent over the last 30 days. Enter 20 to find risers. Negative values are allowed. Printings without a 30-day comparison are skipped. |
| `maxPriceChange30dPct` | number | Optional. Keep only printings whose market price changed at most this many percent over the last 30 days. Enter -15 to find fallers. |
| `sortBy` | string | Order in which TCGplayer returns products before they are scanned. Relevance follows TCGplayer popularity. Highest price first is useful with a minimum price. |
| `maxProductsToScan` | integer | How many products to read the sales history of, per run across all search terms. One product gives one row per printing and language, so you often get more rows than products. TCGplayer search returns at most 9950 products per search. |
| `maxItems` | integer | Stop after this many rows in total. Each row is one billed result. |
| `proxyConfiguration` | object | Optional proxy. Leave disabled: the TCGplayer endpoints used here are public and need none. |

## Output

| Field | Type | Description |
|---|---|---|
| `found` | boolean | False only on the single status row emitted when nothing matched; every other field of that row is empty. |
| `productId` | integer | TCGplayer product id. One product can produce several rows, one per printing and language. |
| `productName` | string | Name of the card or sealed product as listed on TCGplayer. |
| `productLine` | string | Trading card game (product line) the product belongs to. |
| `setName` | string | Set or expansion name. |
| `setCode` | string | Short set code used by TCGplayer, when it has one. |
| `cardNumber` | string | Collector number printed on the card. Empty for sealed products and for cards without a number. |
| `rarity` | string | Rarity as named by TCGplayer. Empty for some sealed products. |
| `releaseDate` | string | Release date of the card or product in YYYY-MM-DD form, when TCGplayer has it. |
| `variant` | string | Printing of the product, for example Normal, Foil, Holofoil or Reverse Holofoil. Each printing has its own price and sales. |
| `language` | string | Language of the printing. |
| `condition` | string | Condition the price and sales figures refer to: Near Mint for cards, Unopened for sealed products. If a printing has no Near Mint sales series, the condition with the most sales is used. |
| `sealed` | boolean | True when the price series is for an unopened sealed product such as a booster box. |
| `marketPrice` | number | TCGplayer market price of the main condition in the most recent 3-day window. It is an average of recent sales, so it carries over when nothing sold. |
| `priceChange30dPct` | number | Change of the market price between the window about 30 days ago and the latest window. Empty when a price is missing or when the printing had no sales in the last 30 days, because the market price then only carries over. |
| `priceChange90dPct` | number | Change of the market price between the oldest window of the 90-day history (about 87 days ago) and the latest window. Empty when a price is missing, when the printing has less than 84 days of history, or when it had no sales in 90 days. |
| `lowSalePrice90d` | number | Lowest single sale price (without shipping) in the 90-day history, taken over windows that had sales. |
| `highSalePrice90d` | number | Highest single sale price (without shipping) in the 90-day history, taken over windows that had sales. |
| `sold30d` | integer | Units sold at the main condition in the last 30 days, summed from the 3-day windows. |
| `sold90d` | integer | Units sold at the main condition in the 90-day history, summed from the 3-day windows. |
| `soldPerDay` | number | Average units sold per day at the main condition over the 90-day history (sold90d divided by the days covered). |
| `lastSaleWindowStart` | string | Start date (YYYY-MM-DD) of the most recent 3-day window in which this printing sold. The sale happened on or up to 2 days after this date. |
| `allConditionsSold90d` | integer | Units sold in the 90-day history across all conditions of this printing and language. |
| `marketPriceLightlyPlayed` | number | Latest market price of the Lightly Played condition. Empty when this condition has no price series. |
| `marketPriceModeratelyPlayed` | number | Latest market price of the Moderately Played condition. Empty when this condition has no price series. |
| `marketPriceHeavilyPlayed` | number | Latest market price of the Heavily Played condition. Empty when this condition has no price series. |
| `marketPriceDamaged` | number | Latest market price of the Damaged condition. Empty when this condition has no price series. |
| `listingsCount` | integer | Number of active seller listings for the whole product, across all printings, languages and conditions. |
| `lowestListingPrice` | number | Lowest active listing price for the whole product, across all printings and conditions. Not specific to this printing. |
| `medianListingPrice` | number | Median price of the active listings for the whole product, when TCGplayer reports it. |
| `daysOfSupply` | number | Active listings of the whole product divided by its average daily sales across all printings and conditions. A low number means stock sells out fast; empty when nothing sold in 90 days. |
| `productUrl` | string | TCGplayer page of the product. |
| `imageUrl` | string | Product image on the TCGplayer image server. |
| `scrapedAt` | string | ISO 8601 timestamp of extraction. |

Sample record:

```json
{
  "found": true,
  "productId": 219059,
  "productName": "Charizard GX - 9/68 (#60 Charizard Stamped)",
  "productLine": "Pokemon",
  "setName": "Battle Academy",
  "setCode": "BTA",
  "cardNumber": "009/068",
  "rarity": "Promo",
  "releaseDate": "2020-07-31",
  "variant": "Holofoil",
  "language": "English",
  "condition": "Near Mint",
  "sealed": false,
  "marketPrice": 14.57,
  "priceChange30dPct": 8.8,
  "priceChange90dPct": 12.1,
  "lowSalePrice90d": 10.2,
  "highSalePrice90d": 20,
  "sold30d": 35,
  "sold90d": 138,
  "soldPerDay": 1.53,
  "lastSaleWindowStart": "2026-09-28",
  "allConditionsSold90d": 275,
  "marketPriceLightlyPlayed": 10.67,
  "marketPriceModeratelyPlayed": 9.6,
  "marketPriceHeavilyPlayed": 7.4,
  "marketPriceDamaged": 5.2,
  "listingsCount": 114,
  "lowestListingPrice": 7.21,
  "medianListingPrice": 13.9,
  "daysOfSupply": 54.2,
  "productUrl": "https://www.tcgplayer.com/product/219059",
  "imageUrl": "https://product-images.tcgplayer.com/fit-in/437x437/219059.jpg",
  "scrapedAt": "2026-09-30T08:00:00.000Z"
}
```

## Call it from code

Runnable examples are in [`examples/`](examples). Replace `YOUR_APIFY_TOKEN` with the token from your Apify account settings.

```bash
curl -X POST "https://api.apify.com/v2/acts/datagrit~tcgplayer-price-trend-tracker/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"searchQueries":["charizard"],"productLine":"pokemon","maxProductsToScan":30,"maxItems":50}'
```

## FAQ

**How fresh is the data?**  
Every run reads TCGplayer live. The market price is the value of the latest 3-day window of sales, so it can differ by a few cents from the price shown on the product page right now (for example 14.57 against 14.63 for the same foil card). Use it for trends and comparisons between runs, not as a live quote.

**How many products and rows can one run read, and how long does it take?**  
One run reads up to 9950 products per search term, set by **Maximum products to scan**. Each product needs one extra request for its sales history, and the Actor keeps a short pause between batches, so expect roughly 6 seconds per 10 products and about a minute for the default 100 products. Rows are limited by **Maximum results**.

**Why do I get more rows than products?**  
Each printing and language is its own row; a card with Normal and Foil copies in two languages gives up to four rows.

**Why are the price change fields empty for some rows?**  
The printing had no sales in the window, or it is newer than 84 days for the 90-day change.

**Can I schedule runs?**  
Yes, use Apify schedules or call the Actor from your own workflow and compare runs over time.

**Something looks wrong.**  
Open an issue with the input you used; changes at the source are fixed quickly.

## More from datagrit

- [TED Contract Expiry Radar - Recompete Leads](https://github.com/getdatagrit/ted-contract-expiry-radar) - Find EU public contracts approaching expiry from TED award notices: incumbent, buyer, value, end date and renewal options.
- [UK Contract Expiry Radar - Recompete Leads](https://github.com/getdatagrit/uk-contract-expiry-radar) - UK public contracts ending soon with incumbent supplier, buyer, value and contact - recompete leads from Contracts Finder award notices.
- [French Company Finder - Sirene Financials](https://github.com/getdatagrit/french-company-finder) - French company lead lists from Sirene screened by net result and revenue, with net margin, size, matching establishment and optional directors.
- [GLEIF LEI Lookup - Parents And Subsidiaries](https://github.com/getdatagrit/gleif-lei-ownership-tree) - GLEIF legal entity records with direct and ultimate parents, reporting exceptions and direct subsidiaries.
- [IRS 990 Nonprofit Officers and Compensation](https://github.com/getdatagrit/irs-990-officer-compensation) - Named officers, directors and key employees with pay, hours and titles from IRS e-filed 990, 990-EZ and 990-PF returns.

All Actors: [https://getdatagrit.github.io/](https://getdatagrit.github.io/) · [Apify Store](https://apify.com/datagrit)

---

This repository holds documentation and usage examples. Questions, bug reports and feature requests: use the **Issues** tab of the Actor page on [Apify Store](https://apify.com/datagrit/tcgplayer-price-trend-tracker). Examples are MIT licensed.
