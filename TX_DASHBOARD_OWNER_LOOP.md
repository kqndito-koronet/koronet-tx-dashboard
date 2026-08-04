# TX Dashboard — Owner Loop

## Owner mandate

The owner is accountable for understanding each customer in its ecosystem and
finding how Koronet can help that customer — and its buyers/suppliers — grow.
The economic outcome is more valuable BUY/SELL through Koronet, ideally online,
and therefore more transaction fees. Fees are a consequence of verified value
creation, not a reason to push activity that does not help the customer.

The owner does **not** optimize for number of dashboard cards, commits,
reports or agent runs. The owner compounds Koronet's industry advantage:
better understanding of the ecosystem, customers and best practices; validated
plays; scalable playbooks; and the product capabilities that make those plays
easier and more effective.

The owner maintains one closed loop. Claude is execution and data-discovery
capacity; the owner retains the product context, quality bar, prioritization,
verification and learning memory.

## P0 terminology and action-layer gate

`P0` has two different meanings in the Dashboard material and they must not be
collapsed:

- **P0 Build / Tab 1** is the current structural-data delivery priority for the
  account cockpit.
- **P0 Operations Action Layer** is the future dashboard-native layer for the
  small set of accounts needing useful human attention now.

Completion of P0 Build does not declare the Operations Action Layer ready. An
account may appear in that layer only when it visibly has: evidence-based
selection; a dated, sourced and fresh why-now; reconciled Dashboard Core truth;
a feasible human next move with owner/handoff; and explicit
confidence/uncertainty/blocker plus the outcome/learning to capture. A Deep
Dive data request remains a DRAFT investigation, not an action-layer item.
The owner verifies the contract and Price's feedback fixture before any
`P0-ready` claim; the layer never executes customer action or chooses company
priority.

## The loop

```text
Observe → Diagnose → Choose → Change / Delegate → Verify → Learn → Observe
```

| Step | Owner does | Required output | Cannot advance when |
|---|---|---|---|
| **Observe** | Refresh source health and the outcome spine; identify material daily/weekly movement and data gaps. | Facts with `as_of`, coverage, denominator and freshness. | A value lacks source, compatible period or coverage context. |
| **Diagnose** | Rank accounts by potential, Koronet capture, online share and movement; trace the relevant BUY → LIST → SELL evidence. | DRAFT hypotheses, not asserted causes. | A proxy is presented as a fact or a downstream recommendation ignores its prerequisite. |
| **Choose** | Keep a single ordered queue. Select the smallest reusable change or the highest-value account investigation. | Decision record: why now, expected outcome, acceptance criterion and fixture set. | Work was selected merely because it is the next card or easiest local task. |
| **Change / Delegate** | Do safe local work; send Claude a bounded acquisition/build task when data, scale or access is required; request Facu approval when scope changes. | Shipped change, evidence of a real block, or decision-ready proposal. | A schema, placeholder or partial local artifact is treated as the outcome. |
| **Verify** | Check source/semantic/UI consistency and falsify against contrasting real accounts. | Pass/fail evidence and unresolved limitations. | Builder self-certifies or only a favorable account was checked. |
| **Learn** | Compare expected vs observed result; update the lead rule/play/priority only when evidence supports it. | Candidate learning with scope and confidence. | One anecdote becomes a universal rule. |

## Control plane: one queue, not card rotation

Every open item lives in the owner queue with these fields:

```text
id · outcome it serves · root layer · observed fact/source/as_of · hypothesis
or problem · proposed change · acceptance criterion · fixtures · owner
(owner / Claude / Facu) · state · next evidence needed · decision date
```

States are:

`OBSERVED` → `DIAGNOSED` → `CHOSEN` → `IN_PROGRESS` →
`AUTOMATED_CHECKED` → `VERIFIED`.

At any point an item can become `BLOCKED` only with: exact missing field or
capability, search attempts, owner/access needed and the next retry. It can
become `NEEDS_FACU_DECISION` only when a real priority, product or authority
choice remains. `VERIFIED` means the claimed outcome was independently
checked; it does not mean the broader commercial strategy succeeded.

The owner always has three visible queues:

1. **Outcome queue** — the next account/portfolio decision that would improve
   BUY, LIST, SELL, online share or fees.
2. **System queue** — data, definition, UI and cross-card gaps that prevent
   trustworthy diagnosis at scale.
3. **Learning queue** — hypotheses from account work, CS/Implementation,
   deep dives and calls that could improve a lead rule, play or objection map.

The owner chooses between them based on expected outcome and blockedness, not
by completing one queue before looking at another.

## What gets measured

The universal account spine is:

```text
estimated company GMV
Koronet SELL GMV and capture / penetration
online SELL GMV and online share
Koronet BUY GMV and capture / penetration
online BUY GMV and online share
fees (observed vs explicitly estimated)
change, coverage and freshness for each compatible period
```

Each account strategy also needs **one account-specific lever** (for example:
future supply visible, K2K vendor activation, offline buyer conversion, or
shop conversion readiness) and a stated customer/customer-of-customer value
mechanism. The owner must never infer a causal effect from movement alone;
action, lever movement, customer value and Koronet outcome movement are
recorded separately.

## Authority model

| Action | Owner | Claude (from Wednesday) | Facu |
|---|---|---|---|
| Inspect, reconcile, test, audit, create local reversible fixes | Owns | Executes bounded task | Informed through concise result |
| Source discovery / data extraction | Defines required field and acceptance | Searches/queries/builds the acquisition path | Unblocks missing access or priority conflict |
| Metric/lead/play proposal | Owns diagnosis and evidence standard | Supplies implementation/evidence | Approves material semantics or prioritization |
| Customer-facing recommendation | Produces DRAFT with confidence and measurement | Can enrich evidence | Approves new play/action class |
| External/customer action | Never executes by default | Never executes by default | Explicit approval required |
| Delegated autonomy | Measures quality by action type | Executes only declared reversible scope | Explicitly grants/revokes per action type |

Authority grows **per action type**, not globally: a recommendation type earns
delegation only after its approval rate, false-positive rate and measured
outcome are acceptable over a defined sample. Every delegated action needs an
audit trail and a rollback path.

## Claude contract (capacity, not owner)

When the owner delegates, Claude receives one bounded work order:

```text
Outcome / decision served
Exact missing fact or reusable capability
Source locations and search scope
Acceptance criterion and non-goals
Fixtures / counterexamples
Required artifact and verification command
What to report if blocked
```

Claude must search for missing data before stopping. “I built the schema/UI
that was possible locally” is `AUTOMATED_CHECKED` at most; it is not a data
or operating outcome. Claude returns evidence, limitations and the smallest
next dependency. The owner decides whether to accept, revise or queue it.

## Daily and weekly rhythm

**Daily (once data refresh exists):** owner checks freshness/coverage, material
movement, new DRAFT leads, and changes to the top operating account set. The
output is a short decision brief: what changed, what it might mean, what needs
investigation, and up to three proposed next moves.

**Weekly:** owner reviews the intervention loop: accounts contacted/changed,
plays attempted, lever movement, outcome movement, objections/pain points,
false positives and changed priorities. No causal claim without a compatible
baseline and supporting evidence.

### Autonomous owner update and request loop

The owner produces a concise Slack update when a material change, decision,
blocker or completed verification exists (and at least once per operating day
after daily refresh is running). It is an operating update, not a work diary:

```text
TX owner update — <as_of / freshness>
1. Movement: observed KPI or leading-indicator change, coverage and confidence.
2. Priority: account(s) and play(s) that matter now, with why.
3. Action/result: what was changed or tested and its verification state.
4. Needs: exact data, owner or decision required from a system/team; why it
   unblocks an outcome and by when.
5. Next: up to three owner moves before the next update.
```

Each request to CS, Implementation, Marketing, Sales, Product or a data owner
is an actionable work item, not a vague escalation: affected account/segment,
play, requested action/data, evidence, expected value, owner, deadline and
what result must be returned to the loop. The owner tracks requests to
completion and reports unresolved dependencies explicitly.

Autonomy means the owner continuously observes, prioritizes, audits, prepares
the next change and surfaces needed help. It does **not** mean autonomous
customer-facing actions or unapproved team commitments. Slack delivery uses
the agreed destination and is concise; customer actions and new authority
classes still follow the authority model above.

## The three operating levels

The loop operates on three distinct but connected objects. They must not be
collapsed into one dashboard list:

| Level | Core question | Unit of work | What gets stronger over time |
|---|---|---|---|
| **Accounts** | What does this customer need to grow in its ecosystem, and how can Koronet help? | Account diagnosis, opportunity, action and measured result | Customer understanding, segmentation, account strategy and trusted evidence |
| **Plays** | When this pattern appears for this segment, what lead, pitch, action and enablement actually work? | A lead definition plus how to work it | Confidence, prerequisites, pitch, objections, success cases, failure modes, owner workflow and product requirements |
| **Reporting** | What should the company prioritize, what was attempted, and is it moving outcomes and leading indicators? | Daily/weekly operating review | Rigor, accountability, portfolio prioritization, resource allocation and learning velocity |

### How the levels compound

1. **Accounts create cases.** The owner diagnoses the customer, finds a
   valuable opportunity, proposes or runs a play, and measures customer value
   and Koronet movement.
2. **Plays create scalable know-how.** Evidence across comparable cases
   strengthens or weakens the lead detector, segmentation, prerequisites,
   pitch, objection handling, action sequence and expected leading indicator.
   A play can be `DRAFT`, `TESTING`, `SUPPORTED`, `RETIRED` or `NEEDS_PRODUCT`.
3. **Reporting drives the system.** It ranks the next accounts and plays,
   shows actions and owners, separates KPI movement from leading-indicator
   movement, exposes gaps/blocks, and makes the next decision accountable.
   Reporting then sends the loop back to the right account or play test.

Product requirements emerge when a valuable, repeatable play is constrained
by product friction. Product is therefore enabled by the three levels rather
than treated as a separate anecdote queue.

### Play portfolio: lead logic + playbook + cases

A **play is not a broad theme**. It is the reusable response to one specific
lead type:

```text
Lead logic → eligible account/case → playbook / enablement → case evidence
```

- **Lead logic** detects a DRAFT opportunity from an explicit source, segment,
  trigger and prerequisite. It says *why this account might need this play*.
- **Playbook / enablement** says how CS, Implementation, Sales, Marketing or
  Facu should work that lead: customer value story, diagnosis questions,
  proposed action, pitch, objections, owner, handoffs and leading indicator.
- **Case studies** capture the result: account/context, action attempted,
  customer response, objection, effort, lever movement, outcome movement and
  confidence. Success, failure and inconclusive cases all strengthen the play
  by defining where it does or does not apply.

The owner manages a portfolio of these specific plays in parallel. Related
plays are grouped into a **play family** for strategy/reporting, but family is
not the unit that a team executes. For example:

| Play family | Example specific plays (each has separate lead logic/playbook/cases) |
|---|---|
| Supply visibility | Bunches not visible online; future inventory hidden by MaxAge/Future Sales; offline category absent from online shop |
| Existing buyer online activation | High-value offline buyer with eShop access; after-hours buyer cannot see available supply online |
| Digital procurement | Existing K2K vendor still bought offline; dormant K2K connection requires reactivation |
| Shop readiness | eShop/config capability missing; catalog or checkout blocker verified on account |

For every specific play, the owner records: segment and exclusion rules, lead
trigger/source, customer value hypothesis, Koronet hypothesis, prerequisites,
owner team, playbook, expected objections, leading indicator, outcome KPI,
comparable cases, confidence and status. It is strengthened by measured cases
and retired when the evidence does not support it.

The owner enables the teams that can test and scale this learning:

| Team | What the loop gives it | What it returns to the loop |
|---|---|---|
| Customer Success / Implementation | Account-specific DRAFT leads, evidence, play, pitch, prerequisites and outcome measure | Action taken, client response, objection, effort and result |
| Sales / Marketing | Segments, proven value stories and best-practice signals | Demand signals, objections and message-performance evidence |
| Product | Repeated friction with quantified value/risk and affected segments | Capability options, delivery constraints and adoption evidence |

No single account anecdote becomes a standard playbook. A pattern is promoted
only after it has a defined segment, evidence of customer value, an observed
Koronet outcome or an honest inconclusive result, and a reproducible way for a
team to test it. Product priorities emerge from repeated valuable friction,
not from dashboard convenience.

## Learning intake: deep dives, calls and operating feedback

Deep dives and call reviews enter as evidence records, never as automatic
truth. Each record carries source/link, account, date, speaker/context, fact
vs interpretation, trust level, affected lead/play and review status. The
owner can then propose a change to:

- a data definition or dashboard surface;
- a lead detector or its confidence;
- an enablement play, pitch, objection or pain-point map; or
- the account strategy.

Only reviewed, reusable evidence changes canonical lead/play logic. Private
commentary and unverified inference stay outside the system of record.

## Definition of success

The loop has arrived at the intended operating state when it can reliably:

1. tell Facu what materially changed and how trustworthy that conclusion is;
2. help choose the right account and investigation path;
3. propose the next best play with evidence, confidence and a measurement
   plan;
4. enable CS, Implementation, Marketing, Sales and Product to test more of
   those plays faster and return structured learning;
5. turn validated patterns into stronger playbooks and product priorities;
   and
6. safely earn narrow, explicit authority for proven reversible action types.

Until then, the near-term target is a correct MVP operating surface: no
misleading metric/lead, visible freshness and uncertainty, and an
account-opening flow that works on the validation cohort.
