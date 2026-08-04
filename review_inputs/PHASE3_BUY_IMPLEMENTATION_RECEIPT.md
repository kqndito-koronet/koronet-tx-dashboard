# Phase 3 BUY implementation receipt

**Scope:** `docs/transactions/index.html`, Card 3 BUY render region only.
**Not done:** no publish, stage/commit, Supabase write, loader rewrite, or edit
to POTENTIAL / OPPORTUNITIES / LIST / SELL.

## Requirement → source → implementation / non-use → test → remaining gap

| Requirement | Source used | Implementation / deliberate non-use | Test | Remaining gap |
|---|---|---|---|---|
| One-line conclusion and progressive handoff | selected V2 BUY observed base | Conclusion names `needs evidence` vs no observed offline BUY and hands off to supply/lifecycle then LIST; never creates a task. | All 4 fixtures render `Conclusion:`. | No canonical cross-card prerequisite object yet. |
| Compact, period-matched observed BUY semantic label | `metrics_v2_buy.json`, `PRODUCTION.ANALYTICS.PROCUREMENTS_SV`, selected timeframe | Shows source, selected window, as-of; online/offline GMV only from that base. | Price renders 60.5% YTD / 22.1% current month / 97.5% prior quarter separately. | The selected UI timeframe still needs a global metadata component later. |
| +10pp unit economics only with valid total-purchases base | `research_display_decisions.json`; approved Arizona BUY range | Renders scenario only for approved candidate range or non-Core explicit estimate. Core-observed values are rejected as denominator. 1.5% is visible as assumed. | Arizona renders approved USD candidate range; Price shows blocked scenario and no `$84K`. | Most accounts need external/ERP total-purchases method. |
| Coverage matrix retains every desired row | `metrics_v2_vendors_full.json`, `vendor_detail.json`, `supply_matrix_full.json` | Renders total/active vendors, categories, varieties, SKUs, and short/medium/long even unavailable. Category source has total + online only: total is retained as separate evidence; offline is `not available`, never `total − online`. Does not use LIST sales proxies for BUY varieties/SKUs. | Four fixtures render `COVERAGE & GAP`; static check confirms all rows and category non-comparability. | Procurement event / availability spine for vendor, product and horizon. |
| Classify gaps honestly; leakage needs proof | same coverage sources | Offline BUY/counted coverage = `needs availability join`; `leakage` is explicitly withheld. No false zero means no gap. | Four fixtures include coverage section; no runtime errors. | Same-need online availability at purchase time and first/last events. |
| Compact connection/lifecycle exceptions | V2 vendor active/dormant/churned; existing K2K counts when present | Shows connected → activated → active L30D plus connected-not-activated/dormant/churned; labels churned→offline `needs evidence`. | Four fixtures render lifecycle section. | K2K connection events and transition-to-offline proof. |
| Horizon/fulfillment not mistaken for logistics proxy | no valid common procurement source | Preserves all horizon rows as unavailable with exact `ORDER/CREATED_DATE + availability` request. Deliberately does **not** render `buy_detail.json` arrival→shipping proxy or SALES Open Market/Prebook as BUY anticipation. | Static source check confirms horizon rows. | Procurement order-created/required/ship dates plus availability. |
| Best-in-class visible, never masquerades as account offline | `benchmarks.json`, key `3_online_buy_pct_ytd` | Same-segment online BUY median/p75/best with source/population; labels it reference, not coverage/causal proof. | Four fixtures render comparator section. | Local comparator resolver and procurement coverage/horizon benchmark. |
| Price reimplementation/prior vendor ecommerce context | Price acceptance feedback, not a data feed | Source-controlled open comparison request; explicitly prohibits Procurement-acceleration attribution. | Price fixture text assertion. | Dated pre/post evidence/source record. |
| DRAFT lead + enablement only | existing `#buyLeadList` surface | Contextual link says DRAFT/no task or outreach; does not manufacture a lead or priority. | Four fixture DOM renders link/no task wording. | Canonical account-specific lead + enablement record. |
| No bunches in BUY | Facu BUY feedback and scope | No bunches/TAM/config content in active BUY renderer. | Four fixture DOM assertions: no `bunch` text. | None; regression guard should be retained. |

## Source-level fixture run

The dashboard was rendered from `index.html` in JSDOM with the local `data/`
fetches mapped to their JSON files. Assertions required the active BUY card to
contain conclusion, coverage, lifecycle and comparator sections, contain no
`bunch` text, and meet the fixture-specific conditions below.

| Fixture | Result |
|---|---|
| Price's Floral Wholesale, LLC (`816515`) | PASS — YTD/current/prior windows separated; no `$84K` potential/scenario display. |
| Kennicott Brothers Company (`44150`) | PASS — active BUY structure renders under company scope. |
| Arizona Family Florist (`193828`) | PASS — approved candidate range + USD renders as scenario, not observed BUY. |
| Maple Grove Floral (`641341`) | PASS — contrasting P1 Core fixture renders. |

Result: **NO_RUNTIME_ERRORS** in the fixture run. This is source-level QA only;
it is not a public-release approval.
