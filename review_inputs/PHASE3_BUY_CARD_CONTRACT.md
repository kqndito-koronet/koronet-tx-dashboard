# Phase 3 — BUY card acceptance contract

**Status:** review contract; no UI/data promotion implied.
**Owner:** TX dashboard loop.
**Decision source:** Facu feedback and review decisions, Aug 1–3 2026.

## Job to be done

After POTENTIAL establishes company scale/capture and OPPORTUNITIES names the
question, BUY must let Facu decide **which procurement hypothesis merits
investigation**:

1. Is meaningful procurement still offline?
2. Is the same needed supply available online, or is this a supply/connectivity
   gap?
3. Is the account unable to procure the mix it needs at short, medium or long
   horizons?
4. Did a connection fail to activate, go dormant, churn, or move back offline?
5. Is there enough evidence for a candidate play, or must we investigate first?

It must not re-state company GMV, Koronet BUY, penetration, or online share
already established in POTENTIAL except as a compact, period-matched base for
the calculation below.

## Required progressive layout

1. **One-line conclusion and handoff.** State the evidenced condition, or
   `Needs evidence`; then say whether to investigate supply/connection behavior
   or continue to LIST. No task/outreach is created here.
2. **Unit-economics scenario.** `+10 percentage points online BUY = $X online
   procurement = $Y indirect fees`. Show total-purchases base, selected window,
   annualization (if any), fee rate, method and confidence. This is a scenario,
   never observed fees.
3. **Coverage & gap table.** Online today / Offline today / gap / online
   availability / evidence status for: active vendors, categories, varieties,
   SKUs, and short/medium/long-horizon offer. Preserve the difference between
   a *variety* and SKU.
4. **Gap reading.** For each material gap, classify exactly one state:
   - `migration candidate`: same need is proven available online and purchase
     is offline;
   - `supply gap`: the needed supply is not available online;
   - `needs evidence`: identity/product/availability/window does not join;
   - `no material gap observed`: only when comparable coverage shows it.
5. **Connections / lifecycle.** Compact funnel: connected → activated → active
   in selected recency window; show only material exceptions by default:
   connected-not-activated, activated-not-active, dormant, churned,
   churned→offline and offline-without-connection. Full vendor detail is
   expandable. K2K is an agreement + selling motion, not infrastructure.
6. **Horizon and fulfillment.** Compare short / medium / long where common
   online/offline definitions exist; interpret sustained offer by horizon as
   planning and fulfillment capacity, not merely product existence. If only
   online is available, label it **partial evidence**, not parity.
7. **Best-in-class / comparator.** Always show a relevant comparator when a
   metric can be benchmarked: first local comparable, then same
   product-model/segment, then broader benchmark. Display source, period,
   population and metric definition. It teaches potential; it is never the
   account's offline value or causal proof.
8. **Candidate lead / enablement handoff.** Each material signal links to the
   relevant DRAFT lead and enablement: why it matters, evidence, causal claim,
   open question, confirmation condition and next investigation. No automatic
   priority, task, client instruction or dollar total across overlapping leads.

## Metric contract

| Metric | Definition and required semantic label | Current likely source | No-go condition |
|---|---|---|---|
| Koronet BUY / online BUY / offline BUY | Procurement GMV observed in Koronet, same company population and selected window. Online share = online / total Koronet BUY. | `metrics_v2_buy.json`, `PRODUCTION.ANALYTICS.PROCUREMENTS_SV`, as-of 2026-07-30 | Do not compare mixed snapshots/windows; observed Koronet BUY is never company total purchases. |
| Total purchases / BUY potential | Observed only if ERP/company coverage is explicit; otherwise modeled/hypothesis with inputs, range, currency, as-of and confidence. | research estimates + decision record; account model | Never use a Core observed number as potential. No unit-economics scenario without a valid base/method. |
| Indirect fee scenario | `scenario online-GMV increment × displayed rate`; rate currently 1.5% only when explicitly marked estimate. | dashboard fee convention | Never call it realized/invoiced fees; do not hide rate or horizon. |
| Vendor coverage | Distinct vendors by procurement channel with same period; active has an explicit recency window. | `vendor_detail.json`; future vendor-event spine | Counts alone do not explain offline GMV or availability. |
| Product coverage | Distinct categories, varieties and SKUs by channel, common taxonomy/grain/window. | future procurement product spine; current aggregates are partial | Never compare counts with unmatched definitions. Never substitute variety for SKU. |
| Availability / leakage | Offline purchase is leakage only when the same need is proved available online at the relevant time. | vendor lifecycle + availability/connectivity event data | Otherwise classify supply gap or needs evidence, never leakage. |
| Lifecycle | Connection and activation state plus first/last transaction; `active` uses stated recency window. | config/vendor lifecycle; future event spine | Do not infer churn from a missing row or call a connection "active" without window. |
| Purchase horizon | Procurement order creation → required/ship date, online and offline under same definition. | requires `ORDER_DATE`/`CREATED_DATE` in `PROCUREMENTS_SV` | Existing `buy_detail.json` arrival→shipping metric is a logistics proxy; may be shown only as such, never as anticipation. Open Market/Prebook from `SALES_SV` is sell-side proxy, not BUY behavior. |

## Explicit blockers

- Do not show 0 offline vendors/categories/varieties/SKUs as evidence of no
  gap when offline coverage is absent.
- Do not call a coverage count an inventory, catalog or vendor-availability
  measure without its definition.
- Do not put bunches in BUY; bunches belong to LIST/SELL.
- Do not infer that Procurement caused improvement without a compatible
  pre/post baseline; individual-vendor eCommerce, Koronet Procurement and
  eShop are separate channels.
- Do not publish a quantified lead unless both the observed base and modelled
  assumption are visible; do not sum overlapping quantified leads.
- A candidate with unresolved source/window/identity is DRAFT / `needs
  evidence`, not an instruction for CS/Implementation.

## P1 fixtures and acceptance checks

| Fixture | What the card must demonstrate | Specific guardrail |
|---|---|---|
| **Price's (`816515`)** | Present observed YTD BUY as observed, distinguish 60.53% YTD from 22.15% current-month and 97.47% prior-window; route to coverage/leakage investigation. | Its legacy `$84,416` Core value must never appear as BUY potential. `26 online / 3 offline vendors` cannot explain the remaining non-online spend without vendor GMV, connection and transition evidence. Preserve reimplementation / individual-vendor-eCommerce baseline as sourced context. |
| **Kennicott (`44150`)** | Demonstrate high-scale Core + Procurement scope without rolling Pittsburgh/West/Kuts into the company. | company/location grain must be explicit; do not use separate legal entities as proof of parent BUY potential. |
| **Arizona Family Florist** | Demonstrate a candidate potential range in USD, clearly low-confidence, against observed BUY. | range is an approved candidate, not observed company purchases; currency shown as USD. |
| **Maple Grove / Zeidler / Ashland / Main / Dreisbach / Basich & Skinner / H&T** | Exercise Core, eSuite, implementation and/or procurement variations and ensure product model limitations are visible. | No benchmark/lead is promoted merely because a setup label matches; fixture must have compatible source/window before an online/offline comparison. |

## Current evidence inventory and exact requests

Available now: temporal observed BUY (417 companies, V2), aggregate vendor
coverage/lifecycle signals, partial online logistics proxy, account/product
configuration and research-backed company-scale estimates. These support the
observed base, cautious unit-economics where total-purchases method is valid,
and `needs evidence` routing.

Required to pass full BUY coverage/leakage acceptance:

1. procurement event spine keyed by company/location and vendor, including
   channel, GMV, category, variety, SKU, connection state, first/last event,
   and matched online availability;
2. `ORDER_DATE` or `CREATED_DATE` plus required/ship date in
   `PROCUREMENTS_SV` for common online/offline purchase anticipation;
3. period-compatible total-purchases evidence/model inputs and explicit fee
   eligibility/rate;
4. comparison population for coverage/horizon best-in-class, with scope and
   period.

## Exit gate

BUY is ready to publish only when Price's and at least one P1 account of a
different product setup render the seven sections above with no mixed-window
comparison, Core-as-potential leakage, false-zero coverage, or unlabelled
proxy. Every surfaced candidate must trace account evidence → detection logic
→ causal interpretation → value at stake → confirmation condition → linked
enablement.

## Evidence consulted

- `TX_DASHBOARD_REVIEW_DECISIONS.md` R4 (approved BUY structure and rules).
- `TX_FACU_FEEDBACK_AUG1.md` BUY1–BUY9, cross-card rules and lead contract.
- `TX_PRICES_ACCEPTANCE_FEEDBACK_2026-08-01.md` and
  `TX_PRICES_CARD_AUDIT_2026-08-01.md` (Price's contradictions/acceptance).
- `TX_DASHBOARD_SCOPE_V2.md` and `TX_DASHBOARD_COMPLETE_GUIDE.md` (intended
  format and current-source limitations).
