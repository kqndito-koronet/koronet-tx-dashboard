# Accounts data contract — review baseline

This is the build contract for the Accounts table and its first expanded card.
It distinguishes data that is already usable from data that is available but
cannot yet support the intended business claim. It is not a claim that
Snowflake access is unavailable; the dated source exports below are already
available to the dashboard.

## Directly usable now

| Decision / field | Available data | Contract for display |
|---|---|---|
| Select accounts to investigate | `accounts.json` (399 accounts) | Account identity, cohort, priority, Core/Procurement/eShop flags and go-live state are selection context, not performance proof. |
| Koronet BUY activity | `metrics_v2_buy.json`, as of 2026-07-30 | Show total BUY, online BUY and online share for an explicit period (YTD, current/prior month); source is `PROCUREMENTS_SV`, `ks_flag=TRUE`. Never label this company-wide total purchasing. |
| Koronet SELL activity | `metrics_v2_sell.json`, as of 2026-07-30 | Show total SELL, online/offline SELL, online share and source-provided deltas for the selected comparable period. Never annualize a partial period and call it actual annual sales. |
| Fees | `metrics_v2_fees_full.json`, generated 2026-07-31 | Show observed/expected fee status and cutoff: billed through Jun 30 plus expected Jul 1–30. Axerrio is explicitly excluded. |
| eShop traffic / session conversion | `ga4_eshop.json`, GA4 automated sheet | Hostname-level sessions, transactions and revenue. Display only where hostname-to-account identity is explicit; call it session CVR. |
| eShop login conversion | `eshop_cvr_by_company.json`, as of 2026-07-31 | Company-ID joined Login CVR; label it **login CVR upper bound**, never session CVR. YTD 2026, customer eCommerce users, >=10 logins. |
| Supply/vendor behavior | `metrics_v2_vendors_full.json`, as of 2026-07-31 | Active, dormant and churned vendors with definitions. Online/offline vendor counts may overlap and must never be summed. |
| Online/offline SKU breadth | `skus_online_offline.json` | Show online, offline and gap as SKU breadth, not varieties/categories. |
| What sells by product division | `product_division_gmv.json` | Fresh-cut, plants and hardgoods online/offline GMV; label the observation window when wired. |
| Offline time depth and best-in-class | `time_depth_offline.json` | Offline variety availability by horizon and benchmark. Do not present it as online catalog coverage. |

## Available, but needs a display contract or explicit identity work

| Data | Why it is not yet a decision metric |
|---|---|
| `annual_sales_est` / company GMV | Available for many accounts, but method and evidence confidence are not consistently stored. Show as an estimate with method/confidence only; no universal inference or fake precision. |
| V2 name-keyed exports | BUY joins 311 and SELL 322 accounts under exact normalized-name rules. Explicit identity exceptions stay blocked; no fuzzy fallback. |
| `metrics_v2_buyers_full.json` | It contains useful buyer/AOV/activity fields but no published source/window contract in the file. Online and offline populations can overlap. Use only after each label/population is declared. |
| GA4 by hostname | The automated eShop sheet is real data; the missing piece for many accounts is hostname-to-company mapping, not GA4 access. |
| SFDC opportunities | The current `sfdc.json` is only 35 rows and needs a clear lifecycle definition before Account cards claim opportunity counts or value. |

## Not usable for the intended claim yet

| Intended claim | Reason |
|---|---|
| Annual BUY potential from observed Koronet BUY | Invalid. The old `CORE` fallback and 54% generic fallback must be removed. A model may be shown only with its inputs/method/confidence. |
| Listing online/offline parity from `listing_detail.json` | Its current file has no metric contract defining population, window or online/offline coverage. |
| Traffic versus shop activation recommendation from generic buyer counts | Buyer population/window must reconcile with SELL and GA4 before it can determine the constraint. |
| Connection leakage/churn | Needs a lifecycle snapshot keyed to the account/vendor relationship; current active-only views cannot prove a connection moved offline. |

## First Accounts release acceptance

1. Every primary metric says observed / estimated / blocked.
2. Period and source are accessible in the card.
3. Price's and Kennicott are fixtures: identity joins and displayed labels must pass for both.
4. A missing or incompatible source yields a clear state, never a fallback estimate.
5. GA4 session CVR and login CVR are never merged.
