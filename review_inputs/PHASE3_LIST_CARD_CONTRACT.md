# Phase 3 — LIST card acceptance contract

**Status:** review-only build contract.
**Scope:** Card 4 (`LIST`) in the Account view only. It does not alter lead/enablement surfaces or authorize an account action.

## Decision the card must enable

Can a buyer find online an offer equivalent to, or better than, the offer the wholesaler makes available offline — now and at the lead times buyers need?

The card must end in exactly one initial diagnosis:

1. **Publishing/configuration prerequisite** — the account cannot reliably expose eligible supply online yet.
2. **Parity / visibility gap** — comparable evidence shows meaningful offline offer or sales breadth not visible online.
3. **Future-supply gap** — current online breadth may exist, but medium/long-horizon supply is materially weaker than comparable offline or benchmark evidence.
4. **Demand / SELL next** — listing parity is sufficiently supported; investigate buyers, conversion or traffic next.
5. **Needs evidence** — the comparison cannot be made. This is not “no gap.”

No diagnosis is a play approval. A candidate may link to the existing lead/enablement context, preserving account + evidence context, but opening it must not create a task.

## Required reading order and layout

Keep the established expandable-card format: concise summary first, then evidence tables and foldable source detail. Do **not** replace it with a flat block of prose or with a score.

1. **Card header + evidence state**
   - `LIST — Can buyers find the offer online?`
   - source, as-of, common period/window, population/channel scope, and coverage state.
   - prominent `Needs evidence` / `Proxy` state when applicable.
2. **Capacity to publish**
   - only configuration/integration facts that determine whether bought/available supply can reach the eShop: e.g. eShop eligibility, inventory sync/on-hand, MaxAge/future setting, bunches flag.
   - a configuration value is a prerequisite fact, not proof of assortment parity.
3. **Offer parity — online vs offline**
   - one compact matrix: categories, varieties, SKUs, and short/medium/long time depth.
   - rows must state the semantic object: `current listed inventory`, `available supply`, or `sold assortment proxy`; never collapse them into one number.
   - online, offline, gap, and interpretation; show no offline value as `Not available`, never `0`.
4. **Bunches — separate material offer**
   - online and offline GMV/breadth where both are comparable; relevant bunch configuration; precise gap statement.
   - do not use a network/industry percentage as account TAM.
5. **Best-in-class comparator — always visible**
   - comparable local account first; then same product/model segment; then wider network.
   - label comparator, period, population, scope and source. It is a learning reference, never a substitute for the account’s offline data or causal proof.
6. **Conclusion / handoff**
   - one of the five diagnoses above, evidence and uncertainty, one next proof/action, and contextual link to the candidate LIST lead/enablement material.
   - if the issue is a SELL-readiness question, link to SELL rather than imply that traffic is ready.

## Semantic contract

| Concept | Meaning allowed in LIST | Never call it |
|---|---|---|
| Inventory | Current publishable / available catalog at a stated snapshot | sales, assortment sold, or time depth without a snapshot |
| Listing | Products actually visible/publishable in the eShop at a stated snapshot | all purchasable supply unless the integration proves it |
| Sales assortment proxy | Distinct products/categories/varieties that **sold** through defined channels in a stated period | current inventory or proof something is listed today |
| Offer parity | Like-for-like online/offline comparison on same object, population and window | parity inferred from one-sided data |
| Time depth | Availability/arrival horizon in common buckets (`0–7d`, `8–14d`, `15–30d`, `31–90d`, `90d+`) with explicit object and date basis | “freshness” / recency of last sale unless clearly labelled as that proxy |
| Bunches | Separate format; configured visibility plus comparable online/offline GMV or breadth | a generic category or account TAM |

`Categories`, `varieties`, and `SKUs` are separate dimensions. A fallback category field must not be labelled variety. This specifically prevents the Price’s-style “67 categories / 10 varieties” interpretation failure.

## Comparison and blocking rules

### Comparable online/offline evidence

Show an online/offline gap only if the two values share:

- the same company/account scope;
- the same semantic object (inventory, listed catalog, available supply, or sold proxy);
- the same channel classification;
- compatible period/as-of and inclusion/exclusion rules; and
- a source and coverage note.

If any condition fails, keep the desired row and render `Not comparable — [exact missing condition]`. It must create/retain a data request describing: account key, desired object, online/offline populations, period, decision enabled, owner/system, and retry condition.

### Absolute blocked states

- **Procurement-only:** no owned eShop/listing; display the product limitation and stop. Do not render a fabricated parity matrix.
- **K2K-only:** explain that availability depends on vendor supply, not owned inventory. A separate vendor-availability contract is needed before claiming own-assortment parity.
- **No catalog snapshot:** sales-derived rows are allowed only under the `sales assortment proxy` label. No “not listed” or “inventory invisible” conclusion from them alone.
- **No offline comparator:** show online evidence + best-in-class, then `Needs evidence`; never imply “no gap.”
- **Conflicting source/window:** block the conclusion and cite the conflicting sources/windows. Do not combine values.

### Directional GMV

GMV hidden/not-visible scenarios are allowed only as labelled scenarios with the allocation method, source, period and uncertainty. A gap count × average GMV/category is not account potential or a forecast. Suppress it if the underlying category/SKU/variety scope is not comparable.

## What current data can support vs must remain gated

| Input | Current useful evidence | Limit that must be visible | UI use now |
|---|---|---|---|
| `config.json` | `max_age`, `future`, `bunches_flag`, `on_hand`, source/audited | configuration does not prove actual published assortment | capacity/prerequisite only |
| `loop2_list_usage_v2.json` | H1 2026 distinct categories/varieties/SKUs **sold** online; `company_id`; R12 | sales proxy, not listing/inventory snapshot | labelled online sales-assortment proxy |
| `listing_detail.json` | H1 SALES_SV categories and SKU counts by online channels | explicitly has no variety field; sales proxy | categories/SKUs only, labelled proxy |
| `time_depth_offline.json` | offline sales-assortment recency by variety, H1 2026; best-in-class distribution | recency of last shipment/sale, not availability horizon | labelled sales-recency comparison only |
| `loop2_list_time_depth_v2.json` | online sales-assortment recency distribution | not a listing/catalog availability horizon | labelled sales-recency comparison only |
| `skus_online_offline.json`, `supply_matrix_full.json` | historical online/offline coverage candidates | validate account key, metric object, source period and join before comparison | gated; no automatic `not listed` assertion |
| `bunches.json` | config flag + observed bunch-related GMV/discrepancy signal | does not itself prove online/offline assortment parity | separate configuration/GMV evidence, with window/source |
| `benchmarks.json` / `time_depth_offline.json.best_in_class` | benchmark/reference | scope may not be local or same model | always-visible comparator with scope label |

Current implementation has material semantic debt that phase 3 must remove: it labels sales proxies as `no listed` and labels sales recency as `time depth` / `freshness` availability. The data supports an honest proxy view; it does **not** support a current-catalog parity claim without a listing/inventory snapshot.

## P1 fixture acceptance

Validate visual and semantic behavior on at least these P1 fixtures before publish:

| Fixture | Required test |
|---|---|
| **Price’s (`816515`)** | Render: config capacity (`max_age=5`, future/bunches/on-hand flags); H1 sold-proxy breadth (8 categories, 10 varieties, 15 SKUs); `listing_detail` differs (1 online category, 2 online SKUs). The card must surface this as a source/semantic conflict, not announce parity or a gap. No online/offline catalog conclusion until a common snapshot exists. |
| **Kennicott (`44150`)** | Render a rich online evidence case plus best-in-class reference without self-comparison ambiguity; keep company scope separate from child/legal entities. Verify categories, varieties and SKUs retain individual labels. |
| **Arizona Family (`743648`)** | Test a zero-online observed-sales case: distinguish zero in the stated sales proxy from zero current eShop inventory/listing; show configuration/product prerequisite and comparator. |
| **Maple Grove (`765491`)** | Test a high-breadth / online evidence case and ensure benchmark is still visible and sourced rather than declaring it automatically healthy. |
| **Procurement-only fixture** | Confirm hard stop after capability statement: no listing/parity inference. |
| **K2K-only fixture** | Confirm vendor-availability limitation and no owned-inventory claim. |

A fixture passes only if a reviewer can identify the exact object, population, date/window and source behind every displayed number; can see the missing comparison instead of a zero; and sees a single honest conclusion.

## Source requirements for any new LIST extract

The required extract is keyed by canonical `company_id` and must carry source record key, as-of, period, timezone, run ID, product/account scope, channel definition, object type and coverage. It needs:

1. current online **listed/publishable inventory** breadth by category/variety/SKU and arrival horizon;
2. comparable offline available/inventory or explicitly scoped offline sales-assortment proxy;
3. online/offline bunches GMV/breadth on the same window/object;
4. configuration/integration state that links supply to visibility;
5. comparator cohort definition and values; and
6. a linked data-gap request when any component cannot be supplied.

This is deliberately account-keyed so it joins the canonical identity layer and does not create a competing account model.

## Build acceptance gate

Do not publish a LIST rewrite until all are true:

- required layout appears in the existing card system and preserves source-detail access;
- every displayed value is classified as observed / proxy / benchmark / not available;
- no proxy is labelled inventory, listing, parity or availability;
- all online/offline conclusions pass the comparable-evidence rules;
- bunches are visibly separate;
- best-in-class is visible, scoped and non-substitutive;
- P1 fixtures and the two product-type gates pass; and
- no new lead logic, play, task or external action is promoted by the card.

## Evidence basis

- `TX_DASHBOARD_SCOPE_V2.md`, Account card design and LIST objective.
- `TX_DASHBOARD_REVIEW_DECISIONS.md`, R5 (approved LIST structure/rules), R8 (Accounts-only scope).
- `TX_PRICES_ACCEPTANCE_FEEDBACK_2026-08-01.md` and `TX_PRICES_CARD_AUDIT_2026-08-01.md`, Price’s contradictions and release gates.
- `TX_ARTIFACT_SCOPE_AND_FEEDBACK.md`, inherited dashboard/card structure and open LIST gaps.
- Current `index.html` and the JSON inputs named in the table above, inspected 2026-08-03.
