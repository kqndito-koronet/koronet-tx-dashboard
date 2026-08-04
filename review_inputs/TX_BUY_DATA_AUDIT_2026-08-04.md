# TX BUY data audit — MVP evidence boundary

**Scope:** BUY only. No dashboard/UI claim is implied. Audited 2026-08-04 from checked-in TX exports and SQL provenance. **Rule:** BUY never uses bunches; an offline purchase is not *leakage* until the same need was available online at that moment.

## Decision-ready now vs. not yet

| Metric / decision | Current source, grain and as-of | Account key / window | Exact semantic | Coverage (P1 / TA / IMPL) | Trust | Contradiction / display rule | Wednesday materialization |
|---|---|---|---|---|---|---|---|
| Koronet BUY GMV, online BUY GMV, online share, MoM/QoQ/YoY | `metrics_v2_buy.json`; `PROCUREMENTS_SV`; company-name aggregate; generated Jul-31, as-of Jul-30 | normalized company name; YTD/current/prior month/quarter | `SUM(total_cost)` for `ks_flag=TRUE`; online is exporter-defined online channel population; **Koronet procurement only**, not company purchases | 4/4, 9/9, 9/14 exact normalized-name joins | **Verified observed** | Windows may be displayed only within the same metric block. Price's YTD/current/prior-quarter percentages are distinct readings, not a trend claim without a monthly series. | Daily company/location × channel rollup with declared channel taxonomy/version. |
| Offline Koronet procurement GMV | derived only as `buy_total - buy_online` from same V2 field/window | same name/window as V2 | Observed non-online **Koronet** procurement, not all offline purchases at the wholesaler | same as V2 | **Verified derived** | Label `non-online Koronet BUY`, not available-to-migrate; it says nothing about online availability. | Included in channel rollup above. |
| Vendor population & recency | `metrics_v2_vendors_full.json`; `PROCUREMENTS_SV`; company-name aggregate; generated Jul-31 | normalized company name; YTD Jan-1–Jul-31; active L30D, dormant 30–90d, churned >90d | distinct supplier `vendor_name`; online/offline vendor populations **overlap**; active/dormant/churned partition total vendors | 4/4, 9/9, 8/14 | **Verified observed, partial** | Do not sum online + offline vendors or call offline vendors an availability gap. One vendor can transact both ways. | Company/location × canonical vendor ID × channel × first/last purchase × GMV; preserve aliases. |
| Legacy vendor gap/leakage cost | `vendor_detail.json`; direct dashboard `company_id`; no file-level provenance/as-of | company_id; varying/undocumented | vendor counts; some records include `gap_vendors/gap_cost` or `leakage_*` | 4/4, 9/9, 8/14 | **Proxy / blocked for leads** | `leakage_cost` is nonzero for only 3/336 records and lacks a documented same-need availability join. Never use as action/$ opportunity. | Replace with evidence spine below; retain only as archival evidence. |
| Category breadth bought | `supply_matrix_full.json.buy_coverage`; `PROCUREMENTS_SV`; 318 rows; generated Jul-30 | company-name + Salesforce `account_id`, H1 Jan-1–Jul-1 | distinct product-category counts: total purchased and online purchased; `SUM(total_cost)`, `ks_flag=TRUE`, sales cap <100k | 4/4, 9/9, 8/14 via normalized name; no stable direct company-id join | **Verified field / proxy for coverage** | `total - online` is **not** offline category population. No varieties/SKUs supplied. H1 incompatible with V2 YTD unless common window selected. | Product event spine with category/variety/SKU IDs and channel flags on same records. |
| Vendor/category availability & leakage | none | — | Needs offline purchased need matched to online availability at purchase/required date, vendor/product scope | 0/4, 0/9, 0/14 | **Blocked** | Correct state is `needs evidence`, never migration/leakage/supply-gap. | Availability snapshot/event keyed by vendor/product/horizon + buyer company/location; match to line. |
| Purchase anticipation / short–medium–long horizon | `buy_detail.json.anticipation`; top 50 by orders | SFDC account id + name; H1 Jan-1–Jul-1 | `ARRIVAL_DATE → SHIPPING_DATE`, not order-created → need/ship | unknown without robust crosswalk; only 50 rows | **Proxy; no BUY-horizon use** | Do not label bars “how far ahead they buy.” Open Market/Prebook in same file is **SALES_SV** sell-side; exclude from BUY. | Order/created timestamp plus required/arrival/ship date and same channel/product/vendor grain. |
| Connections / activation | `metrics.json` / configuration snapshots, partial | company_id; snapshot varies | K2K connection/config state, not proof of procurement behavior | legacy inventory says K2K data 90/399; not re-certified | **Unknown for decisions** | K2K is commercial agreement/selling motion; never infer active connection, churn or offline migration from config/count alone. | Account-vendor connection lifecycle: connected/activated/deactivated dates and commercial state. |
| Total company purchases / BUY potential & 10pp fee scenario | research/estimate snapshots; no stable validated purchase denominator | company_id + external-research key; as-of per evidence | Model only when purchase model, method, range, currency and confidence explicit | account-specific, not established by BUY source | **Unknown unless research validated** | Never use Core/Koronet BUY or generic 54% fallback as total purchases. Scenario is `10pp × stated base × stated estimated fee rate`. | External research evidence table + approved model/version + validation status, linked by company_id. |

## Correct best-in-class BUY comparison

Do not call highest online BUY percentage “most distributed procurement.” A 100% account can have one vendor; a high vendor count can be concentrated in one vendor.

1. **Online procurement adoption — available now.** Same product model / local comparable / broader cohort ranked by `online Koronet BUY ÷ total Koronet BUY` in the same V2 window. Show median, p75 and named top only with cohort size, window, channel definition and a minimum total-BUY threshold. This is a reference for online adoption, not offline coverage, availability or causality.
2. **Procurement distribution / resilience — not available yet.** Same company/location/window with vendor-level GMV: active vendors; procurement GMV share from top 1/top 5; HHI/entropy; online-active vendor share; category/variety/SKU breadth across 0–7d, 8–30d, 31d+. Local comparable first, then same model/segment. This is the “más distribuido” comparator; current JSONs cannot calculate it.

## Concrete Wednesday extract: one procurement event spine

Materialize versioned `tx_buy_procurement_event_spine` at **line or daily company-location × canonical vendor × product × channel** grain:

`company_id`, `company_location_id`, canonical `vendor_id`, raw/vendor alias, `purchase_order_id`, line id, `order_created_at` (or explicitly absent), required/arrival/ship dates, `sales_channel` + normalized channel, category id/name, variety id/name, SKU/product id, quantity, `total_cost`, currency, connection state/history, `available_online_at_purchase`, available supplier, availability horizon bucket.

Derive and persist separately (not in UI semantics):

- `tx_buy_account_channel_daily` — GMV/PO/items by normalized channel;
- `tx_buy_account_vendor_window` — GMV, online/offline flags, first/last event, active/dormant/churned and concentration;
- `tx_buy_account_product_horizon_window` — category/variety/SKU & GMV by channel × 0–7d/8–30d/31d+ **from order-created to required/ship**;
- `tx_buy_availability_match` — each offline need as `available_online`, `not_available_online`, or `unmatched`, with timestamp.

That allows exactly four states: **migration candidate**, **supply gap**, **needs evidence**, **no material gap observed**.

## Findings that change MVP now

- P1 and TA have full V2 BUY and vendor-recency coverage. IMPL: V2 BUY 9/14; vendor recency 8/14. Missing means data state, not zero.
- `supply_matrix_full` is usable only as labelled H1 total/online **category signal** for P1/TA and 8/14 IMPL. Its Salesforce key/name match needs a canonical crosswalk before becoming spine truth.
- Remove BUY horizon bars or rename logistics proxy. They do not answer “cuánto antes compra”; never substitute sell-side Open Market/Prebook.
- No current source supports an availability/leakage conclusion. The card should retain cells as `not available` / `needs evidence` and create a linked DRAFT investigation, never assert migration.

## Sources inspected

`metrics_v2_buy.json`, `metrics_v2_vendors_full.json`, `vendor_detail.json`, `supply_matrix_full.json`, `buy_detail.json`, `accounts.json`, SQL provenance in `loop2_phase1_buy_channel.sql`, `loop2_procurement.sql`, `loop2_phase1v2_channel_audit.sql`, plus the BUY contract and P0 no-loss inventory.
