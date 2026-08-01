# TX Dashboard — MVP and Continuous-Improvement Execution Guide

**Status:** DRAFT REVIEW PACKET — does not change canonical truth or authorize
publication, data write-back, scheduling, or autonomy promotion.
**Purpose:** turn the MVP and operating-loop intent into one buildable,
verifiable system that a future operator can find and run.

## 1. Definition of done

The MVP is done when Tab 1 lets a reviewer answer, for a selected account:

1. Why is this account a priority now, or why is it not?
2. What are the estimated sell and buy opportunities, capture, and trend?
3. What is the first unresolved prerequisite in the sequence `BUY → LIST →
   SELL` (or an explicit configuration blocker)?
4. What evidence supports the conclusion, how fresh is it, and what is
   missing?
5. What is the next owner action and the correct next click: Account Deep Dive
   or opportunity enablement?

It is **not** done merely because an HTML page renders, because all proposed
tabs exist, or because a report was published.

## 2. MVP boundary and build order

| Phase | Deliverable | Must pass before moving on |
|---|---|---|
| P0.0 | Inventory + no-loss baseline | Every currently displayed metric has a source, trust state, coverage and disposition. |
| P0.1 | Tab 1 structural shell | Scope-v2 columns, filters, five-card order, and routes render without changing metric logic. |
| P0.2 | Verified data contract | Coverage audit passes and every material field has `value`, `period`, `source`, `trust_level`, `as_of`, and `freshness_status`. |
| P0.3 | Data-driven Tab 1 | Five representative product profiles pass evidence, matching, totals, prerequisite and browser checks. |
| P0.4 | Facu browser review | Facu accepts or corrects the real-account surface; corrections enter the feedback log. |
| P1 | Lead tabs and historical cycles | Only after P0 earns trust; all new lead logic remains DRAFT. |

Structural work may start at P0.1, but it must first freeze P0.0 and it may
not invent values or hide missing coverage.

## 3. Data contract and release gates

The current files contain 399 accounts and 112 metric records. Until the
coverage audit proves otherwise, the UI must say that the account-level metric
coverage is incomplete and distinguish `MISSING` from zero.

Each material displayed value requires this envelope:

```json
{
  "value": 123,
  "period": "2026-07",
  "source": "PRODUCTION.ANALYTICS.SALE_DETAILS_SV",
  "trust_level": "VERIFIED",
  "as_of": "2026-07-30",
  "freshness_status": "FRESH",
  "filters": ["ks_flag=TRUE"],
  "row_count": 42
}
```

Rose's extract must obey *all* current `agents/rose/memory/data_rules.md`
rules, including channel authority, semantic-view and de-duplication rules,
dump/shrinkage treatment, BUY `total_cost`, and the common fee/GMV cutoff. A
plan that lists only a subset of those rules is not an adequate verification
contract.

Release requires: JSON load; account-ID/name matching; totals reconciliation;
five product-profile inspection; no-loss table; source/freshness check;
browser smoke test; and a verifier other than the builder.

## 4. The L0 operating loop

This is the smallest useful loop. It runs only when Facu triggers it and every
run produces one review packet.

```text
Trigger + run card
  → producers create source-backed change envelopes
  → Codex verifies data integrity, provenance, freshness and rule compliance
  → Mercurio / Socrates interpret only verified evidence and label proposals DRAFT
  → daily TX review packet
  → Facu approves, rejects or corrects
  → decision + outcome enter the run ledger and feedback queue
  → next run checks whether the change improved the result
```

### Per-run contract

| Role | Produces / decides | Cannot do |
|---|---|---|
| Rose | Metric deltas, coverage, red flags with provenance | Certify its own data or change dashboard conclusions |
| Pablito | Call, Slack and commitment signals with source links | Treat private commentary as customer/account fact |
| Codex | Verification result, failures and evidence gaps | Invent source data or approve a proposal |
| Mercurio / Socrates | DRAFT plays, enablement and stakeholder proposals | Promote a lead or mutate canonicals |
| Facu | Accept, reject, adjust, prioritize and promote | — |

The daily packet must contain a `run_id`, `as_of`, source list, verification
status, owner, due date, expected outcome, feedback reference and a link to
the previous comparable run. A missing source or freshness field makes the
claim DRAFT, not fact.

## 5. How the system improves rather than accumulates reports

For every approved action, capture one outcome metric and one learning state:

```text
proposal → Facu decision → owner action → observed outcome
         → worked / failed / inconclusive + reason
         → proposal to keep, revise, retire, or test again
```

Weekly, the loop reviews: runs completed, verifier failures, actions completed,
outcomes observed, repeated data gaps, Facu corrections, and proposed changes.
Only Facu can approve a change to a lead, workflow, canonical, priority or
autonomy level.

### Friday close: adjusted plan + team communication

The Friday biweekly TX meeting is not complete when the meeting ends. Its
approved decisions must become the operating plan for the next two-week cycle
and be communicated to the people who execute it.

```text
Friday TX meeting + post-meeting review
  → Facu approves decisions / corrections
  → priority and action plan is adjusted
  → draft email + Slack update are produced from that approved plan
  → Facu approves outbound communication
  → email + Slack are sent to the defined audience
  → owners acknowledge / execute; the next daily report tracks movement
```

The Friday packet must therefore contain:

| Output | Minimum content |
|---|---|
| Adjusted two-week plan | Priorities entering, remaining or leaving; reason; account owner; next action; due date; expected outcome; blocked dependencies. |
| Team email draft | Sent to the current TX Leaders participants: what changed, why it matters, what each team/owner must do, deadlines, links to the dashboard and relevant account/deep-dive surfaces. |
| Slack draft | Post in **TX Leaders**. Short operational version: top priorities, decisions, owner actions, deadlines, blockers and link to the detailed plan. |
| Change log | What changed from the prior plan, the decision/source, Facu approval, and the loop(s) that must now run. |

At L0/L1, email and Slack are drafts only; Facu approves the exact audience and
content before send. They must never claim an action, owner, priority or result
that has not passed the review gate. Later automation may prepare drafts, but
outbound sending remains an explicit approval boundary unless Facu separately
authorizes a narrow, proven routine.

### Recurring Transactions meeting loop

There are four distinct meeting modes. They share verified inputs but must not
be collapsed into one generic meeting report.

| Meeting | Cadence | Governing question | Required follow-through |
|---|---|---|---|
| TX Leadership | Monday weekly | What strategic, organization, priority or accountability change is required? | Route approved changes into the two-week plan, owner actions or stakeholder/change-management work. |
| TX Managers Execution | Wednesday weekly | Which account actions, owners and blockers must move this week? | Update the execution/commitment queue and trigger deep dives where evidence is insufficient. |
| TX OKR Review | Every two Tuesdays | Are Transactions and the relevant company OKRs on/off track, why, and what must leadership decide? | Reconcile pacing with official OKR truth; route approved target/initiative/accountability decisions to the appropriate loop. |
| Chapter TX | Every other Friday | What did the team learn, what changes next, and how do we align the next two weeks? | Produce the adjusted plan plus approved TX Leaders email/Slack drafts. |

The Daily TX Change Report supplies evidence to these meetings and consumes
their approved decisions afterward. It does not replace them.

## 6. Universal meeting-close loop

Every material meeting — client, account, internal execution, TX leadership,
TX OKR, Chapter TX, product or stakeholder — uses the same closed loop. The
meeting type changes the preparation and routing, not the requirement to close
the loop.

```text
calendar event / meeting request
  → classify meeting and prepare only what its purpose needs
  → meeting occurs; transcript, notes or manual capture becomes the source
  → read-safe extraction of commitments, decisions, learnings, challenges and next steps
  → private Facu Slack review message
  → Facu confirms, corrects, rejects or adds context
  → approved items route to the correct queue / owner / learning record
  → the next prep and daily report check completion and outcome
```

### Prep is decision preparation, not a generic briefing

Every prep starts with five fields: objective, meeting type, decision or
outcome sought, current evidence, and unknowns/questions. This prevents a
heavy account-deep-dive prep from being used for a 1:1, and prevents a casual
status meeting from ending with no decision.

| Type | Minimum prep | What a good close must produce |
|---|---|---|
| Client/account | Account truth, goal, open opportunity, talk track, questions | Client commitments, owner follow-ups, objections, evidence and expected result. |
| Deep dive / problem solving | Hypothesis, evidence gaps, decision to make, people needed | Validated/invalidated hypothesis, investigation/action, data gaps and next proof. |
| TX execution | Priority accounts, blockers, owner actions, overdue commitments | Approved owner/due-date/action list and escalation path. |
| TX leadership / OKR | Pacing, choices, trade-offs, decisions needed | Leadership decision, change-management action, target/initiative challenge and accountable owner. |
| Team learning / Chapter TX | Examples, changes, patterns and questions | Updated two-week plan, learning candidates and team communication draft. |

### Private Slack review message (v1)

The initial review surface is a **private message to Facu** (or a dedicated
private review channel), not TX Leaders. TX Leaders receives only approved
Friday communication. This keeps sensitive client, people and change-management
context out of a broad channel.

Every close message contains:

```text
MEETING CLOSE — [meeting] — [date]

1. What I understood happened (source-backed facts)
2. Decisions / non-decisions
3. Commitments: owner · due date · expected outcome
4. Challenges to the current plan / assumptions
5. Learnings and hypotheses (clearly marked DRAFT)
6. Next steps and the loop each item triggers
7. Feedback for Facu: one to three constructive questions or risks to consider

Reply: CONFIRM | EDIT <item> | DROP <item> | ADD <item>
```

V1 uses those structured replies rather than pretending Slack buttons exist.
An interactive button workflow would require a dedicated Slack app and its own
approval/security design.

### Approval and learning rules

- Raw transcripts and private commentary are not durable system truth.
- Each extracted item has a source, trust state and routing destination.
- A transcript-only inference is `needs_validation`; it cannot silently create
  a priority, owner commitment, play, canonical change or external message.
- Facu's correction is captured as feedback to the meeting-close extractor and
  must be tested in the next comparable meeting.
- Start automatic prep and close *drafts* now; keep confirmation before system
  write-back. Review at least **10 material meeting closes**, spanning several
  meeting types, before proposing any relaxation of the confirmation gate.

## 7. Reconciliation findings for the current plan

Use the canonical autonomy ladder, not a new one: L1 needs at least three
auditable clean L0 runs; L2 needs five clean L1 runs and approved cadence; L3
needs two clean L2 weeks with defined bounds. A failure or correction demotes
the affected routine immediately.

## 6. Reconciliation findings for the current plan

| Finding | Required resolution before execution |
|---|---|
| Scope v2 calls the second card **OPPORTUNITIES**; older documents call it CONFIG. | Build it as OPPORTUNITIES while retaining CONFIG logic and the existing tab framework; record the naming decision in the no-loss table. |
| The plan says no build starts before verified data, but also allows structural work now. | Permit only P0.0/P0.1 shell work before data; data-driven claims wait for P0.2. |
| Current P0 is Tab 1 trust, while the plan treats all lead tabs and the complete daily report as immediate work. | Keep lead work as DRAFT research; do not let it block or expand the P0 Tab 1 release. |
| The plan's promotion gates conflict with `SYSTEM_DESIGN.md`. | Use the canonical 3 L0 / 5 L1 / 2-week L2 evidence gates. |
| Publish and scheduled routines are external actions. | Require the recorded Facu approval and a fresh run card immediately before publish or scheduling. |
| Friday decisions do not yet produce an adjusted-plan communication. | Add an approved-plan email and Slack draft to the Friday close; send only after Facu approves audience and content. |
| A daily report combines multiple producers. | Require one common schema, run ID, source/freshness fields, verifier result and feedback/outcome record. |

## 8. What to do next

1. Produce the P0.0 no-loss metric inventory from the current `index.html` and
   all loaded JSON files.
2. Decide the data-refresh owner/path and record it in the run card; no query
   or write-back is implied by this guide.
3. Build P0.1 only after the inventory is reviewed.
4. Run one L0 daily packet using existing evidence and score it with Facu.
5. Publish or schedule only after a separate, explicit approval and validation
   gate.

## 9. Where to find things

Start at `docs/transactions/README.md`. The dashboard lives in
`docs/transactions/index.html`; its data is `docs/transactions/data/`; scope
and source rules are in this directory; enterprise loop rules live in
`ops/canonical/operating_loops.md`; and each material run must have a card in
`ops/run_cards/` plus a reviewable output in `ops/reports/`.
