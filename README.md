# TX Dashboard — Start Here

**Status:** navigation guide. It does not override an authoritative scope or
canonical decision.

## What this system does

The TX Dashboard is an operational cockpit, not a BI report. It helps a team
select an account, understand the evidence and bottleneck, take the next
action, observe the outcome, and improve the next run.

```text
account evidence → priority / bottleneck → owner action → observed outcome
      ↑                                                        ↓
      └──── verification ← Facu feedback ← learning proposal ─┘
```

## Read in this order

| Need | Read / open |
|---|---|
| Use the current dashboard | `index.html` |
| Know the product and UI contract | `TX_DASHBOARD_SCOPE_V2.md` (authoritative) |
| Know source, trust, no-loss, and publish rules | `TX_DASHBOARD_SOURCE_OF_TRUTH.md` |
| See the proposed workstreams | `TX_DASHBOARD_MVP_AND_LOOP_PLAN.md` (draft) |
| Build or run the MVP safely | `TX_DASHBOARD_MVP_AND_LOOP_EXECUTION_GUIDE.md` |
| Inspect the dashboard data layer | `data/` |
| Understand the wider Koronet loop architecture | `../../ops/canonical/operating_loops.md` |
| Find the live run contract / owner gate | `../../ops/run_cards/` and `../../ops/reports/agent_launch_picker_latest.html` |

## The two delivery tracks

1. **MVP (P0):** make Tab 1 trustworthy: all-account navigation, declared
   coverage, source/trust labels, correct `POTENTIAL → OPPORTUNITIES → BUY →
   LIST → SELL` triage, and valid routes to deep dives and enablement.
2. **Learning loop (L0):** run a Facu-triggered, evidence-backed change report
   that records a decision and checks whether the next run improved. It may
   propose changes; it never silently changes account truth, priorities,
   canonicals, or external systems.

The loop does not replace the dashboard MVP. It supplies verified feedback to
the same data and decision surfaces.
