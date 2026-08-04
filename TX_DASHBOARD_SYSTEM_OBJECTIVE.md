# TX Dashboard & Growth Intelligence System — Objective

## The objective

Build the system through which Koronet understands each customer and its
ecosystem, identifies how to create more value for that customer and its own
buyers/suppliers, and turns what works into repeatable plays, playbooks,
product priorities and commercial results.

More BUY/SELL through Koronet — ideally online — and more transaction fees are
the economic result of this system. They are not the reason to force activity
that does not create value for customers.

The system's broader job is to define and drive the priorities that should
happen across Facu and the relevant teams; report clearly on those priorities,
actions and movement; learn faster than the industry; and progressively scale
the plays that produce real customer and Koronet outcomes.

## Strategy context and anti-flattening rule

TX strategy lane: strategic_enabler

The broader Transactions strategy is to use Koronet's importer strength to
become the best platform for wholesalers, expanding ecosystem GMV and the
share that can happen digitally. The Q3 operating focus for CS and
Implementation is wholesale BUY/procurement activation because it is the
broadest, highest-confidence motion: most operating accounts need usable
online supply before marketing or SELL can create durable value. It is not the
entire strategy.

LIST, SELL, GROW, catalog/PO, Studio, partnerships, monetization and product
enablers can be pursued in parallel for specific accounts or cohorts. Each
parallel bet must state its readiness, confidence, prerequisites, owner,
metric and expected learning. The system must never simplify this into a claim
that every account must move one stage at a time or that a focused team motion
is the whole corporate strategy.

## First: the operational dashboard MVP

The first product is an **operational human-intelligence dashboard**, not an
autonomous decision-maker.

It must give a human the correct metrics, definitions, denominators, periods,
freshness and uncertainty in a format that makes it possible to:

1. filter and find accounts improving, worsening, large, under-captured or
   otherwise worth investigating;
2. open an account and understand its potential, Koronet capture, online
   BUY/SELL share and movement;
3. trace what may be blocking growth through POTENTIAL → BUY → LIST → SELL;
4. investigate an existing DRAFT lead with the underlying evidence; and
5. see patterns that suggest new plays or a better version of an existing
   play.

The MVP succeeds when Facu, CS, Implementation or another knowledgeable human
can use it to make a better account/priority decision without being misled by
an unsupported metric, causal claim or lead. It does **not** need to decide,
message customers or claim causality on its own.

### P0 Operations Action Layer — dashboard-native contract

The Operations Dashboard must eventually contain one **P0 action layer** for
the small set of accounts that need useful human attention now. It is not a
separate workbench and must not create a second account model. It consumes the
dashboard's account truth and links to its existing evidence.

Before this layer can be called ready, each surfaced account must show:

1. **Evidence-based selection:** why it is in the P0 set, rather than an
   arbitrary score or stale queue position.
2. **Why now:** a dated metric movement, material event or source-backed
   change, with the source and freshness visible.
3. **One account truth:** the layer does not duplicate or contradict the
   dashboard's BUY/LIST/SELL/monetization/account evidence.
4. **Useful move:** a concrete next action, feasible owner/handoff and how
   Facu can use it in a call, email or meeting.
5. **Honest uncertainty:** missing facts, blockers, confidence and the result
   or learning that should be captured after action.

Codex verifies the contract, data gates and regression fixtures—including the
Price's fixture—before declaring the P0 layer ready. The dashboard owner and
the relevant evidence/interpretation owners build it; P0 never authorizes an
external action, account-state mutation, priority decision or play promotion.

### Who uses which surface

The operational dashboard itself is the transactions center for **Facu, Cata
and Christine**:

- **Facu** uses the portfolio and account evidence to set priorities, direct
  customer influence and decide what the system/teams need next.
- **Cata** uses it to run the priority-account mechanism with CS: which
  accounts have potential, what discovery/follow-up is needed, and what must
  hand off from Implementation.
- **Christine** uses it to see implementation/configuration readiness, digital
  activation and what is ready or blocked before CS can work the account.

The dashboard must make the Christine → Cata/CS handoff visible: customer
objective, implementation/config evidence, prior proposal/history, discovery
question, next action, owner and next check.

**Cata's CS reps** (Christian, Denisse, Santiago, Sebastián and Juan Diego),
Christine's implementation team and the Activation Lead use the **Artifact**
derived from this dashboard: filtered accounts, approved leads, actions and
enablement. Artifact consumes dashboard diagnosis; it must not create a
second, diverging analytical truth.

**Mauro and Dan** consume the leadership/aggregate view for ecosystem health
and trends. Product, Marketing and Sales consume validated cases, plays and
reporting outputs; they are not the primary daily users of the raw account
dashboard.

Once the MVP is trustworthy, teams implement the best current plays by
priority. Their actions and results then become the evidence that improves the
dashboard, plays and priorities.

## The three connected levels

```text
Accounts create cases → Plays turn cases into repeatable know-how
→ Reporting drives priorities and accountability → better account work
```

### 1. Accounts

Understand the customer's business and ecosystem. Determine where Koronet can
help the customer grow, what is holding it back, and which valuable next
investigation or action is appropriate.

The account view combines the universal outcome spine:

- estimated company GMV / potential;
- Koronet BUY and SELL GMV plus capture/penetration;
- online BUY and SELL GMV/share;
- observed and explicitly estimated fees; and
- compatible movement, coverage and freshness;

with account-specific strategy levers such as supply visibility, vendor/K2K
activation, offline buyer conversion or shop readiness.

The diagnosis uses the relevant comparison, not a generic benchmark: what the
account buys/sells offline versus online, comparable best-in-class accounts,
market/customer needs and the account's own historical movement. This is how a
human can distinguish “this account needs more supply visible” from “this
account needs a different play” before acting.

### 2. Plays

A play is not a generic theme. It is:

```text
Lead logic → playbook / enablement → case studies
```

- **Lead logic:** source-backed segment, trigger, exclusions and prerequisite
  that identify a DRAFT opportunity.
- **Playbook / enablement:** customer value story, diagnosis questions, action,
  pitch, objection handling, owner/handoff, leading indicator and outcome KPI.
- **Case studies:** comparable account context, action, response, objections,
  effort, leading-indicator movement, outcome movement and confidence.

Success, failure and inconclusive cases all make a play stronger by clarifying
where it applies. Over time, the system scales only the plays that have earned
confidence, and turns repeated valuable friction into product priorities.

### 3. Reporting

Reporting is the operating control layer, not a retrospective dashboard. It
must show:

- which accounts and plays are priorities now, and why;
- actions attempted, owner and next step;
- KPI movement separately from leading-indicator movement;
- data freshness, coverage, uncertainty and blockers;
- what teams/system/Facu are needed to unblock results; and
- what was learned and what should change next.

Every operating update should end with up to three ranked moves and explicit
help needed from Facu: a decision, relationship, customer context, team owner,
data access or priority trade-off. The system must make it easy for Facu to
know where his intervention creates the most leverage, rather than merely
reporting activity.

This creates accountability and rigor across Customer Success, Implementation,
Marketing, Sales, Product, data systems and Facu's direct customer influence.

## Operating architecture: dashboard, portfolio, learning

The TX Dashboard is the **account/case layer**. It should not be asked to be
the strategy, Board portfolio and learning system in one UI. The three layers
connect through explicit contracts:

| Layer | Primary question | Minimum output | Does not decide |
|---|---|---|---|
| **Account dashboard / cases** | What is true for this account, what is the bottleneck, and what could we test or do? | Evidence, confidence, account context, possible play, owner path and outcome/learning to capture | Company priority or strategy |
| **TX portfolio review** | Which motions deserve attention now across the four lanes? | Ranked mass focus, selective tests, monetization items and strategic enablers; trade-offs and Facu decision needed | Account truth or unilateral team assignments |
| **Play and learning system** | What did cases teach us about which play works, for whom and why? | Case outcome, leading indicator, confidence change, playbook/product implication and recurrence | That a single case proves a scalable play |

The dashboard feeds the portfolio review with **cases**, not generic leads.
The portfolio review selects or rejects work and sends an explicit charter back
to an account/team. Teams execute or test; reporting records movement; the
learning system updates the play, product signal or future portfolio question.

### The portfolio lanes

Every active item in the portfolio has exactly one primary lane:

1. `mass_operating_focus` — a repeatable, broad motion (currently BUY-led
   procurement activation for wholesalers).
2. `selective_test` — a named account/cohort bet that may run in parallel.
3. `short_term_monetization` — pricing, billing, API or account-economics
   capture.
4. `strategic_enabler` — product, partnership, inventory, fulfillment,
   demand or other capability needed to capture the longer-term opportunity.

An item may support another lane, but it must have one primary lane so the
portfolio can make trade-offs instead of treating unrelated work as one queue.

### Required handoffs

**Dashboard case → portfolio:** account/cohort, evidence and confidence,
customer value, possible play, readiness/prerequisites, feasible owner path,
metric, and the decision question. `DRAFT` cases are evidence for review, not
team instructions.

**Selective test charter → team:** cohort, hypothesis, why now, confidence,
prerequisites, owner, leading indicator, outcome metric, review date and
expected learning. No field means the test remains `DRAFT` or `BLOCKED`.

**Execution/result → learning:** action actually taken, customer response,
leading-indicator movement, outcome movement, blocker, source-backed result,
and whether the play should be strengthened, changed, retired or escalated as
a product/portfolio signal.

### Review rhythm

The control loop runs **three times per day**. Each run ingests material new
evidence, detects account/portfolio movement or blockers, updates the relevant
case/test/result state, and surfaces only decisions or exceptions that need a
human. The next run checks whether the routed action produced new evidence.

A daily portfolio view consolidates material movement across the four lanes:
what changed, what was learned, what should change in a play, and what
decision/trade-off needs Facu. A weekly view is a synthesis for leadership or
planning—not a gate that delays account investigation, execution or learning.

## The compounding loop

```text
Observe correct data → humans detect/diagnose → prioritize account + play
→ teams test/execute → report action and movement → learn from cases
→ strengthen/retire/create plays → prioritize better → scale results
```

At first, humans use the dashboard to discover and decide. Later, the system
can propose priorities and plays with confidence/evidence. It earns limited,
reversible autonomy only per action type after its recommendations have shown
good approval and outcome quality. Customer-facing actions and material
priority/authority changes remain explicitly approved.

## What excellence looks like

Koronet becomes unusually good at digitalizing the flower industry because it
learns faster than anyone else which customers need what, which plays create
value, why they fail, how teams can execute them, and what product capability
makes the valuable behavior easy at scale.

At maturity, the system can truthfully say:

> This account matters now for these observed reasons. This play is the best
> current hypothesis for this segment, with these prerequisites, evidence,
> case studies, objections and expected leading indicator. This is what we
> will measure. This is what happened last time, and this is how the play or
> priority should improve.
