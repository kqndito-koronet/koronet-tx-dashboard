# Phase 3 — audit against Facu feedback

**Status:** review-only. This is a gate before showing or publishing a Phase 3
card rewrite. It audits the proposed BUY, LIST and SELL contracts against the
authoritative scope, Aug 1 feedback and Price acceptance feedback. It does not
claim that a requirement is implemented merely because a contract says it
should be.

**Verdict:** **DO NOT SHOW/PUBLISH Phase 3 cards yet.**

- The proposed contracts preserve much of the intended decision logic and
  prevent the most dangerous false claims.
- They are not yet an implementation, a reconciled account spine, tested
  fixtures or a complete answer to all feedback.
- The highest-priority missing build is a versioned `sell_account_spine` with
  one company scope and selected period for GMV, buyer population and the
  metrics used in the primary SELL table. **This applies to every account.**
  Price is the fixture that exposed the problem; it is not a special or sole
  blocked account.

## Status vocabulary

| Status | Meaning |
|---|---|
| **COVERED IN CONTRACT** | The proposed contract explicitly requires the behavior. It still needs a UI/data test. |
| **DATA-GATED, CORRECTLY RETAINED** | The contract preserves the desired section and names the exact missing source; it must not be replaced by a fake zero or generic prose. |
| **MISSING FROM CONTRACT / BUILD** | The proposal does not yet require or deliver the feedback. It is a pre-publication defect. |
| **OUTSIDE THESE THREE CARDS** | Real feedback, but belongs in Portfolio, Potential/Opportunities, lead/enablement or the operating loop. It must retain an owner rather than silently disappear. |

## Cross-card requirements

| Facu requirement | Evidence in proposals | Status | Required correction before Phase 3 is called complete |
|---|---|---|---|
| Progressive investigation: one-line conclusion and explicit next-card handoff. | BUY §1, LIST §1/6, SELL §1/6. | COVERED IN CONTRACT | Implement and browser-test every state, including `needs evidence`; keep the established dense expandable card format. |
| Preserve the rich account-card format; do not replace cards with flat text/status blocks. | BUY §1–8; LIST reading order; SELL required layout. | COVERED IN CONTRACT | Visual regression test against existing expanded account card on Price + a non-Price P1 account. |
| BUY → LIST → SELL causal chain; no traffic recommendation before supply/listing readiness. | BUY handoff; LIST diagnosis; SELL §1/4 and hard gate. | COVERED IN CONTRACT | Implement a shared prerequisite state, not three independently worded labels. |
| Every surfaced lead remains DRAFT until evidence, logic, owner, outcome and enablement are approved; clicking must not create work. | BUY §8, LIST opening/§6, SELL §6. | COVERED IN CONTRACT | Link each candidate to one canonical lead record/status; no duplicated card-specific lead definitions. |
| Same source/window/scope semantics visible; proxy/benchmark never presented as account truth. | All three contracts, especially LIST semantic table and SELL hard gates. | COVERED IN CONTRACT | Introduce a reusable metric metadata renderer so the rule is enforced rather than hand-written three times. |
| Daily change and weekly-cycle reporting. | Not part of card contracts. | OUTSIDE THESE THREE CARDS | Retain as later operating-loop work; cards need freshness and as-of now, but this does not authorize a fake daily change feed. |
| P1 validation cohort before broad MVP promotion. | Fixtures exist, but are card-specific and not executed. | MISSING FROM CONTRACT / BUILD | Define exact Price + non-Price fixture data/expected DOM assertions, run them, retain receipt/screenshots. |

## Data we can use now — do not hide it

The current files already support a materially richer MVP. A different period
or object does **not** make data unusable: it makes it supplemental evidence
with its own label. It becomes unavailable only for a conclusion that requires
one common population/window.

| Area | Existing source and coverage | What the card can show now | What it cannot yet prove |
|---|---|---|---|
| SELL GMV | `metrics_v2_sell.json`: 435 companies, online/offline GMV and trend, through 2026-07-30 | Primary observed SELL GMV and online share, with its exact as-of. | Customer/AOV comparison on the same period until a selected-period buyer extract exists. |
| Buyers / AOV / new / churned | `metrics_v2_buyers_full.json`: 342 companies, YTD through 2026-07-31 plus L30D | Customer evidence in its own clearly labelled YTD/L30D section; online/offline counts, AOV, new and churned. | A single GMV+buyer comparison table when the GMV source uses a different cut-off. |
| Online concentration | `buyer_concentration_v2.json`: 284 accounts, Jan 1–Jul 30 | Top-5 online concentration, without customer names, alongside the same-window SELL GMV context. | Offline concentration or an online/offline buyer comparison. |
| Retention / repeat | `repeat_rate.json`: 280 accounts, H1 | Clearly labelled H1 historical repeat signal. | Current-period retention or proof that it caused an observed GMV move. |
| Conversion | **Live GA4 Sheet is available and refreshes daily**: all E-Commerce traffic by date/hostname, including shared Core/eShop hostnames; local `ga4_eshop.json` is only an old 29-hostname extract. `eshop_cvr_by_company.json`: 320 company-keyed YTD login-CVR records. | GA4 sessions, transactions, revenue and session CVR at every available hostname/date; company-keyed login CVR separately. Refresh the dashboard from the live Sheet rather than the stale JSON. | Account-level session CVR for traffic on a shared hostname until its account attribution key/rule is extracted and validated. This is an **attribution/modeling task, not a missing GA4-data/access gap**. |
| Channel / format / product mix | `sell_by_channel.json` H1; `loop2_sell_format_time_v2.json` H1; `product_division_gmv.json` 37 accounts | Supplemental H1 channel/format data and product division, including hardgoods where present; standing orders labelled offline. | A primary same-window conversion/activation decision; product division data includes forward/standing orders and needs its limitation shown. |
| BUY GMV | `metrics_v2_buy.json`: 417 companies through 2026-07-30 | Observed Koronet BUY, online share and trend. | Company total purchases unless explicit external/ERP model exists. |
| BUY coverage | `vendor_detail.json`, `supply_matrix_full.json` (318 BUY coverage records), config | Vendor counts and H1 category evidence with source/scope; exact missing rows remain visible. | Vendor/product-level online availability, lifecycle events, true online→offline leakage, or purchase anticipation without raw event fields. |
| LIST config | `config.json`: 397 accounts; `bunches.json`: 43 | eShop-relevant capacity/configuration facts and bunches state. | That configuration proves inventory is currently published or parity exists. |
| LIST sold assortment | `loop2_list_usage_v2.json`: 337 H1 accounts; `skus_online_offline.json`: 341 YTD accounts; `supply_matrix_full.json`: 339 SELL coverage records | Categories, varieties and SKUs **sold** by source/window; SKU online/offline comparison when same source defines both. | Current inventory/listed-catalog parity. |
| LIST recency / comparator | `loop2_list_time_depth_v2.json`: 112 online H1; `time_depth_offline.json`: 110 offline H1; `benchmarks.json` | Online/offline sales-recency evidence where same account/object/window joins, plus labelled comparator. | Future availability / actual listing depth; recency of sale is not arrival horizon. |

### Data genuinely still missing

1. **SELL primary spine:** buyer-level/channel aggregate recomputed to the
   chosen SELL GMV window and canonical company identity, including overlap
   rules. Current sources are useful evidence, but not one atomic table.
2. **Customer activation eligibility:** customer-to-eShop access and channel
   history, to identify a real offline activation cohort rather than call all
   offline buyers eligible.
3. **Current LIST object:** account-keyed catalog/listed/publishable inventory
   snapshot and comparable offline available supply. Sales proxies are already
   available and must be shown as proxies, not discarded.
4. **BUY procurement event/availability spine:** company/location/vendor/
   category/variety/SKU/channel, connection lifecycle, first/last event and
   online availability at the relevant purchase time.
5. **True BUY timing:** `ORDER_DATE` or `CREATED_DATE` plus required/ship date
   for the same online/offline purchase definition.
6. **Shared-host GA4 attribution rule/key:** GA4 data is available in the live
   daily Sheet; the dashboard needs the account key/event parameter or a
   validated attribution rule for shared-host sessions before treating those
   sessions as one account's CVR. This is not a request for new GA4 access.

## BUY — detailed coverage

| Facu feedback / decision needed | Evidence in BUY contract | Status | Required correction or test |
|---|---|---|---|
| Explain observed online/offline procurement and scenario: `+10pp online = GMV = indirect fees`. | Layout §2; metric contract. | COVERED IN CONTRACT | Must use a period-matched total-purchases base; show method, annualization, 1.5% rate, currency, confidence. No generic $ result. |
| Do not repeat POTENTIAL volume/capture. | Job-to-be-done and opening exclusion. | COVERED IN CONTRACT | UI must keep only compact matched base for scenario. |
| Table includes active vendors, categories, varieties, SKUs and short/medium/long horizons, each online/offline/gap/availability/evidence. | Layout §3. | COVERED IN CONTRACT | Build the table even where values are unavailable; render exact gap instead of deleting the row. |
| Include total vendors, connected-not-activated, active L30D, churned/lapsed and connection lifecycle; show online→offline leakage only if proven. | Layout §5, metric contract. | COVERED IN CONTRACT | Need event spine with first/last transaction and channel. Counts alone cannot explain Price's offline share. |
| Differentiate `migration candidate`, supply gap, evidence gap, and no material gap. | Layout §4. | COVERED IN CONTRACT | Require one evidence reference per classification and prohibit aggregate count-only classification. |
| Need supply at short/medium/long horizon, including best growers/importers/exclusive supply. | Layout §6/7. | DATA-GATED, CORRECTLY RETAINED | Procurement `ORDER_DATE/CREATED_DATE`, availability and supplier/product spine are absent. Preserve horizon rows as `needs evidence`; do not convert logistics proxy or sell-side Open Market/Prebook into BUY anticipation. |
| Bunches do not appear in BUY. | Explicit blocker. | COVERED IN CONTRACT | Regression test: BUY DOM contains no bunches/TAM argument. |
| Best-in-class, ideally local, makes the target visible. | Layout §7. | COVERED IN CONTRACT | Comparator resolver must prefer local → same product/model → network and show source/period/population. |
| Price: 60.53% YTD vs 22.15% current vs 97.47% prior clearly separated; $84,416 never BUY potential. | Price fixture. | COVERED IN CONTRACT | Automated fixture needs to assert all three windows and ban `$84,416` from potential/scenario denominator. |
| Price reimplementation / prior individual-vendor eCommerce context. | Mentioned only as sourced context in Price fixture. | MISSING FROM CONTRACT / BUILD | Add a source-controlled `account history/context` line (fact vs open question); no causal Procurement-success claim without pre/post baseline. |
| Arizona candidate buy range in USD and low confidence. | Arizona fixture. | COVERED IN CONTRACT | Display exact approved range, decision/source and USD—not a bare midpoint as observed fact. |
| Links to lead and enablement tell rep why it matters/how to pitch/objections/example. | Layout §8. | COVERED IN CONTRACT | Existing enablement needs canonical lead link and full playbook fields; card cannot claim they exist unless linked record supplies them. |

## LIST — detailed coverage

| Facu feedback / decision needed | Evidence in LIST contract | Status | Required correction or test |
|---|---|---|
| Decide whether buyers can find a comparable/better online offer now and at lead times needed. | Decision section and five diagnoses. | COVERED IN CONTRACT | Each diagnosis must name its evidence state and one next proof; no automatic customer task. |
| Show configuration that can expose bought supply (sync/on-hand, MaxAge, future, bunches) but do not say config proves parity. | Capacity §2 and semantic rules. | COVERED IN CONTRACT | UI needs configuration facts, effect boundary and source/as-of; distinguish setting changed from supply visible / GMV change. |
| Separate current inventory, eShop listing and sales-assortment proxy. | Semantic contract and blockers. | COVERED IN CONTRACT | Existing semantic debt must be removed: sales proxy cannot render as `listed`, inventory or offer parity. |
| Online/offline categories, varieties, SKUs and time horizon must be like-for-like; no misleading zero offline. | Matrix §3; comparison rules. | COVERED IN CONTRACT | Always render the intended comparison row with `Not comparable — exact missing condition`; data request must be durable, account-keyed. |
| Time depth currently means sales recency, not actual future supply. | Semantic contract + source inventory. | COVERED IN CONTRACT | Rename any current metric to sales recency and retain future-supply request. Do not reuse 90d+ sales recency as forward availability. |
| Bunches separate, material offer; do not use industry % as account TAM. | Layout §4 and directional-GMV rule. | COVERED IN CONTRACT | Require comparable account GMV/breadth or display config only plus evidence request. |
| Best-in-class visible at all times; local preferred; never substitute it for account offline data. | Layout §5. | COVERED IN CONTRACT | Comparator scope/source/window must render. Kennicott cannot benchmark itself ambiguously. |
| Open Market is what wholesaler uploads/publishes, not BUY behavior. | Implicit via LIST semantics but not named. | MISSING FROM CONTRACT / BUILD | Add explicit Open Market semantic row: publish/available evidence only, exact source/window; prohibit it from appearing in BUY or as sales. |
| Lost-TAM / unit economics by bunches, categories, varieties, SKUs and timeframe. | Directional GMV allows only cautious scenarios. | DATA-GATED, CORRECTLY RETAINED | The contract correctly prevents a fake account TAM. Add a visible placeholder/data request for each dimension, not a silent omission. |
| Price conflicting sources (H1 8/10/15 vs listing detail 1/2) must surface as conflict, no parity conclusion. | Price fixture. | COVERED IN CONTRACT | Browser fixture must assert both source values and `conflicting proxy source`; cannot render `healthy`, parity, or zero offline. |
| Challenger/enablement messages exist but must not masquerade as evidence. | Candidate link only. | MISSING FROM CONTRACT / BUILD | Link source-scoped candidate to enablement; label the message as talk track and keep it outside the evidence conclusion. |

## SELL — detailed coverage

| Facu feedback / decision needed | Evidence in SELL contract | Status | Required correction or test |
|---|---|---|
| One reconciled online/offline customer + GMV view: buyers, GMV, AOV, L30D, new/churned, repeat, concentration. | Required table and hard gates. | **MISSING FROM BUILD — CRITICAL** | Build `sell_account_spine` keyed to canonical `company_id`, selected period and explicit channel definitions. The contract only asks for it; it does not exist. |
| **No account can mix SELL GMV, H1/L30D buyers and CVR in one claim. Price is the falsification fixture.** | Current defect stated; Price fixture makes it visible. | **MISSING FROM BUILD — CRITICAL** | Choose/recompute one versioned period per account (or split metrics into clearly labelled supplemental sections). Do not display a primary buyer/GMV comparison for any account until its inputs reconcile. This is work to do, not an acceptable final state. |
| Determine shop readiness / existing traffic conversion before asking for more traffic. | SELL causal gate; shop readiness §4. | COVERED IN CONTRACT | Needs actual shared LIST prerequisite state and valid CVR attribution before a traffic recommendation. |
| GA4 session CVR distinct from company login CVR; shared host cannot be account CVR. | SELL §4 and hard gate. | COVERED IN CONTRACT | Build/verify hostname→company mapping; show dedicated domain only. Keep login CVR separately labelled upper bound. |
| Existing offline buyers first activation cohort, only when eligible for eShop. | Existing-offline cohort §3. | COVERED IN CONTRACT | Need account-to-eShop eligibility and customer channel history; otherwise retain exact request, not `offline buyers = activation cohort`. |
| Unit economics `X% more buyers = GMV = fees`, only valid cohort/base/rate. | §5 plus hard gate. | COVERED IN CONTRACT | Do not preserve the current fallback 10% calculation. Implement only after sell spine + eligible cohort. |
| Top-5 concentration without buyer names; offline comparator only if computed. | Table §2. | COVERED IN CONTRACT | Reconcile concentration period or move it to explicitly supplemental section. |
| Keep channel mix/format and hardgoods visibility, but do not mix their window with primary decision. | Source limits acknowledge format/product mix. | PARTIALLY COVERED | Add explicit **supplemental evidence** region: exact window/source, standing orders labelled offline, and hardgoods state `not available` if no valid extract. |
| Retention over time, not point-in-time. | Repeat/churn gate mentions historical indicator. | PARTIALLY COVERED | Add a dedicated retention trend request/section; do not equate H1 repeat rate with retention. |
| Best-in-class conversion examples with lead link. | §4 comparator + §6 lead handoff. | COVERED IN CONTRACT | Metric-equivalent benchmark only; no traffic inference from benchmark. |
| Kennicott scope / dedicated `shop.kennicott.com` only. | Fixture acceptance. | COVERED IN CONTRACT | Browser/source test must reject all shared-host GA4 and child/legal entity joins. |

## Feedback outside BUY/LIST/SELL proposals — retained work

| Requirement | Status | Owner / next artifact |
|---|---|---|
| Portfolio filters actually discover accounts by Est GMV, Koronet capture, online buy/sell share and compatible trend; not cosmetic. | OUTSIDE THESE THREE CARDS | Phase 2 spine + Portfolio verification. |
| Potential: company-scale estimates, confidence/source/freshness; fee table/take rate; no observed Core labelled potential. | OUTSIDE THESE THREE CARDS | Phase 2/Potential audit. |
| Opportunities: all candidate config + BUY + LIST + SELL interventions, priority/prerequisites/value with card links; only APPROVED contributes to `# Opps`. | OUTSIDE THESE THREE CARDS | Opportunities + lead workflow audit. |
| Data & Definitions / source/freshness/gap visibility. | MISSING FROM BUILD | Add a dedicated data/definitions surface or equivalent globally accessible detail before operational MVP claim. |
| Account Deep Dive queue and durable data requests (why, decision enabled, owner/system, retry). | MISSING FROM BUILD | Create request objects, not auto-executing tasks; card gaps link to them. |
| Daily data refresh/change reporting and weekly cycle. | OUTSIDE THESE THREE CARDS | Phase 1C narrow ingestion/quality loop, then reporting layer. |
| CS/Implementation enablement: talk track, objections, case study, owner and measurable outcome. | PARTIALLY COVERED | Contracts link a candidate but do not validate that the linked enablement package is complete. |

## Gate to start UI implementation

The next UI work may start only as a **data-first rewrite**, in this order:

1. Create reusable metric metadata and durable data-gap request objects.
2. Build `sell_account_spine` and test Price reconciliation. If one common
   period cannot be built from current files, retain all intended SELL sections
   but block the primary comparison with the exact request; do not downgrade it
   to a vague status paragraph.
3. Build reusable BUY/LIST coverage rows with semantic object, scope, period,
   source and `not comparable` state.
4. Implement cards in the existing rich expandable layout, then run P1
   fixtures: Price, Kennicott, Arizona Family and at least one contrasting
   product/setup account for each card.
5. Only after evidence/visual fixtures pass, publish the UI and ask Facu to
   review it account by account.

## Audit receipt

**Upstreams used:** `TX_DASHBOARD_SCOPE_V2.md`, `TX_FACU_FEEDBACK_AUG1.md`,
`TX_PRICES_ACCEPTANCE_FEEDBACK_2026-08-01.md`, and the three Phase 3 review
contracts.

**Not used:** no dashboard data or UI was changed; no public page, Supabase,
bridge, staging, priorities or canonical lead was promoted.

**Conclusion:** The contracts are a useful safety spec, not a completed
solution. SELL/Price reconciliation is the immediate non-negotiable build
gate; Open Market semantics, enablement linkage, supplementary SELL evidence
and durable data requests remain explicit gaps.
