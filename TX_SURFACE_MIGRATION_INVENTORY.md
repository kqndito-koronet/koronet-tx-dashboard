# TX Dashboard Core — Surface Migration Inventory

**Purpose:** preserve useful capability while converging on one Dashboard Core.
This is a migration map, not proof that every listed surface is current or
adopted. No frozen surface may regain feature work without an explicit Core
migration item.

## Destination model

| Destination | Job | Does not do |
|---|---|---|
| **Core** | One account/portfolio data model, diagnosis, priority evidence and source-backed lead logic | Duplicate product-level strategy or team-specific independent scoring |
| **Core execution mode** | Role/account-filtered view of approved leads, next action and enablement drawn from Core | Recompute diagnosis or create a separate lead engine |
| **Deep Dive** | Consequential account-specific question/history beyond the compact diagnosis | Recreate portfolio or reusable playbook logic |
| **Reporting** | Priorities, actions, owners, movement, blockers and learning across the operating cycle | Replace account diagnosis |
| **External context** | Pacing, TAM/Opportunity, strategy targets and product context | Account-level execution |
| **Evidence ingress** | Calls, Slack, SFDC and team feedback captured with source/trust | Promote interpretation to truth automatically |
| **Reference archive** | Historical UI/spec/inventory used only for no-loss comparison | Publish, calculate truth or receive feature work |

## Surface-level inventory

| Existing surface / path | Valuable capability to preserve | Destination | State / migration rule |
|---|---|---|---|
| `docs/transactions/index.html` | Portfolio, account expansion, POTENTIAL → OPPORTUNITIES → BUY → LIST → SELL, data/trust labels, DRAFT leads, links/tabs | **Core** | Current working surface. All new account metric and lead logic lands here. |
| `docs/transactions/data/` | Canonical JSON data layer, metric definitions, freshness/coverage inputs | **Core** | Current data layer. New data must add provenance/period/coverage; no shadow JSON model. |
| `docs/transactions/leadership.html` | Leadership aggregate/health presentation | **Core role view** or **Reporting** | Frozen reference. Inventory aggregates and migrate only needed capability to a Core role view or Reporting. |
| `docs/transactions/cs.html` | Rep-filtered accounts, lead/action/enablement presentation | **Core execution mode** | Frozen reference. Preserve user need (“my accounts; what do I do?”), not independent render/data logic. |
| `docs/transactions/implementations.html` | Implementation pipeline, config/go-live readiness | **Core execution mode** | Frozen reference. Preserve Christine → Cata/CS handoff and implementation readiness. |
| `docs/tx-artifact.html`, `docs/tx-operational-artifact-v2.html`, `ops/deliverables/tx_operational_artifact_*.html` | Talking points, checklists, objection handling, per-rep execution concept | **Core execution mode** + **plays** | Frozen historical reference. Migrate only approved playbook content linked to Core lead objects; do not reuse independent health scores/data. |
| `review/scorecard_full_data.js`, scorecard reviews/specs | Historical metric inventory, data-quality flags, attention patterns | **Reference archive → Core no-loss gate** | Never publish as current. Every Core card change compares against this inventory and records preserve/migrate/remove decision. |
| `review/transactions_dashboard_v0.html`, `review/tx_L*.html`, `review/tx_connections_dashboard*.html` | Historical ideas, visual/metric experiments | **Reference archive** | Evidence for a specific migration only; not design authority over Scope V2. |
| `accounts/*/deep_dive*` | Account history and detailed BUY/LIST/SELL investigation | **Deep Dive** | Keep separate. Core needs a verified deep link; Deep Dive never becomes a second portfolio. |
| `dashboards/tx_fee_pacing.html`, `ops/reports/tx_pacing_*` | Fee target/pacing and high-level trend | **External context** | Remains separate. Core consumes relevant target/context only; it does not replace pacing. |
| `strategy/tx_strategy/`, `sites/tx-strategy-v1/` | TAM/SAM, achievable opportunity, initiatives/board context | **External context** | Remains separate. Core helps execution by account; it does not own market sizing. |
| `ops/reports/transactions_*`, meeting prep/review outputs | Priority-review input, existing prep/report artifacts | **Reporting** | Preserve only when they can be fed by Core actions/cases. They are not yet the formal intervention reporting product. |
| Slack, Fathom/Gmail recap, SFDC, CS/Implementation feedback | Customer context, actions, objections, product/process friction and decisions | **Evidence ingress** | Every extracted item needs source, date, account/segment, fact vs interpretation, trust and review state. |

## Capability no-loss checklist

Before retiring/migrating any reference surface, map each needed capability to
one destination and acceptance test:

| Capability | Current evidence | Required Core-loop destination | Acceptance test |
|---|---|---|---|
| Portfolio selection by potential, capture, online share and change | Core Accounts + historical scorecard | Core | A human can filter/rank and explain why an account is priority. |
| Account diagnosis across POTENTIAL/BUY/LIST/SELL | Core cards + Deep Dives | Core + Deep Dive | Account opening routes to evidence or a named unanswered question. |
| Config/implementation readiness and handoff | `implementations.html`, `christine.json`, Core config | Core execution mode | Christine/Cata can see ready/blocked state, owner and next check. |
| Rep's account actions and enablement | Artifact, `cs.html`, lead tabs | Core execution mode + plays | Rep sees only approved, applicable leads with why/how/owner/outcome capture. |
| Lead lifecycle and approval | Artifact registry spec, Scope V2 | Core | DRAFT vs APPROVED is visible; no DRAFT becomes rep instruction. |
| Playbook / objections / case studies | Artifact tabs, definitions/challenger text, calls | Plays linked to Core | Every play has lead logic, enablement and case evidence; source is preserved. |
| Deep investigation | `accounts/*` | Deep Dive | Core deep-links to the correct account/context without duplicating it. |
| Priority/action/outcome accountability | Prep/report scripts, initiative review docs | Reporting | Weekly review shows priority, action, owner, leading indicator, KPI, blocker and learning. |
| Data quality, source, time and coverage | Scorecard-v18 records, Scope V2, current data | Core + Reporting | Every decision metric exposes source/as_of/coverage; discrepancy is a tracked block. |
| Product/process escalation | Calls, Slack and initiative records | Evidence ingress → Reporting | Repeated, source-backed friction produces a scoped request, not a generic feature idea. |

## Immediate migration sequence

1. **No-loss audit:** compare `index.html` against the historical scorecard and
   the three frozen department surfaces; record metric/behavior/user need as
   preserved, Core migration, Reporting migration, Deep Dive migration or
   explicit removal decision.
2. **Core role paths:** make the current Core able to route Facu, Cata and
   Christine without duplicated code: portfolio priority, CS execution view and
   Implementation readiness/handoff.
3. **Deep Dive link:** connect an account row to its existing Deep Dive when
   present; otherwise show an evidence-backed “request deep dive” state.
4. **Execution package:** make approved Core lead objects drive enablement and
   outcome capture; retire the need for Artifact's separate scoring.
5. **Reporting:** connect priority/action/case records to daily/weekly review
   only after the underlying data/source contract is trustworthy.

## Explicit non-goals during migration

- No deletion or folder move of legacy files before the no-loss row is closed.
- No new dashboard HTML or department-specific data/lead engine.
- No claim that an old surface is adopted or correct merely because it exists.
- No merge of Pacing, Opportunity/TAM or Deep Dive into the account Core.
