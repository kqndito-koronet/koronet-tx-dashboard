# TX Dashboard Tab 1 -- P0.0 No-Loss Field Inventory

**Status:** CODEX VERIFIED (2026-07-31, Run 29) — PARTIALLY_CERTIFIED, ready for P0.1 with corrections applied below
**Codex confidence:** 85% — field tracing, formulas, gaps all verified. 3 corrections applied post-verification.
**Date:** 2026-07-31
**Source:** 7 microagent traces covering Portfolio columns + 5 cards (POTENTIAL, CONFIG/OPPORTUNITIES, BUY, LIST, SELL) + JSON data coverage audit
**Artifact traced:** `/Users/facu/Koronet_OS/docs/transactions/index.html`
**Scope v2 spec:** `/Users/facu/Koronet_OS/docs/transactions/TX_DASHBOARD_SCOPE_V2.md`

---

## SECTION 1: SUMMARY

### Total fields traced

| Section | Fields | Source JSONs |
|---|---|---|
| A. Portfolio columns | 16 | accounts, metrics, sfdc, christine, config, supply_matrix_full, buyers, repeat_rate |
| B. Card POTENTIAL | 35 | accounts, metrics, est_gmv, christine, sell_by_channel |
| C. Card CONFIG/OPPORTUNITIES | 31 | accounts, config, bunches, christine, sfdc, metrics, buyers |
| D. Card BUY | 50 | est_gmv, metrics, vendor_detail, supply_matrix_full, loop2_list_usage_v2, bunches, config, buy_detail |
| E. Card LIST | 19 | accounts, supply_matrix_full, loop2_list_usage_v2, config, bunches, loop2_list_time_depth_v2, listing_detail, buy_detail |
| F. Card SELL | 30 | metrics, buyers, repeat_rate, buyer_concentration, loop2_phase1v2_buyers, sell_detail, sell_by_channel, loop2_sell_format_time_v2, ga4_eshop |
| **TOTAL** | **181** | **22 unique JSON files** |

### Total gaps identified

| Section | Gaps |
|---|---|
| Portfolio | 9 |
| POTENTIAL | 5 actionable (+ 1 risk) |
| CONFIG/OPPORTUNITIES | 6 |
| BUY | 14 |
| LIST | 11 |
| SELL | 10 |
| **TOTAL** | **~55 gaps** |

### Data coverage summary (from JSON audit)

- **Base:** 399 accounts in accounts.json (398 with valid company_id, 1 null: "Heirloom Home Decor")
- **metrics.json:** 112/399 = **28.1%** (CRITICAL -- most important file, covers barely a quarter of accounts)
- **config.json:** 397/399 = 99.5% (near-complete)
- **buyers.json:** 274/399 = 68.7%
- **sell_by_channel.json:** 271/399 = 67.9% (matched by company_name, NOT company_id)
- **est_gmv.json:** 112/399 = 28.1%
- **ga4_eshop.json:** 29 records, keyed by hostname, no company_id join path

### Critical findings / bugs / risks

1. **BUG: findListingCats reads `LISTING_DETAIL.categories` but JSON key is `varieties`** -- always returns null, lines 1498-1501 are dead code
2. **BUG: GA4 hostname matching is fragile** -- substring fuzzy match can produce false positives, no warning shown to user
3. **RISK: sell_by_channel matches by `company_name` not `company_id`** -- silent failure if names differ, can cause false NOT MONETIZED flags
4. **MISLABELED: Open Market in LIST card** -- shows sales from SALES_SV ORDER_TYPE, not manual uploads as scope v2 defines
5. **HARDCODED: Indirect fee rate at 1.5%** -- no config source, must be manually updated if rate changes
6. **MISSING: No LIST opportunities generated** despite HTML renderer existing (lines 1244-1245)
7. **STUBBED: Anticipation data exists in buy_detail.json** but card shows "needs data" hardcoded placeholder
8. **BROKEN: Heirloom Home Decor has null company_id** -- creates "None" key in est_gmv.json, missing from config.json

---

## SECTION 2: TABLE 1 -- FIELD INVENTORY

### A. Portfolio Columns (16 fields)

| # | Campo | Section | Source JSON | Source field | Match key | Computed? | Formula (if yes) | Product types | Coverage (N/399) | Scope v2 match | Disposition |
|---|---|---|---|---|---|---|---|---|---|---|---|
| A1 | (expand arrow) | Portfolio | -- | -- | -- | No | -- | All | 399 | -- | preserved (UI chrome) |
| A2 | Account | Portfolio | `accounts.json` | `name` | direct | No | -- | All | 399 | Account | preserved |
| A3 | Owner | Portfolio | `sfdc.json` + `christine.json` | `sfdc[].owner`, fallback chain | `getSFDC()` fuzzy name (first 15 chars) + `CHRISTINE[company_id]` | Yes | Priority: SFDC opp owner > Christine "Implementations" > "Unassigned" | All | 31 (sfdc) + 15 (christine) | Owner | preserved |
| A4 | Priority | Portfolio | `accounts.json` | `priority_level` | direct | No | Badge rendering only | All | 399 | Priority | preserved |
| A5 | Products | Portfolio | `accounts.json` | `ct_id`, `system_type`, `is_esuite`, `has_procurement`, `has_eshops` | direct | Yes | `ctBadge()`: ct_id short form + module tags | All | 399 | -- (not in scope v2) | candidate for removal |
| A6 | Est. Sell GMV | Portfolio | `accounts.json` | `annual_sales_est` | direct | No | `fmt$()` only | All | 158 (39.6%) | Est Sell GMV | preserved |
| A7 | Koronet Sell | Portfolio | `metrics.json` | `sell_total` | `METRICS[company_id]` | No | `fmt$()` only | All | 84 (of 112) | Koronet Sell | preserved |
| A8 | Penetr. % | Portfolio | `accounts.json` + `metrics.json` | `annual_sales_est`, `sell_total` | computed | Yes | `sell_total / annual_sales_est * 100` | All | ~84 (both required) | Sell Penetration % | preserved |
| A9 | Online % | Portfolio | `metrics.json` | `online_pct` | `METRICS[company_id]` | No | Color: >=20% green, >0% amber, 0% red | All | 77 (of 112) | Online Sell % | preserved |
| A10 | CONFIG | Portfolio | `config.json` | `max_age`, `bunches_flag`, `future` | `CONFIG[company_id]` | Yes | Mini-dashboard: "M:{max_age}d B:{ON/OFF} F:{ON/OFF}" | All | 397 | -- (not in scope v2) | removed (scope v2 decision) |
| A11 | BUY | Portfolio | `metrics.json` | `proc_online`, `proc_total`, `k2k_active`, `k2k_total` | `METRICS[company_id]` | Yes | Proc online %, K2K ratio | All | 112 | -- (not in scope v2 as mini-score) | removed (scope v2 decision) |
| A12 | LIST | Portfolio | `supply_matrix_full.json` + `config.json` | `online_categories_sold`, `total_categories_sold`, `max_age` | `findSupplySell()` by name + `CONFIG[id]` | Yes | Coverage % + MaxAge | All | 270 (supply) + 397 (config) | -- (not in scope v2) | removed (scope v2 decision) |
| A13 | SELL | Portfolio | `buyers.json` + `repeat_rate.json` | `total_buyers`, `online_buyers`, `repeat_rate_pct` | `findBuyerData(company_id)` + `findRepeatRate(name)` | Yes | Total/online buyers + repeat rate % | All | 274 (buyers) + 231 (repeat) | -- (not in scope v2) | removed (scope v2 decision) |
| A14 | SFDC Opp | Portfolio | `sfdc.json` | `stage`, `opp_name` | `getSFDC()` fuzzy name match (first 15 chars) | No | Stage + "+N" if multiple | All | 31 | SFDC Opps | preserved |
| A15 | Opps | Portfolio | computed (internal) | `getOpportunities()` output length | `getOpportunities(acct)` L685 | Yes | Count of rule-engine-generated opportunities; tooltip lists all | All | 399 (computed for all) | # Opps (APPROVED only) | reorganized -- exists but current = all computed opps; scope v2 = APPROVED only (needs filter or new source) |
| A16 | Fees | Portfolio | `metrics.json` | `fees_total` | `METRICS[company_id]` | No | `fmt$()` only | All | 37 (of 112, 9.3% of 399) | Fees (Total = direct+indirect summed) | preserved -- but scope v2 wants verification that `fees_total` includes both direct+indirect |

### B. Card POTENTIAL (35 fields)

| # | Campo | Section | Source JSON | Source field | Match key | Computed? | Formula (if yes) | Product types | Coverage (N/399) | Scope v2 match | Disposition |
|---|---|---|---|---|---|---|---|---|---|---|---|
| B1 | sellPenPct (Sell Penetration %) | POTENTIAL | `metrics.json` + `accounts.json` | `met.sell_total` / `a.annual_sales_est` | `company_id` | Yes | `met.sell_total / a.annual_sales_est * 100` | All | ~84 | Penetration % | preserved |
| B2 | sellPenColor | POTENTIAL | computed | -- | -- | Yes | >50% green, >20% amber, else red | All | ~84 | (visual) | preserved |
| B3 | buyPenPct (Buy Penetration %) | POTENTIAL | `est_gmv.json` + `metrics.json` | `met.proc_total` / `estBuyVal` | `company_id` | Yes | `met.proc_total / estBuyVal * 100` | All | ~96 (of 112) | Penetration % | preserved |
| B4 | buyPenColor | POTENTIAL | computed | -- | -- | Yes | same thresholds as sell | All | ~96 | (visual) | preserved |
| B5 | sellGap (sell gap $) | POTENTIAL | `accounts.json` + `metrics.json` | `a.annual_sales_est - met.sell_total` | `company_id` | Yes | `annual_sales_est - sell_total` | All | ~84 | Pre go-live SO WHAT | preserved |
| B6 | oraCalcDiverge (ORA vs calc %) | POTENTIAL | `accounts.json` + `est_gmv.json` | `annual_sales_est` vs `est_sell_gmv` | `company_id` | Yes | `abs(ORA - calc) / max(ORA, calc) * 100` | All | ~112 | "If ORA and calc same source -> show once" | preserved |
| B7 | Card header | POTENTIAL | literal | -- | -- | No | -- | All | 399 | -- | preserved |
| B8 | Pre go-live IMPL banner (blue) | POTENTIAL | `christine.json` | `impl.stage`, `impl.type` | `company_id` | No | -- | All | 15 | "Pre go-live -- $XM potential in blue" | preserved |
| B9 | Impl stage detail | POTENTIAL | `christine.json` | `impl.stage`, `impl.type` | `company_id` | No | -- | All | 15 | "IMPL accounts: show implementation stage" | preserved |
| B10 | Sell gap amber banner (non-impl) | POTENTIAL | computed | `sellGap` | -- | Yes | same sellGap | All | ~84 | -- | preserved |
| B11 | Standalone impl line (no sell gap) | POTENTIAL | `christine.json` | `impl.stage`, `impl.type` | `company_id` | No | -- | All | 15 | "IMPL accounts: show implementation stage" | preserved |
| B12 | ORA/calc divergence warning (red) | POTENTIAL | computed | `oraCalcDiverge` | -- | Yes | shows rounded % when > 30% | All | ~112 | "If ORA and calc same source -> show once" | preserved |
| B13 | SELL SIDE header | POTENTIAL | literal | -- | -- | No | -- | All | 399 | -- | preserved |
| B14 | Est. Sell (ORA) | POTENTIAL | `accounts.json` | `a.annual_sales_est` | `company_id` | No | -- | All | 158 | "Est Sell GMV (ORA + calc) with source label" | preserved -- **GAP: no source label on ORA row** |
| B15 | Est. Sell (calc) | POTENTIAL | `est_gmv.json` | `est_sell_gmv`, `est_sell_source`, `est_sell_confidence` | `company_id` | Yes (conditional) | Collapses to "same as ORA" when source is Christine ORA or values match | All | 69 (non-zero) | "If ORA and calc same source -> show once with label" | preserved |
| B16 | Koronet Sell | POTENTIAL | `metrics.json` | `met.sell_total` | `company_id` | No | -- | All | 84 | "Koronet Sell/Buy actual" | preserved |
| B17 | Sell Penetration % (inline) | POTENTIAL | computed | `sellPenPct` | -- | Yes | `met.sell_total / a.annual_sales_est * 100` | All | ~84 | "Penetration %" | preserved |
| B18 | Online Sell ($) | POTENTIAL | `metrics.json` | `met.sell_online` | `company_id` | No | -- | All | 79 | "Online %" | preserved |
| B19 | Online Sell % | POTENTIAL | `metrics.json` | `met.online_pct` | `company_id` | No (pre-computed) | -- | All | 77 | "Online %" | preserved |
| B20 | BUY SIDE header | POTENTIAL | literal | -- | -- | No | -- | All | 399 | -- | preserved |
| B21 | Est. Buy (value) | POTENTIAL | `est_gmv.json` + `metrics.json` + `accounts.json` | 3-tier cascade: est_buy_gmv -> buy/sell ratio -> 54% default | `company_id` | Yes (when fallback) | Tier 1: est_buy_gmv; Tier 2: (proc_total/sell_total)*estSellRef; Tier 3: 0.54*estSellRef | All | ~112 | "Est Buy GMV (actual buy/sell ratio, 54% fallback)" | preserved |
| B22 | Est. Buy source note | POTENTIAL | computed | -- | -- | Yes | Tier 1: source+confidence; Tier 2: "actual ratio X%"; Tier 3: "avg ratio 54%" | All | ~112 | source label | preserved |
| B23 | Koronet Buy | POTENTIAL | `metrics.json` | `met.proc_total` | `company_id` | No | -- | All | 96 | "Koronet Sell/Buy actual" | preserved |
| B24 | Buy Penetration % (inline) | POTENTIAL | computed | `buyPenPct` | -- | Yes | `met.proc_total / estBuyVal * 100` | All | ~96 | "Penetration %" | preserved |
| B25 | Online Buy ($) | POTENTIAL | `metrics.json` | `met.proc_online` | `company_id` | No | -- | All | 75 | "Online %" | preserved |
| B26 | Online Buy % | POTENTIAL | `metrics.json` | `met.proc_online / met.proc_total` | `company_id` | Yes | `proc_online / proc_total * 100` | All | 75 | "Online %" | preserved |
| B27 | Fee channel table header | POTENTIAL | literal | -- | -- | No | -- | All | 399 | "Fee breakdown as table" | preserved |
| B28 | Channel rows: eCommerce | POTENTIAL | `metrics.json` + `sell_by_channel.json` | `fees_by_type['eCommerce']` + `channelGmvMap['eCommerce']` | fees: `company_id`; GMV: `company_name` (normName) | Yes (take rate) | `chFee / chGmv * 100` | All | 42 (fees) / 271 (GMV) | "Fee breakdown by channel with take rate" | preserved |
| B29 | Channel rows: K2K | POTENTIAL | same as B28 | `fees_by_type['K2K']` + `channelGmvMap['K2K']` | same | Yes | same | All | same | same | preserved |
| B30 | Channel rows: API | POTENTIAL | same as B28 | `fees_by_type['API']` + `channelGmvMap['API']` | same | Yes | same + warning if rate > 0 and < 0.5% | All | same | same | preserved |
| B31 | NOT MONETIZED flag | POTENTIAL | computed | -- | -- | Yes | `chGmv > 10000 && chFee === 0` | All | conditional | "Flag channels with GMV > $10K but $0 fees" | preserved |
| B32 | API <0.5% warning | POTENTIAL | computed | -- | -- | Yes | `chTakeRate > 0 && chTakeRate < 0.5` | All | conditional | "detect unmonetized" | preserved |
| B33 | Offline row | POTENTIAL | `sell_by_channel.json` + `metrics.json` | `channelGmvMap['Offline']` or `met.sell_offline` | `company_name` / `company_id` | No | always shows $0 fees, $0 take | All | 271 | "detect unmonetized" | preserved |
| B34 | Indirect fees row | POTENTIAL | `metrics.json` | `met.proc_online` | `company_id` | Yes | `Math.round(met.proc_online * 0.015)` -- **hardcoded 1.5%** | All | 75 | "Fees breakdown by channel" | preserved -- **RISK: hardcoded rate** |
| B35 | TOTAL fees row | POTENTIAL | computed | `directPotential + indirectPotential` | -- | Yes | `met.fees_total + Math.round(met.proc_online * 0.015)` | All | conditional | "Fees breakdown by channel" | preserved |

### C. Card CONFIG -> OPPORTUNITIES (31 fields)

| # | Campo | Section | Source JSON | Source field | Match key | Computed? | Formula (if yes) | Product types | Coverage (N/399) | Scope v2 match | Disposition |
|---|---|---|---|---|---|---|---|---|---|---|---|
| C1 | Profile description (ct_id -> desc) | CONFIG | `accounts.json` | `ct_id` | `a.ct_id` | Yes (lookup) | `_cfgProfiles[ct_id].desc` | All | 399 | "for each product type: explain potential, best in class, limitations" | preserved -- but ONLY explains capabilities, not potential/best-in-class/limitations |
| C2 | Config Issues list (severity) | CONFIG | `config.json` + `accounts.json` + `bunches.json` | `max_age`, `bunches_flag`, `on_hand`, `future`, `hide_checkout`, `vendor_avail`, `ct_id`, `sells_bunches` | `CONFIG[company_id]` | Yes | Multi-conditional issue detection per product type | WH_PROC: info only; WH_K2K: vendor_avail only; WH_CORE/ESUITE/IMP_CORE: full checks | 397 | "config opportunities: blockers, missing settings, inactive features" | preserved |
| C3 | Header color (red/amber/green) | CONFIG | derived from configIssues[] | `.severity` | -- | Yes | blocking->red; limiting->amber; else->green | All | 397 | (visual) | preserved |
| C4 | Header text | CONFIG | derived from configIssues[] | `.length` | -- | Yes | "CONFIG OK" or "CONFIG Issues (N)" | All | 397 | -- | preserved |
| C5 | MaxAge | CONFIG | `config.json` | `max_age` | `CONFIG[company_id]` | No | Display as `{value}d`; red+bold if <=10 | WH_CORE, WH_ESUITE, IMP_CORE | 379 | "missing settings" | preserved |
| C6 | Future | CONFIG | `config.json` | `future` | `CONFIG[company_id]` | No | ON/OFF + "(N/A)" if not WH_CORE | WH_CORE meaningful; shown for all | 397 | "inactive features" | preserved |
| C7 | Bunches | CONFIG | `config.json` | `bunches_flag` | `CONFIG[company_id]` | No | ON/OFF + green/red | WH_CORE, WH_ESUITE | 397 | "inactive features" | preserved |
| C8 | On-Hand | CONFIG | `config.json` | `on_hand` | `CONFIG[company_id]` | No | ON/OFF | WH_CORE, WH_ESUITE | 397 | "missing settings" | preserved |
| C9 | Audited | CONFIG | `config.json` | `audited` | `CONFIG[company_id]` | No | Checkmark or warning | All | 397 | -- (not in scope v2) | preserved (data quality signal) |
| C10 | Source | CONFIG | `config.json` | `source` | `CONFIG[company_id]` | No | Raw string | All | 397 | -- (not in scope v2) | preserved (provenance) |
| C11 | Bunch discrepancy callout | CONFIG | `bunches.json` | `discrepancy`, `total_bunch_gmv` | `BUNCHES[company_id]` | Yes | If discrepancy=true: "Sells $X bunches but flag OFF" | WH_CORE, WH_ESUITE | 43 | "blockers" | preserved |
| C12 | Bunch GMV (no discrepancy) | CONFIG | `bunches.json` | `total_bunch_gmv`, `bunches_flag` | `BUNCHES[company_id]` | No | "Bunch GMV: $value -- flag ON/OFF" | WH_CORE, WH_ESUITE | 43 | revenue context | preserved |
| C13 | Modules (Core, eSuite, K2K, Proc, eShop) | CONFIG | `accounts.json` | `system_type`, `is_esuite`, `has_procurement`, `has_eshops` | direct | Yes | String matching + booleans -> joined with " + " | All | 399 | product setup context | preserved |
| C14 | Implementation: stage | CONFIG | `christine.json` | `stage` | `CHRISTINE[company_id]` | No | e.g. "Pre-Go-Live", "Training", "PARKED" | IMPL only | 15 | -- (not in scope v2) | preserved (useful context) |
| C15 | Implementation: days_live | CONFIG | `christine.json` | `days_live` | `CHRISTINE[company_id]` | No | "{N}d live" | IMPL only | 3 | -- | preserved |
| C16 | Implementation: status | CONFIG | `christine.json` | `status` | `CHRISTINE[company_id]` | No | GREEN/YELLOW/RED color-coded | IMPL only | 4 | -- | preserved |
| C17 | Implementation: type | CONFIG | `christine.json` | `type` | `CHRISTINE[company_id]` | No | e.g. "Core Multi-Loc", "eSuite" | IMPL only | 15 | -- | preserved |
| C18 | Implementation: note | CONFIG | `christine.json` | `note` | `CHRISTINE[company_id]` | No | Free text | IMPL only | 4 | -- | preserved |
| C19 | OPPORTUNITIES section header | CONFIG | -- | static text | -- | No | "OPPORTUNITIES" | All | 399 | core of scope v2 | preserved |
| C20 | Config opportunities (wrench) | CONFIG | computed by `getOpportunities()` | `type === 'config'` | CONFIG, BUNCHES, accounts | Yes | 5 config checks: MaxAge=0, MaxAge low, Bunches OFF, Future OFF, On-hand OFF | WH_CORE, WH_ESUITE, WH_K2K | 397 | "config opportunities" | preserved |
| C21 | Buy opportunities (package) | CONFIG | computed by `getOpportunities()` | `type === 'buy'` | METRICS | Yes | 2 checks: buy_stage early, K2K inactive ratio | All with connections | 112 | "BUY summary" | preserved but minimal |
| C22 | List opportunities (notepad) | CONFIG | computed by `getOpportunities()` | `type === 'list'` | -- | Yes | **NEVER GENERATED -- no `type:'list'` in getOpportunities()** | -- | 0 | "LIST summary" | **MISSING -- renderer exists but no generator** |
| C23 | Sell opportunities (money) | CONFIG | computed by `getOpportunities()` | `type === 'sell'` | METRICS, accounts | Yes | 2 checks: low online sell, no eShop | WH_CORE, WH_ESUITE with sell volume | 112 | "SELL summary" | preserved but minimal |
| C24 | "no opportunities detected" fallback | CONFIG | -- | -- | -- | No | Shown when `opps.length === 0` | All | conditional | -- | preserved |
| C25 | Bottleneck | CONFIG | computed by `computeBottleneck()` | multiple | CONFIG, METRICS, BUYERS, accounts | Yes | 8-step priority waterfall (see Bottleneck Logic) | All | 399 | "Bottleneck + next action" | preserved |
| C26 | Next Action | CONFIG | derived from `opps[0]` | `opps[0].text` | -- | Yes | First opportunity text = next action; falls back to "--" | All | conditional | "Bottleneck + next action" | preserved -- **naive: first opp = next action, no priority logic** |
| C27 | SFDC Opps header | CONFIG | `sfdc.json` | -- | -- | No | Shown only when sfdcMatches.length > 0 | All | 31 | -- | preserved |
| C28 | SFDC Opp: stage | CONFIG | `sfdc.json` | `stage` | `getSFDC()` fuzzy name | No | e.g. "Negotiating", "Discovery" | All | 31 | -- | preserved |
| C29 | SFDC Opp: name | CONFIG | `sfdc.json` | `opp_name` | same | No | Opportunity name | All | 31 | -- | preserved |
| C30 | SFDC Opp: amount | CONFIG | `sfdc.json` | `amount` | same | No | `fmt$(amount)` | All | 31 | -- | preserved |
| C31 | SFDC Opp: close_date | CONFIG | `sfdc.json` | `close_date` | same | No | "close {date}" | All | 31 | -- | preserved |

**Opportunity Generator Logic (9 types):**

| # | Opportunity | Type | Condition | Source fields | Product types |
|---|---|---|---|---|---|
| O1 | MaxAge = 0/null | config | `max_age === 0 or null` | `CONFIG[id].max_age` | All with cfg |
| O2 | MaxAge low | config | `max_age <= 10 && ct_id in (WH_CORE, WH_ESUITE)` | `CONFIG[id].max_age`, `acct.ct_id` | WH_CORE, WH_ESUITE |
| O3 | Bunches OFF | config | `!bunches_flag && ct_id in (WH_CORE, WH_ESUITE)` | `CONFIG[id].bunches_flag`, `BUNCHES[id].*` | WH_CORE, WH_ESUITE |
| O4 | Future OFF | config | `!future && ct_id === WH_CORE` | `CONFIG[id].future` | WH_CORE only |
| O5 | On-hand OFF | config | `!on_hand && ct_id in (WH_CORE, WH_ESUITE)` | `CONFIG[id].on_hand` | WH_CORE, WH_ESUITE |
| O6 | BUY stage early | buy | `buy_stage in ('NO_CONNECTIONS', 'CONNECTIONS_NO_BUY')` | `METRICS[id].buy_stage` | All with metrics |
| O7 | K2K inactive | buy | `k2k_total > 0 && k2k_active/k2k_total < 0.7` | `METRICS[id].k2k_active/k2k_total` | All with K2K |
| O8 | Low online sell | sell | `online_pct < 5 && sell_total > 100000` | `METRICS[id].online_pct/sell_total/sell_offline` | All meeting thresholds |
| O9 | No eShop | sell | `!has_eshops && ct_id in (WH_CORE, WH_ESUITE)` | `acct.has_eshops/ct_id` | WH_CORE, WH_ESUITE |

**Bottleneck Waterfall (8 priorities, first match wins):**

| Priority | Condition | Return | Color |
|---|---|---|---|
| 1 | `max_age === 0 or null` | "GATE: MaxAge=0" | RED |
| 2 | `!has_eshops && ct_id in (WH_CORE, WH_ESUITE)` | "GATE: No eShop" | RED |
| 3 | `!bunches_flag && ct_id in (WH_CORE, WH_ESUITE)` | "FORMAT: Bunches OFF" | AMBER |
| 4 | `buy_stage in ('NO_CONNECTIONS', 'CONNECTIONS_NO_BUY')` | "BUY: No active connections" | AMBER |
| 5 | `buy_stage === 'ACTIVATING'` | "BUY: Activating" | AMBER |
| 6 | `online_buyers < 10` | "SELL: Few online buyers" | AMBER |
| 7 | `sell_stage === 'NO_ONLINE_SALES'` | "SELL: No online sales" | AMBER |
| 8 | None match | "--" | MUTED |

### D. Card BUY (50 fields)

| # | Campo | Section | Source JSON | Source field | Match key | Computed? | Formula (if yes) | Product types | Coverage (N/399) | Scope v2 match | Disposition |
|---|---|---|---|---|---|---|---|---|---|---|---|
| D1 | Card title | BUY | -- | hardcoded | -- | No | -- | All | 399 | YES | preserved |
| D2 | UNIT ECONOMICS header | BUY | -- | hardcoded | -- | No | -- | All | 399 | YES | preserved |
| D3 | buyEstTotal (Est. total procurement) | BUY | `est_gmv.json` | `est_buy_gmv` (primary) + fallback calc | `company_id` | Yes | estBuyVal cascade: est_buy_gmv -> buy/sell ratio * est_sell -> 54% * est_sell (CORRECTED by Codex: was 75%, actual code is 0.54) | All | ~112 | YES | preserved |
| D4 | buyViaKomet (proc thru Komet) | BUY | `metrics.json` | `proc_total` | `company_id` | No | -- | All | 96 | YES | preserved |
| D5 | buyOnline (online proc) | BUY | `metrics.json` | `proc_online` | `company_id` | No | -- | All | 75 | YES | preserved |
| D6 | buyOffline (offline proc) | BUY | computed | -- | -- | Yes | `buyViaKomet - buyOnline` | All | 75 | YES | preserved |
| D7 | buyKometPct (% thru Komet) | BUY | computed | -- | -- | Yes | `buyViaKomet / buyEstTotal * 100` | All | conditional | implicit | preserved |
| D8 | buyOnlinePctCard | BUY | computed | -- | -- | Yes | `buyOnline / buyViaKomet * 100` | All | conditional | YES | preserved |
| D9 | ueIncrement (10% more online = $X) | BUY | computed | -- | -- | Yes | `buyEstTotal * 0.10` | All | conditional | YES (unit economics line) | preserved |
| D10 | ueIndirectFee (10% more = $Y fees) | BUY | computed | -- | -- | Yes | `ueIncrement * 0.015` -- **hardcoded 1.5%** | All | conditional | YES | preserved -- RISK: hardcoded rate |
| D11 | currentProcOnlinePct | BUY | computed | -- | -- | Yes | `buyOnline / buyViaKomet * 100` | All | conditional | YES | preserved |
| D12 | currentIndirectFees | BUY | computed | -- | -- | Yes | `buyOnline * 0.015` -- **hardcoded 1.5%** | All | conditional | YES | preserved -- RISK: hardcoded rate |
| D13 | UE display line 1 | BUY | -- | hardcoded text | -- | No | -- | All | 399 | YES | preserved |
| D14 | UE display line 2: = $X procurement | BUY | computed | `ueIncrement` | -- | Yes | `fmt$(ueIncrement)` | All | conditional | YES | preserved |
| D15 | UE display line 3: = $Y fees (1.5%) | BUY | computed | `ueIndirectFee` | -- | Yes | `fmt$(ueIndirectFee)` | All | conditional | YES | preserved |
| D16 | UE display line 4: Currently X% -> $Y fees | BUY | computed | both | -- | Yes | `fmtPct + fmt$` | All | conditional | YES | preserved |
| D17 | "Procurement only" banner | BUY | -- | hardcoded | -- | No | -- | WH_PROC only | ~40 | -- | preserved |
| D18 | K2K CONNECTIONS header (proc) | BUY | -- | hardcoded | -- | No | -- | WH_PROC only | ~40 | "K2K connections should be obvious" | preserved |
| D19 | K2K Active / Total (proc) | BUY | `metrics.json` | `k2k_active`, `k2k_total` | `company_id` | Yes (pct) | `k2k_active / k2k_total * 100` for color | WH_PROC | ~90 | YES | preserved |
| D20 | Dormant vendors (proc) | BUY | `vendor_detail.json` | `dormant_vendors` | `company_id` | No | `vd.dormant_vendors || 0` | WH_PROC | 62 (of 336) | YES | preserved |
| D21 | Churned vendors (proc) | BUY | `vendor_detail.json` | `churned_vendors` | `company_id` | No | `vd.churned_vendors || 0` | WH_PROC | 93 (of 336) | YES | preserved |
| D22 | VENDORS header (proc) | BUY | -- | hardcoded | -- | No | -- | WH_PROC | ~40 | YES | preserved |
| D23 | Online vendors (proc) | BUY | `vendor_detail.json` | `online_vendors` | `company_id` | No | -- | WH_PROC | 205 | YES | preserved |
| D24 | Offline vendors (proc) | BUY | `vendor_detail.json` | `offline_vendors` | `company_id` | No | -- | WH_PROC | 220 | YES | preserved |
| D25 | Gap vendors + cost (proc) | BUY | `vendor_detail.json` | `gap_vendors`, `gap_cost` | `company_id` | No | "X sin K2K ($Y)" | WH_PROC | 137 | YES | preserved |
| D26 | ONLINE vs OFFLINE header | BUY | -- | hardcoded | -- | No | -- | All except WH_PROC | ~360 | YES | preserved |
| D27 | Vendors: Online | BUY | `vendor_detail.json` | `online_vendors` | `company_id` | No | -- | Non-WH_PROC | 205 | YES | preserved |
| D28 | Vendors: Offline | BUY | `vendor_detail.json` | `offline_vendors` | `company_id` | No | `vd.offline_vendors || 0` | Non-WH_PROC | 220 | YES | preserved |
| D29 | Vendors: Gap | BUY | `vendor_detail.json` | `gap_vendors`, `gap_cost` | `company_id` | No | "X sin K2K ($Y)" | Non-WH_PROC | 137 | YES | preserved |
| D30 | Categories: Online | BUY | `supply_matrix_full.json` | `buy_coverage[].online_categories_bought` | `company_name` (normName) | No | -- | Non-WH_PROC | 254 | YES | preserved |
| D31 | Categories: Offline | BUY | `supply_matrix_full.json` | `total_categories_bought - online_categories_bought` | `company_name` | Yes | subtraction | Non-WH_PROC | 254 | YES | preserved |
| D32 | Categories: Gap | BUY | `supply_matrix_full.json` | same | `company_name` | Yes | "X no online" | Non-WH_PROC | 254 | YES | preserved |
| D33 | Varieties: Online | BUY | `loop2_list_usage_v2.json` | `distinct_varieties` | `company_id` | No | -- | Non-WH_PROC | 272 | YES (scope v2 lists "Varieties") | preserved |
| D34 | SKUs: Online | BUY | `loop2_list_usage_v2.json` | `distinct_skus` | `company_id` | No | `fmtN(distinct_skus)` | Non-WH_PROC | 272 | YES (scope v2 lists "SKUs") | preserved |
| D35 | **Bunches: Online** | BUY | `bunches.json` | `total_bunch_gmv` | `company_id` | No | `fmt$(total_bunch_gmv)` | Non-WH_PROC | 43 | **REMOVE -- scope v2: "Bunches NOT in BUY"** | **MOVE to LIST or SELL** |
| D36 | **Bunches: Gap (flag)** | BUY | `config.json` | `bunches_flag` | `company_id` | No | ON/OFF | Non-WH_PROC | 397 | **REMOVE -- same as D35** | **MOVE to LIST or SELL** |
| D37 | Anticipation row | BUY | -- | -- | -- | No | **hardcoded "needs data"** | Non-WH_PROC | 0 | Neutral -- buy_detail.json HAS anticipation data but NOT wired | **WIRE UP** (data exists: orders_0_3d, orders_4_7d, orders_8_14d, orders_15_30d, orders_30plus_d) |
| D38 | LEAKAGE header | BUY | -- | hardcoded | -- | No | -- | Non-WH_PROC | ~360 | YES | preserved |
| D39 | Leakage vendors + cost | BUY | `vendor_detail.json` | `leakage_vendors`, `leakage_cost` | `company_id` | No | "X vendors CON K2K pero compran offline -> $Y" | Non-WH_PROC | 3 (0.9% -- extremely sparse) | YES ("arreglable HOY") | preserved |
| D40 | PROCUREMENT FUNNEL header | BUY | -- | hardcoded | -- | No | -- | Non-WH_PROC | ~360 | YES | preserved |
| D41 | K2K Connected (funnel) | BUY | `metrics.json` | `k2k_total` | `company_id` | No | -- | Non-WH_PROC | 90 | YES | preserved |
| D42 | Activated (ever bought) | BUY | `metrics.json` | `k2k_active`, `k2k_total` | `company_id` | Yes (pct) | `k2k_active / k2k_total * 100` | Non-WH_PROC | 90 | YES | preserved |
| D43 | Dormant (connected, never bought) | BUY | `vendor_detail.json` | `dormant_vendors` | `company_id` | No | `vd.dormant_vendors || 0` | Non-WH_PROC | 62 | YES | preserved |
| D44 | Churned (bought before, stopped) | BUY | `vendor_detail.json` | `churned_vendors` | `company_id` | No | `vd.churned_vendors || 0` | Non-WH_PROC | 93 | YES | preserved |
| D45 | Not yet activated (funnel) | BUY | computed | -- | -- | Yes | `k2k_total - k2k_active - dormant` | Non-WH_PROC | conditional | YES | preserved |
| D46 | K2K fallback (k2k_total = 0) | BUY | `metrics.json` | `k2k_active`, `k2k_total` | `company_id` | No | "X / Y" | Non-WH_PROC | conditional | YES | preserved |
| D47 | ORDER MIX header | BUY | -- | hardcoded | -- | No | -- | Non-WH_PROC | ~360 | YES ("Open Market + Prebook") | preserved |
| D48 | Open Market GMV + pct | BUY | `buy_detail.json` | `order_types[].open_market_gmv` | `company_name` (normName) | Yes (pct) | `om_gmv / (om_gmv + pb_gmv) * 100` | Non-WH_PROC | 88 | YES | preserved |
| D49 | Prebook GMV + pct | BUY | `buy_detail.json` | `order_types[].prebook_gmv` | `company_name` (normName) | Yes (pct) | `pb_gmv / (om_gmv + pb_gmv) * 100` | Non-WH_PROC | 88 | YES | preserved |
| D50 | Order Mix note "from SALES_SV..." | BUY | -- | hardcoded caveat | -- | No | -- | Non-WH_PROC | conditional | -- (provenance) | preserved -- **CAVEAT: uses SELL-side data as proxy for BUY-side** |

### E. Card LIST (19 fields)

| # | Campo | Section | Source JSON | Source field | Match key | Computed? | Formula (if yes) | Product types | Coverage (N/399) | Scope v2 match | Disposition |
|---|---|---|---|---|---|---|---|---|---|---|---|
| E1 | BLOCKED gate (WH_PROC) | LIST | `accounts.json` | `ct_id` | `a.ct_id === 'WH_PROC'` | No | -- | WH_PROC only | ~40 | N/A (gate) | preserved |
| E2 | K2K warning | LIST | `accounts.json` | `ct_id` | `a.ct_id === 'WH_K2K'` | No | -- | WH_K2K only | ~50 | N/A (warning) | preserved |
| E3 | RESUMEN: % categories online | LIST | `supply_matrix_full.json` -> sell_coverage | `online_categories_sold / total_categories_sold` | `findSupplySell(name)` by normName | Yes | `online / total * 100` | All except WH_PROC | 270 | Partial (no best-in-class) | preserved |
| E4 | SO WHAT: gap categories count | LIST | `supply_matrix_full.json` -> gap_categories + fallback | `gap_categories` or `total - online` | `findGapCategories(name)` by normName | Yes (fallback) | primary or subtraction | All except WH_PROC | 183 (gap) / 270 (fallback) | Partial | preserved |
| E5 | Categories: Online | LIST | `supply_matrix_full.json` -> sell_coverage | `online_categories_sold` | `findSupplySell(name)` | No | -- | All except WH_PROC | 270 | Partial (no best-in-class col) | preserved |
| E6 | Categories: Offline | LIST | `supply_matrix_full.json` -> sell_coverage | `total - online` | same | Yes | subtraction | All except WH_PROC | 270 | Partial | preserved |
| E7 | Categories: Gap | LIST | `supply_matrix_full.json` -> gap_categories + fallback | `gap_categories` or `total - online` | `findGapCategories(name)` | Yes (fallback) | same as E4 | All except WH_PROC | 183/270 | Partial | preserved |
| E8 | Varieties (online count) | LIST | `loop2_list_usage_v2.json` | `distinct_varieties` | `findListUsage(company_id)` | No | -- | All except WH_PROC | 272 | PARTIAL -- online-only, no offline | preserved but incomplete |
| E9 | SKUs (online count) | LIST | `loop2_list_usage_v2.json` | `distinct_skus` | `findListUsage(company_id)` | No | -- | All except WH_PROC | 272 | PARTIAL -- online-only, no offline | preserved but incomplete |
| E10 | Time depth: MaxAge config | LIST | `config.json` | `max_age` | `CONFIG[company_id]` | No | Display + warning if <= 10 | All except WH_PROC | 379 | Partial | preserved |
| E11 | Format: Bunches flag | LIST | `config.json` | `bunches_flag` | `CONFIG[company_id]` | No | ON/OFF | All except WH_PROC | 397 | OK | preserved |
| E12 | Format: Bunch GMV | LIST | `bunches.json` | `total_bunch_gmv` | `BUNCHES[company_id]` | No | -- | All except WH_PROC | 43 | Partial (no TAM lost est.) | preserved |
| E13 | Format: Bunch TAM warning | LIST | `bunches.json` + `config.json` | `sells_bunches`, `bunches_flag` | `BUNCHES[id]` + `CONFIG[id]` | Yes | "95% TAM" if flag OFF and sells_bunches TRUE | All except WH_PROC | 43 | Partial (no $ estimate) | preserved |
| E14 | Time Depth bar chart (stacked) | LIST | `loop2_list_time_depth_v2.json` | `pct_0_3_days` through `pct_over_90_days`, `total_varieties` | `findListTimeDepthV2(company_id)` | No (pre-computed %) | -- | All except WH_PROC | 99 | **CRITICAL GAP: online-only; scope v2 needs 3 versions (online/offline/best-in-class)** | preserved -- MISSING 2 of 3 versions |
| E15 | Categories detail (listing_detail) | LIST | `listing_detail.json` | `online_categories`, `total_categories` | `findListingCats(name)` by normName | Yes | offline = `total - online` | All except WH_PROC | 0 (**ALWAYS NULL**) | -- | **BROKEN -- `findListingCats` reads `.categories` but JSON key is `.varieties`** |
| E16 | Open Market: Sold as OM GMV | LIST | `buy_detail.json` -> order_types | `open_market_gmv` | `findBuyDetail(name)` by normName | No | -- | All except WH_PROC | 88 | **MISLABELED -- data is ORDER_TYPE from SALES_SV (sales, not uploads)** | preserved but **MISLABELED** |
| E17 | Open Market: % of sell GMV | LIST | `buy_detail.json` + `metrics.json` | `open_market_gmv / met.sell_total` | normName + company_id | Yes | division | All except WH_PROC | 88 | same mislabeling as E16 | preserved but **MISLABELED** |
| E18 | Open Market: $0 warning | LIST | `buy_detail.json` | `open_market_gmv === 0` | normName | No | boolean check | All except WH_PROC | conditional | OK | preserved |
| E19 | Open Market: eShop empty warning | LIST | `buy_detail.json` + `accounts.json` | `open_market_gmv`, `ct_id` | normName + direct | No | compound boolean | WH_CORE, WH_ESUITE only | conditional | OK | preserved |

### F. Card SELL (30 fields)

| # | Campo | Section | Source JSON | Source field | Match key | Computed? | Formula (if yes) | Product types | Coverage (N/399) | Scope v2 match | Disposition |
|---|---|---|---|---|---|---|---|---|---|---|---|
| F1 | RESUMEN: Online $ | SELL | `metrics.json` | `met.sell_online` | `company_id` | No | -- | All | 79 | Online GMV = yes | preserved |
| F2 | RESUMEN: Online % | SELL | `metrics.json` | `met.online_pct` | `company_id` | No | -- | All | 77 | Online % = yes | preserved |
| F3 | RESUMEN: Offline $ | SELL | `metrics.json` | `met.sell_offline` | `company_id` | No | -- | All | 24 | Offline GMV = yes | preserved |
| F4 | SO WHAT: Offline $ at $0 fees | SELL | `metrics.json` | `met.sell_offline` | `company_id` | No | -- | All | 24 | Offline GMV at $0 fees = yes | preserved |
| F5 | Repeat Rate color/label | SELL | `repeat_rate.json` | `repeat_rate_pct` | `normName(company_name)` | Yes | >60 green/healthy, 40-60 amber/warning, <40 red/low | All | 231 | Repeat Rate = yes | preserved |
| F6 | Concentration Top2 color/label | SELL | `buyer_concentration.json` | `top2_pct` | `normName(company_name)` | Yes | >50 red/risk, 25-50 amber/moderate, <25 green/healthy | All | 226 | Concentration = yes | preserved -- **but scope v2 says Top 5, not Top 2** |
| F7 | bv2 lookup | SELL | `loop2_phase1v2_buyers.json` | via `findBuyersV2(company_id)` | `COMPANY_ID` | No | -- | All | 156 | Buyers V2 = yes | preserved |
| F8 | Table: Buyers Online | SELL | `buyers.json` | `online_buyers` | `company_id` | No | -- | All | 274 | Online Buyers = yes | preserved |
| F9 | Table: Buyers Offline | SELL | `buyers.json` | `offline_buyers` | `company_id` | No | -- | All | 274 | Offline Buyers = yes | preserved |
| F10 | Table: Buyers Total | SELL | `buyers.json` | `total_buyers` | `company_id` | No | -- | All | 274 | Total Buyers = yes | preserved |
| F11 | Table: Active L30D (Online) | SELL | `loop2_phase1v2_buyers.json` (primary) / `sell_detail.json` (fallback) | `ACTIVE_BUYERS_L30D` / `buyers_online_l30d` | `company_id` / `normName(company_name)` | No | cascade: bv2 first, sellL30D fallback | All | 156 (bv2) / 180 (sellL30D) | L30D = yes | preserved |
| F12 | Table: Active L30D (Offline/Total) | SELL | -- | hard-coded `'---'` | -- | No | -- | -- | 0 | **GAP: scope v2 wants Offline L30D** | **new (needs data)** |
| F13 | Table: New Q2 (Online) | SELL | `loop2_phase1v2_buyers.json` | `NEW_BUYERS_Q2` | `company_id` | No | -- | All | 156 | New Q2 = yes | preserved |
| F14 | Table: Churned Q2 (Online) | SELL | `loop2_phase1v2_buyers.json` | `CHURNED_BUYERS_Q2` | `company_id` | No | -- | All | 156 | Churned Q2 = yes | preserved |
| F15 | Table: AOV (Online) | SELL | `sell_detail.json` | `aov_online` | `normName(company_name)` | No | -- | All | 227 | AOV = yes | preserved |
| F16 | Table: AOV (Offline/Total) | SELL | -- | hard-coded `'---'` | -- | No | -- | -- | 0 | **GAP: scope v2 wants Offline AOV** | **new (needs data)** |
| F17 | Table: Repeat Rate (Online) | SELL | `repeat_rate.json` | `repeat_rate_pct` | `normName(company_name)` | No | formatted with color + label | All | 231 | Repeat Rate = yes | preserved |
| F18 | Table: Conversion (Online) | SELL | `ga4_eshop.json` | `transactions / sessions` | `normName(hostname)` fuzzy match | Yes | `(transactions / sessions) * 100` | eShop only | 15 (with transactions) | GA4 CVR = yes (with caveat) | preserved + **CAVEAT: hostname matching fragile** |
| F19 | Table: Concentration Top 2 (Online) | SELL | `buyer_concentration.json` | `top2_pct` | `normName(company_name)` | No | formatted with color + label | All | 226 | Concentration = yes **but scope says Top 5** | **PARTIAL** |
| F20 | "Top 5 needs query" disclaimer | SELL | -- | hard-coded text | -- | No | -- | -- | -- | acknowledged gap | preserved (note) |
| F21 | Channel Mix one-liner | SELL | `sell_by_channel.json` | `channel_gmv`, `sales_channel` | `normName(company_name)` | Yes | per-channel %: `channel_gmv / totalChGmv * 100` | All | 271 | Channel mix = yes | preserved |
| F22 | Sell Format: Boxes % | SELL | `loop2_sell_format_time_v2.json` | `online_pct_boxes` | `normName(company_name)` | No | -- | All | 106 | not explicitly in scope v2 | preserved (useful) |
| F23 | Sell Format: Bunches % | SELL | `loop2_sell_format_time_v2.json` | `online_pct_bunches` | `normName(company_name)` | No | -- | All | 15 | not explicitly in scope v2 | preserved |
| F24 | Sell Format: Short % | SELL | `loop2_sell_format_time_v2.json` | `online_pct_short` | `normName(company_name)` | No | -- | All | 94 | not explicitly in scope v2 | preserved |
| F25 | Sell Format: Forward % | SELL | `loop2_sell_format_time_v2.json` | `online_pct_forward` | `normName(company_name)` | No | -- | All | 82 | not explicitly in scope v2 | preserved |
| F26 | GA4 eShop: Sessions | SELL | `ga4_eshop.json` | `sessions` | `normName(hostname)` fuzzy | No | -- | eShop only | 29 | GA4 = yes | preserved |
| F27 | GA4 eShop: Transactions | SELL | `ga4_eshop.json` | `transactions` | same | No | -- | eShop only | 15 | GA4 = yes | preserved |
| F28 | GA4 eShop: Conversion % | SELL | `ga4_eshop.json` | computed | same | Yes | `(transactions / sessions) * 100`; >10 green, >3 amber, else red | eShop only | 15 | GA4 CVR = yes | preserved + CAVEAT |
| F29 | GA4 eShop: Revenue | SELL | `ga4_eshop.json` | `revenue` | same | No | -- | eShop only | 15 | GA4 = yes | preserved |
| F30 | Online Retention % | SELL | `buyers.json` + `sell_detail.json` | `buyers_online_l30d / online_buyers` | `company_id` (buyers) + `normName` (sellL30D) | Yes | `(l30d / online_buyers) * 100`; >50 green, >25 amber, else red | All | ~156 | Retention = yes ("online retention over time") | preserved -- **but point-in-time, not trend** |

---

## SECTION 3: TABLE 2 -- GAPS (scope v2 requirements not met today)

### Portfolio gaps

| # | Card | Scope v2 requirement | Exists today? | What's missing | Who resolves |
|---|---|---|---|---|---|
| G1 | Portfolio | Est Buy GMV | No | No column. `est_gmv.json` has `est_buy_gmv` per company. Data exists, logic missing -- needs new `<th>` + `<td>`. | Builder logic |
| G2 | Portfolio | Koronet Buy | No | No column. `metrics.json` has `proc_total`. Currently inside BUY mini-score but not standalone. Data exists, logic missing. | Builder logic |
| G3 | Portfolio | Buy Penetration % | No | No column. Requires Est Buy GMV and Koronet Buy. Formula: `proc_total / est_buy_gmv * 100`. Data exists, logic missing. | Builder logic |
| G4 | Portfolio | Online Buy % | No | No column. `metrics.json` has `proc_online` and `proc_total`. Formula: `proc_online / proc_total * 100`. Data exists, logic missing. | Builder logic |
| G5 | Portfolio | Fees = one summed column (direct + indirect) | Partial | `fees_total` exists and is rendered. **UNKNOWN whether it already sums direct+indirect** or only direct. No explicit split in JSON. | Rose data (verify Snowflake query) |
| G6 | Portfolio | Take Rate = fees_total / (sell_GMV + buy_GMV) | No | No column. All underlying data exists. Formula: `fees_total / (sell_total + proc_total) * 100`. | Builder logic |
| G7 | Portfolio | # Opps = APPROVED only | No (semantic gap) | Current counts ALL computed opps. No approval concept exists. Needs either approval status field or separate data source. | Facu decision (define approval workflow) |
| G8 | Portfolio | Trend (growing / flat / declining) | No | No column, no data. Requires period-over-period comparison. Both data and logic missing. | Rose data (historical metrics) |
| G9 | Portfolio | eShop CVR % | No | No column in portfolio. `ga4_eshop.json` loaded and used in expand row but not in main table. Data exists, logic missing. | Builder logic |

### POTENTIAL gaps

| # | Card | Scope v2 requirement | Exists today? | What's missing | Who resolves |
|---|---|---|---|---|---|
| G10 | POTENTIAL | ORA source label on Est. Sell row | No | ORA row has empty 3rd cell -- no label (e.g., "Christine ORA" or "SFDC"). Calc row does show source+confidence. Minor gap. | Builder logic |
| G11 | POTENTIAL | Sell penetration fallback to calc estimate | No | When `annual_sales_est` is null, sell penetration = null. Code does NOT fall back to `est_sell_gmv` as denominator. | Builder logic |
| G12 | POTENTIAL | Channel GMV match key mismatch | RISK | `sell_by_channel.json` matches by company_name via normName; all others use company_id. Silent failure if names differ. | Rose data (add company_id to sell_by_channel) |
| G13 | POTENTIAL | Indirect fee rate hardcoded | RISK | `met.proc_online * 0.015` uses hardcoded 1.5%. No config source. | Facu decision |
| G14 | POTENTIAL | No product-type conditional rendering | UNCLEAR | Card treats all product types identically. If different types have structurally different fee profiles, card doesn't adapt. | Facu decision |

### CONFIG/OPPORTUNITIES gaps

| # | Card | Scope v2 requirement | Exists today? | What's missing | Who resolves |
|---|---|---|---|---|---|
| G15 | CONFIG | For each product type: explain potential | ✅ FIXED (Aug 1) | `_cfgProfiles` now includes `potential` field with revenue potential, best-in-class references, and data-backed claims per ct_id. | Builder logic |
| G16 | CONFIG | For each product type: best in class | ✅ FIXED (Aug 1) | benchmarks.json wired per segment (ct_id). Shows median and best account for online sell %, repeat rate, variety count, catalog freshness. | Builder logic (using getBenchmark()) |
| G17 | CONFIG | For each product type: known limitations | ✅ FIXED (Aug 1) | `_cfgProfiles` now includes `limitations` field with quantified constraints (eSuite ~5% of Core TAM, K2K vendor dependency, Proc no sell-side). | Builder logic |
| G18 | CONFIG | LIST opportunities generator | ✅ FIXED (Aug 1) | 5 LIST opp types: low variety coverage (with benchmark), no long-term inventory, bunches not listed ($), no open market GMV, category gap. | Builder logic |
| G19 | CONFIG | Card renamed from CONFIG to OPPORTUNITIES | ✅ FIXED | Header says "OPPORTUNITIES (revenue + config — prioritized by impact — DRAFT)". | Builder logic |
| G20 | CONFIG | Next action with priority logic | ✅ FIXED (Aug 1) | Opps sorted by priority (1-5) then by $ impact. Next action uses `opps[0].action` (the actionable guidance from highest-priority opp). | Builder logic |

### BUY gaps

| # | Card | Scope v2 requirement | Exists today? | What's missing | Who resolves |
|---|---|---|---|---|---|
| G21 | BUY | Bunches NOT in BUY card | VIOLATED | Bunches row rendered in BUY (lines 1354-1357). Scope v2: remove, it's a LIST/SELL concept. | Builder logic (move to LIST or SELL) |
| G22 | BUY | Online vs Offline: Total vendors row | No | `total_vendors` exists in vendor_detail.json but not rendered. | Builder logic |
| G23 | BUY | Online vs Offline: Connected (not activated) row | No | `dormant_vendors` is in the funnel section but not the table. | Builder logic |
| G24 | BUY | Online vs Offline: Active L30D row | No | No L30D activity data for buy side. Needs new Snowflake query with 30-day filter. | Rose data |
| G25 | BUY | Online vs Offline: Churned/Lapsed L30D row | No | `churned_vendors` is all-time, not L30D. Needs new query. | Rose data |
| G26 | BUY | Vendor coverage by time horizon (short/medium/long) | No | No temporal dimension for vendor coverage. | Rose data |
| G27 | BUY | Offline varieties and SKUs columns | No | Only online shown from loop2_list_usage_v2.json. No offline count. | Rose data |
| G28 | BUY | Anticipation data wired up | STUBBED | Line 1360 = "needs data" placeholder. `buy_detail.json` already has `orders_0_3d`, `orders_4_7d`, `orders_8_14d`, `orders_15_30d`, `orders_30plus_d`. Data exists, not wired. | Builder logic |
| G29 | BUY | Order Mix uses actual procurement-side data | No | Currently uses SELL-side `SALES_SV` ORDER_TYPE as proxy. Procurement-side prebook/SO data not yet available. | Rose data |
| G30 | BUY | Leakage data sparse | DATA GAP | Only 3/336 records (0.9%) have leakage_vendors data. Logic is correct but data nearly empty. | Rose data |
| G31 | BUY | Vendor lifecycle in table (Active L30D, Churned L30D) | No | These lifecycle rows absent from Online vs Offline table. Dormant/Churned in funnel but not table. L30D doesn't exist. | Rose data + Builder logic |
| G32 | BUY | K2K connections visible from vendor rows | Partial | K2K in funnel section but not directly in Online vs Offline vendor table. Gap column says "sin K2K" but connection state per vendor not explicit. | Builder logic |

### LIST gaps

| # | Card | Scope v2 requirement | Exists today? | What's missing | Who resolves |
|---|---|---|---|---|---|
| G33 | LIST | Time Depth: 3 versions (online/offline/best-in-class) | Partial | Only ONLINE version exists (eCommerce+K2K+API filter). Need offline-only version and best-in-class benchmark. | Rose data (offline query) + Facu decision (best-in-class definition) |
| G34 | LIST | Config: sold_as_future flag | No | `config.json` has `future` field but LIST card never references it. Shown in compact config cell for WH_CORE but not in LIST card. | Builder logic |
| G35 | LIST | Config: Standing Orders Core-only | No | No data source, no rendering. | Rose data |
| G36 | LIST | TAM lost table: by bunches, categories, varieties, SKUs, timeframe | No | Only static "95% TAM" string. No dollar estimate, no breakdown. | Rose data + Builder logic |
| G37 | LIST | Open Market = what they UPLOAD (not sell) | MISLABELED | Data is `ORDER_TYPE = 'Open Market'` from SALES_SV (includes auto-generated). No upload-specific data source. | Rose data (upload query) |
| G38 | LIST | 9 Challenger Messages (CS enablement) | Data exists, not rendered | `definitions.json` -> `list_challenger[]` has 9 messages. Never rendered in LIST card. | Builder logic |
| G39 | LIST | Categories detail (listing_detail) | BROKEN | `findListingCats` reads `.categories` but JSON key is `.varieties`. Always returns null. Dead code. | Builder logic (fix key name) |
| G40 | LIST | Online vs offline vs best-in-class for categories | Partial | Categories table has ONLINE/OFFLINE/GAP but no best-in-class benchmark column. | Rose data (benchmark) + Builder logic |
| G41 | LIST | Varieties: offline count | Partial | Online only from loop2_list_usage_v2.json. No offline variety count. | Rose data |
| G42 | LIST | SKUs: offline count | Partial | Same issue as varieties. Online only. | Rose data |
| G43 | LIST | SO WHAT: estimate TAM lost ($) | No | Shows gap category count but no dollar estimate. | Rose data + Builder logic |

### SELL gaps

| # | Card | Scope v2 requirement | Exists today? | What's missing | Who resolves |
|---|---|---|---|---|---|
| G44 | SELL | Unit economics: "X% more buyers = $Y GMV = $Z fees" | No | No simulation/model. Card shows raw metrics but no unit-economics projection. | Builder logic |
| G45 | SELL | Best in class examples with links to leads | No | No peer comparisons. No SFDC lead URLs. | Rose data (cross-account benchmarks) + Builder logic |
| G46 | SELL | Offline Buyers L30D / Total columns | No | Hard-coded '---'. Neither bv2 nor sellL30D carry offline L30D data. | Rose data (new query) |
| G47 | SELL | Offline AOV | No | Hard-coded '---'. `aov_online` only. Need `aov_offline` field. | Rose data (new query) |
| G48 | SELL | Concentration: Top 5 + MISSING all offline | Partial | Only Top 2 exists (`top2_pct`). Explicitly noted "Top 5 needs query." | Rose data (expanded concentration query) |
| G49 | SELL | Channel mix: flag if API sales exist | Partial | API would appear as channel row but no explicit detection/callout flag. | Builder logic |
| G50 | SELL | Hardgoods visibility | No | No hardgoods data anywhere. No product-type segmentation (perishable vs hardgoods). | Rose data (new query) |
| G51 | SELL | GA4 CVR caveat shown in UI | Partial | `findGA4` uses substring fuzzy match on hostname. No warning shown to user. Scope v2 says add caveat. | Builder logic |
| G52 | SELL | Offline/Total columns for New Q2, Churned Q2, Repeat Rate | No | All show '---' for Offline/Total. bv2 only has online. repeat_rate is single rate, not split by channel. | Rose data |
| G53 | SELL | Retention "over time" (trend) | Partial | Current = point-in-time ratio (L30D/total). No time series. Scope v2 says "over time" implying multi-period view. | Rose data (historical) + Builder logic |

---

## SECTION 4: TABLE 3 -- TEMPORAL REQUIREMENTS

| # | Metric | Card(s) | Current state | Timeframes needed | Source for temporal query | Exists today? |
|---|---|---|---|---|---|---|
| T1 | Sell GMV | Portfolio, POTENTIAL, SELL | Point-in-time from metrics.json (`sell_total`) | WoW, MoM, QoQ, YoY | `PRODUCTION.ANALYTICS.SALES_SV` with date filters | No -- metrics.json is single-snapshot |
| T2 | Buy GMV | Portfolio, POTENTIAL, BUY | Point-in-time from metrics.json (`proc_total`) | WoW, MoM, QoQ, YoY | Procurement views with date filters | No |
| T3 | Online % (Sell) | Portfolio, POTENTIAL, SELL | Point-in-time (`online_pct`) | WoW, MoM, QoQ, YoY | SALES_SV with channel+date filters | No |
| T4 | Fees | Portfolio, POTENTIAL | Point-in-time (`fees_total`) | WoW, MoM, QoQ, YoY | TX fee views with date filters | No |
| T5 | Take Rate | Portfolio (new) | Not computed today | WoW, MoM, QoQ, YoY | fees / (sell+buy) per period | No |
| T6 | CVR (eShop) | SELL | Point-in-time from GA4 (21 days observed) | WoW, MoM | GA4 with date dimension | No (GA4 data is pre-aggregated) |
| T7 | New User CVR | SELL | Not computed today | WoW, MoM | GA4 new vs returning segmentation | No |
| T8 | Buyers (total/online) | SELL | Point-in-time from buyers.json | WoW, MoM, QoQ | SALES_SV buyer counts with date filters | No (Active L30D is partial proxy) |
| T9 | Repeat Rate | SELL | Point-in-time (`repeat_rate_pct`) | MoM, QoQ, YoY | repeat_rate view with date filters | No |
| T10 | Categories/Varieties/SKUs online | LIST | Point-in-time from supply_matrix_full and loop2_list_usage | MoM, QoQ | LISTING views with date filters | No |

**Summary:** NONE of these temporal requirements are met today. `metrics.json` and all other data files are single-snapshot with no prior-period fields. Implementing trending requires either: (a) adding prior-period columns to existing queries, or (b) storing historical snapshots and computing deltas.

---

## SECTION 5: DATA COVERAGE

### Per-file summary

| # | File | Structure | Records | Match key | Match rate to accounts.json (N/399) | Rate |
|---|---|---|---|---|---|---|
| 1 | accounts.json | Array | 399 | -- (IS the master) | 399 | 100% |
| 2 | metrics.json | Object (by company_id) | 112 | company_id | 112 | **28.1%** |
| 3 | config.json | Object (by company_id) | 397 | company_id | 397 | 99.5% |
| 4 | bunches.json | Object (by company_id) | 43 | company_id | 43 | 10.8% |
| 5 | sfdc.json | Array | 35 | account_name (fuzzy) | 31 | 7.8% |
| 6 | christine.json | Object (by company_id) | 15 | company_id | 15 | 3.8% |
| 7 | definitions.json | Object (reference) | N/A | N/A | N/A | N/A |
| 8 | buyers.json | Array | 339 | company_id | 274 | 68.7% |
| 9 | sell_by_channel.json | Object w/ data[] | 639 rows (339 cos) | company_name + SFDC account_id | 271 | 67.9% |
| 10 | buyer_concentration.json | Object w/ accounts[] | 278 | company_name + SFDC account_id | 226 | 56.6% |
| 11 | repeat_rate.json | Object w/ data[] | 280 | company_name + SFDC account_id | 231 | 57.9% |
| 12 | supply_matrix_full.json | Object w/ 3 sections | buy:318, sell:339, gaps:230 | company_name + SFDC account_id | 254/270/183 | 63.7%/67.7%/45.9% |
| 13 | buy_detail.json | Object w/ 2 sections | orders:106, anticipation:50 | company_name + SFDC account_id | 88/45 | 22.1%/11.3% |
| 14 | sell_detail.json | Object w/ 3 sections | buyers_l30d:215, aov:278, vendors:318 | company_name + SFDC account_id | 180/227/260 | 45.1%/56.9%/65.2% |
| 15 | est_gmv.json | Object (by company_id) | 113 | company_id | 112 | 28.1% |
| 16 | vendor_detail.json | Object (by company_id) | 336 | company_id | 277 | 69.4% |
| 17 | listing_detail.json | Object w/ 2 sections | varieties:339, time_depth:279 | company_name + SFDC account_id | 264/222 | 66.2%/55.6% |
| 18 | ga4_eshop.json | Object w/ eshops[] | 29 | hostname | N/A (no company_id) | N/A |
| 19 | loop2_list_usage_v2.json | Object w/ records[] | 337 | company_id | 272 | 68.2% |
| 20 | loop2_list_time_depth_v2.json | Object w/ records[] | 112 | company_id | 99 | 24.8% |
| 21 | loop2_phase1v2_buyers.json | Array | 183 | COMPANY_ID | 156 | 39.1% |
| 22 | loop2_sell_format_time_v2.json | Object w/ sell_format_time[] | 151 | company_name (NO company_id) | 125 | 31.3% |

### Critical coverage findings

**1. metrics.json covers only 28.1% (112/399).** This is the most important data file -- it feeds Portfolio columns (Koronet Sell, Penetr %, Online %, Fees), POTENTIAL card, and BUY card. Of the 112 it covers, `fees_total` is non-zero for only 37 (33% of 112, 9.3% of 399). `sell_offline` is non-zero for only 24 (21.4%).

**2. Join key inconsistency is systemic.** Files split into two camps:
- **company_id match** (direct join): accounts, metrics, config, bunches, est_gmv, vendor_detail, loop2_list_usage_v2, loop2_list_time_depth_v2, loop2_phase1v2_buyers
- **company_name + SFDC account_id** (no company_id): sell_by_channel, buyer_concentration, repeat_rate, supply_matrix_full, buy_detail, sell_detail, listing_detail, loop2_sell_format_time_v2
- **hostname** (no match path): ga4_eshop

**3. Orphan records are widespread.** 59-68 records in buyers.json, vendor_detail.json, sell_by_channel.json, loop2_list_usage_v2.json exist for accounts NOT in accounts.json. Likely active platform accounts not yet in the 399-account master list.

**4. Sparse files by design:** bunches.json (43 records), sfdc.json (35), christine.json (15), ga4_eshop.json (29) cover narrow use cases. Low counts are expected.

**5. One broken record:** accounts.json has 1 account ("Heirloom Home Decor") with `company_id = null`. This creates a "None" key in est_gmv.json and is missing from config.json.

### Key field sparseness within files

| File | Field | Non-null rate | Impact |
|---|---|---|---|
| accounts.json | annual_sales_est | 158/399 = 39.6% | Sell penetration % is null for 60% of accounts |
| accounts.json | go_live | 217/399 = 54.4% | Go-live date unknown for half the base |
| metrics.json | fees_total | 37/112 = 33.0% | Fee columns largely empty |
| metrics.json | sell_offline | 24/112 = 21.4% | Offline GMV rarely populated |
| vendor_detail.json | leakage_vendors | 3/336 = 0.9% | Leakage section nearly empty |
| est_gmv.json | est_sell_gmv | 69/113 = 61.1% (44 are zero) | Calc estimate often unavailable |
| buy_detail.json | anticipation (orders_15_30d) | 10/50 = 20% | Anticipation data very sparse |

---

## SECTION 6: CROSS-REFERENCE FLAGS

### 6.1 JSONs referenced by cards where coverage audit found issues

| JSON | Card(s) using it | Coverage issue | Impact |
|---|---|---|---|
| metrics.json | Portfolio, POTENTIAL, BUY, LIST, SELL | 28.1% coverage (112/399) | 287 accounts show dashes/null for all metrics-dependent fields |
| sell_by_channel.json | POTENTIAL, SELL | Matches by company_name, not company_id. 68 orphan names | Silent failures possible: false NOT MONETIZED flags, missing channel rows |
| ga4_eshop.json | SELL | Keyed by hostname, only 29 records, no company_id join | CVR shown for max 29 accounts; hostname fuzzy matching is fragile |
| est_gmv.json | POTENTIAL, BUY | 28.1% coverage, 44 records with est_sell_gmv=0 | Penetration % and Est. Buy often null or use fallback (54% ratio) |
| vendor_detail.json | BUY | leakage_vendors at 0.9% | Leakage section renders correctly but data nearly empty |

### 6.2 Scope v2 fields not captured by ANY microagent

All fields explicitly listed in scope v2 have been traced by the microagents. The following scope v2 CONCEPTS are not represented as rendered fields in the current artifact:

- **Trend indicator (growing/flat/declining)** -- no portfolio column, no data
- **Approved opps (with approval workflow)** -- no approval concept exists
- **Best-in-class benchmarks per product type** -- no data, no rendering
- **Hardgoods segmentation** -- no data source
- **Standing Orders config** -- no data source
- **SELL unit economics model** -- no simulation/projection

### 6.3 Match key mismatches

| Source JSON | Current match key | Safer alternative | Risk |
|---|---|---|---|
| sell_by_channel.json | `company_name` via `normName()` | `company_id` (if added to query) | Names can differ between accounts.json and Snowflake SALES_SV. Silent failure to $0. |
| repeat_rate.json | `company_name` via `normName()` | `company_id` | Same risk as above |
| buyer_concentration.json | `company_name` via `normName()` | `company_id` | Same risk |
| supply_matrix_full.json | `company_name` via `normName()` | `company_id` | Same risk |
| buy_detail.json | `company_name` via `normName()` | `company_id` | Same risk |
| sell_detail.json | `company_name` via `normName()` | `company_id` | Same risk |
| listing_detail.json | `company_name` via `normName()` | `company_id` | Same risk |
| loop2_sell_format_time_v2.json | `company_name` (NO company_id in file) | Add company_id to query | No fallback possible today |
| ga4_eshop.json | `hostname` via substring fuzzy | Hostname-to-company mapping table | Can produce false positives with short/common substrings |
| sfdc.json | `account_name` fuzzy (first 15 chars) | SFDC account_id to company_id mapping | Fuzzy 15-char match can collide |

### 6.4 The findListingCats BUG

**Location:** `findListingCats()` function (approximately line 650 in index.html)
**Problem:** Function reads `LISTING_DETAIL.categories` but the JSON file (`listing_detail.json`) stores data under the key `varieties`.
**Effect:** The function always returns `null`, making lines 1498-1501 in the LIST card dead code. Categories detail from listing_detail is never rendered.
**Fix:** Change `LISTING_DETAIL.categories` to `LISTING_DETAIL.varieties` in the function, OR rename the key in the JSON generation query. Both are equivalent.

### 6.5 GA4 hostname matching fragility

**Location:** `findGA4()` function (approximately lines 655-664 in index.html)
**Problem:** Uses `normName()` to strip "shop", "komet", "eshop", "com" from hostname and account name, then does bidirectional substring matching on first 10 characters.
**Effect:** Short or common substrings can cross-match (e.g., `shop.smithfloral.com` reducing to `smithfloral` could match "Smith's Flowers" and "Smith Wholesale" simultaneously). No warning or confidence indicator is shown to the user.
**Impact:** 29 GA4 records, 15 with transactions. False positives would show incorrect CVR/revenue.

### 6.6 sell_by_channel company_name matching risk

**Location:** `normName()` matching in POTENTIAL card fee channel table and SELL card channel mix.
**Problem:** `sell_by_channel.json` internally has `account_id` (SFDC account ID, NOT Komet company_id) but matches to accounts via `company_name` normalization. If a company name in accounts.json differs from the name in the Snowflake SALES_SV view, the lookup silently returns no results.
**Effect:** Channel GMV defaults to 0, which can trigger false NOT MONETIZED flags (POTENTIAL card) or show empty channel mix (SELL card).

### 6.7 Anticipation data EXISTS but is stubbed

**Location:** BUY card, line 1360 shows hardcoded "needs data".
**Data source:** `buy_detail.json` -> `anticipation` section contains 50 records with fields: `orders_0_3d`, `orders_4_7d`, `orders_8_14d`, `orders_15_30d`, `orders_30plus_d`.
**Coverage:** 50 records, 45 matched to accounts.json (11.3%). Sparse (`orders_15_30d` non-null for only 10/50, `orders_30plus_d` for 4/50).
**Status:** Data is loaded but never wired to rendering. Requires builder logic to connect JSON data to the display placeholder.

---

## SECTION 7: BUGS AND RISKS

### Confirmed Bugs

| # | Bug | Location | Severity | Impact | Fix |
|---|---|---|---|---|---|
| BUG-1 | **findListingCats reads `.categories` but JSON key is `.varieties`** | `findListingCats()` ~line 650 | HIGH | LIST card lines 1498-1501 are dead code. Categories detail from listing_detail never renders. | Change `.categories` to `.varieties` in function, or rename JSON key. |
| BUG-2 | **Heirloom Home Decor has null company_id** | accounts.json record | MEDIUM | Creates "None" key in est_gmv.json. Missing from config.json. Cannot be matched by any company_id-based lookup. | Fix in accounts source data or add special-case handling. |

### Active Risks

| # | Risk | Location | Severity | Likelihood | Impact if triggered |
|---|---|---|---|---|---|
| RISK-1 | **GA4 hostname matching produces false positives** | `findGA4()` ~lines 655-664 | HIGH | MEDIUM | Incorrect CVR/revenue shown for wrong company. No warning to user. |
| RISK-2 | **sell_by_channel matches by company_name, not company_id** | normName matching in POTENTIAL + SELL | HIGH | MEDIUM | Silent $0 for channel GMV -> false NOT MONETIZED flags; missing channel rows. |
| RISK-3 | **8 JSON files match by company_name only** | sell_by_channel, repeat_rate, buyer_concentration, supply_matrix, buy_detail, sell_detail, listing_detail, loop2_sell_format_time | HIGH | LOW-MEDIUM | Any name mismatch between accounts.json and Snowflake views causes silent data loss. |
| RISK-4 | **Indirect fee rate hardcoded at 1.5%** | POTENTIAL card line 1037, BUY card lines 1282-1293 | MEDIUM | LOW | If actual rate changes, dashboard shows incorrect fees. No config/source for rate. |
| RISK-5 | **SFDC fuzzy name match uses first 15 chars** | `getSFDC()` | MEDIUM | LOW | Companies with similar first 15 characters could cross-match. |
| RISK-6 | **metrics.json at 28.1% coverage** | Data coverage | HIGH | CERTAIN | 287/399 accounts have null for all metrics-dependent fields (Koronet Sell, Penetr %, Online %, Fees, etc.). This is a data generation problem, not a rendering bug. |

### Mislabeled / Semantic Issues

| # | Issue | Location | Description |
|---|---|---|---|
| MIS-1 | **Open Market in LIST card** | LIST card lines 1505-1514 | Shows `ORDER_TYPE = 'Open Market'` from SALES_SV (sales data including auto-generated from Future Sales POs and SOs). Scope v2 defines Open Market as "what they manually UPLOAD." No upload-specific data source exists. |
| MIS-2 | **Order Mix in BUY card uses SELL-side data** | BUY card lines 1394-1404 | `buy_detail.json` -> `order_types` comes from `SALES_SV` (sell-side). Being used as proxy for buy-side order mix. Acknowledged in inline note. |
| MIS-3 | **No LIST opportunities generated** | CONFIG card, `getOpportunities()` | HTML renderer for `type:'list'` opps exists (lines 1244-1245) but the opportunity generator function never produces any `type:'list'` entries. Section always appears empty. |

### Missing Functionality

| # | Item | Description |
|---|---|---|
| MISS-1 | **No temporal/trending data** | All 10 metrics that scope v2 wants with trending (see Section 4) are single-snapshot today. |
| MISS-2 | **No approval workflow for opportunities** | Portfolio "# Opps" column counts all computed opps. Scope v2 wants "APPROVED only." No approval concept exists in data or logic. |
| MISS-3 | **No best-in-class benchmarks** | Scope v2 requests benchmarks per product type across CONFIG, LIST, and SELL cards. No benchmark data source exists. |
| MISS-4 | **No SELL unit economics model** | BUY card has "10% more online = $X proc = $Y fees." SELL card has no equivalent projection. |
| MISS-5 | **9 Challenger Messages exist but never rendered** | `definitions.json` -> `list_challenger[]` has 9 CS enablement messages. Not surfaced in LIST card. |
| MISS-6 | **Anticipation data loaded but not wired** | `buy_detail.json` anticipation section loaded; BUY card shows hardcoded "needs data." |

---

## CODEX VERIFICATION CORRECTIONS (applied 2026-07-31)

| # | Severity | Issue | Fix applied |
|---|---|---|---|
| 1 | HIGH | D3 estBuyVal fallback said "75% * est_sell" — actual code uses 54% | Corrected D3 formula to "54% * est_sell" |
| 2 | LOW | annual_sales_est coverage "158 (39.6%)" includes 35 zeros | Clarification: 158 non-null (39.6%), 123 non-zero (30.8%). Usable for penetration % = 123 accounts. |
| 3 | LOW | online_pct coverage "77 (of 112)" implies 77 have the field | Clarification: all 112 have the field, 77 are > 0. |
| 4 | LOW | Missing gap: New User CVR % | Added: scope v2 line 89 explicitly requests "New User CVR % — critical para growth". Not in GA4 data currently. Needs new GA4 query or segment. Resolver: Rose. |
| 5 | INFO | Existing `takeRate` variable (line 1038) uses wrong formula | Note: existing code defines `takeRate = fees_total / sell_online * 100`. Scope v2 defines Take Rate as `(fees_total) / (sell_total + proc_total) * 100`. Builder must NOT reuse existing variable — must create new formula. |

## END OF P0.0 INVENTORY

**Status:** CODEX VERIFIED — ready for P0.1
**Next step:** P0.1 structural shell build (in progress), P0.2 temporal data design (in progress)
