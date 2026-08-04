# Phase 3 SELL implementation receipt

Status: **implemented locally for review; not published, staged, committed, or
connected to Supabase.** Scope was limited to the expanded account SELL card
and two SELL-only GA4 attribution helpers in `index.html`.

## What changed

| Feedback / requirement | Source used | Implementation / non-use | Test / remaining gap |
|---|---|---|---|
| Preserve the dense expandable SELL format | Existing card format; `TX_DASHBOARD_SCOPE_V2.md` Card 5 | Replaced only the rendered SELL region with six compact evidence sections; did not turn it into a prose/status card. Legacy renderer remains non-executing reference during review. | Browser fixtures render the card for all required accounts. |
| Primary observed SELL, not a proxy | `metrics_v2_sell.json`, as-of Jul 30 | First block is observed online/offline/total SELL and delta for selected V2 period. | Name-normalized identity remains the current join; canonical company-keyed sell spine is still required. |
| No mixed-window buyer/GMV claim | `metrics_v2_buyers_full.json` (Jan 1–Jul 31) vs SELL V2 (through Jul 30) | Customer table remains visible but explicitly separate evidence; Decision Gate says activation/traffic conclusion is blocked. | Build `sell_account_spine` to make a common-period conclusion possible. |
| Buyers, L30D, AOV, new/churned | `metrics_v2_buyers_full.json` | All shown in a labelled YTD/L30D table; overlap note prevents `online + offline = total` inference. | Buyer extract must be recomputed to selected SELL window for primary comparison. |
| Price: no online-customers/zero-online-GMV contradiction | Price acceptance feedback; V2 SELL + buyers-full | Price visibly shows observed V2 online GMV separately from the 6 YTD online customers and blocks their conversion inference. | The historic source conflict is not erased; only a versioned spine can reconcile it. |
| LIST before traffic | `TX_DASHBOARD_REVIEW_DECISIONS.md` R6; SELL contract | Explicit LIST state/link plus hard traffic block in every account. | Current LIST is proxy/partial, so no account receives a traffic recommendation. |
| Offline buyers first, but only if eligible | SELL contract; feedback Aug 1 | Cohort/unit-economics section says `NEEDS EVIDENCE` and names customer-to-eShop eligibility/access + channel history. | No eligible cohort source exists yet. |
| Unit economics | Scope SELL + Price feedback | Generic “10% more buyers” calculation and fallback fee rate no longer render. | Enable only with eligible cohort, same-period GMV/AOV and verified rate/channel. |
| GA4 is available, not “missing” | Live GA4 Sheet capability from `PHASE3_FEEDBACK_COVERAGE_AUDIT.md`; local `ga4_eshop.json` | Dedicated mapped domains display sessions, transactions, revenue, CVR and local-extract window. Other accounts state GA4 live/shared-host attribution model status, not “GA4 absent.” | Pull the live Sheet into daily dashboard refresh; shared-host account attribution key/rule still required. |
| GA4 session CVR distinct from login CVR | `ga4_eshop.json`; `eshop_cvr_by_company.json` | Separate blocks and labels. Login metric is explicitly an upper bound and never called session CVR. | More dedicated hostname mappings can be added after evidence review. |
| No fuzzy/shared-host GA4 attribution in SELL | Confirmed hostname map: Kennicott, Bill Doran, Pikes Peak, Allure | SELL-only `GA4_ACCOUNT_HOSTNAMES` resolver uses exact `company_id`→hostname. Shared hosts are visible as an attribution state. | Portfolio’s pre-existing fuzzy helper was intentionally out of scope. |
| Best-in-class comparator | `benchmarks.json` | Online SELL share comparator is shown with source-context disclaimer; dedicated-GA4 comparator appears when applicable; repeat comparator appears when available. | Resolve local/comparable comparator precedence and non-degenerate percentiles in benchmark pipeline. |
| Repeat, concentration, retention | `repeat_rate.json`, `buyer_concentration_v2.json` | Top-5 (online only, Jan–Jul 30) and H1 repeat render as supplemental, period-labelled evidence. | No valid selected-period retention trend yet; card requests it rather than calculating L30D/total retention. |
| Channel, format, hardgoods visible | `sell_by_channel.json`, `loop2_sell_format_time_v2.json`, `product_division_gmv.json` | All appear in supplemental evidence with H1/forward-order limitations. | Channel has SFDC-account scope; product divisions need aligned period before primary decision use. |
| All-account rule, not a Price exception | SELL contract; coverage audit | Same card logic renders for every account, including missing-record sections. | Canonical identity/reconciled spine remains universal work. |

## Exact code ranges

Line ranges refer to the local post-change `docs/transactions/index.html`:

- `findGA4ForCompany` and shared-host evidence helpers: **lines 951–968**.
- SELL renderer Phase 3 section: **lines 2913–3015**.
- Legacy SELL renderer wrapped as non-executing reference: **lines 3017–3237**.

## Fixture browser test

Used JSDOM against the local dashboard server after data load. Assertions for
each fixture: SELL section exists; renders `Customer evidence`, `GA4 session
CVR`, `Login CVR upper bound`, `Traffic recommendation: BLOCKED`, and
`Offline cohort & unit economics`; does **not** render the legacy generic
`For every 10% more online buyers` scenario.

| Fixture | Result |
|---|---|
| Price's (`816515`) | PASS — V2 $3K online SELL and six YTD online customers are separately labelled; traffic/unit economics blocked. |
| Kennicott (`44150`) | PASS — exact `shop.kennicott.com` dedicated-domain mapping; no child entity mapping in SELL helper. |
| Arizona Family (`193828`) | PASS — observed zero online SELL and buyer evidence remain visible; non-traffic gate. |
| Maple Grove (`641341`) | PASS — partial evidence renders without fabricated cohort/scenario. |
| Zeidler (`743648`) | PASS — partial evidence renders without fabricated cohort/scenario. |
| H&T (`498865`) | PASS — partial evidence renders without fabricated cohort/scenario. |

Browser result: **0 JSDOM errors; 6/6 assertions passed.**

### Zeidler correction / explicit assertion

Zeidler is not a missing-buyer-data case. The active card displays the
available `metrics_v2_buyers_full.json` evidence: **162 total, 34 online, 162
offline; L30D 28 online / 77 offline; AOV $314.66 / $417.27**. It remains a
separate Jul-31 customer-evidence block and is not combined into a conversion
claim with Jul-30 SELL GMV.

Its config snapshot (`bunches=false`, `max_age=0`) is source-labelled
`scorecard (NOT VERIFIED)` and conflicts with observed online bunch behavior.
The SELL checkout/config line now renders **“Config snapshot NOT VERIFIED — do
not infer checkout, bunches, or shop readiness from it”**. No configuration
conclusion is made from this snapshot.

## Not done by this change

- No publish, git stage/commit, Supabase write, research mutation, lead
  promotion, traffic recommendation, or customer task.
- No SELL data-spine build or data refresh. The displayed source files remain
  snapshots with their stated windows.
- No BUY/LIST/Potential/Opportunities/Portfolio rendering changes.
