# Portfolio, Potential & SELL — data audit for the Accounts MVP

**Scope:** the pre-card account spine, POTENTIAL and SELL only. Read-only audit on 2026-08-04; no dashboard logic was changed. “Current” below means the dated export is available, **not** that it is live or universally comparable.

## Decision: what can be shown now

| Field / decision | Source | Grain + account key | Period / as-of | Coverage | Trust / display contract | Contradiction or present risk | Wednesday materialization |
|---|---|---|---|---:|---|---|---|
| Portfolio row identity, products, cohort | `accounts.json` | company-location; `company_id` | snapshot; no common as-of | 426 | High for identity/product context; not performance | `priority_level` is a collapsed mirror, not canonical multi-tag portfolio ownership | Refresh from canonical registry with `company_id`, owner/tags and snapshot time |
| Observed Koronet SELL / online share / movement | `metrics_v2_sell.json` (`SALES_SV`) | company name, normalized exact join | YTD through Jul 30; prior-YTD / prior month provided | 336/426 current (435 source companies) | Medium-high for explicitly labelled Koronet SELL; windows within this file are comparable | Not total company revenue. **Do not mix** current month, YTD, H1 or buyer cutoffs in one calculation | Produce company-id keyed daily snapshot: total/online/offline GMV, current/prior aligned windows, exclusions and `as_of` |
| Buyers, active buyers, AOV, new/churned | `metrics_v2_buyers_full.json` (`SALE_DETAILS` + `COMPANIES` + `CUSTOMERS`) | company name, normalized exact join; buyer populations overlap | YTD Jan 1–Jul 31; L30D at Aug 1; churn is Q2 buyer absent in July | 287/426 current (342 source companies) | Medium. Useful as separate evidence panel | Buyer cutoff is one day later than SELL; online/offline buyers cannot be summed; “churned” is an early partial-Q3 proxy; AOV is daily customer-channel total, not necessarily order AOV | One company-id keyed buyer spine with one common ending date, `customer_id`, channel eligibility/access and an explicit overlap policy |
| Fees / actual take rate | `metrics_v2_fees_full.json` | `company_id` in source; dashboard currently name joins in places | billed Jan–Jun + expected Jul 1–30 | 203/426 current; 217 companies have YTD fees in file | High for observed organic fees when labelled billed/expected | July is expected, not billed; Axerrio excluded; fee window differs from full YTD SELL cutoff by one day; zero is not automatically $0 take rate | Daily fee fact keyed by company-id + channel; retain `fee_status` (billed/expected), transaction window and exclusions |
| Company GMV / Est. Sell | `est_gmv.json`, SFDC/ORA/Christine legacy inputs; external research receipts | company id | many records lack as-of/method inputs | 69/426 in precard spine | Estimate only: source, method, confidence, evidence and freshness must appear | **357 accounts have no estimate.** Some “SFDC” values have not passed external validation (Price is explicit); current UI can show `undefined` when estimator fields are absent | Persist research evidence in Supabase by `company_id`: claim/evidence key/source URL/date/method/range/confidence/reviewer. Publish estimate only after model record is complete |
| Est. Buy / total purchases | `est_gmv.json` legacy; account buy/sell ratio and 54% fallback documented | company id | method/as-of incomplete | 69/426 values, quality unknown | A model only, never observed purchasing | The repo conflicts: older scope says account ratio then 54% fallback; current Accounts contract says generic 54% fallback is **invalid**. Therefore it must not drive penetration/opportunity until input + method are stored | Define one estimator: `estimated_sell × account-specific purchase:sell ratio`; use 54% only as explicit low-confidence fallback with cohort/rationale or leave blank. Store inputs/version/confidence |
| SFDC sales / product opportunities | `sfdc.json` | account name; 35 rows | snapshot; no extracted-at field | 35 rows only | Discovery context, not account opportunity truth | No lifecycle/universe contract; cannot claim “SFDC opp count/value” for every account | Materialize Account + Opportunity ids, stage, type, amount, close date, owner, `is_open`, extraction timestamp; join through account-id crosswalk |
| GA4 sessions / session CVR | `ga4_eshop.json`, live GA4 automated Sheet is available | hostname (not company) | local extract Jul 11–31, 21 days, 29 hostnames | 0 company rows until mapping (426 JOIN_REQUIRED) | Data exists; account attribution does not | Shared app/eShop hostnames cannot become account CVR. Do not say GA4 is missing; do not substitute login CVR | Maintain `hostname → company_id` mapping or event/account-key attribution, then materialize daily sessions/transactions/revenue/CVR with attribution state |
| Login CVR upper bound | `eshop_cvr_by_company.json` (`USER_STATS`) | company id | YTD Jan 1–Jul 31; >=10 logins | 285/426 in coverage (320 source companies) | Medium; label **login CVR upper bound**, not GA4/session CVR | 13–31% overstatement vs distinct orders documented; must never merge with GA4 CVR | Keep separate conversion fact with method, cutoff, denominator and `min_logins` |
| Trend comparisons | `metrics_v2_sell.json`, `metrics_v2_fees_full.json` | company/name or company id | metric-specific | varies | Valid only inside a metric’s declared parallel windows | Current UI mixes YTD GMV (Jul 30), buyer YTD (Jul 31), H1 channels and possibly current-month figures. These must be separate labelled modules, never one rate/conclusion | Adopt `metric_window_id` and enforce a comparison gate: no calculation across nonmatching windows |

## External-estimate finding

External research is a real workflow, not a missing-data excuse. The Price receipt shows the right behavior: public evidence confirms type, offer and possible locations, but not a second sizing signal; therefore it **does not replace** the $1.8M SFDC estimate. It needs active-location / facility / headcount / route / import evidence plus an independent sizing method.

The useful stored unit is:

`company_id + research_run_id + claim_key + source_url + observed_text + evidence_date + method + estimate_range + confidence + validation_state`

That lets an estimate be updated without losing why it existed. It must be written to Supabase when available; JSON staging is acceptable only as a queue with exactly the same keys.

## Outputs that are misleading today and must not ship

1. `undefined (undefined)` for Est. Sell or Est. Buy: a renderer error, never a data state. Show `Not estimated — research queued` plus the required next evidence instead.
2. Company GMV, observed Koronet SELL and penetration shown as if the first validates the other. They answer different questions and must have source/method/confidence.
3. Generic 54% total-purchases estimate presented as observed or high-confidence. The source-of-truth documents conflict; resolve the model before it powers an opportunity.
4. Offline SELL × a fee rate presented as actual fees or as a verified opportunity. It is a labelled scenario only after channel eligibility and fee-rate basis are explicit.
5. Buyer counts added across online/offline, or used to calculate a conversion rate against SELL GMV. Their populations overlap and have a different cutoff.
6. H1 `sell_by_channel` / format / repeat metrics rendered beside YTD SELL as a unified same-window statement.
7. GA4’s shared hostname data rendered as company session CVR, or login CVR relabelled as GA4 CVR.
8. The 35-row SFDC snapshot rendered as an exhaustive account-opportunity inventory.

## MVP-safe Portfolio/Potential/SELL spine

For each row, retain these discrete labelled states rather than deriving a fake complete score:

- **Observed SELL:** YTD total / online / offline / online share + comparable source-provided movement.
- **Estimate:** Est. company GMV and Est. Buy only when `method + inputs + confidence + freshness` exist; otherwise `research queued`.
- **Capture:** Koronet SELL ÷ Est. Sell only when same entity boundary and estimator state allow it.
- **Monetization:** observed organic fees + fee status; scenario is separately labelled and user-selected.
- **SELL evidence:** buyers, activity, AOV, retention and concentration each retain own window; GA4 and login conversion remain distinct.

## Wednesday priority order

1. **Company identity spine:** canonical `company_id`, entity boundary (company vs location vs parent), normalized aliases, SFDC account id and owner/tags.
2. **Daily observed facts:** SELL, BUY, fees and buyer fact tables keyed by company id and one declared `as_of` / window id.
3. **Estimator registry:** research evidence and model output with confidence, range, method version and review status. Do the external research queue for all target accounts; do not silently substitute 54%.
4. **SFDC opportunity fact:** full open-opportunity inventory with Account ID and lifecycle definition.
5. **Conversion attribution:** GA4 hostname map/event key followed by company daily sessions; retain login CVR separately.
6. **Release gate:** reject `undefined`, unnamed periods, implicit name joins, cross-window rates and unlabelled scenarios.

## Bottom line

The Accounts MVP can truthfully rank and investigate the 336 accounts with observed SELL now. It cannot truthfully claim complete company scale, total purchases, per-account GA4 conversion, exhaustive SFDC opportunity coverage or a unified buyer-to-GMV funnel yet. Those are concrete materialization jobs—not reasons to blank the structure or invent a conclusion.
