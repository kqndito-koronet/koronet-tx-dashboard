# Phase 3 SELL card — acceptance contract

Status: **review-only build contract**. It authorizes neither a UI change nor a
play/customer action.  The implementation must pass this contract on fixtures
before the SELL card is published.

## The decision this card enables

SELL answers, in this order:

1. Are existing customers buying online, repeating, and converting?
2. If not, is the first constraint shop/list readiness, activation of known
   offline customers, or demand acquisition?
3. What is the smallest next test, with what evidence and expected learning?

It does **not** repeat company size, Koronet capture, or the POTENTIAL
monetization summary. Those belong in Card 1. A compact online/offline GMV
context line is allowed only when it has the exact same population and period
as the buyer view.

The causal dependency is mandatory: `BUY → LIST → SELL`. A weak SELL metric is
not itself a traffic lead. The card must first establish whether relevant supply
is purchasable and visible/discoverable online.

## Required card layout

Preserve the expanded-account card format: one card headed **SELL — ¿Los
clientes compran online?**, with compact evidence sections below it. Do not
replace it with a text-only status block.

1. **Decision header / data state**
   - Selected period, as-of date, `company_id` scope, sources, and state
     (`observed`, `partial`, or `blocked`).
   - One sentence: `first viable move: fix shop/list | activate offline cohort |
     test traffic | blocked` and the evidence/gate behind it.

2. **Customer base — one reconciled table**
   - Columns: `Online`, `Offline`, `Total`; rows: active customers in the
     selected period, active L30D, new, churned, AOV, repeat rate, and
     concentration (Top 5 online; offline comparator only if actually computed).
   - A compact GMV context line is permitted directly above the table:
     online GMV / offline GMV / online share — all from the same
     `sell_account_spine` period.
   - If online and offline customers overlap, say so; `Total` must not imply
     `Online + Offline`.

3. **Existing-offline cohort**
   - Show an eligible cohort only when its members are active offline in the
     selected period and fit/access to the relevant eShop is proven.
   - Show count, observed offline GMV/AOV, eligibility rule, period, and the
     reason it is not online when known. Do not expose buyer names in the card.
   - Otherwise show the exact gap: `need account-to-eShop eligibility and
     customer-channel history`; do not call all offline buyers an activation
     cohort.

4. **Shop readiness and conversion gate**
   - LIST prerequisite state: `parity verified | parity partial | blocked` with
     a link to the account's LIST evidence.
   - Checkout/config state only for controls that affect customer purchase.
   - **GA4 session CVR:** sessions, transactions, session CVR, domain,
     date-window, and identity mapping. Show only with an explicit
     hostname→`company_id` mapping; never use shared `app.kometsales.com` or
     `eshops.kometsales.com` as account performance.
   - **Login CVR:** may be shown separately as `login CVR upper bound`, YTD
     company-ID data; it must never be merged with GA4 session CVR or used as
     a traffic-conversion claim.
   - Always show a best-in-class reference for the comparable metric. Prefer a
     local comparable; then same product/setup segment; then network. Label
     source, period, scope, and that it is a benchmark—not account offline
     truth or proof of causality.

5. **Unit-economics scenario**
   - Only after a valid eligible cohort exists: `convert N / X% of this cohort
     → scenario GMV → scenario fees`.
   - Base population, period, observed AOV/GMV method, fee rate, channel,
     assumptions, and confidence must be visible. Label it **scenario**, never
     forecast or realized impact.
   - If those inputs cannot share a contract, show the missing inputs instead
     of a $0 or a generic 10%-buyers calculation.

6. **Conclusion and handoff**
   - One of: `fix LIST/shop first`, `activate eligible offline cohort`,
     `test traffic`, or `data blocked`.
   - Link to the contextual SELL lead/play/enablement only for a confirmed
     candidate. A hypothesis gets its evidence, unknown, confirmation test,
     and no auto-task.

## Hard gates / blocked rules

| Claim or recommendation | Required evidence | If absent or incompatible |
|---|---|---|
| Buyer count | Same company scope, channel definition and period as channel GMV | Hide/relabel the count as unavailable; do not show it next to GMV. |
| Online vs offline comparison | One reconciled population/window; overlap disclosed | `BLOCKED — buyer/GMV population or period does not reconcile`. |
| Repeat, churn, retention | Explicit cohort and lookback, separate from selected-period counts if different | Show as a separately labelled historical indicator, never in the primary comparison table. |
| Traffic acquisition | LIST parity, checkout readiness and attributable conversion of existing traffic | `BLOCKED — improve / verify [LIST, checkout, conversion] first`. |
| Offline activation | Active offline customers plus explicit eShop eligibility/access | `NEEDS EVIDENCE — cohort not yet proven`. |
| Unit economics / fees | Cohort base + period + rate/channel | Do not calculate a generic fee scenario. |
| GA4 CVR | Explicit hostname identity mapping and GA4 window | Show `GA4 attribution unavailable`; never borrow shared-host CVR. |
| Best-in-class comparison | Metric-equivalent benchmark with source/window/scope | Show no benchmark rather than a mismatched percentage. |

No fallback chain may silently combine `metrics.json`, V2, H1, L30D, or a
name-fuzzy join. Missing data must retain the intended section and display the
exact data request, not collapse the card into a generic “no data” paragraph.

## Available inputs and their permitted use

| Input | What it can support | Constraint |
|---|---|---|
| `metrics_v2_sell.json` | SELL GMV, online/offline share, comparable deltas; as-of 2026-07-30 | Primary GMV spine only after identity join is explicit. |
| `metrics_v2_buyers_full.json` | Buyer/AOV/new/churned values, Jan 1–Jul 31; online = eCommerce + K2K + API | One-day mismatch with the sell V2 as-of and overlapping online/offline populations. Cannot populate the primary table until reconciled into the chosen spine. |
| `buyer_concentration_v2.json` | Online Top-5 concentration, Jan 1–Jul 30 | Online only; never imply an offline concentration comparison. |
| `repeat_rate.json` | Online repeat rate, H1 Jan 1–Jun 30 | Historical indicator only until a selected-period version is available. |
| `loop2_sell_format_time_v2.json` | Online-only boxes/bunches and short/forward format, H1 | Supplementary context; not a buyer/readiness gate and not online/offline parity. |
| `product_division_gmv.json` | Fresh cut/plants/hardgoods online/offline visibility | Its period includes forward/standing orders through Dec 31; show only with that limitation until aligned. |
| `sell_by_channel.json` | H1 channel mix | H1 only, keyed by SFDC account ID; not interchangeable with YTD company-ID metrics. |
| `ga4_eshop.json` | Session CVR on mapped dedicated domains | Jul 11–31 only; account attribution must be explicit. |
| `eshop_cvr_by_company.json` | Login CVR upper bound, company ID | Jan 1–Jul 31; separate from GA4. |
| `benchmarks.json` | Network/segment benchmark references | Must match the displayed metric; 100% online-share percentiles are not a shop-readiness benchmark. |

## Current implementation defects to remove

The existing card has useful density and should inform the visual structure,
but cannot pass this contract as-is:

- It combines V2 SELL (through Jul 30), buyers-full (through Jul 31), H1
  repeat/channel data, and L30D values in a single table.
- It calculates “10% more online buyers” from inconsistent buyer/GMV/fallback
  data and a fallback fee rate.
- It shows a GA4 block through name/hostname matching; shared-host traffic must
  not be attributed to an account.
- It renders offline customers/GMV before establishing a valid activation
  cohort and can lead to an unjustified traffic recommendation.
- It exposes format, product mix and channel mix without a single shared
  period. They must be labelled as supplemental evidence or withheld.

## Fixture acceptance

The first visual/semantic pass must use these fixtures before extension:

| Fixture | What must be proven |
|---|---|
| Price's Floral Wholesale (`816515`) | No contradiction such as online buyers with zero online GMV. Its prior $0 / 4–6 online-customer conflict remains blocked until a single versioned SELL spine reconciles GMV, buyers, format and time. No traffic/activation recommendation meanwhile. |
| Kennicott Brothers Company (`44150`) | Company scope only; do not mix Pittsburgh location, Kennicott West, Kuts, or ATL. GA4 can appear only via its explicit `shop.kennicott.com` mapping; benchmark/reference must retain its own window. |
| Arizona Family Florist (`193828`) | Core/eShop customer base is shown only with period/source controls; validates the partial-data state and a non-traffic gate. |
| Maple Grove (`641341`), Zeidler (`743648`), H&T (`498865`) | Validate the ordinary P1/wholesaler state: missing fields retain sections and precise gaps rather than being fabricated. |
| Ashland Addison (`507479`), Main Pennsauken (`821576`), Dreisbach (`765491`) | Validate product-type limitation: no eShop/traffic conclusion for K2K/procurement-only or constrained setups. |

## Implementation acceptance checklist

The SELL card is eligible to publish only when all are true:

1. A versioned `sell_account_spine` supplies selected-period GMV and buyer
   population, keyed by canonical `company_id`, or the primary comparison is
   visibly blocked.
2. Every displayed metric exposes source, exact window, scope and state.
3. GA4 session CVR and login CVR are separate and correctly attributed.
4. The card always preserves best-in-class context when a comparable benchmark
   exists; otherwise it says why no comparison is valid.
5. The traffic/activation conclusion obeys the BUY→LIST→SELL gate.
6. Price's and Kennicott pass the fixture assertions above, including a
   screenshot/browser smoke test and source-level reconciliation receipt.

## Evidence consulted

- `TX_DASHBOARD_REVIEW_DECISIONS.md` R6 (approved SELL structure and gates).
- `TX_DASHBOARD_SCOPE_V2.md` Card 5 (approved dense table/format, GA4 caveat).
- `TX_PRICES_ACCEPTANCE_FEEDBACK_2026-08-01.md` and
  `TX_PRICES_CARD_AUDIT_2026-08-01.md` (Price's contradiction and exit gate).
- `TX_FACU_FEEDBACK_AUG1.md` (progressive investigation and causal chain).
- `TX_ACCOUNTS_DATA_CONTRACT.md` (source, identity and CVR constraints).
- Current `index.html` SELL section and the source metadata listed above.
