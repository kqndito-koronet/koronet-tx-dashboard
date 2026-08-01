# TX Dashboard — Session Errors + Loop Design

**Date:** 2026-08-01
**Purpose:** Honest error log + research on how to build a loop that actually works

---

## ERRORS THIS SESSION (honest)

### ERROR 1: Audit theater instead of shipping
**What happened:** Spent 8+ hours on audit loop that produced beautiful Slack messages but zero shipped changes. The loop couldn't push, so every "fix" it built was lost.
**Root cause:** Optimized for looking busy (text output) instead of shipping value (code in production).
**Rule:** A loop that can't ship is a loop that wastes time. Every loop run must either SHIP or SKIP.

### ERROR 2: Data was the blocker from hour 1 — didn't act on it
**What happened:** The audit at hour 2 identified that 34% of scope v2 gaps need Rose data. I spent the next 6 hours on UI fixes instead of launching Rose.
**Root cause:** UI fixes feel productive (you can see them). Data queries feel invisible. But without data, the UI shows dashes.
**Rule:** If >50% of gaps need data → data is the job, not UI.

### ERROR 3: Over-planned, under-executed
**What happened:** Created TX_DASHBOARD_MVP_AND_LOOP_PLAN.md (detailed plan), TX_DASHBOARD_MVP_AND_LOOP_EXECUTION_GUIDE.md (execution guide), TX_DASHBOARD_SCOPE_V2.md (scope), P0 inventory, etc. All good docs. But the dashboard didn't change fast enough.
**Root cause:** Planning feels like progress. Facu's feedback: "tu timing es malísimo" and "me mandaste muchas palabras => al pedo."
**Rule:** 1 hour planning max. Then ship. Learn from what you shipped, not from what you planned.

### ERROR 4: One loop doing everything instead of specialized loops
**What happened:** The weekend audit loop tried to audit 12 topics in rotation AND build AND push. Too many responsibilities, too little depth.
**Root cause:** Designed a Swiss Army knife instead of sharp tools.
**Rule:** Each loop does ONE thing well. Separate: data refresh, UI build, audit, Facu communication.

### ERROR 5: Loop couldn't push — should have verified before launching
**What happened:** Launched the build loop without verifying it could push. Wasted 3 runs before discovering the 403.
**Root cause:** Didn't test the critical path (push) before deploying the loop.
**Rule:** Test the FULL cycle (read → change → push → verify live) before scheduling.

### ERROR 6: Summarized Facu's feedback instead of preserving it
**What happened:** First version of scope v2 was a summary. Facu: "lo estas resumiendo.. necesito que lo guardes en detalle."
**Root cause:** Default to compression when the job is preservation.
**Rule:** Facu's words verbatim first. Compress later if needed.

### ERROR 7: Didn't read canonical build docs before proposing
**What happened:** Proposed a build plan without reading WORKFLOW_CONTRACT, agent_launch_governance, session_closure_governance.
**Root cause:** Assumed I knew how the system works instead of reading how it SAYS it works.
**Rule:** Read the rules before proposing anything. feedback_delegate_deep_research.md exists for a reason.

### ERROR 8: Built the shell before having data
**What happened:** P0.1 (structural shell) was done fast. But it showed dashes everywhere because data wasn't ready.
**Root cause:** Wrong sequencing. Shell without data = empty skeleton.
**Rule:** Data and shell in parallel, not sequential. Ship the shell ONLY when data is ready to fill it.

### ERROR 9: Proposed features Facu didn't ask for
**What happened:** Added things like "Pacing KPI bar" without Facu asking. Meanwhile things he DID ask for (offline buyers, time depth 3 versions) weren't done.
**Root cause:** Building what's easy instead of what's needed.
**Rule:** Facu's feedback list is the ONLY backlog. Don't invent.

### ERROR 10: Focused on what I could see (UI) instead of what matters (data quality)
**What happened:** 28% data coverage was flagged from the start. Still building UI on top of incomplete data.
**Root cause:** UI is visible, data quality is invisible. But data quality IS the product.
**Rule:** Coverage audit is step 0 of every cycle. If coverage < 80%, fill gaps before touching UI.

---

## HOW TO BUILD A LOOP THAT ACTUALLY WORKS

### What doesn't work (proven this session)
- A loop that audits but doesn't ship
- A loop that produces text but not changes
- A loop that can't push its own changes
- A loop that tries to do everything
- A loop that focuses on UI when data is the blocker

### What works (proven this session)
- Microagents with specific tasks (7 inventory agents all delivered)
- Rose queries against Snowflake (435 companies in one pull)
- Builder agents with clear scope (P0.1 shell shipped fast)
- Codex verification (caught real bugs: Mayesh/Kennicott swap, Ninfa artifact)
- Parallel execution (4 Rose agents running simultaneously)

### The loop that would actually work

```
EVERY DAY (automated, no human needed):

  STEP 1: DATA (Rose — 30 min)
    - Refresh sell/buy/fees from Snowflake
    - Run coverage audit
    - Flag: what's new, what changed, what's missing
    → Output: updated JSON files

  STEP 2: BUILD (Builder — 30 min)
    - Read scope v2 + latest feedback
    - Apply changes that use the new data
    - Wire any data that was loaded but not displayed
    → Output: updated index.html

  STEP 3: VERIFY + PUSH (Codex + git — 10 min)
    - Spot-check 3 accounts
    - Verify no data loss
    - Push to repo
    → Output: live dashboard updated

  STEP 4: REPORT (Pablito — 5 min)
    - What changed in the data (accounts that moved)
    - What changed in the dashboard (fixes applied)
    - What's still missing (top 3 gaps)
    → Output: Slack DM to Facu, 5 lines max

TOTAL: ~75 min per cycle. Ships every time.
```

### Short-term (this weekend): 3 sharp loops

**Loop A: Data refresh (every 4 hours)**
- Rose pulls latest from Snowflake
- Saves to data/ directory
- Reports coverage delta

**Loop B: Build + push (triggered after Loop A)**
- Builder reads new data + scope v2 feedback
- Wires new data into HTML
- Pushes to repo
- Reports what shipped

**Loop C: Facu digest (once daily, morning)**
- Reads Slack DM for feedback
- Reads dashboard state
- Sends: what changed, what's next, what needs decision
- 5 lines max

### Medium-term (next week): add intelligence

**Loop D: Account change detection (daily)**
- Compare today's metrics vs yesterday's
- Flag: accounts that grew, declined, churned buyers, new connections
- Feed into daily report

**Loop E: Meeting prep (triggered by calendar)**
- Before each TX meeting: top 3 accounts to discuss + why
- Before each client call: account brief + talk track
- After each call (Fathom): capture insights by account

### Long-term (month 2): autonomous improvement

**Loop F: Lead evolution (weekly)**
- Which leads had action taken? What was the result?
- Propose new leads from patterns
- Archive leads that don't work
- All proposals DRAFT — Facu approves

**Loop G: Enablement evolution (biweekly)**
- Which talk tracks worked in client calls?
- What objections came up?
- Update enablement content
- Add case studies from successful accounts

---

## HOW TO SHOW VALUE SHORT-TERM

The dashboard shows value when someone opens it and:
1. Sees CURRENT data (not stale)
2. Can answer "which accounts need attention?" in 10 seconds
3. Can open an account and see WHERE the opportunity is
4. Can show the account data to a client or stakeholder

### This weekend's goal: all 4 working

1. **Current data** ✅ temporal data from Snowflake (as of Jul 30)
2. **Which accounts?** ✅ sort by trend, filter by priority, see gaps
3. **Where's the opportunity?** 🔄 cards need offline comparison + benchmarks (Rose running now)
4. **Show to client?** ⏳ needs cleanup, best-in-class, SO WHATs that make sense

### The metric that matters:
**Can Facu open the dashboard before Monday's Chapter TX and use it to decide which 3 accounts to discuss?**

If yes → the dashboard is working.
If no → we failed, regardless of how many audits we ran.
