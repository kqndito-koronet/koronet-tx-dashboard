# TX Dashboard Core — No-Loss Audit (Aug 1, 2026)

**Compared surfaces:** current Core `index.html`; frozen `leadership.html`,
`cs.html`, `implementations.html`; historical Artifact v4/v2; scorecard
references. This audit records a capability/user need, not a certification that
the historical calculation remains correct.

## Decision summary

The Core already preserves the richer account diagnosis and must remain the
only calculation/lead-logic surface. The missing capabilities are primarily
workflow and consumption: role-specific execution, Implementation → CS handoff,
case/action capture, verified Deep Dive route, and a formal reporting loop.
Historical health scores and independent lead calculations must **not** migrate.

| Capability / user need | Evidence in old surfaces | Current Core state | Destination | Status / acceptance test |
|---|---|---|---|---|
| Portfolio: account, owner, priority, product, potential, capture, online share, fees, trend | Leadership/CS/Artifact tables | **Preserved and expanded** in Accounts with Est Buy, Buy penetration, take rate, trend, CVR caveat | Core | Keep; verify every priority cohort has comparable periods/coverage. |
| Priority selection and “why now” | Artifact priority lists; leadership health/opps | Partial: filters + metrics, no explicit priority reason/history per cycle | Core + Reporting | **Migrate.** Priority record needs reason, evidence, selected date and next check. |
| Account diagnosis CONFIG → BUY → LIST → SELL | Scorecard/Artifact layers | **Preserved, reorganized** as POTENTIAL → OPPORTUNITIES → BUY → LIST → SELL | Core | Keep. Preserve prerequisite/trust semantics. |
| Estimated BUY, Buy capture and online BUY | Weak/absent in old Leadership/CS | **Preserved/expanded** in Core | Core | Keep; no legacy migration. |
| Temporal GMV/fees movement | Historical H1 static fields | Partial: V2 period deltas + one outcome baseline | Core + Reporting | **Migrate/complete.** Two compatible snapshots required before daily movement. |
| Data source/trust/coverage | Scorecard quality flags; Artifact caveats | Partial source/trust labels; coverage banner | Core + Reporting | **Migrate.** Every decision metric needs source/as_of/coverage; discrepancy is a blocker. |
| CONFIG health scores / traffic-light columns | Leadership/CS/Artifact calculate independent scores | Core uses evidence/issues rather than opaque scores | Do not migrate calculations | **Intentional removal.** Preserve config evidence and first unresolved prerequisite only. |
| Implementation pipeline, stage, status, days live, notes | `implementations.html`, `christine.json` | Partial inline implementation context; no dedicated handoff/workflow | Core execution mode + Reporting | **Migrate.** Christine can see stage/readiness; Cata sees explicit ready/blocked handoff, owner and next check. |
| Implementation OKRs (speed, online adoption, config completion, procurement) | `implementations.html` | Not in Core | Reporting | **Migrate after metric definitions are reconciled.** Do not copy static dashboard formulas. |
| Rep's “my accounts” workflow | `cs.html`, Artifact account filter | Core owner/rep filter exists; no approved-only execution view or outcome capture | Core execution mode | **Migrate.** Rep sees assigned accounts, approved leads, why/how, action owner and outcome form. |
| Approved lead registry / lifecycle | Artifact v2 registry | Core has generated DRAFT contract; approval workflow absent | Core | **Migrate.** DRAFT/APPROVED are the only operational states visible to teams; Facu gate recorded. |
| Lead logic definitions | Artifact v2 + historical tabs | Core normalizes generated leads with source/prerequisite/outcome | Core | **Preserved partially.** Add per-lead detection logic/period/observed signal/causal hypothesis before approval. |
| CONFIG enablement/checklist | Artifact/CS enablement tabs | Core glossary/config enablement exists | Core execution mode + Plays | **Preserve content, revalidate claims.** Link every instruction to applicable approved lead/type. |
| BUY discovery, procurement objections and connection funnel | `cs.html`, Artifact | Core has BUY card/leads/discovery content | Core execution mode + Plays | **Preserve/normalize.** Separate customer workflow, commercial setup, config and product blocker. |
| LIST challenger messages and inventory paths | Artifact + `definitions.json` | Core exposes challenger content; List data is labeled proxy | Core execution mode + Plays | **Preserve with proxy caveat.** No claim that sales recency proves catalog depth. |
| SELL activation/onboarding/7-stage flow | Artifact/CS | Core provides SELL data/leads but incomplete reusable play/case object | Plays + Core execution mode | **Migrate.** Define exact trigger, prerequisites, pitch, leading indicator and outcome KPI. |
| Best-practice examples (Maple, Kennicott, Price's, etc.) | Artifact narratives / calls | Core has segment benchmark snippets | Plays / Cases | **Migrate as evidence, not rules.** Record segment, source, context and confidence. |
| Case studies: success/failure/inconclusive | Artifact historical claims, Fathom/Gmail | No structured case record in Core | Plays + Reporting | **Migrate.** Case record must include action, response, objection, effort, lever/KPI and source. |
| Deep Dive access from account | Historical design + `accounts/*` | Existing Deep Dives; Core now has a data-driven published-link/gap state | Core → Deep Dive | **Migrated.** Eight verified routes; absence is shown as an investigation gap, never as no opportunity. |
| SFDC opportunities/context | All surfaces | Core includes SFDC matching | Core | Keep; validate name matching and source freshness. |
| Customer objective, proposal/history and discovery question | Activation-loop requirement, Artifact | Not a first-class Core record | Core execution mode | **Migrate.** Required for a Cata/CS handoff, sourced from SFDC/call evidence. |
| Action, owner, due/next check, customer result | Artifact intention, weekly loop | Not a first-class Core record | Reporting + Core execution mode | **Migrate.** This is the minimum intervention log; no reporting claims before it exists. |
| Product/process escalation | Calls/Artifact “product gaps” | No structured route | Evidence ingress → Reporting | **Migrate.** Repeated source-backed friction only; include owner, impacted segment and proof needed. |
| Leadership aggregates / health distribution | `leadership.html` | Core has account-level health and some aggregates | Reporting or Core role view | **Migrate only required aggregates.** Do not recreate separate HTML. |
| What-If fee scenarios | Artifact / Core What-If | Core has What-If but static/limited | Reporting context | **Keep as scenario.** Label assumptions, never present as forecast/outcome. |
| Pacing, TAM and board target | Pacing/Opportunity sites | Not Core | External context | **Keep separate.** Core can reference target/strategic context only. |

## Surface disposition

| Surface | Disposition now | What must happen before retirement or reroute |
|---|---|---|
| `index.html` + `data/` | Current Core | Continue audited incremental work only. |
| `leadership.html` | Frozen reference | Inventory aggregate presentation and migrate any accepted need to role view/Reporting. |
| `cs.html` | Frozen reference | Migrate rep execution needs onto approved Core leads; do not retain score columns. |
| `implementations.html` | Frozen reference | Migrate stage/readiness/OKR/handoff contract; reconcile source definitions first. |
| Artifact v4/v2 | Frozen reference | Extract verified enablement/case content into Play objects; discard independent analytics. |
| Scorecard review files | No-loss reference archive | Keep metric/quality inventory until every row is mapped or explicitly removed. |
| Account Deep Dives | Active companion | Core navigation uses `data/deep_dive_registry.json`; update its source path/as-of when a published dive changes. |
| Pacing/Opportunity | Active external context | Preserve separate ownership; document any Core input. |

## First migration work, in dependency order

1. **Priority/intervention record:** one object with account, objective, reason,
   lead/play, owner, next action, due/next check, outcome/objection and source.
2. **Christine → Cata handoff:** readiness/blocker/owner/proof needed in that
   same record, product-type aware.
3. **DRAFT → APPROVED gate:** approved lead is the only executable rep input;
   the execution mode consumes the same object.
4. **Reporting from intervention records:** only then produce weekly
   accountability and daily movement; attach snapshot coverage/freshness.

## Explicit audit limits

- This is a static code/content comparison. It does not establish actual user
  adoption or validate historical metric formulas.
- Fathom summaries and Slack are evidence inputs; call claims are not promoted
  to lead/play truth until their source/context is reviewed.
- The historical artifacts contain useful enablement but may have stale data,
  product assumptions or unsupported causal language.
