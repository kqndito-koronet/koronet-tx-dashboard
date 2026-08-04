# Price's account spine audit — 2026-08-01

## Purpose

Define the minimum trustworthy input for the Accounts MVP using Price's Floral
Wholesale, LLC as the acceptance account. This is an audit and data contract,
not a dashboard redesign or a play recommendation.

**Canonical account key:** `account_id = 816515` (Koronet company id).

The alternate Salesforce-style id `0014p000022EYtaAAG` appears in older detail
files. It must be recorded as an alias in an identity map; it must never be
used as a silent substitute for the canonical key.

## What the current files actually support

| Question / field | Current value | Source and window | Status | How the MVP may use it |
|---|---:|---|---|---|
| Company GMV estimate | $1.80M | `COMPANIES_SV.annual_total_sales`; reflected as SFDC, medium confidence | **Modeled / unvalidated** | Show as `Company GMV estimate`, with source/confidence; not as observed actual. |
| Koronet BUY GMV | $165,666.67 | `PROCUREMENTS_SV`, YTD 2026-01-01 to 2026-07-30 | **Observed, usable** | Show as Koronet procurement observed for this exact period. |
| BUY online GMV / share | $100,275.17 / 60.53% | Same V2 source and YTD window | **Observed, usable** | Show with the same total and period. |
| BUY current-month movement | $81,250.61 total; $17,993.12 online; 22.15% | Same V2 source, Jul through Jul-30 | **Observed, usable** | Show separately from YTD; do not compare it to a different cutoff without label. |
| Koronet SELL GMV | $140,053.99 | `SALES_SV`, YTD 2026-01-01 to 2026-07-30 | **Observed, usable** | Show as Koronet sell observed, not company total sales. |
| SELL online/offline | $3,278.00 / $136,775.99 (2.34% online) | Same V2 source and YTD window | **Observed, usable** | Show only with this same period and population. |
| Buyer counts | 107 total; 6 online; 107 offline | `SALE_DETAILS + COMPANIES + CUSTOMERS`, YTD to Jul-31 | **Observed, but window mismatch** | Hold from the first MVP card until the count extract is aligned to the SELL-GMV cutoff and channel population. |
| BUY vendor coverage | 40 total; 34 active L30D; 26 online; 27 offline | `PROCUREMENTS_SV`, YTD to Jul-31 / L30D Jul-31 | **Observed, partial** | Show later as overlapping channel coverage: online + offline may exceed total. It does not tell us available-online, connected/not-active, or churned→offline. |
| LIST breadth | 8 categories; 10 varieties; 15 distinct sale items | `SALE_DETAILS`, H1 through Jun-30 | **Proxy only** | This is past sales breadth, not catalogue/inventory visibility. Do not label as active online catalogue or use it to assert offline parity. |
| LIST time depth | 2 sold 0–7d; zero after | older listing detail, unclear population | **Blocked** | Do not show as inventory/future availability. |
| Current daily snapshot | sell $151,807.71; buy $95,760.07 | `outcome_spine_2026-08-02`, Jul-01 to Jul-31 | **Blocked by as-of** | The snapshot is dated Aug-02 while the operating clock is Aug-01. Do not use or calculate deltas until regenerated with a truthful cutoff. |

## Explicitly rejected paths

1. `$84,416` in `est_gmv.json` is an older observed CORE procurement value,
   not estimated total purchases and not BUY potential.
2. The older `metrics.json` reports Price's $0 SELL and a different BUY window.
   It cannot be a fallback after V2 data exists.
3. `sell_by_channel.json` reports zero GMV with transactions and uses the
   Salesforce-style id. It cannot support SELL or fee conclusions.
4. BUY, SELL, buyer, vendor, LIST and fees must not be silently selected from
   different versions because a file happened to load.

## Minimum account-spine contract

One versioned daily record per `account_id`, `as_of_date`, `metric_period`,
and `metric_name`. Every record requires:

- `account_id` and any source-system aliases;
- `metric_name`, `value`, `unit`, channel if applicable;
- period start/end and `as_of_timestamp`;
- source table/view, query id, filters and grain;
- coverage (`Koronet transactions`, `Core ERP`, `CRM estimate`, `unknown`);
- evidence state (`observed`, `modeled`, `proxy`, `blocked`);
- explicit missing reason, never a default zero.

The first MVP Portfolio can consume only:

- company GMV estimate + method/confidence;
- observed Koronet BUY total / online share, shared window;
- observed Koronet SELL total / online share, shared window;
- compatible change field once there are two truthful daily snapshots;
- SFDC opportunity count/stage only after it is keyed to `account_id` rather
  than a fuzzy name match.

## Minimum replacement extract requested

Replace the overlapping JSON families with a single account-spine extract for
Price's first, keyed to `816515`, then expand to the portfolio:

1. Account identity map: canonical Koronet id, CRM/SFDC id, normalized name.
2. BUY and SELL GMV by calendar month and YTD, with online/offline components
   from the same cutoff and channel definitions.
3. Distinct buyers and orders from the exact SELL population/window.
4. Vendor lifecycle and channel status: active, connected-not-active,
   activated-not-active, churned, churned-to-offline; include `available_online`
   separately from observed purchase channel.
5. Actual listing/inventory catalogue (not sales proxy): categories, varieties,
   SKUs, bunches, future availability and fulfillment horizon; online and
   offline definitions must be stated.
6. Fees by channel and fee source, using the same period as the GMV it is
   interpreted against.

## Acceptance test before UI work

For Price's, every displayed Portfolio/POTENTIAL number must resolve to exactly
one account key, source definition, period and evidence state. If a required
field is not available, the card shows `not measured` and a scoped data request
instead of a zero, proxy or inferred opportunity.
