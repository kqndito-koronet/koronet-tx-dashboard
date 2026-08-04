# Accounts MVP rebuild plan

## Objective

Make the Accounts dashboard a reliable human decision surface: select the right
account by potential, Koronet capture, online adoption and movement; then open
an account and decide the next investigation across POTENTIAL → OPPORTUNITIES
→ BUY → LIST → SELL. It must never present a proxy, unverified configuration,
or mismatched time window as a fact.

## Non-negotiable acceptance criteria

- The historical layout is a visual baseline only; it is not a data source.
- No `undefined`, duplicated percentage symbol, zero-as-missing, or raw source
  dump appears in a customer card.
- Unverified config is `UNKNOWN`: no displayed setting value, blocker, lead,
  TAM or action can derive from it.
- OPPORTUNITIES starts closed. It shows at most three candidate investigations,
  not an automatically expanded lead feed.
- Each compact opportunity shows: lane, impact only if valid, evidence state,
  and one blue contextual link to its card + eventual play/enablement. Detail
  opens deliberately; it never creates a task or outreach.
- Every material displayed metric carries an honest scope, period, source and
  evidence state at point of interpretation.
- Missing data retains its intended place in the card as an explicit gap plus
  a materialization request; it does not disappear or become zero.

## Card contracts

### Portfolio spine

Account, owner/cohort, company GMV and method, Koronet Sell, online Sell %,
Koronet Buy, online Buy %, compatible change, SFDC opportunity context and
approved TX opportunity count. Details such as config, CVR, take rate and
reasoning remain inside the account.

### POTENTIAL

Read in this order: company scale/method/confidence; SELL capture and online
share/change; total purchases method then BUY capture/share/change; Koronet
economics; one verified gap/question. Monetization by channel is collapsible.
It never shows an estimate calculation that lacks a value or method.

### OPPORTUNITIES

Closed by default. Context (only sourced product/reimplementation/SFDC
history), current decision gate, then no more than three BUY/LIST/SELL
candidates. A candidate has hypothesis, evidence, unknown, confirmation rule,
impact only when valid, and a blue link to contextual evidence/enablement.
Config is shown only as a verified prerequisite of that candidate.

### BUY

Does not repeat POTENTIAL volume. Valid total-purchases basis unlocks a +10pp
online procurement scenario. The main comparison is Online / Offline /
availability state / why it matters for active vendors, connected-not-active,
dormant/churned, categories, varieties, SKUs and short/medium/long horizon.
It includes offline procurement and a best-in-class procurement/distribution
reference. Bunches never appear. `Leakage` requires the same-need online
availability join. Time bars mean how far ahead purchases occur only if their
date contract supports it.

### LIST

Explains whether buyers can find equivalent or better supply online. Shows
capacity-to-publish only from audited config/integration facts; parity matrix
for account online proxy, account offline proxy and best-in-class online;
separate bunches comparison; and three comparable time bars. Sales proxy is
always labelled; it is not current catalog/inventory. Open Market means what
the wholesaler publishes, not what it buys. Missing offline/current-catalog
data is explicit and does not become a zero/gap claim.

### SELL

Starts with customer base, shop readiness and decision gate, not repeated
company GMV. Buyer comparison has a single compatible contract across online,
offline and total: GMV, buyers, active L30D and AOV only where reconciled.
Then new/churn/repeat/concentration/format/channel mix, shop quality and GA4
only when account attribution exists. It decides listing/shop first, offline
cohort activation, or traffic. No traffic recommendation precedes parity,
checkout and existing-customer conversion evidence.

## Execution phases

1. Freeze visual baseline and acceptance contract (this document).
2. Audit all current data fields by card and priority account: source, key,
   grain, period, freshness, semantic meaning, coverage, trust and conflict.
3. Produce a Wednesday materialization backlog: exact source/query owner,
   company key, grain, refresh cadence and consuming card for every gap.
4. Build the structural shell from the accepted layout with data slots and
   `UNKNOWN/BLOCKED` states; no automatic opportunities.
5. Populate only data that passed the field audit, including external research
   evidence and model trails for company GMV/total purchases.
6. QA every filter and fixtures (Price, Kennicott, Zeidler, Arizona Family and
   a non-Core/IMPL account): visual comparison, source/timeframe contract,
   no misleading claim, and responsive navigation.
7. Publish only after Facu reviews the rebuilt cards. Keep the historical
   preview separate until the new MVP is accepted.

## Wednesday outputs

- Automated raw extracts/materializations, not token-driven manual pulls.
- Company identity crosswalk shared by SFDC, transaction data, GA4 and research.
- Audited eShop/config snapshot.
- Procurement product/availability/lifecycle/horizon spine.
- Current catalog/listing or a declared proxy plus offline counterpart.
- Account-attributed GA4 + buyer/event population contract.
- Daily refresh, freshness receipt and weekly movement/change report.

## Evidence incorporated after the initial audit

- **GA4 attribution v1:** `ga4_company_attribution_v1.json` has 10 exact
  hostname→company matches proven by Salesforce `Account.Website` and the
  identity crosswalk. Shared platform hosts remain unassignable; fuzzy matches
  are candidates only and never render as account CVR without review.
- **K2K lifecycle/upsell:** `k2k_connection_leads_2026-08-04.json` preserves
  902 buyer×vendor evidence records from the Apr–Jun upsell/easy-handshake and
  stopped-online exports. It contains source row, period, online/offline
  amounts, connection state and last channel. Direct, normalized-name and
  high-confidence fuzzy account matches remain labelled separately.
- **Salesforce:** live Account fields are authoritative for current product
  enablement/context (`E_Shops__c`, `Koronet_Procurement__c`,
  `Bunch_Inventory__c`, `Consumer_Bunches__c`, `K2K__c`). They do **not**
  expose actual MaxAge/Future/publish settings; those remain UNKNOWN until an
  audited configuration source exists.
- **Transactions Scorecard:** the Jul-6 Snowflake scorecard is a dated
  historical backup with useful company-ID coverage, channel GMV, buyer,
  vendor/category and procurement fields. It cannot overwrite current facts:
  it uses a rolling 6m window, a different 229-company Core scope, dated fee
  treatment and documented bad future transaction dates. Reuse only after a
  field/window/definition match is recorded.
