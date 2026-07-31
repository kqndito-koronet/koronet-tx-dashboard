# TX Dashboard / Operational Artifact — scope, feedback and recovery record

**Cut-off:** 2026-07-31  
**Current surface:** `docs/transactions/index.html`  
**Purpose:** preserve the exact logic being reviewed locally and make the review history recoverable when the terminal or localhost session is unavailable.

## What is currently published locally

The working dashboard is a single account-first surface with eight tabs:

`Accounts` · `CONFIG` · `BUY` · `LIST` · `SELL` · `GROWTH` · `What-If` · `Glossary`

The Accounts tab is functional and loads the JSON files in `data/`. It uses five expandable cards per account, in this order:

1. **POTENTIAL** — estimated sell/buy, Koronet activity, penetration, implementation stage and fees by channel.
2. **CONFIG** — product profile, blocking/limiting configuration evidence, opportunities, bottleneck and next action.
3. **BUY** — procurement funnel, online/offline comparison, unit economics, vendors, categories, varieties, SKUs and leakage.
4. **LIST** — catalogue/listing availability, time depth, categories, varieties, SKUs and eShop blockers.
5. **SELL** — online/offline buyers and GMV, channel mix, fees, retention, repeat/churn, concentration and format.

## Feedback applied

- One unified dashboard, not three separate views.
- POTENTIAL is first; MONETIZE is merged into it and duplicate fee summaries were removed.
- Opportunities are part of CONFIG.
- No unexplained traffic-light semaphores; show the underlying data inline.
- Leads are organized by CONFIG → BUY → LIST → SELL → GROWTH.
- Implementation is a filter, not a separate operating concept.
- Est. Buy uses the account buy/sell ratio where available, with 54% as fallback; a flat 60–70% assumption is not used.
- Pre-go-live accounts are labeled as potential, not as failed Koronet activity.
- Implementation stage appears in POTENTIAL for IMPL accounts.
- Fees are shown by channel with GMV, fees and take rate, including unmonetized-channel flags.
- CONFIG explains the product profile (Core, K2K, Procurement-only, eSuite/implementation).
- CONFIG issues are product-type-aware; empty catalogue conditions are treated as major blockers.
- LIST explicitly blocks Procurement-only accounts from listing and warns for K2K limitations.
- BUY includes unit economics and varieties/SKUs; Procurement-only accounts get a simplified buy view.
- The total-buying denominator is used for online penetration, not only Koronet buying.
- Plays follow the dependency chain BUY → LIST → SELL.

## Feedback still open

- Add best-in-class comparison by metric.
- Add anticipation horizons (7/14/30/+30 days); currently blocked because `CREATED_DATE` is absent from the semantic view.
- Add box-vs-bunch context by account profile.
- Add best-in-class/offline time-depth context to LIST.
- Add multi-tag filtering so accounts such as Mayesh and Dreisbach appear under every relevant tag.
- Complete the SELL card formatting and carry forward every scorecard-v18 metric without losing context.
- Complete missing priority-account data before presenting the dashboard as a decision surface.

## Data and coverage notes

- Est. Sell GMV was backfilled from Christine ORA, SFDC or an explicitly labeled estimate.
- Tennessee (`650986`) and Springfield (`828336`) were identified and backfilled.
- Riverside (`664096`) was added with estimated metrics.
- Arizona Family, A Florist First, Nova and Avon Valley were backfilled from Snowflake.
- Coverage backfill reached 254 accounts for time depth; seven accounts were confirmed at zero sell-side activity.
- The account/session detail and unresolved work are recorded in `agents/pablito/memory/tx_dashboard_session_state_jul31.md`.

## Recovery and publishing rule

The source of truth for this surface is the complete `docs/transactions/` directory, not a screenshot or a localhost cache. Publish that directory together with this document. Do not commit the entire dirty repository blindly: the worktree contains unrelated agent and archive changes.

Before pushing, verify:

```bash
cd /Users/facu/Koronet_OS
python3 -m http.server 8771 --directory docs/transactions
open http://localhost:8771/index.html
```

Then commit only the TX dashboard directory and this scope record. The commit message should identify the dashboard version and date.

