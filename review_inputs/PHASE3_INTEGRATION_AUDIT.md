# Phase 3 — root integration audit

**Status:** local review passed for the checks below. **Not yet public-release
approved.** No Supabase write, lead promotion, task, customer action, commit or
Pages publication occurred.

## What was audited

| Area | Result | Evidence |
|---|---|---|
| BUY active card | PASS with data limits visible | Price window separation; Arizona USD candidate; vendors/categories/horizons preserve `not available`; no bunches. |
| LIST active card | PASS with proxy semantics | Account online sales proxy / account offline sales proxy / Kennicott online best-in-class reference are distinct. No catalog/inventory claim. |
| SELL active card | PASS as evidence-first view | Observed V2 SELL primary; buyers, CVR, repeat, concentration, format, channel and product division remain visible with their own source/window. No generic buyer scenario. |
| Zeidler source reconciliation | PASS as a visible contradiction, not a conclusion | Live SFDC Account confirms Core + Procurement + eShop; local config is `SCORECARD (NOT VERIFIED)` with MaxAge 0 / bunches off; H1 observed online format is $163,709 and 100% bunches. |
| Zeidler buyer visibility | PASS | `metrics_v2_buyers_full.json`: 162 total, 34 online, 162 offline; L30D 28/77; AOV $314.66/$417.27. |
| Category semantics | CORRECTED | Removed active card and opportunity-generator `total - online = offline category` claims. The sources do not provide a disjoint offline category population. |
| Config semantics | CORRECTED | `audited:false` snapshots no longer create config blockers, bottlenecks or auto-generated config opportunities. |
| GA4 semantics | PASS with attribution state | Live daily E-Commerce GA4 Sheet exists; stale local export is not treated as all availability. Shared host is attribution/model state, not a lack-of-data state. |
| Search filter | PASS | Local browser test filtering `Zeidler` returns exactly one account row. |
| Syntax and runtime | PASS | `git diff --check`, JavaScript parse, and local JSDOM data-load test: zero errors. |

## Config-versus-observed coverage check

Root joined every `config.json` record marked `audited:false` to H1 observed
online format where available.

- **26** unverified configuration snapshots have observed online-format data.
- **21** contradict one or more observed signals (bunches, forward behavior,
  MaxAge/Future interpretation).
- The P1 member of this conflict set is **Zeidler**; additional operating
  examples include WE GOT FLOWERS and Shamrock.

This is why the active dashboard now treats `audited:false` configuration as
**unknown**, never as a confirmed blocker, bottleneck or automatic
opportunity. The data remains visible for reconciliation with observed
behavior; it is not discarded.

## Root test receipt

The local browser test loaded `index.html` and its JSON assets, searched for
Zeidler, and asserted:

- one filtered account row;
- Zeidler buyer values visible;
- unverified configuration warning visible;
- no false `MaxAge=0` / `Bunches OFF` bottleneck;
- active BUY coverage, LIST three-proxy and SELL customer-evidence sections
  render; and
- zero browser errors.

All assertions passed.

## Remaining gates before public review

1. Remove the now-inert legacy BUY/LIST/SELL renderers wrapped in `if(false)`;
   they are not executing, but retaining competing historical logic risks
   future regressions.
2. Perform visual review of the dense cards in browser for Price, Kennicott,
   Zeidler and Arizona—not only DOM/source tests.
3. Build the selected-period `sell_account_spine` before allowing any
   buyer-to-GMV conversion, offline activation cohort or traffic scenario.
4. Refresh/materialize the live GA4 Sheet into dashboard data and define a
   validated account attribution key/rule for shared hosts.
5. Obtain current catalog/listed and comparable offline available-supply
   snapshots. Until then, LIST correctly remains a three-proxy sales view,
   not a parity assertion.

## Upstream-use receipt

Used: Phase 3 receipts; `metrics_v2_*`, `loop2_*`, `config`, `bunches`,
`benchmarks`, `anticipation_real`, live Salesforce Account query for Zeidler,
and the live GA4 Sheet metadata/current range.

Not used as truth: a non-audited scorecard config snapshot; category
`total - online` subtraction; shared-host GA4 as an account CVR; sales proxies
as inventory/listing data.
