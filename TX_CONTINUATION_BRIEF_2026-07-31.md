# TX / Pablito — continuation brief

**Date:** 2026-07-31  
**Purpose:** resume this conversation without losing scope, feedback or operating intent.

## Core decision

TX is a **unified operational dashboard**. Operational means:

- understand and define priorities;
- make clear to CS and Implementations which client needs what, why now, and what to do;
- route from account evidence to Client Deep Dive or to the relevant opportunity/enablement tab;
- later track two-week cycles: priorities, conversations, actions, changes, blockers, results and learnings;
- aggregate cycles to measure execution capacity and learning capacity for long-term decisions.

## P0

Make Tab 1 (**Accounts**) structurally correct and trustworthy before building the full operating layer.

Tab 1 must show all accounts, with easy current and historical priority views. It contains compact triage summaries in this order:

`POTENTIAL → CONFIG → BUY → LIST → SELL`

It should route in two ways:

1. Account understanding → existing Client Deep Dive workflow/repository.
2. Opportunity execution → the relevant CONFIG / BUY / LIST / SELL / Revenue Growth enablement tab, filtered to the account.

If no Client Deep Dive exists, the account goes to the Deep Dive backlog, ordered by evidence-backed opportunity.

## Framework that must remain unchanged

Current tabs:

`Accounts · CONFIG · BUY · LIST · SELL · GROWTH · What-If · Glossary`

CONFIG / BUY / LIST / SELL / GROWTH retain their current structure:

`A. Leads → B. Lead List → C. Enablement`

Do not replace or reorder this framework without Facu approval.

`GROWTH` means **Revenue Growth**. It is not the learning loop. Current mockup leads include product upgrades, Payments, API at zero fees, importer buyers and K2K upgrades.

## Current implementation reality

- `docs/transactions/index.html` is the functional local/public dashboard.
- Accounts is substantially built; the other tabs are mostly lead/enablement mockups and partial lead lists.
- Account Deep Dive is a separate workflow/repository surface, not yet wired as a complete click path.
- Historical cycle tracking is not yet integrated into the dashboard.
- Workflow and loop documents exist, but not all are autonomous, scheduled or connected end-to-end.

## Operating loop target

```text
all accounts → select next-two-week priorities and reasons
→ assign CS/Implementation work
→ record conversations/actions
→ record what changed and blockers
→ capture result and learning
→ measure execution and learning capacity
→ aggregate cycles for long-term decisions
```

Loops may detect drift and propose improvements. They must not silently mutate priorities, canonicals, workflows, plays or account truth. Facu approval remains the gate.

## Broader Pablito scope

Pablito should help make the user's other loops real and progressively more autonomous, with approval and no drift. This includes:

- TX dashboard MVP and daily improvement loop;
- TX follow-ups for pending top accounts;
- listening to calls, reviewing actions and updating the right workflow/state;
- preparing for the many calls next week;
- ensuring priorities, delegation, execution tracking and feedback loops work across the rest of Facu's priorities.

## Existing relevant repository surfaces

- `docs/transactions/TX_DASHBOARD_SOURCE_OF_TRUTH.md`
- `docs/transactions/TX_ARTIFACT_SCOPE_AND_FEEDBACK.md`
- `docs/transactions/index.html`
- `ops/workflows/account_intelligence_workflow.md`
- `ops/workflows/account_deep_dive/`
- `ops/canonical/operating_loops.md`
- `ops/canonical/learnings.md`
- `ops/workflows/feedback/loop_learning_audit.md`
- `ops/workflows/feedback/transactions_biweekly_friday_prep.md`
- `ops/workflows/feedback/initiative_report.md`

## Immediate sequence

1. Clarify and lock the Tab 1 contract, using the existing framework.
2. Validate account routing to Client Deep Dive and opportunity enablement.
3. Build the no-loss metric/source comparison.
4. Make the minimum enablement context actionable without expanding scope.
5. Only then add two-week historical tracking and daily loop integration.
6. In parallel, use Pablito to coordinate calls, top-account follow-ups and the rest of Facu's approved priorities.

## Open questions to ask one at a time

1. What must one Accounts row/card show for Facu to decide priority and the next click?
2. Which account fields and metrics are non-negotiable in P0?
3. What exact Client Deep Dive repository/workflow should each account link to?
4. Which opportunity tab filters are actually needed for the first MVP (do not invent extra filters)?
5. What is the minimum acceptable enablement content for CS vs Implementations?
6. Which daily loop should be made real first, and what approval gate does it need?

## Pablito top-of-mind operating portfolio

These are standing priorities for Pablito and must remain visible while TX Tab 1 is being built:

- Account Deep Dive loop, with token-aware automation controls.
- External TAM loop for wholesalers and retailers.
- Pablito's three-times-daily loop: bring what changed, what is inconsistent with the system, what needs review, and what may require reprioritization across all sources.
- Account follow-up/change loop: Mercurio identifies and interprets changes as they happen.
- Top-account and play loop: track which plays are being run, what moved and what did not.
- Transactions meeting loop: keep distinct operating context for leadership, OKRs, TX meetings with Mauro/Cata/Christine, and Friday team education/update.
- Daily close: all client follow-ups current; next-day meetings prepared; TX operational-evolution movement updated; weekly and daily priorities clear; deadlines visible; medium/long-term ideas captured.
- The TX operational-evolution update must include newly added accounts and the known tracks, including sell-by-catalog, partnerships and long-term TX product priorities.
- Medium/long-term capacity should include time for strengthening the loops themselves, not only running their outputs.

## Desired daily-close output

At the end of each day, Pablito should be able to present one concise, evidence-linked closeout covering:

1. Client follow-ups: done, overdue, blocked and next owner.
2. Tomorrow's meetings: agenda, context, decisions needed and missing preparation.
3. TX movement: accounts, plays, initiatives, blockers and operational changes.
4. Priority control: what is top priority this week, what is today's priority, what changed and why.
5. Deadlines and commitments at risk.
6. Inconsistencies or drift across sources and canonicals.
7. Longer-horizon ideas and loop-strengthening work that may deserve time, clearly separated from immediate execution.

Automation should produce drafts, evidence and alerts. It must not silently reprioritize, send follow-ups, mutate canonicals or change loop autonomy without Facu's approval gate.
