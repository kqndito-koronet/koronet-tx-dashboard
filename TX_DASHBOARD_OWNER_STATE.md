# TX Dashboard — owner state and non-regression contract

Status: ACTIVE BUILD — Accounts MVP is **not ready**.
Owner: Codex (dashboard truth, QA and evidence contract).
Last updated: 2026-08-04.

TX strategy lane: strategic_enabler

This is the continuity file for the owner loop. It is deliberately a state
record, not a claim that the dashboard is finished.

## Outcome and boundary

The MVP is a human-intelligence Accounts dashboard. It lets Facu, Cata and
Christine select an account and correctly understand company potential,
Koronet BUY/SELL capture, online adoption and movement; then investigate no
more than three evidence-backed candidate plays. It does not decide priority,
create a task, contact a customer, or promote a play.

The economic outcome is more customer value and, as a result, more online
Koronet BUY/SELL and transaction fees. The system earns that outcome by
learning which plays help which accounts, not by forcing a generic funnel.

Current build scope: **Accounts only** — portfolio + POTENTIAL →
OPPORTUNITIES → BUY → LIST → SELL. Leads/enablement can receive contextual
links/suggestions, but are not a second active build surface.

## Decisions that must not regress

Source: `TX_DASHBOARD_REVIEW_DECISIONS.md` (R1–R8).

1. Portfolio selects accounts; it is not a dump. It needs company GMV,
   Koronet SELL/BUY, online share, compatible movement, SFDC opportunity
   context and verified TX candidates.
2. POTENTIAL reads business first: company scale/method/confidence; SELL;
   BUY; Koronet economics; conclusion. Channel fee breakdown is collapsible.
   Never render `undefined`, duplicate estimates or call Koronet events total
   company purchases.
3. OPPORTUNITIES is closed by default and contains at most three **DRAFT
   investigations**. No summed overlapping opportunity, no automatic P1/P2,
   no task/outreach. The compact action is primary; rationale is evidence on
   the linked card.
4. BUY shows online/offline coverage and availability state for vendors,
   categories, varieties, SKUs and purchase horizon; lifecycle is compact.
   “Leakage” requires an availability join. Otherwise call it supply gap or
   needs evidence. Bunches never belong in BUY.
5. LIST is a clearly labelled sales-assortment proxy until a current catalog
   exists. It shows online/offline proxy and best-in-class, bunches separately,
   and only last-sale recency until real availability/arrival data exists.
6. SELL begins with compatible customer cohorts and shop readiness. Do not mix
   GMV/buyers/CVR windows. Do not recommend traffic before LIST, checkout and
   attributable conversion are established.
7. Unverified config is UNKNOWN. It can neither produce “Bunches OFF”,
   “MaxAge=0”, “Future OFF” nor a blocker/lead. Salesforce is a product-feature
   source, not evidence for MaxAge/Future settings.
8. `Observed` means only the defined source observed the event. It does not
   mean total business coverage. Total purchases model default is 54% of
   company GMV, labelled model/hypothesis with method/date/confidence.
9. Best-in-class is reference, never account truth or causal proof. Prefer
   local, then same model, then broader segment.
10. Do not publish a partial rewrite or say “fixed” based only on code review.
    Run contract validation and browser QA on Price's, Kennicott, Zeidler,
    Arizona Family and one non-Core/implementation account.

## What is true today

### Usable now

| Evidence | Status | Correct use |
|---|---|---|
| Current V2 SELL/BUY metrics | Solid observed Koronet event spine for much of portfolio | Koronet sell/buy, channel mix, selected account comparisons; scope and date visible |
| Historical TX Scorecard | Audited backup: 229 Core companies, rolling six months, generated 2026-07-06 | Historical/comparison only; never overwrite current observations |
| Salesforce account fields | Live source | Product/features, Account ID/name/website, SFDC opportunities; not shop setting truth |
| GA4 extract | 29 hosts; exact identity matches materialized for 10 companies | Exact hostname ↔ company only for account CVR; shared hosts and fuzzy candidates are not attributable CVR |
| K2K XLSX lead lists | 902 vendor/buyer signals materialized with source rows and match status | Candidate investigation input; exact identity usable, fuzzy requires review |
| GMV research registry | Existing researched candidates plus external-research workflow | Annual company-GMV estimate only with evidence/method/range/confidence/date |
| LIST sources | sales/category/variety/SKU and last-sale recency proxies | Clearly labelled sales proxy; no current catalog or future availability claim |

### Known missing / blocked (not permission to invent)

| Need | Why MVP needs it | Interim treatment | Required from Rose/Snowflake |
|---|---|---|---|
| Company/location identity crosswalk | Join SFDC, transactions, GA4, K2K and research correctly | exact and reviewed fuzzy only; expose unmatched | canonical company/entity/location IDs and aliases |
| Current catalog and available supply | Online/offline LIST parity and real customer-visible assortment | sales-assortment proxy only | company-keyed current online/offline inventory, publishability, category/variety/SKU, arrival horizon |
| Availability join at purchase | Distinguish migration/leakage from supply gap | `needs evidence`, never leakage | vendor/product/time availability for each offline procurement event |
| Compatible buyer channel spine | Sell cohort, conversion and unit economics | no generic buyer conversion scenario | account/buyer/window/channel GMV + eShop eligibility/access |
| Purchase/sale anticipation | “How far ahead” demand/offer bars | last-sale recency only | order/created date + promised/arrival/fulfillment buckets |
| GA4 full attribution | Account-specific traffic/CVR across all eShops | exact host only; shared marked unassigned | authoritative hostname/property ↔ account/location mapping |
| Full current SFDC opportunity history | Account context / opportunity counts by ID | current snapshot and exact/fuzzy matching with confidence | account-ID keyed opportunities with stage, amount, close, owner, timestamps |
| Daily movement spine | Portfolio changes and later reporting | display a single period/as-of, no false deltas | daily company/channel/metric snapshot, immutable as-of date |

## Current build status

| Component | State | Honest status |
|---|---|---|
| Portfolio spine | Rendered from staged spine; source/evidence states visible | needs fixture + browser QA across priority cohorts |
| POTENTIAL | Rendered in approved reading order with estimates separated from observed Koronet activity | needs per-fixture source/timeframe review |
| OPPORTUNITIES | Compact, default-closed candidate investigations | needs visual review; no play/task creation allowed |
| BUY | Evidence-first matrix now exposes observed vendor population/lifecycle and retains availability as a gap | needs fixture QA and procurement-event materialization before leakage/distribution claims |
| LIST | Historical sales-proxy matrix, audited setting snapshot state and explicit current-parity gap rendered | needs fixture QA; no catalog/availability claim until Rose materialization |
| SELL | Observed GMV and GA4 attribution gate rendered; buyer evidence remains blocked | needs compatible buyer/fee spine before activation or traffic conclusion |
| Data Gaps | New tab renders coverage plus materialization requests/acceptance tests | needs browser QA and owner review of request priority |
| Evidence materialization | GA4/K2K v1 files and script exist uncommitted | must validate, integrate and commit only with source/contract QA |
| Public dashboard | Staged Accounts MVP passed visual and staged-contract QA | publish only with legacy sources quarantined and gaps visible |

### Latest hard QA result (2026-08-04)

The historical-source audit reports **428 findings** (counts can change as
exports are reconciled). They remain material data debt, but the staged Account
MVP does not read those unsafe fields and marks the relevant decision as blocked.
`python3 scripts/validate_tx_card_contract.py` therefore validates the staged
spine and returns **PASS WITH LEGACY QUARANTINE**, rather than pretending the
legacy raw exports are the shipped data contract:

- 16 `est_buy_gmv` records use source `CORE`, which the current contract reads
  as observed Koronet BUY rather than annual company purchases. Audit each
  record: either document Core ERP coverage/method or reclassify it as observed
  Koronet activity and retain the 54%-of-company-GMV model separately.
- 203 buyer rows report online buyers alongside zero online SELL in the base
  metric. These require source/window reconciliation before display.
- 213 rows expose buyer counts from incompatible all-time/L30D windows. They
  must not appear together as a conversion statement.

They remain in the Data Gaps queue and cannot be promoted into a card until the
underlying contracts are resolved. This is a bounded release waiver for fields
that are safely excluded from the renderer, not a waiver of the data defects.

### Active source-contract conflict — do not silently choose

`definitions.json` says `WH_CORE` uses Core as ERP and the historical scorecard
build describes `CORE` BUY as a Snowflake `proc_total` source. In contrast,
the current card validator treats every `est_buy_source: CORE` as observed
Koronet BUY and rejects it as company total purchases. Both cannot be true
without a per-account grain/window/coverage contract. This matters especially
for Kennicott, where Facu has explicitly said the internal system should be
trusted. The correct resolution is **not** a blanket rewrite to 54%: add the
source coverage, window, annualization method and evidence per Core estimate;
then classify it as `CORE_ERP_OBSERVED` where it truly covers the business, or
as a low-confidence model otherwise. Until then it remains quarantined from
the shipped card and visible as a data gap.

### Evidence materialized this run

- `sfdc_open_opportunities_v1.json`: 222 live, open Salesforce opportunities
  keyed by Salesforce AccountId. Dashboard join uses the canonical identity
  crosswalk, never a false `company_id == AccountId` comparison. Where the
  crosswalk exists and no record is open, the UI must report no **current**
  open SFDC opportunity rather than revive a stale name-only record.

## Execution order from here

1. Finish active card renderer repairs: POTENTIAL undefined/config regression,
   compact OPPORTUNITIES, evidence-first BUY/LIST/SELL. **Completed in staging;
   now verify fixtures and remove any regression.**
2. Integrate existing sources only when their semantics pass their contract:
   SFDC product/opp data, GA4 exact matches, K2K exact/reviewed matches,
   Scorecard historical comparison, research registry.
3. Validate every displayed account field with source/period/scope/freshness;
   leave a visible format and explicit gap where data is absent.
4. Run the staged-MVP acceptance gate plus browser QA fixtures. The legacy
   source audit is a blocker for promoting any old-source field into a card;
   it is not proof that the staged card currently renders those blocked fields.
5. Share a reviewable preview only after those checks; publish only after
   Facu’s visual review confirms no regression.

## Acceptance gate: MVP → beta

MVP is perfect enough to call complete only when all are true:

- Portfolio shows real accounts and working filters, including P1 plus the
  agreed expanded universe; every column has correct definition/period.
- Five account cards follow R2–R6 for all fixture types; no unsupported
  metric, config conclusion, false zero, mixed period or `undefined` survives.
- Absence is rendered as a useful visible gap in the intended format, with a
  named source/field needed, rather than deleting the section or inventing a
  conclusion.
- Source, scope, as-of and evidence state are visible for every material
  metric; estimates include method/range/confidence/date.
- Contract validation and browser fixture QA pass; Facu has visually reviewed
  the candidate public/preview surface.

Only then start beta, in this order:

1. Daily immutable account snapshots and change detection for inputs
   (metrics/config/connection/activity), outputs (candidate/decision) and
   actions/results. Never overwrite history.
2. Human-approved candidate → play/enablement → action/result workflow with
   owner, next check, expected leading indicator, outcome KPI and learning.
3. Filterable reporting by team, rep, account cohort, lead type and multi-select
   filters, showing changes, actions, outcomes, freshness and blockers. It is
   reporting, not autonomous priority assignment.
4. Case evidence strengthens or weakens plays; only repeated validated cases
   can request limited authority by decision type.

## Required Rose/Snowflake deliverable (after MVP)

Rose should provide a versioned daily materialization—not ad-hoc extracts—of:

1. `company_identity_crosswalk` (canonical company/location IDs, SFDC IDs,
   aliases, domains, confidence, effective dates);
2. `account_daily_metrics` (sell/buy GMV and online/offline components,
   fees, buyer/vendor counts, as-of/period/source);
3. `procurement_event_supply_match` (buyer/vendor/product/event, channel,
   availability-online flag and evidence time);
4. `catalog_supply_snapshot` (company/location, online/offline,
   publishable/visible, category/variety/SKU, availability/arrival horizon);
5. `buyer_channel_daily` plus eShop eligibility/access and attributable GA4
   mapping; and
6. `sfdc_opportunity_snapshot` keyed to canonical account ID.

Each table needs grain, denominator, timezone, freshness SLA, immutable
snapshot date and owner. This gives the dashboard enough truth to report
changes without manufacturing them.

## Learning protocol for the owner loop

After every dashboard change or case review, record:

- decision/assumption and source;
- fixture/account types tested;
- approval, correction or contradiction;
- recurrence against the existing finding; and
- whether the correction changes a data contract, renderer pattern or future
  Snowflake requirement.

Never learn the wrong lesson: a source present is not a semantic match; a
visually better card is not a correct card; a candidate lead is not a play;
and a single successful case is not scalable proof.
