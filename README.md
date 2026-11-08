# Takealot Price Drop Tracker (South Africa)

Watch Takealot for the products you care about and get **only what changed**: price drops, price rises, new listings and back-in-stock events. Built for resellers, deal-hunters, price-comparison sites and anyone tracking South African retail prices.

## Who it is for
- Resellers and dropshippers who need to spot supplier price drops or competitor undercutting.
- Deal sites and Telegram/WhatsApp channels that post bargains.
- Analysts tracking South African consumer electronics and appliance prices.

## How the alert works
1. Enter keywords (e.g. `airfryer`, `rtx 4060`, `iphone 15`).
2. First run records every product's price and stock (and emits them all as `new`).
3. Every later run with the same **State name** emits only rows whose `change` is `price_drop`, `price_rise`, `new` or `back_in_stock`. Set *Minimum price change %* to ignore noise.

**Schedule tip:** run it once or twice a day with Apify Schedules, then connect an Integration (Slack, email, Google Sheets, webhook).

## Example output
```json
{"id":98945485,"title":"Russell Hobbs Digital 9L Air Fryer","change":"price_drop","priceZAR":1199,"previousPriceZAR":1499,"changePercent":-20,
 "rating":4.8,"reviews":2212,"inStock":true,"url":"https://www.takealot.com/.../PLID98945485","checkedAt":"2026-11-08"}
```

## Notes
- Uses Takealot's public search endpoint; no login, no personal data. Prices in ZAR; when a product has several sellers, the lowest price is used.
- Use a different State name for each watchlist.
- Not affiliated with Takealot.

## Call it from code
Get a token at https://console.apify.com/settings/integrations, then:

```python
from apify_client import ApifyClient

client = ApifyClient("<YOUR_APIFY_TOKEN>")
run = client.actor("datakit/takealot-price-tracker").call(run_input={"keywords":["airfryer"],"stateKey":"airfryers"})
for row in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(row)
```

```bash
curl -X POST "https://api.apify.com/v2/acts/datakit~takealot-price-tracker/run-sync-get-dataset-items?token=<YOUR_APIFY_TOKEN>" \
  -H "Content-Type: application/json" -d '{"keywords":["airfryer"],"stateKey":"airfryers"}'
```

## Run it on the Apify Store
Store page: https://apify.com/datakit/takealot-price-tracker  (free to try; you only pay Apify platform usage)

Made by [datakit](https://github.com/datakit457). Part of the [datakit tools list](https://datakit457.github.io/).

## FAQ
**Why did my first run return everything?** The first run has no history, so every product is `new`. Later runs with the same State name return only changes.

**How often should I run it?** Once or twice a day is enough; Takealot prices rarely move hourly.

**Does it track one specific product?** Search by its exact title or model number; use *Max products per keyword* = 36 to keep the run small.

**Is it free?** Platform usage only (a typical run costs a fraction of a cent of compute).

## Notes
- Uses Takealot's public search endpoint; no login, no personal data. Prices in ZAR; when a product has several sellers, the lowest price is used.
- Use a different State name for each watchlist.
- Not affiliated with Takealot.
