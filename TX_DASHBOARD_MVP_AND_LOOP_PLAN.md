# TX Dashboard — MVP Build + Operating Loop Plan

**Date:** 2026-07-31
**Status:** DRAFT — requires Facu review + Codex verification before execution
**Supersedes:** `ops/canonical/tx_dashboard_build_plan.md` (Phase 1 only — phases 2-5 remain valid)
**Canonical reads completed:** INDEX.yml, OWNERS.yml, tx_dashboard_system.md, tx_dashboard_build_plan.md, scorecard_tab_definitions_wip.md, operating_loops.md, scheduled_operating_loops.yml, WORKFLOW_CONTRACT.md, agent_launch_governance.md, session_closure_governance.md, claude_standing_rules.md, SYSTEM_DESIGN.md, 00_CHARTER.md
**Scope doc:** `docs/transactions/TX_DASHBOARD_SCOPE_V2.md` (authoritative — all build work reads this first)

---

## APPROVAL FORECAST

| Item | Status |
|---|---|
| Scope v2 (Tab 1 columns, cards, 4-layer structure) | NEEDS_FACU — in review now |
| Data refresh approach (Rose direct query vs cron pipeline) | NEEDS_FACU |
| Publish to koronetlabs.com | APPROVED (Facu confirmed, grootctl + cloudflared installed) |
| Daily loop design | NEEDS_FACU — direction approved, detail needs review |
| Lead definitions for tabs 2-6 | CAN_PROCEED as DRAFT (Facu approves individual leads) |
| Autonomy promotion | MUST_NOT_DECIDE — requires proven runs per L0→L1 gate |

---

## WHAT THIS PLAN COVERS

Two things that run in parallel:

1. **MVP Dashboard** — the dashboard works with current data, behind login, usable for the next Chapter TX
2. **Operating Loop** — the daily loop that makes the dashboard improve continuously and helps Facu operate TX

These are NOT sequential phases. They are parallel streams with defined dependencies.

---

## PART 1: MVP DASHBOARD

### Trust chain for dashboard builds

```
Scope v2 (Facu's feedback)
  → Rose produces data (L1: INGEST)
  → Codex verifies data + logic (L2: VERIFY)
  → Builder constructs HTML (L3: BUILD — follows scope v2 contract)
  → Facu reviews in browser (L4: REVIEW)
  → Feedback captured in scope v2 feedback log
  → Next build applies feedback
```

No build starts without data verified by Codex. No build ships without Facu browser review.

### Stream A: Data (Rose)

**Objective:** Replace H1 2026 snapshots with current data including temporal dimension.

**Why parallel:** Data pull doesn't depend on anything else. Everything else depends on it.

**Agent:** Rose
**Verifier:** Codex
**Canonical reads:** data_rules.md (14 rules), TX_DASHBOARD_SCOPE_V2.md (columns + cards), tx_dashboard_build_plan.md (data inventory)

**What Rose pulls:**

#### A1. Coverage audit (FIRST — before any queries)

```
INPUT: accounts.json (399 accounts), metrics.json (112 accounts)
OUTPUT: coverage_audit.json
  - total_accounts: N
  - accounts_with_metrics: N (% of total)
  - priority_accounts_with_metrics: N (% of priority) — target 100%
  - per_metric_coverage: { metric_name: { have: N, missing: N, accounts_missing: [...] } }
  - gap_tickets: [ { account, company_id, missing_metrics, priority } ]

RULE: If priority account coverage < 100%, Rose fills gaps BEFORE any other query.
RULE: If total coverage < 50%, flag to Facu — data foundation is insufficient.
```

#### A2. Current metrics with temporal dimension

```
For ALL accounts with company_id:

SELL SIDE:
  - sell_gmv_current_month, sell_gmv_prior_month, sell_gmv_yoy
  - online_sell_gmv (same temporal splits)
  - online_sell_pct
  - fees_direct, fees_indirect, fees_total
  - take_rate = (fees_direct + fees_indirect) / (sell_gmv + buy_gmv)
  - trend: growing / flat / declining (based on MoM + QoQ direction)

BUY SIDE:
  - buy_gmv_current_month, buy_gmv_prior_month, buy_gmv_yoy
  - online_buy_gmv (same temporal splits)
  - online_buy_pct
  - total_vendors, connected_vendors, active_vendors_l30d, churned_vendors_l30d
  - categories_online, categories_offline, categories_gap
  - varieties_online, varieties_offline
  - skus_online, skus_offline

LIST SIDE:
  - time_depth_online: { 0-3d: %, 4-7d: %, 8-14d: %, 15-30d: %, 31-90d: %, 90d+: % }
  - time_depth_offline: same buckets
  - time_depth_best_in_class: same buckets (Kennicott or computed benchmark)
  - bunches_available: true/false
  - open_market_uploads_l30d: $ value

SELL SIDE (buyers):
  - buyers_online, buyers_offline (if available)
  - new_buyers_l30d
  - churned_buyers_l30d
  - repeat_rate
  - concentration_top5
  - aov_online

ESHOP:
  - cvr_total (orders/visits — from GA4, computed by company not hostname)
  - cvr_new_users (if available from GA4)
  - cvr_trend: improving / flat / declining

OUTPUT: metrics_v2.json (replaces metrics.json)
  - Every value carries: { value, period, source, trust_level, trend }
  - Trust levels: VERIFIED / ESTIMATED / UNRELIABLE / MISSING (per data_rules.md)

TEMPORAL CONSTRAINT: TX fee process runs Monday, Snowflake data available Tuesday.
Rose queries on Monday will have stale fee data — document which metrics are affected.
```

#### A3. Best-in-class benchmarks

```
For each key metric, compute:
  - Network median
  - Network p75
  - Network p90
  - Best-in-class account + value

Metrics: online_sell_pct, online_buy_pct, cvr, take_rate, repeat_rate,
         time_depth_30d+_pct, categories_online, active_vendors

OUTPUT: benchmarks.json
```

#### A4. Post-go-live tracking (Christine's accounts)

```
For each account with implementation_status = post-go-live:
  - days_since_go_live
  - first_online_purchase_date (if any)
  - first_online_sale_date (if any)
  - days_to_first_tx
  - current_online_pct
  - ora_gmv vs actual_gmv (pacing %)

OUTPUT: impl_tracking.json
SOURCE: SFDC implementation records + Snowflake activity
```

**Codex verification for Stream A:**
- Every query follows data_rules.md 14 rules
- Temporal dimensions are consistent across metrics
- Coverage audit numbers match actual file counts
- Benchmark computation is statistically sound
- No metric claims VERIFIED without source proof

### Stream B: Publish (Nahua)

**Objective:** Get the current dashboard behind Cloudflare Access immediately. Security first.

**Why parallel:** Doesn't depend on data refresh. The current dashboard with H1 data is better behind login than public with any data.

**Agent:** Nahua (system infrastructure)
**Tools:** grootctl (v0.1.40 installed), cloudflared (installed)

```
Step 1: cloudflared access login https://groot-api.koronet.sh
  → Browser opens, Facu approves Cloudflare Access

Step 2: grootctl auth status --output json
  → Verify API-owned identity, check for groot:labs-user capability

Step 3: grootctl labs templates --output json
  → Verify static-site template is available/executable

Step 4: grootctl labs bootstrap tx-scorecard \
    --source-repo https://github.com/kqndito-koronet/koronet-scorecard.git \
    --source-branch main \
    --template static-site \
    --execute --output json

Step 5: grootctl labs publish tx-scorecard \
    --source existing-repo \
    --source-repo https://github.com/kqndito-koronet/koronet-scorecard.git \
    --source-branch main \
    --execute --output json

Step 6: grootctl labs status tx-scorecard --verbose --output json
  → Verify ready/healthy

Step 7: Verify https://tx-scorecard.koronetlabs.com
  → Must show Cloudflare Access challenge for non-Koronet users
  → Must load dashboard for koronet.com/kometsales.com/axerrio.com users

Step 8: Consider making kqndito-koronet/koronet-scorecard PRIVATE
  → Removes public GitHub Pages exposure
  → Facu approval needed before repo visibility change

EVIDENCE: Request ID, operation ID, route proof, edge protection confirmation
```

**IMPORTANT:** The groot-labs-developer skill (installed at ~/.claude/skills/groot-labs-developer/SKILL.md) has the full runbook. The agent executing this MUST read that skill first.

### Stream C: Rebuild Dashboard (Builder)

**Objective:** Apply scope v2 feedback to index.html

**Depends on:** Stream A (needs metrics_v2.json) for data-driven changes. Can start structural changes (column order, card renaming) before data arrives.

**Agent:** Dedicated builder sub-agent (not Rose, not Codex, not Pablito)
**Canonical reads:** TX_DASHBOARD_SCOPE_V2.md (authoritative), TX_DASHBOARD_SOURCE_OF_TRUTH.md (build rules), tx_dashboard_build_plan.md (data inventory)

**Build rules from SOURCE_OF_TRUTH (non-negotiable):**
1. Evidence before interpretation — every value has source + trust level
2. Scorecard/artifact boundary — artifact consumes scorecard, doesn't invent scores
3. Facu gate — new logic is DRAFT until tested on real accounts
4. Conditional logic — every lead states product types it applies to
5. **No data loss** — compare old vs new metric inventories before changing
6. Prerequisite sequence — BUY before LIST before SELL
7. No divergence — all views read same JSON
8. Validation before publish — load JSONs, test matching, inspect 5 product types, verify totals, browser smoke test

**What changes (from scope v2):**

#### C1. Portfolio view columns (structural — can start immediately)

| Remove | Add |
|---|---|
| Config/BUY/LIST/SELL mini-scores | Est Buy GMV, Koronet Buy, Buy Penetration %, Online Buy % |
| Separate Direct/Indirect fees | Fees (Total) = direct + indirect summed |
| | Take Rate = (fees total) / (sell GMV + buy GMV) |
| | # Opps (APPROVED only) |
| | Trend indicator (growing/flat/declining) |
| | eShop CVR % with percentile band |

#### C2. Card 2 rename + restructure (structural — can start immediately)

- Rename from "CONFIG" to "OPPORTUNITIES"
- Structure: config opportunities + revenue opportunities (BUY/LIST/SELL) prioritized
- Add: product type profile with potential, best-in-class, known limitations
- Mark entire card as DRAFT until lead logic is reviewed by Facu

#### C3. Card 3 BUY (needs data from Stream A)

- Add vendor rows: Total → Connected (not activated) → Active L30D → Churned L30D
- Add vendor coverage analysis: categories covered online at short/medium/long term
- Remove bunches from BUY (bunches = LIST/SELL concept)
- Reframe Open Market + Prebook as temporal: WHEN do they buy (spot/week/month/season)
- Keep unit economics

#### C4. Card 4 LIST (needs data from Stream A)

- Time depth: add ONLINE vs OFFLINE vs BEST-IN-CLASS (three bars)
- Add TAM lost analysis: by bunches, categories, varieties, SKUs, timeframe
- Clarify Open Market = what they UPLOAD, not what they buy

#### C5. Card 5 SELL (needs data from Stream A)

- Add unit economics like BUY
- Add offline comparison for concentration (Top 5)
- Add hardgoods visibility
- Add best-in-class examples with links to leads

#### C6. CVR in portfolio (needs GA4 data, computed by company not hostname)

**No-loss verification:** Before any card change, builder produces comparison table:

```
| Metric | Old location | New location | Source | Trust | Status |
|---|---|---|---|---|---|
| ... | Card X | Card Y | file.json | VERIFIED | preserved / reorganized / new / removed (decision) / missing |
```

A build is not complete while any field is "missing" without explicit Facu decision.

### Stream D: Lead Definitions (Mercurio)

**Objective:** Define the leads for tabs 2-6 using the 4-layer structure.

**Why parallel:** Lead definition is intellectual work that doesn't depend on the dashboard HTML. Mercurio can work from the data that exists + today's call insights.

**Agent:** Mercurio
**Verifier:** Codex (logic verification)
**Approver:** Facu (each lead individually)

**Per tab, Mercurio produces:**

```
LAYER 1 — Define + Reasoning:
  - Lead name
  - Lead thesis: WHY is this an opportunity? (connected to the master thesis:
    offer everything → confirm fast → deliver quality = more buyers, more wallet share)
  - Detection logic: how do you know this account has this lead?
  - Product types it applies to (and when it does NOT apply)
  - Prerequisite: what must be true before this lead is actionable?
  - Expected impact: if resolved, what changes?

LAYER 2 — Lead list:
  - Which accounts currently match detection logic?
  - Status: DRAFT (all new leads start here)
  - Facu reviews → APPROVED or rejected with reason

LAYER 3 — Enablement:
  - What does the rep SAY to the client?
  - What does the rep CHECK before the conversation?
  - What does the rep DO after the conversation?
  - Common objections + responses
  - Relevant challenger messages (from definitions.json)

LAYER 4 — Case studies:
  - Account that had this lead and resolved it
  - What they did, what changed, what metric moved
  - Replicable? Under what conditions?
```

**Starting leads (from today's calls + scope v2 + existing definitions.json):**

CONFIG tab:
- MaxAge too low for profile (blocks inventory visibility)
- Bunches flag OFF for eShop-dependent accounts
- hideCheckoutWithoutPayment misconfigured (Price's case)
- eSuite account that should be Core (Shamrock, Tutuli pattern)

BUY tab:
- Low vendor coverage online at medium/long term
- K2K connections created but not activated
- Price parity broken (landed cost incomplete)
- Procurement adoption blocked by pricing trust

LIST tab:
- eSuite enabled, no sales L30D (79 enabled, 6 sold)
- Time depth online << offline (inventory not published)
- Bunches not enabled (3-5% TAM addressable)
- Open Market not used (no future inventory uploads)
- Categories/varieties gap online vs offline

SELL tab:
- 0TX post-go-live >30 days (Christine's 14 accounts)
- CVR declining (shop quality degrading)
- Buyer churn > new buyer acquisition
- High concentration risk (Top 5 buyers > 80% of GMV)

GROWTH tab:
- Payments 2.0 candidate (EZ pattern)
- API at 0% fees (Mayesh pattern)
- Core upgrade from eSuite (Shamrock, Tutuli)
- Standing orders automation opportunity (Core accounts)

**Each lead is DRAFT until Facu reviews. Mercurio proposes, Facu decides.**

### Stream E: Socrates — Stakeholder Analysis

**Objective:** Understand what stakeholders (CS, Impl, Product) are NOT doing and how to influence them.

**Why parallel:** Pure analysis work, no dependencies.

**Agent:** Socrates
**Sources:** Today's 23 call transcripts, Slack threads, Christine's IMPL wrap

**Output:**
```
Per stakeholder group:
  - What they DO well
  - What they DON'T do that they should
  - WHY they don't (fear? knowledge? tools? incentives?)
  - How to CONVINCE them (data, framing, examples, authority)
  - Change management approach (ADKAR or equivalent)
  - Quick wins vs structural changes
```

---

## PART 2: OPERATING LOOP

### How this connects to existing loops

The existing loop architecture (operating_loops.md) defines 4 core loops. This plan doesn't replace them — it WIRES them to the dashboard:

```
EXISTING:                          THIS PLAN ADDS:
Loop 1 TX Pacing (stale)     →    Wire live Snowflake to data refresh (Stream A solves this)
Loop 2 Wholesaler Scorecard  →    Dashboard IS the scorecard (unified per Jul 31 decision)
Loop 3 Account Intelligence  →    Deep dives feed into cards + leads + Mercurio learning
Loop 4 Pablito Control       →    Daily TX Change Report replaces/augments the 12-script pipeline
```

### Daily TX Change Report

**Publication:** koronetlabs.com (same infra as dashboard, behind Cloudflare Access)
**Consumers:** Facu, Cata, Christine, Mauro, CS reps
**Frequency:** Tuesday-Friday (post fee data on Tuesday, daily after)

**Architecture:**

```
PRODUCERS (run daily, output JSON):

  Rose:
    - metrics_delta.json: what changed in target accounts vs yesterday/last week
    - red_flags.json: 0TX accounts, declining CVR, lost vendors, inactive eSuite
    - coverage_delta.json: data gaps filled / new gaps found

  Pablito:
    - calls_intelligence.json: Fathom calls from yesterday → insights per account
    - slack_intelligence.json: relevant threads → per account
    - commitments.json: new commitments, overdue follow-ups, completed actions
    - meeting_prep.json: upcoming meetings with context + prep needs

  Mercurio:
    - play_proposals.json: new plays or play adjustments (all DRAFT)
    - enablement_proposals.json: new talk tracks, discovery questions, objection responses
    - deep_dive_learnings.json: what was learned from recent deep dives
    - lead_evolution.json: leads that should be added/modified/archived + reasoning
    - cross_account_patterns.json: patterns Mercurio sees across accounts

  Socrates:
    - stakeholder_moves.json: what stakeholders did/didn't do + influence proposals
    - change_management.json: ADKAR progress per initiative
    - sound_bites.json: data-backed talking points for Facu's meetings

VERIFIER:
  Codex: spot-checks data deltas, validates lead detection logic,
         flags claims without source

CONSUMER SURFACE:
  daily_tx_report.html → published on koronetlabs.com

  Sections:
    1. CHANGES: what moved in target accounts (data-backed)
    2. RED FLAGS: accounts needing attention NOW
    3. CALLS: yesterday's call insights by account
    4. PROPOSALS: Mercurio's play/enablement/lead proposals (DRAFT)
    5. STAKEHOLDERS: Socrates's influence recommendations
    6. COMMITMENTS: what's due, what's overdue, what's done
    7. MEETINGS: upcoming meetings with prep status
    8. LEARNINGS: cross-account patterns, deep dive insights

  Every proposal has: APPROVE / REJECT / ADJUST buttons (conceptually —
  Facu responds in chat, feedback captured in scope v2 log)
```

### Meeting Integration

**Pre-meeting (automated when loop reaches L1+):**

| Meeting | What the loop produces | When |
|---|---|---|
| Chapter TX (weekly) | Top 3 accounts + status + red flags + proposals | Morning of |
| OKR review (biweekly) | Portfolio trending + what moved + what didn't + proposals | Day before |
| CS 1:1 (Christine/Cata) | Their account list + actions + results + next | Morning of |
| Client call | Account brief + CVR trend + opportunities + talk track + client-facing surface | Day before |
| Friday close | Week review + learnings + next week priorities | Friday AM |

**Post-meeting (Pablito captures):**

```
FROM FATHOM (automated when accessible):
  - Transcript → insights per account
  - Commitments made → follow-up tracker
  - Decisions made → scope v2 log if dashboard-relevant
  - Client signals → Mercurio learning

FROM MANUAL (Facu provides in chat):
  - Key decisions
  - Corrections to proposals
  - New priorities
  - Feedback on meeting prep quality
```

### Mercurio Learning System

```
EVERY DEEP DIVE:
  Mercurio records:
    - What type of account (product profile)
    - What questions revealed the most
    - What data was missing
    - What play was proposed
    - What the client responded (if available from Fathom)

  Mercurio updates:
    - Play effectiveness per account type
    - Discovery question ranking (which questions reveal most)
    - Enablement gaps (what reps needed but didn't have)
    - Cross-account patterns (what keeps coming up)

EVERY WEEK:
  Mercurio produces:
    - Plays that worked → case studies (layer 4 of tabs)
    - Plays that failed → anti-patterns with reasoning
    - Leads validated → proposal to move DRAFT → APPROVED
    - Leads invalidated → proposal to archive with reason
    - New patterns → proposal for new leads
    - Enablement improvements → updated talk tracks/questions

ALL PROPOSALS ARE DRAFT. Facu approves or rejects. The system learns from both.
```

### Autonomy Progression

Per system design (L0 → L4), per standing rule SR-017 (no promotion without repeated proof):

```
L0 — NOW:
  Everything manual. Facu asks → agents execute → Facu reviews.
  The daily report is produced but Facu triggers it.
  All leads are DRAFT.

  PROMOTION GATE: 5 daily reports produced + consumed by Facu + feedback applied.

L1 — AFTER 5 CLEAN L0 RUNS:
  Daily report generates automatically (cron or scheduled trigger).
  Pablito flags top 3 items without being asked.
  Mercurio proposes plays proactively.
  Facu still reviews everything.

  PROMOTION GATE: 2 weeks of L1 + Facu confirms daily report quality is useful.

L2 — AFTER 2 WEEKS CLEAN L1:
  Pre-meeting briefs generate automatically.
  Post-call capture runs from Fathom without prompt.
  Rose refreshes data on schedule.
  Deep dive queue runs automatically (one per day from ranked list).
  Facu reviews outputs, not triggers.

  PROMOTION GATE: Monthly review + Facu confirms system is helping not creating noise.

L3 — AFTER 1 MONTH CLEAN L2:
  Learning system proposes lead/enablement changes.
  Socrates proposes stakeholder influence moves.
  Meeting prep is ready before Facu asks.
  Facu approves exceptions and direction changes only.
  Weekly system review replaces daily micro-review.
```

---

## DEPENDENCIES MAP

```
Stream A (Rose data) ──────────────────→ Stream C (rebuild dashboard)
                                              ↓
Stream B (publish) ──→ Dashboard live on koronetlabs.com
                                              ↓
Stream D (Mercurio leads) ──→ Feed into tabs 2-6 + daily report proposals
                                              ↓
Stream E (Socrates) ──→ Feed into daily report stakeholder section
                                              ↓
                         ALL STREAMS → Daily TX Change Report surface
                                              ↓
                                     Facu reviews + feedback
                                              ↓
                                     System learns + improves
                                              ↓
                                     Next day is better
```

**True dependencies (must wait):**
- Stream C (rebuild) waits for Stream A (data) for data-driven changes
- Stream C structural changes (column reorder, card rename) can start immediately

**No dependencies (start now):**
- Stream A (Rose data pull)
- Stream B (publish to koronetlabs.com)
- Stream D (Mercurio lead definitions)
- Stream E (Socrates stakeholder analysis)

---

## VERIFICATION REQUEST FOR CODEX

Before execution, Codex should verify:

1. **Scope alignment:** Does this plan match TX_DASHBOARD_SCOPE_V2.md exactly? Any drift?
2. **Data rules compliance:** Does Stream A follow all 14 data rules?
3. **No-loss rule:** Does Stream C include proper comparison tables?
4. **Build rules compliance:** Does Stream C follow all 8 build rules from SOURCE_OF_TRUTH?
5. **Loop architecture:** Does the daily loop follow the trust chain (Producer → Codex → Interpreter → Facu)?
6. **Canonical consistency:** Does this plan contradict any canonical doc? If so, which decision resolves it?
7. **Governance compliance:** Agent Run Card, session closure, RACI — all followed?
8. **Autonomy gates:** Are L0→L1→L2→L3 gates concrete and measurable?
9. **Missing dependencies:** Are there dependencies I missed?
10. **Anti-patterns:** Does anything in this plan match the 7 anti-patterns from SYSTEM_DESIGN.md?

---

## SESSION CLOSE

**Terminal status:** DRAFT — awaiting Facu review + Codex verification
**Files created:** This file
**Files that should be updated after approval:** TX_DASHBOARD_SCOPE_V2.md (add reference to this plan), ops/canonical/tx_dashboard_build_plan.md (annotate as superseded for Phase 1)
**Decisions needed from Facu:** Approve plan, confirm stream priorities, approve publish approach
**What not to trust:** Timeline estimates (deliberately omitted — Facu feedback: "tu timing es malísimo")
**Next step:** Codex verification, then parallel stream launch
