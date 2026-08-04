# Phase 3 LIST implementation receipt

**Status:** local implementation only; not staged, committed, published, or promoted.
**Scope written:** `docs/transactions/index.html`, Card 4 / LIST only (active renderer at lines 2866–2912). The prior LIST renderer remains non-executing inside `if(false)` for review/audit; it emits no UI.

## Feedback → implementation

| Feedback / contract requirement | Implementation | Source / use or no-use |
|---|---|---|
| Preserve dense expandable card; do not flatten it | Retained `expand-section`, compact tables, inline evidence states and conclusion handoff. | Existing Accounts card format used. |
| No current catalog/listing claim without snapshot | Header explicitly says `sales-assortment proxies, not catalog/listing inventory`; every section retains a current snapshot gap. | No catalog snapshot exists: correctly not used. |
| Three views: account ONLINE, account OFFLINE, best-in-class ONLINE | One matrix renders all three columns; recency section repeats all three distinctly. | `listing_detail.json`, `loop2_list_usage_v2.json`, `skus_online_offline.json`, `time_depth_offline.json.best_in_class`. |
| Categories / varieties / SKUs distinct | Separate matrix rows and source labels. No category fallback is called variety. | Category/SKU values from `listing_detail`; varieties from `loop2_list_usage_v2`; SKU comparison remains separately labelled YTD. |
| Do not compare incompatible values | Every incompatible row says exact `Not comparable` reason; no synthetic gap/TAM is emitted. Category offline is `Not available`: `total_categories - online_categories` is prohibited because distinct channel sets overlap. Conflicting H1 proxies render a warning. | `supply_matrix_full.json` deliberately **not used** for conclusions because its category semantics conflict with the card sources. |
| Time depth is sales recency, not availability | Section is named `Sales recency by variety` and explicitly requests true available-supply horizon buckets. | `loop2_list_time_depth_v2.json` + `time_depth_offline.json`, H1 sales/ship recency only. |
| Config capacity separate from parity | MaxAge, Future, On-hand/sync and audited source appear in a prerequisite table. | `config.json`; never used as parity proof. |
| Bunches separate; no industry TAM | Separate bunches section with setting + observed GMV and a same-window comparability gap. | `bunches.json`; no TAM calculation. |
| Best-in-class always visible, not substitute | Kennicott online cohort is visible with scope/reference-only label. | `time_depth_offline.json.best_in_class`; no use as account offline metric. |
| Open Market means publish/available, not BUY or sales | Explicitly says current source is sales-classified only, so it cannot prove uploads/current availability. | `buy_detail.json` / sales-classified evidence deliberately not used as publish proof. |
| Exact conclusion + DRAFT enablement link/no task | One conclusion chooses configuration prerequisite or needs evidence; contextual button returns to Opportunities and states review-only/no task. | Existing account-card navigation only; no lead/task mutation. |
| Product limitations | Procurement-only hard stop; K2K vendor-availability limitation. | `ct_id` current account profile. |
| Unverified config cannot trigger a LIST blocker | Config records with `audited:false` render `UNVERIFIED` plus observed-behavior reconciliation; conclusion stays `NEEDS EVIDENCE`. | `config.json` is weak evidence; `loop2_sell_format_time_v2.json` is observed supplemental format evidence. |
| Procurement timing is not LIST depth | Timing appears only in a visibly separate note, defined as order creation → shipping. | `anticipation_real.json`, YTD through 2026-07-31; never used for availability, listing depth or an online/offline LIST comparison. |

## Fixture / smoke tests

Ran local JSDOM smoke test on `http://127.0.0.1:8767/index.html` after JSON load.

| Fixture | Result |
|---|---|
| Price’s (`816515`) | PASS. Config renders MaxAge 5d / future/on-hand/bunches. H1 SALES_SV shows total 8 / online 1 categories and explicitly `Not available` offline category population; list-usage H1 carries 8 categories / 10 varieties / 15 SKUs; conflict warning renders and no parity conclusion is made. |
| Kennicott (`44150`) | PASS. Main company row expands to five cards; LIST has the distinct best-in-class online reference and does not join child/legal accounts. |
| Arizona Family (`743648`) | PASS. Zero online sales-proxy values remain labelled proxy; config prerequisite conclusion renders because Future is OFF. It does not claim zero current listing/inventory. |
| Maple Grove (`765491`) | PASS. Rich online evidence renders; best-in-class remains visible as a sourced reference, not a health declaration. |
| Procurement-only | PASS by code path: hard stop, no matrix/parity inference. |
| K2K-only | PASS by code path: vendor-availability limitation appears before evidence; no owned-inventory claim. |
| Zeidler Floral | PASS. `SCORECARD (NOT VERIFIED)` configuration (`max_age=0`, Future/Bunches OFF) renders as `UNVERIFIED`, while observed H1 online format shows 100.0% bunches / $163,709. It does **not** render a configuration-prerequisite conclusion. The distinct YTD anticipation note renders 98.4% online orders at 0–3d from order creation to shipping, explicitly outside LIST depth. |
| JS runtime | PASS. JSDOM captured no runtime errors. |

## Known retained gaps

1. The durable account-keyed current listing/publishable inventory + comparable offline available-supply extract does not exist. The card preserves the full desired layout and names that request rather than inventing values.
2. True arrival/availability horizon requires current supply snapshots in `0–7d`, `8–14d`, `15–30d`, `31–90d`, `90d+` buckets; current data is only sales recency.
3. The enablement control is a contextual DRAFT review link, not a validated canonical playbook link. No task/lead was created.
4. The legacy renderer is intentionally retained non-executing during review so semantic regression can be audited; remove only after the Phase 3 UI is accepted.
