# TX Dashboard — source of truth

**Version:** v19 working surface · **cut-off:** 2026-07-31  
**Canonical UI:** `docs/transactions/index.html`  
**Public URL:** https://kqndito-koronet.github.io/koronet-scorecard/transactions/

This document explains what the dashboard is for, what each tab is allowed to answer, how the information is obtained, and which Facu feedback is already applied or still open. It is the operating contract for future updates.

## What “operational” means

This is not a reporting destination. It is the weekly operating loop for priority accounts:

```text
priority account → evidence → bottleneck → next action → owner → customer result → learning → next review
```

The dashboard is operational only when a person can use it to answer, for a specific account:

- Why is this account a priority now?
- What is the first unresolved prerequisite: CONFIG, BUY, LIST or SELL?
- What evidence proves the bottleneck and how trustworthy is it?
- What action should the owner take next, with whom and by when?
- What customer outcome will confirm that the action worked?
- What changed since the previous review, and what should be escalated or learned?

At portfolio level it must also:

- understand the business priorities and the evidence behind them;
- help define and rank the priority accounts and priority problems;
- make the chosen priority explicit rather than leaving CS or Implementations to infer it;
- tell CS **which client**, **why now**, **what conversation or follow-up is needed**, and **what outcome to capture**;
- tell Implementations **which client**, **which configuration or activation step is blocking**, **who owns the fix**, and **what proves readiness**;
- give Facu a review surface where priorities, exceptions and proposed changes can be accepted, corrected or rejected.

The dashboard does not remove Facu's approval gate. It makes the reasoning and recommended order visible so that priorities can be defined deliberately and then executed consistently by the responsible team.

Metrics are inputs to that loop. A tab, card or lead that does not help move the loop forward is informational, not operational, and must not be presented as a completed capability.

## Scope

The dashboard is an account-first operating surface for Transactions. It answers: **how much potential does this account have, what do we capture, what is blocking the next stage, who acts next, and how will we know it worked?**

The operating sequence is **CONFIG → BUY → LIST → SELL**. SELL interpretations are not valid when LIST is not ready; LIST interpretations are not valid when BUY is not happening. The dashboard shows evidence and bottlenecks. Facu decides priorities; it does not autonomously make strategy decisions.

Out of scope: replacing the pacing dashboard, board/TAM opportunity sizing, unsupported strategic conclusions, or a second disconnected data model.

## Tabs and exact scope

| Tab | Question it may answer | Current state |
|---|---|---|
| Accounts | Which accounts matter and what is their current evidence across all stages? | Functional; five expandable cards per account |
| CONFIG | Is the product configured so this account can use the relevant motion? | Functional card; tab-level lead surface is a mockup |
| BUY | What do they buy, from whom, at what horizon, and how much is online vs offline? | Functional card; lead surface is a mockup |
| LIST | Are the products they buy available online, in what format and depth? | Functional card; lead surface is a mockup |
| SELL | Do their customers buy online, and is that growing, repeating or churning? | Functional card; lead surface is a mockup |
| GROWTH | Which product, capability or upgrade is the next evidence-backed opportunity? | Mockup; requires SFDC cross-reference |
| What-If | What changes if a defined share of eligible volume moves online? | Mockup; filtrable logic remains to be built |
| Glossary | What does each field and calculation mean? | Built from `data/definitions.json` |

## Data acquisition and lineage

The UI reads local JSON files from `docs/transactions/data/`. The build/update process is: **source query or export → validated JSON → account matching → UI render → browser review → commit**. A value must retain its source or confidence label when it is estimated.

| Data needed | File(s) currently used | Meaning / rule |
|---|---|---|
| Account identity, type, owner, priority | `accounts.json`, `companies_sv_full.json` | Match by `company_id`; normalized name only as fallback |
| Est. total sell/buy GMV | `est_gmv.json`, `christine.json`, `sfdc_ora_backfill.json` | ORA/SFDC/Christine source preferred; estimate explicitly labeled |
| Koronet sell/buy, penetration | `metrics.json`, `buy_detail.json`, `sell_detail.json` | Penetration denominator is estimated total buying/selling, not only Koronet volume |
| Configuration and implementation | `config.json`, `config_backfill.json`, `definitions.json` | Product-type-specific blockers; do not infer a product conclusion from missing input |
| Bunches, catalogue, varieties, SKUs | `bunches.json`, `loop2_list_usage_v2.json`, `listing_detail.json`, `supply_matrix_full.json` | Used to distinguish catalogue/listing opportunity and box-vs-bunch context |
| Time depth / future horizons | `loop2_list_time_depth_v2.json` | Six horizon buckets; anticipation 7/14/30/+30 remains blocked by missing `CREATED_DATE` |
| Buyers, new, repeat, churn | `buyers.json`, `loop2_phase1v2_buyers.json`, `repeat_rate.json`, `buyer_concentration.json` | SELL buyer health and concentration |
| Channel mix and fees | `sell_by_channel.json`, `sell_detail.json`, `ga4_eshop.json` | Show GMV, fees and take rate by channel; flag material unmonetized channels |
| Vendor and leakage context | `vendor_detail.json`, `buy_detail.json`, `supply_matrix_full.json` | BUY opportunity, K2K connections, online/offline leakage |
| Format and order mix | `loop2_sell_format_time_v2.json`, `buy_detail.json` | Boxes/bunches/short/forward and order mix |

## Applied Facu feedback

- One unified dashboard; POTENTIAL first; MONETIZE merged into POTENTIAL; OPPORTUNITIES merged into CONFIG.
- No unexplained semaphores; show real evidence inline.
- Product profiles explain what Core, K2K, Procurement-only and eSuite mean for the account.
- Fees are by channel with take rate and unmonetized-channel flags.
- Est. Buy uses the account ratio where available and 54% fallback; never a flat 60–70% assumption.
- Pre-go-live accounts are labeled as potential, not as failed activity.
- Procurement-only LIST is explicitly blocked; K2K limitations are visible.
- BUY includes unit economics, varieties and SKUs; total-buying penetration is the denominator.
- Plays follow BUY → LIST → SELL prerequisites.

## Open feedback / acceptance queue

1. Best-in-class comparison by metric.
2. Anticipation 7/14/30/+30 once `CREATED_DATE` is available.
3. Box-vs-bunch opportunity by profile.
4. Best-in-class and offline time-depth context in LIST.
5. Multi-tag filtering for accounts such as Mayesh and Dreisbach.
6. Finish SELL formatting while retaining all scorecard-v18 metrics.
7. Complete missing priority-account data before using the surface for a formal decision review.

## Authority and change control

Use this document plus the session record at `agents/pablito/memory/tx_dashboard_session_state_jul31.md` before changing the artifact. The canonical design references are `ops/canonical/tx_dashboard_build_plan.md`, `ops/canonical/scorecard_tab_definitions_wip.md` and `ops/canonical/tx_dashboard_system.md`. If these disagree, stop and surface the conflict to Facu; do not silently choose a new definition.

Every update should record: date, feedback or source, files/data changed, logic changed, validation performed, and remaining gaps. Publish only the TX directory and its source-of-truth documents; do not commit the unrelated dirty worktree.

## Build rules that must not be lost

These are acceptance rules, not suggestions:

1. **Evidence before interpretation.** Every displayed value has a source, trust level (`VERIFIED`, `ESTIMATED`, `UNRELIABLE` or `MISSING`) and, where relevant, a data-gap note.
2. **Scorecard/artifact boundary.** The scorecard produces data and diagnosis. The artifact consumes that diagnosis and provides actions and enablement. The artifact may run a smell test and flag discrepancies, but it must not invent a second score or silently replace the scorecard source.
3. **Facu gate.** A new lead type is `DRAFT` until its logic is tested on real accounts; it moves through `ALPHA → MVP ENABLEMENT → BETA → APPROVED`. Only Facu can approve a logic change or a canonical change.
4. **Conditional logic.** Every lead states the product types to which it applies and when it does not apply. A missing input is not evidence that a capability is absent.
5. **No data loss.** Before changing a card or tab, compare the old and new metric inventories. Existing scorecard-v18 metrics may be reorganized, but not dropped without an explicit Facu decision recorded here.
6. **Prerequisite sequence.** BUY evidence precedes LIST interpretation; LIST readiness precedes SELL plays. A downstream opportunity must show its blocked prerequisite.
7. **No divergence.** All public views must read the same JSON data layer. A local preview, GitHub Pages copy and future department view must be byte-equivalent in logic and differ only in approved filtering/presentation.
8. **Validation before publish.** Load every referenced JSON file, test account matching, inspect five representative product types, verify totals and run the browser smoke test before committing.

## Current implementation vs canonical target — explicit gaps

The current `index.html` is an eight-tab working surface. The canonical design documents also describe a nine-tab lead/registry structure and three department views. These are not the same thing yet. Specifically:

- Current UI has `Accounts, CONFIG, BUY, LIST, SELL, GROWTH, What-If, Glossary`.
- The WIP tab contract additionally calls for `Account Detail, Views, Flags, Overview, Data Dictionary` and a lead-type registry.
- The canonical system describes `leadership.html`, `cs.html` and `implementations.html` as views over one data layer; these files exist, but the current review is centered on `index.html`.
- The canonical system describes the artifact as an action/enablement consumer of scorecard output, while the current UI contains diagnostics and calculated potential. Until Facu resolves this boundary, no new score or lead logic should be added silently.
- Trust labels, explicit per-account data gaps, lead lifecycle status and a discrepancy audit are required by the canonical rules but are not yet complete in the current UI.

These are tracked gaps, not reasons to rewrite the dashboard. The next build must either implement them or record an explicit Facu decision to defer each one.

## No-loss release gate

Before each release, produce a small comparison table with: metric/field, old location, new location, source, trust level, account coverage, and status (`preserved`, `reorganized`, `new`, `removed by explicit decision`, or `missing`). A release is not complete while a field is merely absent from the comparison.
