# TX Dashboard — Loop State (live, don't delete)

**Started:** 2026-08-01 ~16:00 PT
**Ends:** 2026-08-03 ~04:00 PT (36 hours)
**Mode:** ACTIVE — v2 loop running. Systemic fixes, not card rotation.

## Current state

| Field | Value |
|---|---|
| Current pass | 1 (v2 active) |
| Current cycle | Lead engine fix VERIFIED, fees data pipeline in progress |
| Pushes this session | 4 (`cf2f44b`, `e8f9bc8`, `3657127`, fees pending) |
| Loop version | v2 applied |

## v2 cycle log

### Cycle 1: Lead engine (semantic/lead layer)
**Problem:** 16 opp types, 4 unsupported/blocked, 3 needs-qualification, duplicate L3/C3
**Change:** 6 fixes — duplicate removed, segment filters, honest claims, actual take rate
**Fixtures:** Kennicott (Tier 1 WH_CORE positive case); Rosaprima (IMP_K2K product-type boundary); WH_PROC (buy-only boundary)
**Verification:** Kennicott (WH_CORE) ✓, Rosaprima (IMP_K2K excluded) ✓, WH_PROC (excluded) ✓
**State:** VERIFIED
**Commit:** `3657127`

### Cycle 2: Fees data pipeline (data layer)
**Problem:** fees coverage 11/399 = 2%.
**Change:** Rose queried ALL companies → metrics_v2_fees_full.json (244 cos)
**Verification:** Kennicott $83.8K, Sole $138.5K, Bill Doran $43K, Mayesh $12.5K — all match
**State:** VERIFIED
**Commit:** `935c828`

### Cycle 3: Vendor lifecycle data pipeline (data layer)
**Problem:** vendor lifecycle coverage 7/399 = 1%.
**Change:** Rose queried ALL companies → metrics_v2_vendors_full.json (322 cos)
**Verification:** Kennicott 183 active L30D, DKAY 24, AquaRose 0 (renders correctly). as_of bug fixed.
**State:** VERIFIED
**Commit:** `fc89958`
**Next systemic problem:** Assess remaining coverage gaps and pick highest impact

### Cycle 4: Cross-card consistency (semantic layer)
**Problem:** Portfolio take rate (0.078%) ≠ POTENTIAL (0.199%) — portfolio excluded indirect fees
**Change:** Portfolio take rate formula now includes indirect est. (1.5% × buy_online)
**Fixtures:** Kennicott — both surfaces now show ~0.199%. DKAY (WH_PROC, buy-only) — take rate = 0 (correct, no sell GMV).
**State:** VERIFIED
**Commit:** `afe5301`

### Cycle 5: Outcome spine schema (data pipeline contract)
**Problem:** No canonical definition for daily per-account snapshot
**Change:** outcome_spine_schema.json with field definitions, sources, delta rules
**Fixtures:** Kennicott all fields present; DKAY buy-only (expected)
**State:** VERIFIED (schema). First snapshot = Cycle 6.
**Commit:** `925ed6e`

### Cycle 6: First outcome spine snapshot (data pipeline)
**Problem:** No snapshot exists yet — can't compute real day-over-day deltas
**Change:** Rose generating outcome_spine_2026-08-02.json from Snowflake
**State:** IN_PROGRESS

### Cycle 6b: Codex audit findings
**Problem:** Coverage banner shows lower numbers than expected (V2 files have companies not in portfolio)
**Finding:** Fees 188/399 (47%), Vendors 262/399 (66%) — honest but lower than raw file totals (244, 322)
**Root cause:** Rose queried ALL Snowflake companies; ~20-25% not in 399-account portfolio
**State:** DOCUMENTED (not a bug — banner is honest). Consider expanding portfolio in future.
**Finding 2:** Duplicate account name in accounts.json ("LD Trading" 2x)
**State:** NEEDS_DECISION (Facu: keep both or deduplicate?)

### Cycle 7: LIST TAM quantification (semantic layer)
**Problem:** TAM lost only showed bunches. Varieties and categories gap not quantified in $.
**Change:** "TAM Not Visible Online" section: variety gap × sell_offline = directional estimate; category gap × avg GMV/cat
**Fixtures:** Kennicott variety gap 3,717 → ~$24.9M (PASS). Category gap 2 → ~$1.8M (PASS).
**Evaluator:** 9/10
**State:** VERIFIED
**Commit:** `7b7d836`

### Cycle 8: Lead tabs respond to filters (frontend/routing)
**Problem:** Lead lists showed all 399 accounts regardless of active portfolio filters
**Change:** FILTERED_ACCOUNTS stored by renderAccounts(); renderLeadLists() iterates filtered set
**Fixtures:** P1 filter → fewer leads; All → all leads. Filter indicator shows "(filtered: N of 399)"
**Evaluator:** 9/10
**State:** VERIFIED
**Commit:** `9dbcdb4`

### Cycle 9: Enablement content verification (in progress)
**State:** IN_PROGRESS

## Card rotation order
POTENTIAL → OPPORTUNITIES → BUY → LIST → SELL → PORTFOLIO → META REVIEW → repeat

## Pass history

### Pass 1 (paused after checkpoint)
| Card | Status | Fixes | Data queries | Learnings |
|---|---|---|---|---|
| POTENTIAL | completed | Take-rate, penetration, buy-gap and source/qualifier corrections | No new query; existing temporal data wired | A value is not useful without an honest denominator, source and operator guidance |
| OPPORTUNITIES | completed checkpoint | Opportunity structure, $ impact where viable, benchmarks, links and enablement notes | No new query; uses existing benchmark/data files | A lead requires a source/causal/action contract; a visual opportunity is not automatically an approved rep instruction |
| BUY | paused | — | — | Must begin with the feedback-to-verification contract, not a free-running card audit |
| LIST | paused | — | — | Same |
| SELL | paused | — | — | Same |
| PORTFOLIO | paused | — | — | Same |
| META | paused | — | — | Same |

## Loop improvements (how the loop itself gets better each pass)

### v1 (used for the two completed card checkpoints)
- Read full scope + card feedback + audit
- Think field by field
- Build + push
- Document enablement notes

### v2 (required before resuming BUY)

For each feedback item, classify the root layer: **data pipeline**, **semantic
definition/calculation**, **frontend/routing**, **enablement**, or
**cross-card consistency**. Then record:

1. exact Facu requirement and observable acceptance criterion;
2. smallest change and source/data contract affected;
3. 2–3 account checks plus post-build verification evidence;
4. final state: `VERIFIED`, `BLOCKED`, or `NEEDS_DECISION` — never merely
   “applied”; and
5. cross-card checks for denominator, timeframe, proxy/trust label,
   prerequisite and lead status.

**Hard gate:** a card cannot advance because it was reviewed or because code
was committed. It advances only when its chosen feedback items have evidence
of verification in the MVP validation cohort. Data/semantic failures are
fixed at their source, not concealed by UI changes.

### Resume sequence (set Aug 1 after current-code audit)

1. **Reconcile the current code, feedback and no-loss inventory:** the
   inventory is a historical baseline and predates the POTENTIAL/
   OPPORTUNITIES checkpoints. Update only its affected card entries before
   treating it as a verification source.
2. **Lead contract audit before another card build:** classify every current
   generated opportunity as evidence-backed DRAFT, unsupported/blocked, or
   ready for a defined verification test. Remove causal wording that the data
   does not establish; preserve the underlying signal as a hypothesis where
   useful.
3. **Validate the account-opening flow on the Tier 1 cohort:** Portfolio
   selection → POTENTIAL diagnosis → OPPORTUNITIES hypothesis → relevant
   BUY/LIST/SELL evidence → Deep Dive when needed. Do not redesign cards in
   isolation.
4. **Only then resume the next card:** start with the smallest verified gap,
   not automatically with BUY merely because it is next in the rotation.
5. **Build daily/weekly measurement after the above baseline is trustworthy:**
   daily outcome spine + account-specific lever; weekly cycle report separates
   action, lever movement, outcome movement and learning.

## Next work order (after current data-pipeline cycle)

Do not resume card rotation by default. Work in this order, and stop at the
first missing source rather than fabricating a trend:

1. **Daily outcome spine — data contract first.** The dashboard currently has
   period deltas, but no append-only daily account snapshot. Define and source
   `as_of, company_id, estimated_gmv, koronet_sell_gmv, sell_online_gmv,
   koronet_buy_gmv, buy_online_gmv` plus their denominators. Deliver one
   repeatable snapshot writer and freshness/coverage output; do not present it
   as a daily trend until two compatible snapshots exist.
2. **Portfolio selection proof.** With that spine, verify that the filters and
   sorting can answer: which selected account has the biggest potential/capture
   gap, which is improving/worsening, and which is appropriate to investigate.
   Test on Kennicott plus two contrasting cohort accounts; no account-specific
   thresholds unless the definition says so.
3. **Lead-to-enable-ment handoff.** Add a separate, DRAFT-aware payload for
   CS/Implementation: why it fired, evidence/source, prerequisite, proposed
   pitch, likely objection, action owner and outcome metric. It must consume
   the same lead object, not fork its own lead logic.

**Acceptance gate for any item:** reusable source/semantic/UI capability,
explicit fixture set, deterministic check, and evaluator report with no P0/P1.

## Facu feedback queue (check each cycle)
- Slack DM: U0B9P1F5ALA
- This chat
- TX_FACU_FEEDBACK_AUG1.md
