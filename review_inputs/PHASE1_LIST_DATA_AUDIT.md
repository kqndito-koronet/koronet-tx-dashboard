# LIST Data Audit — Accounts MVP

**Scope:** data only; no dashboard edits. **Decision:** the MVP can show a
three-way **sales-assortment proxy** (online / offline / best-in-class) and a
separate **last-sale recency** chart. It cannot claim current catalogue,
published inventory, Open Market uploads, or true forward anticipation.

## Evidence inventory

| Field / intended UI | Source | Grain / account key | As-of & window | Semantics / trust | Coverage & contradiction | Wednesday materialization |
|---|---|---|---|---|---|---|
| Sales categories, varieties, SKUs — account breadth proxy | `loop2_list_usage_v2.json` | company × `company_id`; `sale_item_id` / `product_variety` / `product_category` | run Jul-27; H1 Jan-Jun | **Sales proxy**, all channels; not listed/current inventory. High for historical breadth. | 339 rows. SKU grain is broad `sale_item_id`; it is not comparable to `skus_online_offline`. | Standardize a one-row company/date/channel/dimension table, with explicit grain. |
| Online sales-recency bars | `loop2_list_time_depth_v2.json` | company × `company_id` × `product_description` | run Jul-27; H1; cutoff Jun-30 | MAX shipping date per online-sold variety. **How recently it sold**, not how far ahead sold/listed. High for stated recency. | 280 rows; online only. Product-description grain differs from `list_usage`. | Persist six recency buckets as `last_sale_recency`; never call time depth / availability horizon. |
| Offline sales-recency bars | `time_depth_offline.json` | company × `company_id` × `product_description` | run Jul-31; H1; cutoff Jun-30 | Same last-shipment recency calculation offline. High for stated recency. | 110 supplied records / full query says 205 companies; selection is 100+ offline varieties plus keys. | Re-run full result; use same company IDs / variety grain / cutoff as online. |
| Best-in-class online recency | `time_depth_offline.json.best_in_class` pointing to `loop2_list_time_depth_v2.json` | Kennicott `44150`, online product-description | H1 cutoff Jun-30 | Comparable **online recency reference** for large wholesalers, not a causal target or current availability. | One named benchmark; valid only with comparable cohort label. | Materialize comparator selection/version/rationale in benchmark table. |
| Online/offline SKU breadth | `skus_online_offline.json` | normalized company name × `product_description`; dedup `sale_item_id` | generated Jul-31; YTD Jan-Jul | Channel-specific **sold SKU proxy**, not catalogue. High, with explicit channel definitions. | 342 companies. Cannot combine with `list_usage` SKU values: Zeidler is 1,042 here vs 34,928 there because grain/dedup differs. | Canonicalize the product-grain definition; publish both only if named distinctly. |
| Categories and online sell-side sale-items | `listing_detail.json.varieties` | company name + SFDC `account_id`; category / sale_item | Jul-30; H1 | Historical sales proxy. Source says precise SKU count may be fan-out inflated. | 339 category / 279 time rows. No explicit offline category population—`total - online` is not an offline population. | Replace with deduped channel-specific category query; use account/company key bridge. |
| Shipping→arrival buckets | `listing_detail.json.time_depth` | company + SFDC account_id + sale_item | H1 | DATEDIFF(shipping, arrival); operational shipping/arrival relationship, **not order placement or sell-ahead**. | Zeidler's buckets are all zero although total is 8,424: missing/invalid date availability; do not render. | Needs `CREATED_DATE`/`ORDER_DATE` + shipping/fulfilment fields for true anticipation. |
| Bunch format observed | `bunches.json`; online format supplemental `loop2_sell_format_time_v2.json` | `company_id`; latter name/SFDC join | window unspecified in `bunches`; format H1 where present | Historical bunch GMV / online sales format. **Not current eShop visibility**. | 43 accounts only; config conflicts exist (e.g. Zeidler flag OFF while observed online bunch sales). | Materialize online/offline bunch GMV + count/breadth with one timeframe; separately verified setting snapshot. |
| Config flags (bunches, MaxAge, Future, on-hand) | `config.json`, `config_backfill.json` | `company_id` | snapshot date not consistently carried | Potential capability evidence only if audited. | 26 relevant snapshots are unaudited and 21 conflict with observed behavior; treat those values as UNKNOWN. | Pull audit/version/updated_at per setting; store raw + verified truth state. |
| Open Market uploads | none | — | — | No direct upload/published-inventory event in available extracts. `buy_detail.json` has SELL-side `ORDER_TYPE`, not uploads. | Cannot say zero Open Market GMV means zero uploads/future inventory. | Product/data export: company-keyed upload/listing events, created/updated, item, qty, availability, source. |

## Required MVP display contract

1. **Matrix:** Account online sales proxy | Account offline sales proxy | Best-in-class online sales proxy. Rows are categories, varieties, SKUs, bunches and recency. Each cell names its source/window/grain; unavailable stays `—`, not zero.
2. **Bars:** label `Last sale recency by variety — H1, cutoff Jun-30`, with online / offline / benchmark. They answer *how recently products sold*, not *how far in advance buyers can buy*.
3. The desired `0–7 / 8–14 / 15–30 / 31–90 / 90d+ ahead` bars are a different metric. They remain empty with a request: company/channel/product `created_at`, promised/arrival date and shipped/fulfilled date. Only after those fields exist can labels say “sold/purchased X days ahead.”
4. **Bunches** is LIST/SELL, never BUY. Show observed online/offline format only on a common period; show setting only when audited. Do not use industry 95% as account TAM.
5. **Open Market** in LIST means what the wholesaler uploads/publishes. Until upload data exists, show an explicit `not measured` gap, never an order-type proxy as upload truth.

## Material contradictions to protect in UI

- Zeidler: `list_usage` all-channel varieties = 1,011; online recency varieties = 616; offline recency = 926. These are compatible only as different historical channel proxies, not a parity calculation.
- Zeidler: `skus_online_offline` total = 1,042, while `listing_detail` total sale items = 34,928. Different product grain/dedup; never place them as a single comparable SKU row.
- Zeidler: unaudited config says bunches OFF, but observed online format says 100% bunches. Mark config UNKNOWN; no blocker/lead.
- `listing_detail.time_depth` gives Zeidler zero in every timing bucket with total 8,424: suppress it as invalid for timing.

## Wednesday request package

1. **Current catalog/listings snapshot** keyed by Koronet company ID: listing ID, product/category/variety/SKU, publish status, quantity, price, availability start/end, updated_at, listing path (K2K / manual / future PO / SO / API), bunch flag.
2. **Comparable offline availability** on the same product taxonomy and snapshot date.
3. **True anticipation** for BUY and SELL: order/created timestamp, promised/arrival date, shipping/fulfillment date, channel, company ID, product ID. Aggregate to common 0–7 / 8–14 / 15–30 / 31–90 / 90d+ buckets.
4. **Open Market events**: company ID, upload/listing ID, created/updated, source, future availability and active status.
5. **Configuration audit feed**: company ID, key, current value, updated_at, source, audit status/version—so configuration can become evidence rather than a speculative lead.

## Release gate

Ship the matrix/recency bars only under the labels above. Do not emit LIST "blockers", lost-TAM dollars, parity claims or future-availability claims from current LIST data. Those need the Wednesday materializations or an account-specific verified review.
