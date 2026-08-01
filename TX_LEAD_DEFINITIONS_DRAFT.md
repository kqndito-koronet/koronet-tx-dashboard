# TX Lead Definitions -- DRAFT

**Date:** 2026-07-31
**Author:** Mercurio (GTM Growth Agent)
**Status:** ALL LEADS ARE DRAFT -- awaiting Facu review
**Source:** TX_DASHBOARD_SCOPE_V2.md (4-layer structure), TX_DASHBOARD_P0_NO_LOSS_INVENTORY.md (Section 3 gaps), definitions.json, Jul 31 call insights
**Master thesis:** Wholesalers win when they offer everything their florists want, confirm fast, and deliver quality. This drives more buyers, more wallet share, and Koronet captures fees as a byproduct of their success.

---

## How to Read This Document

Each lead follows the 4-layer structure from scope v2:

- **Layer 1: Define + Reasoning** -- what is the opportunity and WHY
- **Layer 2: Lead List Concept** -- how Rose detects which accounts match
- **Layer 3: Enablement Sketch** -- what the rep says/checks/handles
- **Layer 4: Case Study Pointer** -- proof it works (or NEEDS CASE STUDY)

Trust labels: **DRAFT** (proposed, do not act) or **APPROVED** (Facu reviewed, team can act).

---

# TAB 2: CONFIG

Config leads are about removing barriers that prevent the wholesaler from selling online at all. These are GATE issues -- without fixing them, nothing else matters.

---

## CONFIG-01: MaxAge Too Low for Profile

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** MaxAge Too Low for Profile
- **Thesis:** If a wholesaler's eShop only shows inventory arriving in the next 5-10 days, florists who plan ahead (weddings, events, weekly standing orders) see an empty store. MaxAge=10 blocks 47% of Kennicott's sales volume. Increasing MaxAge means florists find what they need for longer-horizon buying, so they buy more from this wholesaler instead of going to Mayesh or calling.
- **Detection logic:** `config.json -> max_age <= 10` AND `ct_id IN (WH_CORE, WH_ESUITE)`. Exclude WH_K2K (their MaxAge is vendor-controlled) and WH_PROC (no eShop).
- **Product types:** WH_CORE (primary), WH_ESUITE (secondary). Does NOT apply to WH_K2K, WH_PROC, IMP_CORE.
- **Prerequisite:** Account has eShop enabled (ecommerceEnabled=ON). If eShop is OFF, CONFIG-05 takes priority.
- **Expected impact:** More inventory visible on eShop = more categories and time depth for florist buyers = higher conversion rate. Kennicott at MaxAge 300+ achieves 22.8% CVR. Accounts at MaxAge 10 are functionally spot-only shops.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From config.json, select accounts where `max_age <= 10` AND `ct_id IN ('WH_CORE', 'WH_ESUITE')` AND eShop is enabled.
- **Threshold:** MaxAge <= 10 = lead. MaxAge 11-29 = potential improvement opportunity. MaxAge 30+ = acceptable (not a lead).
- **Severity split:** MaxAge = 0 or null = GATE (blocking, red). MaxAge 1-10 = LIMITING (amber). This maps to the existing bottleneck waterfall (Priority 1 in the inventory doc).
- **Coverage:** config.json has 397/399 coverage, so this lead list is near-complete.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Your eShop only shows inventory that arrives in the next [X] days. That means florists planning their weekly orders or event work don't see your products -- they look somewhere else. We can change one setting and immediately show everything arriving in the next 30-90 days. This is a 5-minute fix."
- **What the rep should CHECK before the conversation:**
  - Current MaxAge value (from config.json)
  - How the wholesaler buys: if they prebook 14-21 days ahead, MaxAge should be at least 30
  - Whether sold_as_future is ON (for Core accounts -- see CONFIG-06 below)
  - Whether time depth distribution (from list_time_depth) shows truncation at the MaxAge boundary
- **Key objection:** "I don't want to show stale inventory or products I might not have."
  - **Response:** "MaxAge controls visibility, not commitment. Your inventory still shows real availability. If a product sells out, it disappears. Maple Grove uses MaxAge 300 and has 76% repeat rate -- their florists trust the catalog because it's deep, not because it's narrow."

### Layer 4: Case Study Pointer

- **Proven:** Maple Grove -- MaxAge 300 days, 56 buyers, 76% repeat rate, 22.8% conversion, $3,609 fees H1 (+33% YoY). Their deep catalog is why florists come back.
- **Counter-example:** Price's Floral -- MaxAge was 5, limiting catalog to spot-only. Identified Jul 21. Fix = 5 minutes.
- **Benchmark:** Kennicott -- MaxAge 300+, 47% of their sales volume is beyond 10 days. MaxAge=10 would block nearly half their online business.

---

## CONFIG-02: Bunches Flag OFF for eShop-Dependent Accounts

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Bunches Flag OFF
- **Thesis:** 95-98% of retail florist purchases are bunches, not full boxes. If bunches are OFF on the eShop, the wholesaler is only addressable to 2-5% of the retail TAM. Their florists literally cannot buy what they need in the quantities they need. Turning bunches ON immediately makes the eShop relevant to their actual customer base.
- **Detection logic:** `config.json -> bunches_flag = false` AND `ct_id IN (WH_CORE, WH_ESUITE)`. Cross-reference with `bunches.json -> sells_bunches = true` to detect discrepancy (account sells bunches offline but flag is OFF online).
- **Product types:** WH_CORE, WH_ESUITE. Does NOT apply to WH_K2K (vendor-controlled), WH_PROC (no eShop), IMP_CORE (sell boxes to wholesalers).
- **Prerequisite:** eShop enabled. If no eShop, CONFIG-05 first.
- **Expected impact:** TAM expansion from 2-5% of retail to ~100%. Florists who previously couldn't buy (wrong unit size) can now order. This is one of the highest-leverage config changes.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From config.json, `bunches_flag = false` AND `ct_id IN ('WH_CORE', 'WH_ESUITE')`. Enhanced version: cross-join with bunches.json where `sells_bunches = true AND bunches_flag = false` (discrepancy = selling bunches offline but not online = active revenue lost).
- **Threshold:** Binary -- flag is ON or OFF. If OFF and account sells bunches offline, it's a lead.
- **Priority boost:** If `bunches.json -> total_bunch_gmv > 0` AND `bunches_flag = false`, this account is ACTIVELY selling bunches offline while blocking them online. Higher priority.
- **Coverage:** config.json 397/399, bunches.json 43/399 (only accounts with bunch sales data).

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Right now your eShop only shows full boxes. But [X]% of your sales are bunches -- that's how florists actually buy. We can turn on bunches so your existing customers can order online the same way they order by phone. This is the single biggest thing blocking your online TAM."
- **Reference (Challenger Message #4):** "Si solo vendes boxes online, el 95% de tu mercado no te puede comprar."
- **What the rep should CHECK:**
  - Current bunch GMV from bunches.json (quantify what's sold offline as bunches)
  - Whether the account is WH_CORE (can open boxes -> units) or WH_ESUITE (upload bunches via XLS)
  - Whether "Sell by Units" company-level toggle is ON (foundation setting)
  - Whether "eShop bunches tab" add-on is purchased (paid feature)
- **Key objection:** "Bunches are too much work to manage online."
  - **Response:** "For Core accounts, when you open a box into units, those bunches appear automatically on the eShop -- no extra work. For eSuite, you upload once via XLS and it's live. The 43 accounts already selling bunches online don't report extra operational burden."

### Layer 4: Case Study Pointer

- **Proven:** Zeidler -- 100% bunches on eShop, MaxAge 365, uses "open inventory" workaround for live cooler sync. Only wholesaler doing full cooler-to-eShop sync.
- **Data point:** 43 accounts currently sell bunches. 92% of offline sales are bunches (from Jul 21 Price's call analysis). The gap is massive.
- **Benchmark from definitions.json:** "95-98% of retail buys bunches. Without bunches = 2-5% TAM."

---

## CONFIG-03: hideCheckoutWithoutPayment Misconfigured

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Checkout Payment Gate Misconfigured
- **Thesis:** This setting silently blocks checkout if buyers don't have a payment method on file. When misconfigured, the wholesaler has a perfect eShop that generates $0 revenue -- buyers browse, add to cart, and hit a wall at checkout. No error, no warning, just abandonment. The wholesaler doesn't even know they're losing sales. Fixing this removes a hidden wall between "interested florist" and "paying customer."
- **Detection logic:** `config.json -> hide_checkout_without_payment` status, cross-referenced with buyer payment method enrollment. Hard to detect from config alone -- need behavioral signal: accounts with eShop traffic (GA4 sessions > 0) but $0 online sales AND hideCheckoutWithoutPayment=ON.
- **Product types:** WH_CORE, WH_ESUITE, WH_K2K. Does NOT apply to WH_PROC (no eShop).
- **Prerequisite:** eShop enabled with inventory visible (MaxAge > 0, some products listed).
- **Expected impact:** Unblocks conversion immediately. Price's Floral had all config correct EXCEPT this -- $0 revenue because buyers had no payment method on file.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** Two detection methods:
  1. **Direct config check:** `hide_checkout_without_payment = ON` for WH_CORE/WH_ESUITE accounts. Not all ON cases are problems -- the issue is when ON AND buyers lack payment methods.
  2. **Behavioral detection (stronger):** Accounts with GA4 sessions > 50 AND online_sell = $0 AND eShop enabled AND inventory visible (MaxAge > 0). This catches the symptom regardless of the specific cause.
- **Threshold:** Behavioral detection is binary -- traffic + zero conversion = investigate. Config-only detection requires manual verification.
- **Coverage:** config.json 397/399 for the setting. GA4 has only 29 records (limits behavioral detection).

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "We noticed your eShop has [X] visits but zero orders. There's a setting that blocks checkout if the buyer doesn't have a payment method saved. Let's check if that's what's happening and fix it -- it's a 5-minute change."
- **What the rep should CHECK:**
  - hideCheckoutWithoutPayment current value
  - How many buyers have payment methods on file (needs buyer-level data)
  - GA4 session count and conversion funnel (if available)
  - Whether the account is post-go-live and expected to have orders by now
- **Key objection:** "I want buyers to have payment on file before ordering."
  - **Response:** "That's understandable for credit risk. But right now buyers are trying to buy and can't. Two options: (1) help your top buyers add payment methods -- we can identify who's trying and failing, or (2) allow checkout and collect payment afterward, which is how phone orders work today."

### Layer 4: Case Study Pointer

- **Proven:** Price's Floral -- all config correct, $0 revenue because hideCheckoutWithoutPayment blocked buyers without payment methods. Identified Jul 21. Fix = toggle + help buyers add payment methods.
- **Pattern:** Tennessee and Floropolis have the same pattern (Jul 31 call insights). This is a recurring issue across at least 3 accounts.

---

## CONFIG-04: eSuite Account That Should Be Core

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** eSuite-to-Core Upgrade Candidate
- **Thesis:** eSuite accounts are limited to ~5% of Core's addressable market (boxes only, no Standing Orders, no Future Sales, no opening boxes to bunches natively). An eSuite account generating significant volume is being artificially constrained by its product tier. Upgrading to Core unlocks Standing Orders, Future Sales, and native bunch handling -- fundamentally changing what their eShop can offer to florists.
- **Detection logic:** `ct_id = WH_ESUITE` AND one or more of: (a) sell_total > $500K/year (significant volume constrained), (b) sells_bunches = true (needs native bunch support), (c) has active K2K connections AND procurement volume (using the platform seriously).
- **Product types:** Only WH_ESUITE accounts. This IS the lead -- they should potentially be WH_CORE.
- **Prerequisite:** Account must be using the platform actively. An eSuite account at $0 activity doesn't need an upgrade; it needs activation first.
- **Expected impact:** Access to Standing Orders (~$1.2M TAM currently blocked for eSuite), Future Sales (forward inventory), native bunch handling. The eSuite-to-Core upgrade is a revenue multiplier because it unlocks capabilities that directly increase what florists can buy.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From accounts.json, `ct_id = 'WH_ESUITE'`. Cross-reference with metrics.json for `sell_total` and `proc_total`. Flag accounts where `sell_total > $500K` OR `proc_total > $200K` OR `k2k_total > 5` (significant platform engagement).
- **Threshold:** Volume-based: high enough GMV to justify Core licensing cost. Engagement-based: active K2K, active procurement, active eShop sales.
- **Known eSuite count:** 5 accounts (from definitions.json). Small pool, each one matters.
- **Coverage:** accounts.json 100%, metrics.json 28.1% (may miss eSuite accounts without metrics data).

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You're doing really well with eSuite -- [$X] in sales, [Y] K2K connections. But your current setup limits what you can offer online. With Core, you'd unlock Standing Orders (your regulars get automatic weekly deliveries), Future Sales (show what's arriving in 2-3 weeks), and native bunch handling. The accounts that do this best are running 30%+ online sales."
- **What the rep should CHECK:**
  - Current sales volume and trajectory (growing = stronger case)
  - Whether they already use a separate ERP (Core would REPLACE their ERP, not layer on top)
  - What specific capabilities they're missing that Core would unlock
  - Whether Standing Orders would apply (do they have recurring buyers?)
- **Key objection:** "We already have an ERP, Core would mean switching."
  - **Response:** "That's the biggest consideration. Core IS the ERP -- it's not an add-on. For some accounts, eSuite + their existing ERP is the right fit. But if your ERP is limiting what you can do online (no Standing Orders, no Future Sales, manual everything), Core removes those limits. Let's map which capabilities you're actually missing."

### Layer 4: Case Study Pointer

- **Reference:** Shamrock -- Jul 22 was a Core demo, exploring eSuite-to-Core upgrade. Decision pending from Meredith.
- **Context from Jul 31 calls:** eSuite addressable market = ~5% of Core's (only boxes). 79 eSuite accounts enabled, only 6 sold in 30 days. The limitation is structural, not behavioral.
- **NEEDS CASE STUDY** -- no proven eSuite-to-Core upgrade with measured before/after impact yet.

---

## CONFIG-05: No eShop Enabled for WH_CORE / WH_ESUITE

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** No eShop Enabled
- **Thesis:** Without an eShop, the wholesaler has zero online sales channel. Their florists cannot browse inventory, compare products, or place orders digitally. Every interaction is phone/walk-in. Enabling the eShop is the absolute minimum gate -- everything in LIST, SELL, and GROWTH depends on it existing. This is not an optimization; it's a prerequisite for the entire digital motion.
- **Detection logic:** `accounts.json -> has_eshops = false` AND `ct_id IN (WH_CORE, WH_ESUITE)`. These account types CAN have eShops but don't.
- **Product types:** WH_CORE, WH_ESUITE. Does NOT apply to WH_K2K (eShop depends on vendor inventory setup), WH_PROC (buy-only, no sell channel), IMP_CORE (different eShop model for importers).
- **Prerequisite:** None. This IS the first prerequisite for everything else.
- **Expected impact:** Creates the possibility of online sales. Without this = $0 online revenue, permanently.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From accounts.json, `has_eshops = false` AND `ct_id IN ('WH_CORE', 'WH_ESUITE')`.
- **Threshold:** Binary -- eShop exists or doesn't.
- **Context from P0.0 inventory:** Bottleneck waterfall Priority 2: "GATE: No eShop" (red). 149 accounts in the scorecard are NO_ESHOP.
- **Coverage:** accounts.json 100%.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You have Core/eSuite with inventory management, K2K connections, and procurement -- but no eShop. That means your existing customers can't see your inventory online or order digitally. Enabling the eShop is the foundation for everything we do together -- it's what lets your florists buy from you 24/7 without a phone call."
- **What the rep should CHECK:**
  - Why is the eShop not enabled? Implementation incomplete? Client declined? Technical blocker?
  - Is this account in Christine's implementation pipeline? (Check christine.json)
  - Does the account have an external website that COULD link to the eShop?
  - What's the account's priority level? (IMPL, P1, CS_P2, etc.)
- **Key objection:** "My customers prefer calling."
  - **Response (Challenger Message #6):** "You don't know how many sales you're losing. Florists who search your website at 9 PM or compare availability on their phone -- they can't find you. They go to Mayesh or call another wholesaler. The eShop doesn't replace phone sales; it captures the ones you're missing."

### Layer 4: Case Study Pointer

- **Context:** 149 accounts lack eShops. This is the largest single blocker in the portfolio (from Jul 25 scorecard interpretation: LIST = 206/206 RED, of which 149 are NO_ESHOP).
- **Implementation reference:** Christine's IMPL wrap from Jul 31 -- 14 post-go-live accounts, several at 0TX after 30-86 days. The eShop existing is necessary but not sufficient.
- **NEEDS CASE STUDY** -- need a before/after example of an account that enabled eShop and achieved measurable results within 90 days.

---

# TAB 3: BUY

BUY leads focus on the supply side -- helping wholesalers buy better through the platform. Procurement is the anchor (zero churn), and every improvement here strengthens the foundation for LIST and SELL.

---

## BUY-01: Low Vendor Coverage at Medium/Long Term

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Vendor Coverage Gap by Time Horizon
- **Thesis:** Wholesalers buy at different time horizons -- spot (today/tomorrow), short (this week), medium (2-4 weeks), and long (seasonal, 30-90+ days). If a wholesaler's K2K vendor network only covers spot buying, they're forced offline for everything else -- prebooks, seasonal planning, Valentine's/Mother's Day prep. Broadening vendor coverage by time horizon means the wholesaler can plan and buy digitally for ALL their needs, not just last-minute fills.
- **Detection logic:** From vendor_detail.json + buy_detail.json anticipation data, identify accounts where: (a) they have K2K connections but most purchases are orders_0_3d (spot-only), (b) they buy at longer horizons offline but not online, (c) their connected vendors don't offer forward availability.
- **Product types:** WH_CORE (primary -- can do prebooks, Future Sales, Standing Orders), WH_ESUITE (K2K buying), WH_K2K (fully dependent on vendor availability). Does NOT apply to WH_PROC (procurement-only, different dynamic) or IMP_CORE (they SELL, not buy).
- **Prerequisite:** Account must have at least 1 active K2K connection. If zero connections, BUY-02 (connection activation) is the prerequisite.
- **Expected impact:** More online procurement volume = more indirect fees. But more importantly: the wholesaler can plan supply better, negotiate better prices (advance buying = better pricing from vendors), and offer forward availability to THEIR customers (which feeds LIST and SELL).
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From buy_detail.json anticipation section: compute `orders_0_3d / total_orders` ratio per account. If > 80% of purchases are 0-3 day, the account is spot-heavy. Cross-reference with offline buying patterns (offline purchases at medium/long term where online purchases are spot-only = leakage to time horizon gap).
- **Threshold:** >80% spot (0-3d) purchases AND total procurement > $50K = lead. The signal is concentration in short-term buying when the account's business requires longer-term planning.
- **Data limitation:** Anticipation data in buy_detail.json is sparse (50 records, 11.3% coverage). `orders_15_30d` non-null for only 10/50, `orders_30plus_d` for 4/50. Lead list will be small until data coverage improves.
- **Reference (Buy Discovery Question #5):** "Planificas compras a futuro o compras para manana?"
- **Reference (Buy Challenger):** "Sin compra anticipada, pagas precios spot. Vendors con 14-30d de visibilidad dan mejor precio porque pueden planificar."

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Right now, [X]% of your online purchases are for delivery within 3 days. But your business needs flowers for next week, events in 2-3 weeks, and Valentine's planning 90 days out. Your current vendor connections don't cover those horizons online. We can connect you with vendors who offer forward availability so you can plan digitally -- and get better pricing because vendors reward advance commitment."
- **What the rep should CHECK:**
  - Anticipation distribution (orders by time horizon from buy_detail.json)
  - Which categories they buy at longer horizons offline (from supply_matrix_full)
  - Whether connected vendors offer prebook/forward availability
  - Seasonal buying patterns (Valentine's, Mother's Day -- when do they start ordering?)
- **Key objection:** "I get better prices by calling my vendors directly for advance orders."
  - **Response (from definitions.json objection_procurement_expensive):** "That might be true for some vendors. Let's do a side-by-side comparison -- not 2 samples, but systematic: same product, same vendor, same period. If procurement is more expensive, we'll escalate with data. If it's comparable or cheaper, you save time ordering digitally."

### Layer 4: Case Study Pointer

- **Benchmark:** Buy Challenger #4 -- "Valentine's = 90 dias de anticipacion. Mother's = 60 dias. Sin compra anticipada = spot a 2-3x."
- **NEEDS CASE STUDY** -- no proven case of a wholesaler expanding from spot-only to multi-horizon online buying with measured price/efficiency improvement. Data on anticipation is too sparse to identify current examples.

---

## BUY-02: K2K Connections Created but Not Activated

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Dormant K2K Connections
- **Thesis:** K2K connections are the infrastructure for digital buying. But having a connection without transacting is like having a phone line with nobody answering. The connection exists, the vendor's inventory is available, but the wholesaler isn't buying through it. This is a behavioral problem, not an infrastructure problem -- the road is built but nobody's driving on it. Activating dormant connections converts infrastructure investment into actual purchasing volume.
- **Detection logic:** `metrics.json -> k2k_total > 0 AND k2k_active / k2k_total < 0.7` (existing opportunity O7 in the dashboard). Enhanced: `vendor_detail.json -> dormant_vendors > 0` (connected but never bought).
- **Product types:** All accounts with K2K connections -- WH_CORE, WH_ESUITE, WH_K2K, and WH_PROC (buy side).
- **Prerequisite:** Account has at least 1 K2K connection (k2k_total > 0).
- **Expected impact:** Each activated connection = new online procurement channel = indirect fees for Koronet. But for the wholesaler: more online sourcing options = better pricing through competition, less dependency on phone calls, more visibility into vendor availability.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From metrics.json: `k2k_active / k2k_total < 0.70` AND `k2k_total > 0`. From vendor_detail.json: `dormant_vendors > 0`. Combine: accounts with significant dormant connections (> 3 dormant vendors OR > 50% inactive).
- **Threshold:** < 70% activation rate = lead. Severity scales with dormant count: 1-3 dormant = monitor, 4+ dormant = active lead.
- **Cross-reference with buy_funnel stages from definitions.json:** Stage CONEXIONES -> "Conexion activa -> 30D sin compra."
- **Coverage:** metrics.json k2k data = 90/399 accounts. vendor_detail.json dormant = 62/336 records.
- **Historical context from hypotheses.md:** 20,904 active connections but only 34% ever transacted online. 9,699 pairs stopped online but still buy offline ($109.5M at stake).

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You have [X] vendor connections set up, but only [Y] are actively used for purchasing. That means [Z] vendors have their inventory available for you online, but you're still ordering from them by phone. Each one you activate gives you price visibility, order tracking, and comparison ability without making extra calls."
- **Reference (Buy Discovery Question #1):** "Que compras, a quien, cuanto, cuando?"
- **What the rep should CHECK:**
  - Which specific vendors are dormant (vendor_detail.json)
  - Whether those vendors' inventory appears in the wholesaler's procurement view
  - Whether the wholesaler is buying from those same vendors offline (= leakage)
  - Whether Easy Handshake connections are in the dormant pool (EH had 2.1% activation -- specific intervention needed)
- **Key objection:** "I already buy from those vendors by phone. It's faster."
  - **Response (Buy Challenger #3):** "Your top 3 vendors by volume are [X, Y, Z]. If they have K2K, you can compare their prices in 2 minutes instead of making 3 calls. And you get a record of every order, every price, every delivery -- no more chasing confirmations."

### Layer 4: Case Study Pointer

- **Data point:** 22 CONNECTIONS_NO_BUY accounts with $52M+ offline procurement through connected vendors (from Jul 25 scorecard).
- **Pattern from definitions.json:** `buy_leads -> SUPPLY_EH_NO_PURCHASE` -- "Se conecto por Easy Handshake, nunca compro. Window shopping o blocker?"
- **NEEDS CASE STUDY** -- no documented before/after of a dormant connection activation campaign with measured procurement shift. Sofia's connection dashboard is a CS discovery tool for this (live, track adoption + findings per state.md).

---

## BUY-03: Procurement Pricing Trust Broken (Price Parity)

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Procurement Price Parity Gap
- **Thesis:** If wholesalers believe procurement is more expensive than calling the vendor, they will never shift volume online. This is a TRUST problem, not a feature problem. The objection is RECURRING and NOT closed (Bill Doran, WGF raised it). Without price parity, procurement adoption stalls because the wholesaler rationally chooses the cheaper channel. But the root cause is often incomplete: landed cost components differ (delivery, volume discounts, promotional pricing) rather than actual product cost.
- **Detection logic:** No direct data signal in current JSONs. Behavioral proxy: accounts with `k2k_active > 5` AND `proc_online / proc_total < 0.3` AND `proc_total > $100K` (they're buying a lot through Koronet but mostly offline = possible price friction). Need: explicit price parity audit data per account.
- **Product types:** WH_CORE, WH_ESUITE, WH_PROC -- any account that buys through procurement.
- **Prerequisite:** Account has active procurement (proc_total > 0) and active K2K connections. If no connections, BUY-02 first.
- **Expected impact:** If price parity is resolved (proven comparable or better), procurement adoption accelerates because the barrier to shifting volume is removed. If procurement IS more expensive, this is a product/vendor issue to escalate.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** Proxy detection -- accounts with `proc_total > $100K` AND `proc_online / proc_total < 0.3` AND `k2k_active > 3`. These are accounts buying significantly through Koronet infrastructure but keeping most of it offline -- possible price friction signal.
- **Threshold:** <30% online procurement when connections exist = investigate. Combined with the account having raised pricing as an objection (from Fathom/Intercom records -- qualitative signal).
- **Ideal data (not available today):** Side-by-side price comparison: same product, same vendor, procurement price vs offline price, same period. This data does not exist systematically. Each audit is manual.
- **Reference (definitions.json):** `objection_procurement_expensive -> root_causes`: volume discounts not available, landed cost calculation differences, delivery cost differences.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "We hear from some wholesalers that procurement prices feel higher than calling vendors directly. Let's investigate for YOUR account specifically -- not with 2 examples, but systematically. We'll compare procurement prices vs offline for the same product, same vendor, same period. If there's a real gap, we escalate it. If the prices are actually comparable, you'll have the data to feel confident buying online."
- **What the rep should CHECK:**
  - Has the client explicitly raised pricing concerns? (Fathom, Intercom, Slack)
  - What % of procurement is online vs offline? (Low online % after months of connections = possible friction)
  - Which specific vendors are they buying offline despite having K2K? (vendor_detail.json leakage data)
  - Are volume discounts, delivery costs, or promotional pricing part of their offline deals?
- **Key objection:** "I already told you procurement is more expensive."
  - **Response:** "We take that seriously. Magali's 2-vendor sample wasn't enough. Let's do a real audit -- your top 5 vendors, last 30 days, every line item online vs offline. If procurement IS more expensive, I'll escalate with data to get it fixed. I'm not here to argue -- I'm here to make sure buying online is at least as good as calling."

### Layer 4: Case Study Pointer

- **Known cases:** Bill Doran (raised objection), WGF (raised objection). Neither has been resolved with a systematic price audit.
- **Committed action:** Price audit committed to Mauro but not executed (from Mercurio feedback.md, Jul 24).
- **NEEDS CASE STUDY** -- no documented case where a price audit was completed, findings shared with the client, and procurement adoption measurably improved.

---

## BUY-04: High Offline Procurement Leakage

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Offline Procurement Leakage (Vendors WITH K2K)
- **Thesis:** LEAKAGE = buying OFFLINE from a vendor with an active K2K connection. The infrastructure exists. The road is paved. But the wholesaler picks up the phone instead of using the platform. This is purely behavioral -- addressable right now. Every dollar of leakage is a dollar that COULD be generating indirect fees AND giving the wholesaler better visibility, tracking, and comparison. Fixing leakage is the highest-ROI BUY play because the infrastructure already works.
- **Detection logic:** `vendor_detail.json -> leakage_vendors > 0 AND leakage_cost > 0`. This is distinct from GAP (buying offline from vendors WITHOUT K2K -- infrastructure problem requiring vendor onboarding).
- **Product types:** All accounts with K2K connections -- WH_CORE, WH_ESUITE, WH_K2K.
- **Prerequisite:** Active K2K connections with transacted history. If connections are dormant, BUY-02 first.
- **Expected impact:** Direct conversion of offline spend to online spend with zero infrastructure change. Each converted dollar = indirect fees for Koronet + better tracking for the wholesaler.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From vendor_detail.json: `leakage_vendors > 0` AND `leakage_cost > $10K` (significant leakage worth addressing).
- **Threshold:** Leakage cost > $10K/period = active lead. Leakage cost > $50K = high priority.
- **CRITICAL DATA GAP:** Leakage data is at 0.9% coverage (3/336 records). The lead list is nearly empty NOT because leakage doesn't exist, but because the data hasn't been generated. Rose needs to expand leakage detection queries.
- **Reference (definitions.json):** `leakage_vs_gap -> leakage`: "Compra OFFLINE de vendor con K2K activo = behavioral problem. The infrastructure exists, they're not using it."

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "We see you're buying from [Vendor X] by phone, but you're already connected to them on the platform. That means you can compare their prices, place orders online, and track deliveries -- all without changing vendors. You're just using a different door to the same store."
- **Reference (Buy Challenger #1):** "Estas gastando $[X]/mes comprando offline de vendors que ESTAN en la red. Eso es leakage -- el producto existe online pero llamas por telefono."
- **What the rep should CHECK:**
  - Which specific vendors have leakage (vendor_detail.json)
  - Dollar amount of leakage per vendor
  - Whether the leakage is for specific categories (seasonal? specialty?) or across the board
  - Whether the wholesaler has Standing Orders with these vendors offline (Core limitation -- SOs are 100% offline)
- **Key objection:** "I have a relationship with my vendor rep, I prefer calling."
  - **Response:** "Your relationship doesn't change. The vendor is the same person. But when you order through the platform, you get price history, delivery confirmation, and comparison across all your vendors in one view. You can still call them for the relationship -- but the order goes through Koronet so everything is tracked and visible."

### Layer 4: Case Study Pointer

- **Data reference:** $52M+ offline procurement through connected vendors across 22 CONNECTIONS_NO_BUY accounts (from Jul 25 scorecard). Exact leakage within those is unknown due to 0.9% data coverage.
- **From definitions.json buy_leads:** `SUPPLY_OFFLINE_CONNECTED` -- "Tiene K2K pero compra offline A ESE MISMO vendor. Discovery: precio? SO offline? servicio?"
- **NEEDS CASE STUDY** -- need a documented leakage-to-online conversion with measured dollar shift and wholesaler feedback. Data gap must be closed first.

---

# TAB 4: LIST

LIST leads are about getting inventory visible online. The Rubik's Cube framework: VARIETIES x TIME x FORMAT. Each dimension that's not online = volume invisible to buyers.

---

## LIST-01: eSuite Enabled, No Sales L30D

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** eSuite Enabled but Empty/Inactive
- **Thesis:** 79 eSuite accounts are enabled, but only 6 sold in the last 30 days. That's a 7.6% activation rate. The eSuite exists but the shelves are empty -- no inventory is being published, so there's nothing for florists to buy. This is an inventory problem, not a demand problem. The store is open but there's nothing on display. Getting these accounts to upload inventory is the difference between a dead eShop and an active one.
- **Detection logic:** `ct_id = 'WH_ESUITE'` AND `is_esuite = true` AND (sell_online_l30d = 0 OR sell_online_l30d = null). Cross-reference with listing_detail.json and loop2_list_usage_v2.json to confirm whether they have any listings at all.
- **Product types:** WH_ESUITE only. This lead IS about the eSuite product type's specific inventory problem.
- **Prerequisite:** eShop enabled. If not, CONFIG-05 first.
- **Expected impact:** Each eSuite account that starts listing and selling online = new fee-bearing activity. But the opportunity is capped at ~5% of Core's TAM (eSuite = boxes only, no Standing Orders, no Future Sales -- Jul 31 call insight).
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From accounts.json: `ct_id = 'WH_ESUITE'` OR `is_esuite = true`. Cross with sell_detail.json or metrics.json: `sell_online = 0 OR null` in L30D.
- **Threshold:** Zero online sales in L30D with eSuite enabled = lead. Any online sales, even small, means the account has crossed the activation barrier (different lead).
- **eSuite count:** 5 accounts per definitions.json (WH_ESUITE type). But 79 eSuite-enabled accounts per Jul 31 call context -- many may be WH_CORE with eSuite features.
- **Coverage:** accounts.json 100%, sell data varies (sell_detail at 45-57%).

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Your eSuite storefront is set up and live, but it's showing zero inventory. Your florists visiting the eShop see an empty store. We need to get your products uploaded -- even a first batch via XLS would make the difference. What inventory do you have right now that we could start with?"
- **Reference (Challenger Message #1):** "Lo que no esta en tu eShop no existe para tus clientes online."
- **What the rep should CHECK:**
  - Does the account have inventory in their other ERP? (eSuite = digital layer + another ERP)
  - Have they been trained on XLS upload? (definitions.json: eSuite listing paths = manual upload, K2K vendor availability)
  - Do they have K2K connections with Vendor Availability ON? (could populate inventory via vendor)
  - What blocked them after go-live? (Technical? Don't see value? Don't know how?)
- **Key objection:** "It's too much work to upload inventory manually."
  - **Response:** "Start small. Your top 10 products -- the ones florists ask for most. Upload takes 15 minutes with our XLS template. Once those are live, you'll see whether florists engage. If they do, we can talk about API integration with your ERP for automatic sync."

### Layer 4: Case Study Pointer

- **Data point from Jul 31:** 79 eSuite enabled, 6 sold in 30 days. 73 accounts with enabled eSuite doing nothing.
- **eSuite limitations from Jul 31 calls:** eSuite addressable market = ~5% of Core's. Only boxes. No Standing Orders (decision made Jul 31 -- not coming to eSuite). This is the ceiling.
- **NEEDS CASE STUDY** -- need a documented eSuite activation where inventory upload led to first sales within 30 days, with specific steps taken.

---

## LIST-02: Time Depth Online << Offline

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Time Depth Gap (Online vs Offline)
- **Thesis:** If a wholesaler's cooler has inventory arriving in the next 30-90 days but their eShop only shows 10 days ahead, florists see a fraction of what's available. The eShop becomes a "today only" shop while the phone gets the "what's coming next week" orders. This is the DEPTH face of the Rubik's Cube (Varieties x TIME x Format). Matching online time depth to offline capability means florists can plan ahead digitally, which drives repeat behavior and larger orders.
- **Detection logic:** Compare `config.json -> max_age` with actual time depth distribution from `loop2_list_time_depth_v2.json`. If the time depth distribution shows >80% concentrated in 0-3 days when MaxAge should allow longer = the wholesaler isn't publishing forward inventory. If MaxAge itself is low = CONFIG-01 problem.
- **Product types:** WH_CORE (full control -- can do Future Sales, Standing Orders, Open Market), WH_ESUITE (upload only, limited forward), WH_K2K (vendor-dependent). Does NOT apply to WH_PROC.
- **Prerequisite:** MaxAge > 10 (if MaxAge is the constraint, CONFIG-01 is the lead). This lead is for when MaxAge is adequate but the wholesaler still doesn't publish forward.
- **Expected impact:** More forward inventory visible = florists can plan events, weekly orders, seasonal needs digitally. Maple Grove shows 300+ days of time depth and achieves 76% repeat rate. Time depth drives repeat behavior because florists learn they can rely on the eShop for planning.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From loop2_list_time_depth_v2.json: compute `pct_0_3_days + pct_4_7_days` (short-term concentration). If > 70% of listed inventory is 0-7 days AND `config.json -> max_age > 14` (MaxAge isn't the constraint) = time depth gap caused by listing behavior, not config.
- **Threshold:** >70% short-term concentration with adequate MaxAge = lead. Compare with Kennicott benchmark (significant volume beyond 30 days).
- **CRITICAL GAP from P0.0:** Only ONLINE time depth exists (99/399 coverage). Scope v2 requires 3 versions: online, offline, best-in-class. Offline and best-in-class versions need Rose queries. Lead detection today is limited to online-only.
- **Coverage:** loop2_list_time_depth_v2.json = 99/399 (24.8%).

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Your eShop shows products arriving in the next [X] days. But we know you buy inventory [Y] weeks ahead. That means your florists planning an event next month can't see your availability -- they call you or go to a competitor. If you publish what's coming, florists can plan with you digitally."
- **Reference (Challenger Message #3):** "Si solo compras para manana, tu eShop solo tiene lo de hoy."
- **What the rep should CHECK:**
  - MaxAge value (config.json) -- is the config adequate?
  - Time depth distribution (loop2_list_time_depth_v2.json) -- where is inventory concentrated?
  - Whether sold_as_future is ON (Core only -- CONFIG-06 check)
  - Whether the wholesaler does prebook POs (buy_detail.json order_types -> prebook_gmv > 0)
  - Whether Future Sales is enabled (config.json -> future = ON, Core only)
- **Key objection:** "I don't know what I'll have in 2 weeks."
  - **Response:** "You do -- your purchase orders for next week and the week after are already in the system. With sold_as_future turned ON, those POs automatically create listings. You're already buying this inventory; we just make it visible to your customers before it arrives."

### Layer 4: Case Study Pointer

- **Proven:** Maple Grove -- MaxAge 300, 62% manual upload, 43 Standing Orders. Their time depth distribution shows significant inventory beyond 30 days.
- **Proven:** Zeidler -- MaxAge 365, 100% open market, live cooler sync. Extreme time depth example.
- **Gap example:** Maple Grove -- futureSalesWindow=7 (buys at 18d with SOs but only shows 7d). Fix = raise to 30. Would unlock 57% more visible inventory.
- **Reference:** Time depth bar chart from scope v2 (the one Facu "ENCANTA") -- needs 3 versions for full diagnostic.

---

## LIST-03: Bunches Not Enabled (Listing Perspective)

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** No Bunches Listed Online
- **Thesis:** This is the FORMAT face of the Rubik's Cube. Even if the wholesaler has great variety and time depth, if they only list boxes, 95-98% of retail florists cannot buy from them in the quantities they need. A florist ordering for a $200 funeral arrangement doesn't need a full box of 25 stems -- they need 5 stems. Without bunches on the eShop, the wholesaler's online store is only relevant to event planners and large buyers. The FORMAT gap makes the eShop irrelevant to the majority of their customer base.
- **Detection logic:** Same core logic as CONFIG-02, but viewed from the LIST perspective: `config.json -> bunches_flag = false` AND the account has inventory listed online (varieties > 0 in loop2_list_usage_v2.json). The difference from CONFIG-02: here we quantify the TAM that's invisible, not just the config state.
- **Product types:** WH_CORE (can open boxes to units), WH_ESUITE (upload bunches via XLS). Does NOT apply to WH_K2K (depends on vendor bunch offering), WH_PROC, IMP_CORE.
- **Prerequisite:** eShop enabled with some inventory listed. If no inventory at all, LIST-01 first.
- **Expected impact:** TAM expansion from 2-5% to ~100% of retail. Quantifiable per account: what is the bunch GMV sold offline that could move online?
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** `config.json -> bunches_flag = false` AND `loop2_list_usage_v2.json -> distinct_varieties > 0` (they have listings but no bunches). Cross with `bunches.json -> total_bunch_gmv > 0` for dollar-quantified opportunity.
- **Threshold:** Binary (flag ON/OFF) but prioritized by offline bunch GMV. Accounts with high offline bunch GMV and flag OFF = highest priority.
- **TAM estimation:** From bunches.json, `total_bunch_gmv` = the dollar volume currently sold as bunches offline. If this could move online with bunches enabled, that's the addressable TAM.
- **Scope v2 gap (G36):** TAM lost table should show estimates by bunches, categories, varieties, SKUs, timeframe. Today only a static "95% TAM" string exists.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Your eShop has [X] varieties listed, which is great. But they're all listed as full boxes. Your florists who buy bunches -- and that's [Y]% of your sales -- can't order online. Here's a concrete number: you sold $[Z] in bunches last quarter, all by phone. Turning on bunches means that money can move online."
- **Reference (Challenger Message #4):** "Si solo vendes boxes online, el 95% de tu mercado no te puede comprar. (bunches = retail)"
- **Reference (Challenger Message #9):** "Si consistentemente ofreces boxes Y bunches, con fulfillment local y mejor servicio -- asi te diferencias de Mayesh."
- **What the rep should CHECK:**
  - Current online listings (varieties, SKUs from loop2_list_usage_v2.json)
  - Offline bunch GMV (bunches.json)
  - Whether "Sell by Units" is ON (foundation toggle)
  - For WH_CORE: whether they open boxes to units already (operational readiness)
  - For WH_ESUITE: whether they know the XLS upload process for bunches
- **Key objection:** "Bunches are complicated, inventory changes hourly."
  - **Response:** "For Core, when you open a box to sell from the cooler, those bunches automatically appear on the eShop. You're already doing the work -- this just makes it visible online. The quantity updates as you sell. It's not extra work; it's making your existing process visible to online buyers."

### Layer 4: Case Study Pointer

- **Proven:** Zeidler -- 100% bunches, MaxAge 365, live cooler sync. Proves bunches can work on eShop.
- **Data:** 43 accounts sell bunches. 92% of offline sales are bunches (Jul 21 analysis). The gap between what's sold and what's listed is the opportunity.
- **Benchmark from definitions.json:** "30% listing indicators: 40% adding product by the bunch." This is a leading indicator of listing health.

---

## LIST-04: No Open Market Uploads (No Future Inventory)

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** No Open Market / Future Inventory Published
- **Thesis:** Open Market uploads are how wholesalers publish inventory that's coming (in-transit POs, future availability) but not yet in the warehouse. Without this, the eShop is limited to TODAY's cooler. Florists planning ahead see nothing coming -- they assume the wholesaler doesn't carry what they need and go elsewhere. Publishing future inventory is the bridge between "what's here now" and "what's arriving this week/month."
- **Detection logic:** From buy_detail.json order_types: `open_market_gmv = 0` for accounts that have inventory and eShops. CAVEAT: current data shows ORDER_TYPE from SALES_SV (sell-side), not actual manual uploads. This lead needs to be reframed once upload-specific data is available.
- **Product types:** WH_CORE (Future Sales POs, manual upload, Standing Orders), WH_ESUITE (manual upload via XLS). Does NOT apply to WH_K2K (vendor-controlled), WH_PROC.
- **Prerequisite:** eShop enabled with basic config correct (MaxAge > 10, ecommerce features ON).
- **Expected impact:** Forward inventory visibility = florists can plan purchases digitally. This drives advance ordering, larger basket sizes, and repeat behavior. 80% of in-transit POs published = a leading indicator of listing health.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** Accounts with eShop AND `open_market_gmv = 0` (from buy_detail.json) AND `sell_total > $50K` (significant business, not a dormant account).
- **Threshold:** Zero Open Market activity with active eShop = lead.
- **MISLABEL CAVEAT (from P0.0 MIS-1):** Current Open Market data is from SALES_SV ORDER_TYPE (sales including auto-generated from Future Sales POs and SOs). Not actual manual uploads. This lead may need reframing when upload-specific data becomes available.
- **Reference (Challenger Message #5):** "Lo que compras por Koronet PUEDE aparecer automaticamente -- pero no esta configurado. (sold_as_future, 5 min fix)"

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You're buying inventory through procurement -- we see purchase orders for [X] arriving this week and next. But none of that shows up on your eShop. Your florists can't see what's coming. With one setting change (sold_as_future = YES), your procurement purchases automatically appear as available inventory on your eShop. It's a 5-minute fix."
- **Reference (Challenger Message #8):** "Si queres ahorrar tiempo y tomar ordenes automaticas, subir inventario te lo ahorra. Tus vendedores pueden ser PROACTIVOS."
- **What the rep should CHECK:**
  - sold_as_future setting (Core only -- is it YES or NO?)
  - Prebook PO volume (buy_detail.json -> prebook_gmv -- this is future inventory that could be published)
  - Whether Future Sales is enabled (config.json -> future = ON)
  - For eSuite: whether they've been shown the XLS upload workflow
- **Key objection:** "I don't want to commit to selling something that might not arrive."
  - **Response:** "You're already committed -- you placed the purchase order. The listing shows what you're expecting. If something changes, you update the PO and the listing updates automatically. This is how Maple Grove runs 43 Standing Orders that auto-generate listings. The risk isn't in showing it; the risk is in NOT showing it and losing the sale to a competitor who does."

### Layer 4: Case Study Pointer

- **Proven:** Price's Floral -- 93% of prebook items had sold_as_future=No, making cooler inventory invisible online. Fix = 5-minute toggle to YES.
- **Proven:** Maple Grove -- 62% manual upload, 43 Standing Orders. Their inventory pipeline is highly automated.
- **Reference from definitions.json list_leading_indicators:** "80% of in-transit POs published" = a leading indicator of listing health.

---

## LIST-05: Categories/Varieties Gap Online vs Offline

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Category/Variety Coverage Gap
- **Thesis:** This is the VARIETIES face of the Rubik's Cube. A wholesaler with 100 categories in their cooler but only 5 on the eShop has a store that looks empty. The florist visiting the eShop sees a fraction of what's actually available. They assume the wholesaler doesn't carry what they need -- when in reality, the product is right there in the cooler, just not listed online. Every missing category = lost online sales AND a florist learning to go elsewhere.
- **Detection logic:** From supply_matrix_full.json sell_coverage: `gap_categories > 0` (total categories sold - online categories sold). Or compute `online_categories_sold / total_categories_sold < 0.5` (less than half the catalog is online).
- **Product types:** WH_CORE (full control), WH_ESUITE (upload-dependent), WH_K2K (vendor-dependent). Does NOT apply to WH_PROC.
- **Prerequisite:** eShop enabled with at least some listings. If zero listings, LIST-01 first.
- **Expected impact:** More categories online = more reasons for florists to buy online instead of calling. The BREADTH readiness layer (definitions.json): HIGH = >70% categories online, MED = 30-70%, LOW = 10-30%, MINIMAL = <10%.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From supply_matrix_full.json sell_coverage: `online_categories_sold / total_categories_sold < 0.50`. Enhanced: `gap_categories > 5` (significant number of categories missing online).
- **Threshold:** <50% category coverage = lead. <30% = high priority. <10% = critical.
- **Variety-level analysis:** From loop2_list_usage_v2.json: `distinct_varieties` (online only). No offline variety count exists (P0.0 gap G41). Lead list is limited to category-level until variety-level offline data is available.
- **Coverage:** supply_matrix_full sell_coverage = 270/399. loop2_list_usage_v2 = 272/399. Good coverage for detection.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You sell [X] categories through Koronet, but only [Y] are visible on your eShop. That's [Z] categories your online customers can't see. Each missing category is a reason for a florist to call you or go to a competitor. Let's look at which categories are missing and figure out the fastest way to get them listed."
- **Reference (Challenger Message #2):** "No podes vender lo que no mostras. (100 cats en cooler, 5 en eShop = tienda vacia)"
- **Reference (Challenger Message #7):** "Si lo que mostras online es PEOR que lo que tenes en el cooler -- tus clientes van al cooler o compran en Mayesh."
- **What the rep should CHECK:**
  - Category coverage ratio (supply_matrix_full sell_coverage)
  - Which specific categories are missing (gap_categories from supply_matrix_full)
  - Whether missing categories are high-value (roses, mixed bouquets, seasonal) or niche
  - Which listing path fits this account (K2K vendor avail, manual upload, Future Sales POs, SO)
- **Key objection:** "I can't list everything online, it changes daily."
  - **Response:** "You don't need to list everything today. Start with your top 10 categories by volume -- the ones your florists always ask for. Those are probably stable enough to keep online. Once those are listed, you'll see which ones florists actually order online, and you can decide what to add next."

### Layer 4: Case Study Pointer

- **Benchmark:** Maple Grove -- 112 categories visible online. LIST FIRST approach -- eCom from Day 1, K2K came 10 months later.
- **Data point:** KBC has 89% of units offline not listed online. The category gap is the visible symptom.
- **Reference from definitions.json list_coverage_metrics:** "coverage_pct = categorias listadas online / categorias vendidas total."

---

# TAB 5: SELL

SELL leads are about getting florists to buy online and come back. This is where the master thesis meets reality: offer everything, confirm fast, deliver quality = more buyers, more repeat, more wallet share.

---

## SELL-01: Zero Transactions Post-Go-Live (>30 Days)

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** 0TX Post-Go-Live
- **Thesis:** An account that goes live and has zero online transactions after 30 days is a failed activation. The implementation investment produced no return. The longer an account sits at 0TX, the harder it is to recover -- the client loses confidence and the champion disengages. Every day at 0TX is a day where the wholesaler's florists are learning that the eShop doesn't exist or doesn't work. This is the most urgent SELL lead because the clock is ticking.
- **Detection logic:** From christine.json: accounts with `days_live > 30` AND `sell_online = 0` (from metrics.json or sell_detail.json). Also: `metrics.json -> sell_stage = 'NO_ONLINE_SALES'` combined with a go-live date > 30 days ago.
- **Product types:** All post-go-live accounts -- WH_CORE, WH_ESUITE, WH_K2K with eShop. Not WH_PROC (no sell-side).
- **Prerequisite:** Account must be post-go-live (implementation completed). If still in implementation, this is a Christine IMPL issue, not a SELL lead.
- **Expected impact:** Getting the first online transaction breaks the ice. Once a florist buys online and has a good experience, repeat behavior follows. The 0TX wall is the hardest to break; once past it, the trajectory typically improves.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From accounts.json: `go_live` date is > 30 days ago. Cross with metrics.json: `sell_online = 0 OR null`. Or from the existing bottleneck waterfall: Priority 7 = "SELL: No online sales."
- **Threshold:** >30 days post-go-live with $0 online = lead. >60 days = urgent. >90 days = at-risk of permanent churn.
- **Context from Jul 31:** Christine's IMPL wrap identifies 14 post-go-live accounts, several at 0TX after 30-86 days. This is an active problem.
- **Coverage:** christine.json = 15 accounts (implementation-specific). go_live in accounts.json = 217/399 (54.4%).

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You went live [X] days ago and your eShop hasn't had an order yet. Let's diagnose why. Usually it's one of three things: (1) inventory isn't visible, (2) buyers don't know the eShop exists, or (3) there's a config issue blocking checkout. Which of these do you think is most likely?"
- **What the rep should CHECK:**
  - Is inventory listed? (loop2_list_usage_v2.json -> distinct_varieties > 0)
  - Is the eShop findable? (linked from website? GA4 sessions > 0?)
  - Are there config blockers? (MaxAge, bunches, hideCheckoutWithoutPayment)
  - Have any buyers been set up with eShop access?
  - Has the wholesaler promoted the eShop to their florists at all?
- **Key objection:** "Nobody is buying online, so I don't think it works for my business."
  - **Response:** "Nobody is buying because nobody has been invited. Let's start with your top 5 offline buyers -- the ones who already trust you and buy regularly. We'll set them up with eShop access and walk them through the first order. One repeat buyer buying online is worth more than 100 new visitors who don't convert."

### Layer 4: Case Study Pointer

- **Pattern from Jul 31:** 14 post-go-live accounts, several at 0TX. Christine's IMPL team tracks these closely.
- **Reference:** KBC Pittsburgh -- before onboarding: 14 customers offline = $688K. After onboarding: 9 of 14 now purchasing online.
- **Reference from definitions.json sell_buyer_onboarding:** Step-by-step buyer activation process (identify top offline buyers, create eShop users, send invitations, walk through first order, follow up in 7 days).

---

## SELL-02: CVR Declining

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** eShop Conversion Rate Declining
- **Thesis:** Conversion rate is the thermometer of the master thesis. If a wholesaler offers everything florists want, confirms fast, and delivers quality = CVR goes up. If CVR is declining, something broke -- the catalog got stale, a category disappeared, pricing changed, or the experience degraded. A declining CVR means the wholesaler is losing sales RIGHT NOW because florists visit but don't buy. This is the most sensitive leading indicator of eShop health.
- **Detection logic:** From ga4_eshop.json: `transactions / sessions` trending down period-over-period. CURRENTLY NOT POSSIBLE -- GA4 data is point-in-time, no trend data exists (P0.0 gap T6). This lead needs temporal data from P0.2.
- **Product types:** All accounts with eShop and GA4 data. Currently ga4_eshop.json = 29 records (15 with transactions).
- **Prerequisite:** Account must have meaningful traffic (sessions > 50) to measure CVR. If sessions are near zero, the problem is discovery (SELL-05 territory), not conversion.
- **Expected impact:** Identifying and fixing declining CVR prevents revenue erosion. Each percentage point of CVR is worth: (sessions x avg_order_value x 0.01) in additional GMV.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** BLOCKED until temporal data exists. When available: compare CVR (transactions/sessions) in current period vs prior period. Flag accounts where CVR dropped > 3 percentage points.
- **Threshold:** CVR drop > 3pp = investigate. CVR drop > 5pp = urgent.
- **Benchmarks from scope v2:** Kennicott ~22.8% (best in class), Sole Farms ~47% (outlier). Network average TBD (Rose needs to compute).
- **GA4 CAVEAT (from scope v2):** hostname != eShop performance. Only Kennicott uses own domain. For 117 of 150, buyers enter via app.kometsales.com. CVR by hostname can be misleading -- compute by company, not by host.
- **Coverage:** ga4_eshop.json = 29 records. Massive coverage gap.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Your eShop conversion dropped from [X]% to [Y]% in the last [period]. That means florists are visiting but buying less. Let's figure out what changed -- did you lose a category? Did pricing change? Did a vendor stop shipping? Something shifted and we need to find it."
- **What the rep should CHECK:**
  - CVR trend (when temporal data is available)
  - Category coverage change (did categories/varieties drop?)
  - Time depth change (did MaxAge or inventory publishing change?)
  - Pricing change (did the wholesaler raise prices?)
  - Vendor churn (did a key vendor disconnect or go inactive?)
  - Format change (did bunches get turned off?)
- **Key objection:** "My conversion rate doesn't matter -- the right customers know to call me."
  - **Response:** "The customers who DO visit your eShop are trying to buy. When they don't convert, they either call you (which means the eShop isn't saving you time) or they buy elsewhere (which means you're losing sales). Either way, a declining CVR means something that used to work stopped working."

### Layer 4: Case Study Pointer

- **Benchmark:** Kennicott at 22.8% CVR -- what do they do right? Deep catalog (300+ day MaxAge), bunches, wide variety, competitive pricing.
- **NEEDS CASE STUDY** -- no documented case of CVR decline diagnosis and recovery. Need temporal CVR data first (P0.2 requirement).
- **NEEDS DATA** -- GA4 temporal data, CVR by company (not hostname), New User CVR (scope v2 line 89).

---

## SELL-03: Buyer Churn > New Buyer Acquisition

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Net Negative Buyer Trend
- **Thesis:** If more buyers are leaving than arriving, the wholesaler's online business is shrinking regardless of what individual buyers spend. This is a sustainability crisis. It means the eShop experience is not retaining florists -- either the catalog is inadequate, the experience is poor, or competitors are winning them. Reversing churn is about making the existing shop work BETTER (supply, pricing, convenience), not about marketing to new florists.
- **Detection logic:** From loop2_phase1v2_buyers.json: `CHURNED_BUYERS_Q2 > NEW_BUYERS_Q2` (net negative). Enhanced with repeat_rate.json: `repeat_rate_pct < 40%` (low repeat = leaking buyers).
- **Product types:** All accounts with online sell-side activity -- WH_CORE, WH_ESUITE, WH_K2K with eShop.
- **Prerequisite:** Account must have had online buyers at some point. If zero online buyers, SELL-01 (0TX) is the lead.
- **Expected impact:** Stopping churn is 5-10x more valuable than acquiring new buyers. Each retained buyer = ongoing repeat GMV = stable fee base. The master thesis says deliver quality = repeat. If they're not repeating, something in the delivery chain is broken.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From loop2_phase1v2_buyers.json: `CHURNED_BUYERS_Q2 > NEW_BUYERS_Q2`. Cross-reference with repeat_rate.json: `repeat_rate_pct < 40%` (red threshold per existing card logic).
- **Threshold:** Churned > New = lead. Churned > 2x New = urgent (accelerating decline).
- **Nuance:** "Churned" needs definition -- Q2 churn means buyers who bought in Q1 but not Q2. Could be seasonal (Valentine's buyers who only order once). Need to distinguish seasonal from structural churn.
- **Coverage:** loop2_phase1v2_buyers.json = 156/399. repeat_rate.json = 231/399.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You gained [X] new online buyers last quarter but lost [Y]. That means your online customer base is shrinking. Let's look at who churned -- were these regular buyers who stopped, or one-time buyers? If regular buyers left, we need to understand why and try to win them back."
- **What the rep should CHECK:**
  - New vs churned buyer counts (loop2_phase1v2_buyers.json)
  - Repeat rate (repeat_rate.json -- healthy > 60%, warning 40-60%, red < 40%)
  - Who specifically churned? (buyer-level data if available)
  - Did anything change in the catalog that would drive churn? (categories removed, pricing up, vendor loss)
  - Are churned buyers still buying OFFLINE from this wholesaler? (= experience problem, not relationship problem)
- **Key objection:** "Those buyers were one-time, they wouldn't have come back anyway."
  - **Response:** "Let's verify. If they ordered 3+ times before stopping, they were regulars who chose to stop. If they truly were one-time, that still means we need to ask: why didn't they come back? What would make them repeat? The accounts with 60%+ repeat rates figured something out -- usually it's catalog depth and availability consistency."

### Layer 4: Case Study Pointer

- **Benchmark:** Maple Grove -- 76% repeat rate, 56 buyers. Their retention is high because their catalog is deep and consistent.
- **Reference from definitions.json sell_sustainability_score:** `repeat_rate > 60% of buyers repeat in 90D = sticky`.
- **From hypotheses.md (H2-SELL):** "Share of wallet > new acquisition. Opportunity is share of wallet + efficiency, not acquisition. New customers only works importer->wholesaler."

---

## SELL-04: High Buyer Concentration Risk (Top 5 > 80%)

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Buyer Concentration Risk
- **Thesis:** If the top 5 buyers represent > 80% of online GMV, the wholesaler's online revenue is fragile. Losing one buyer = catastrophic revenue drop. This isn't about whether those buyers are good -- it's about vulnerability. A healthy online business has diversified demand. High concentration often means the eShop only works for a few power users while the rest of the customer base buys by phone.
- **Detection logic:** From buyer_concentration.json: `top2_pct > 50%` (existing data, Top 2). SCOPE V2 WANTS Top 5, not Top 2 -- needs expanded query (P0.0 gap G48). Proxy: if Top 2 > 50%, Top 5 is almost certainly > 80%.
- **Product types:** All accounts with online sell-side activity.
- **Prerequisite:** Account has online buyers (> 5 to make concentration meaningful). If < 5 online buyers, concentration is structurally high and the lead is really "too few buyers" (SELL-01/SELL-03 territory).
- **Expected impact:** Reducing concentration = more resilient online revenue. The path: onboard more offline buyers to online (definitions.json sell_buyer_onboarding). Each new online buyer reduces concentration AND grows total online GMV.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From buyer_concentration.json: `top2_pct > 50%` (red threshold in existing card). When Top 5 data is available: `top5_pct > 80%`.
- **Threshold:** Top 2 > 50% = risk (red). Top 2 25-50% = moderate (amber). Top 2 < 25% = healthy (green).
- **Context:** K2K pass-through may inflate concentration -- a single K2K partner showing as a "buyer" with high volume (from Jul 25 scorecard: "SELL STRONG count misleading -- K2K pass-through inflates").
- **Coverage:** buyer_concentration.json = 226/399. All offline buyers are missing (P0.0 gap: "FALTA todo lo offline").

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Your top 2 online buyers represent [X]% of your online sales. That's great that they're buying, but if one of them stops, you lose [Y]% of your online revenue overnight. The safest path is to bring more of your offline buyers online -- you already have the relationship, we just need to give them eShop access."
- **What the rep should CHECK:**
  - Concentration percentages (buyer_concentration.json)
  - Who are the top buyers? Are they K2K pass-through or real end customers?
  - How many offline buyers exist who could be onboarded? (buyers.json -> offline_buyers)
  - Have the top buyers been growing or are they flat? (temporal data needed)
- **Key objection:** "My big buyers are loyal, they're not going anywhere."
  - **Response:** "They might be -- but your online business shouldn't depend on that. The real opportunity isn't protecting against loss; it's growing by bringing your other [X] offline buyers online. If even 5 more buyers start ordering online, your concentration drops and your total online GMV grows."

### Layer 4: Case Study Pointer

- **Reference:** KBC Pittsburgh -- 14 customers with eShop = $953K (58%), 14 without = $688K (42%). Onboarding offline buyers reduces concentration naturally.
- **Update:** 9 of 14 offline customers at KBC Pittsburgh now have online purchases (from definitions.json). Active proof that buyer onboarding works.
- **Reference from definitions.json sell_sustainability_score:** `concentration: Top 2 buyers < 50% of online GMV = healthy`.

---

## SELL-05: No Payment Method Enrolled (Checkout Wall)

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** No Payment Method Enrolled
- **Thesis:** A florist visits the eShop, finds what they want, adds to cart -- and can't check out because they don't have a payment method on file. This is a SILENT conversion killer. The wholesaler may not even know it's happening. Unlike CONFIG-03 (which is about the hideCheckoutWithoutPayment setting), this lead is about the BUYER side: do the wholesaler's customers have payment methods set up to complete purchases?
- **Detection logic:** Behavioral signal: accounts with GA4 sessions > 50 AND low conversion rate AND hideCheckoutWithoutPayment = ON. Buyer-level payment method data is not available in current JSONs -- this is a proxy detection.
- **Product types:** All accounts with eShop -- WH_CORE, WH_ESUITE, WH_K2K.
- **Prerequisite:** eShop enabled, inventory visible, config correct. If config is wrong (CONFIG-03), fix config first.
- **Expected impact:** Removing the checkout wall for buyers who are READY to buy = immediate conversion improvement. These are the highest-intent florists -- they browsed, selected, and hit a wall at the last step.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** No clean automated detection today. Proxy: accounts with `hideCheckoutWithoutPayment = ON` AND `ga4 sessions > 50` AND `online_sell / ga4_sessions < expected_CVR`. Ideal: buyer-level data showing registered buyers without payment methods.
- **Threshold:** >10 registered buyers without payment methods = lead (when buyer data is available).
- **Known accounts:** Price's Floral, Floropolis, Tennessee -- all identified with this pattern.
- **Coverage:** Limited -- GA4 = 29 records, buyer payment data not available.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Some of your customers are visiting the eShop and trying to buy, but they can't complete checkout because they don't have a payment method saved. Let's identify who these buyers are and help them set up their accounts. This is the last step between a browse and a sale."
- **What the rep should CHECK:**
  - hideCheckoutWithoutPayment setting (config.json)
  - How many registered eShop users exist
  - How many have payment methods on file (needs buyer-level query)
  - Whether the wholesaler has communicated payment setup to their customers
- **Key objection:** "I can't force my customers to add credit cards."
  - **Response:** "You don't have to force them. You can: (1) send them a simple email with setup instructions, (2) walk them through it on their next phone call, or (3) turn off the payment requirement and collect payment the same way you do for phone orders -- on account. The goal is to remove the barrier, not add paperwork."
- **Reference from definitions.json:** `sell_payment_gate`: "Before escalating 'mandatory payment blocks conversions' -- need FUNNEL DATA first. How much traffic? How many registered? How deep does the funnel go?"

### Layer 4: Case Study Pointer

- **Known pattern:** Price's Floral (all config correct, $0 revenue -- payment wall), Tennessee (same pattern), Floropolis (same pattern).
- **NEEDS CASE STUDY** -- need a documented case where payment method enrollment campaign led to measurable conversion improvement.

---

# TAB 6: GROWTH

GROWTH leads are for accounts that have the foundation right (config, buy, list, sell) and are ready to scale. These are the highest-tier opportunities -- they require proving the model works before investing in expansion.

---

## GROWTH-01: Payments 2.0 Candidate

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Koronet Payments 2.0 Upgrade
- **Thesis:** Payments 2.0 is an upgraded payment infrastructure. Accounts that qualify are typically high-volume, stable, and ready for a more integrated payment experience. Upgrading strengthens the relationship (more embedded in their financial workflow) and can improve checkout experience for their florists (faster, more reliable payments = better buying experience).
- **Detection logic:** Accounts with `sell_total > $500K` AND stable online activity AND current Koronet Payments active. Specific eligibility criteria for Payments 2.0 need to be defined (product team input required).
- **Product types:** WH_CORE with high volume (primary). Potentially WH_ESUITE with significant volume.
- **Prerequisite:** Account must have active online sales, existing payment infrastructure working, and a track record of stability. This is not for accounts still solving CONFIG/BUY/LIST/SELL problems.
- **Expected impact:** Deeper platform integration = higher switching cost = better retention. Improved payment experience = better buyer satisfaction = more repeat purchases.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From metrics.json: `sell_total > $500K` AND `online_pct > 20%` AND `fees_total > 0`. Cross-reference with payment infrastructure data (if available -- not in current JSONs).
- **Threshold:** High volume + stable online activity + existing payments = candidate. Exact criteria TBD pending product team input.
- **Coverage:** metrics.json = 112/399.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You're doing [$X] in online sales with strong buyer activity. Our Payments 2.0 upgrade gives your buyers a smoother checkout experience and gives you better payment tracking. Since you're already running well, this is a natural next step."
- **What the rep should CHECK:**
  - Current payment setup and volume
  - Any payment-related complaints from buyers (Intercom, Fathom)
  - Whether the account has requested payment improvements
- **Key objection:** "What's wrong with our current payment setup?"
  - **Response:** "Nothing's wrong -- this is an upgrade, not a fix. Payments 2.0 gives your buyers more payment options and faster processing. It's for accounts like yours that have outgrown the basic setup."

### Layer 4: Case Study Pointer

- **Reference:** Sole Farms -- payments migration + ACH mentioned in state.md.
- **NEEDS CASE STUDY** -- need a documented Payments 2.0 migration with measured improvement in checkout completion or buyer satisfaction.

---

## GROWTH-02: API at 0% Fees

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** API Channel at Zero Take Rate
- **Thesis:** Some accounts sell significant volume through API integrations (e.g., Mayesh via Mayesh.com, BySpeaks) at 0% fee rate. This is revenue leakage for Koronet -- the volume is digital, uses Koronet infrastructure, but generates no fees. API monetization is a HIGH OPPORTUNITY, short-term push. The master thesis applies: these API channels serve florists well (they buy online, which is what we want). Koronet's job is to capture fair fees for the infrastructure that makes it possible.
- **Detection logic:** From sell_by_channel.json: channels with `sales_channel = 'API'` AND `take_rate < 0.5%` AND `channel_gmv > $10K`. Existing detection in POTENTIAL card: API < 0.5% warning + NOT MONETIZED flag for $10K+ at $0 fees.
- **Product types:** Accounts with API integrations -- typically WH_CORE with developer resources. Key accounts: Mayesh ($329K fee gap at 0.05% effective vs 1.5% SFDC rate).
- **Prerequisite:** API channel exists and has significant volume. This is a contract/commercial conversation, not a product/config fix.
- **Expected impact:** Mayesh alone = $329K fee gap. API consumption tripled in 12 months. Each API channel monetized at even 1% = significant fee capture on existing volume.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From sell_by_channel.json: filter for `sales_channel = 'API'` AND `channel_gmv > $10K` AND `channel_fee = $0 OR take_rate < 0.5%`.
- **Threshold:** API GMV > $10K at < 0.5% take rate = lead. API GMV > $100K at < 0.5% = high priority. Mayesh at $94M with 0.05% = strategic.
- **Approach:** "Navy SEAL approach" per hypotheses.md. Blumnet = contract target.
- **Coverage:** sell_by_channel.json = 271/399.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Your API integration is driving [$X] in sales through [channel]. That's great for your buyers -- they get a seamless experience. Right now that volume isn't generating platform fees. As we evolve our API pricing, we want to make sure the value exchange is fair for both sides. Let's discuss how we can structure this."
- **What the rep should CHECK:**
  - API volume and growth trend
  - Current contract terms (is 0% intentional? Promotional? Oversight?)
  - Whether the API integration is critical to the account's business model
  - Competitor API pricing (what do alternatives charge?)
- **Key objection:** "We built the integration ourselves, why should we pay fees?"
  - **Response:** "The integration uses Koronet's inventory, order management, and fulfillment infrastructure. The API is the channel, but the platform powering it is ours. We want to find a rate that's fair for both sides -- you're generating significant volume and we want to support that growth."

### Layer 4: Case Study Pointer

- **Key account:** Mayesh -- $94.2M GMV, $329K fee gap (0.05% effective vs 1.5% SFDC rate). THE benchmark for API monetization.
- **Reference:** BySpeaks at 3.5% take rate -- proves API can be monetized.
- **Context from hypotheses.md:** API consumption tripled in 12 months. "Navy SEAL approach."

---

## GROWTH-03: Core Upgrade from eSuite

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** eSuite-to-Core Upgrade (Growth Path)
- **Thesis:** This is CONFIG-04 seen from the GROWTH tab. An eSuite account that has proven online success (active sales, repeat buyers, growing volume) is being held back by eSuite's limitations. Upgrading to Core unlocks Standing Orders (automated recurring inventory), Future Sales (forward availability), and native bunch handling -- capabilities that multiply what an already-successful account can do. This is the growth play for accounts that have earned the upgrade.
- **Detection logic:** `ct_id = 'WH_ESUITE'` AND `sell_online > $50K` AND `repeat_rate_pct > 40%` AND `online_buyers > 10` (evidence of active online success constrained by eSuite limitations).
- **Product types:** WH_ESUITE only.
- **Prerequisite:** The account must ALREADY be succeeding with eSuite. If they're at 0TX or inactive, CONFIG-04 is the lead, not GROWTH-03.
- **Expected impact:** Unlocks Standing Orders ($1.2M TAM currently blocked), Future Sales, native bunches. For a high-performing eSuite account, Core is the multiplier. The upgrade decision is commercial (licensing change) and operational (ERP migration).
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** `ct_id = 'WH_ESUITE'` AND `sell_online > $50K` AND (`repeat_rate_pct > 40%` OR `online_buyers > 10`).
- **Threshold:** Proven online success + hitting eSuite limitations = lead.
- **Count:** 5 WH_ESUITE accounts total. Very small pool -- each one is individual.
- **Differentiation from CONFIG-04:** CONFIG-04 identifies ANY eSuite that might benefit from Core. GROWTH-03 is specifically for eSuite accounts with PROVEN success that need Core to grow further.

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "You've built a strong online business with eSuite -- [$X] in sales, [Y] repeat buyers. But eSuite has a ceiling: no Standing Orders, no Future Sales, no native bunch handling. Core removes those limits. Your florists could set up weekly recurring orders, you could show what's arriving in 2-3 weeks, and bunches would be handled natively. You've earned the upgrade."
- **What the rep should CHECK:**
  - Current eSuite performance (sales, buyers, repeat rate)
  - Whether Standing Orders would apply (recurring buyer patterns)
  - Whether Future Sales would apply (prebook inventory patterns)
  - Whether the ERP migration is feasible (Core replaces ERP)
  - Cost/benefit of the upgrade for the account
- **Key objection:** "Core is more expensive and we'd have to change our whole system."
  - **Response:** "That's the biggest consideration. But look at what you're leaving on the table: [$X] in potential Standing Orders, forward availability for your regular buyers, native bunches. The accounts running Core at your volume level earn [X]x more in online sales because they can offer their florists more. Let's map whether the numbers work."

### Layer 4: Case Study Pointer

- **Reference:** Shamrock -- exploring eSuite-to-Core upgrade (Jul 22 demo). Decision pending from Meredith.
- **NEEDS CASE STUDY** -- no proven eSuite-to-Core upgrade with before/after metrics.

---

## GROWTH-04: Standing Orders Automation (Core Accounts)

**Status:** DRAFT

### Layer 1: Define + Reasoning

- **Name:** Standing Orders Automation Opportunity
- **Thesis:** Standing Orders are recurring auto-POs -- they generate inventory automatically every week. For a Core account, each Standing Order = a product that's always listed on the eShop without manual work. Maple Grove runs 43 SOs that automatically populate their catalog. SOs are the highest-leverage listing mechanism because they're set-and-forget: the wholesaler sets them up once, and inventory appears consistently. Consistent inventory = consistent buyer experience = repeat behavior.
- **Detection logic:** `ct_id = 'WH_CORE'` AND (standing_order_count = 0 OR standing_order_count < 10) AND sell_total > $100K (significant business that would benefit from automation). Standing Order data is not in current JSONs (P0.0 gap G35).
- **Product types:** WH_CORE ONLY. Standing Orders are NOT coming to eSuite (decision made Jul 31). This is exclusively a Core play.
- **Prerequisite:** Account must be WH_CORE with active procurement and some online selling activity. If they're not buying or listing anything, earlier-stage leads first.
- **Expected impact:** Each SO added = one more consistent product on the eShop. Maple Grove's 43 SOs are a core part of why they have 300+ day time depth, 76% repeat rate, and growing fees. BUT: SOs today are 100% offline ($1.2M TAM blocked by lack of digital SOs). The play is about maximizing offline SOs as an inventory automation tool.
- **Status:** DRAFT

### Layer 2: Lead List Concept

- **Rose query logic:** From accounts.json: `ct_id = 'WH_CORE'`. Need Standing Order count data (not in current JSONs -- needs new query). Proxy: Core accounts with `sell_total > $100K` AND low listing coverage (categories online / total < 50%) = candidates where SOs could help fill the catalog.
- **Threshold:** WH_CORE with < 10 SOs AND > $100K sell volume = lead.
- **DATA GAP:** Standing Order data does not exist in current JSONs. Rose needs a new Snowflake query to count SOs per account.
- **Known limitation from definitions.json:** "No digital standing orders -- SOs are 100% offline (Core). $1.2M TAM blocked."

### Layer 3: Enablement Sketch

- **What the rep should SAY:** "Standing Orders automate your weekly purchasing for regular products -- roses every Tuesday, greens every Monday. Each Standing Order automatically creates inventory on your eShop. Maple Grove runs 43 Standing Orders and they barely touch their catalog manually. It's the most efficient way to keep your eShop stocked."
- **What the rep should CHECK:**
  - Current Standing Order count (when data is available)
  - Which categories the wholesaler buys every week (candidates for SOs)
  - Whether the wholesaler already has SOs set up offline (these may not be creating eShop listings)
  - Whether sold_as_future is ON (needed for SOs to create listings)
- **Key objection:** "Standing Orders are rigid -- what if I don't need roses next week?"
  - **Response:** "You can skip or adjust any SO week by week. The default just saves you from placing the same order manually every week. And the inventory listing automatically adjusts -- if you skip, the listing doesn't appear. It's a default, not a commitment."

### Layer 4: Case Study Pointer

- **Proven:** Maple Grove -- 43 Standing Orders, 300-day MaxAge, 62% manual upload, 76% repeat rate, $3,609 fees H1 (+33% YoY). SOs are the backbone of their automated listing strategy.
- **Proven:** KBC Pittsburgh -- 31 Standing Orders (all boxes, zero bunch usage). Shows SOs work but are incomplete without bunches.
- **Reference from definitions.json list_inventory_paths:** "Standing Orders -> auto-PO. Effort: HIGH (setup, then automated). Who: Core ONLY. Covers: Recurring categories auto-generate. Maple Grove model."

---

# SUMMARY

## Lead Count by Tab

| Tab | Leads | IDs |
|---|---|---|
| CONFIG | 5 | CONFIG-01 through CONFIG-05 |
| BUY | 4 | BUY-01 through BUY-04 |
| LIST | 5 | LIST-01 through LIST-05 |
| SELL | 5 | SELL-01 through SELL-05 |
| GROWTH | 4 | GROWTH-01 through GROWTH-04 |
| **TOTAL** | **23** | |

## Data Dependencies for Lead Detection

| Lead | Data Available? | Gap |
|---|---|---|
| CONFIG-01 | YES (config.json 99.5%) | -- |
| CONFIG-02 | YES (config.json + bunches.json) | Bunch GMV only 43 accounts |
| CONFIG-03 | PARTIAL (config.json + GA4 29 records) | Need buyer payment method data |
| CONFIG-04 | YES (accounts.json + metrics.json) | Metrics only 28.1% |
| CONFIG-05 | YES (accounts.json 100%) | -- |
| BUY-01 | SPARSE (buy_detail.json anticipation 11.3%) | Need expanded anticipation + offline buying horizon data |
| BUY-02 | YES (metrics.json k2k + vendor_detail.json) | Metrics 28.1%, vendor_detail 69.4% |
| BUY-03 | NO (no price parity data) | Need systematic price comparison data |
| BUY-04 | SPARSE (vendor_detail leakage 0.9%) | Need expanded leakage detection |
| LIST-01 | PARTIAL (accounts.json + sell data) | Sell data 45-57% |
| LIST-02 | PARTIAL (time_depth 24.8%) | Need offline time depth + best-in-class |
| LIST-03 | YES (config.json + bunches.json) | Same as CONFIG-02 |
| LIST-04 | PARTIAL (buy_detail 22.1%) | Open Market data is mislabeled (sell-side, not uploads) |
| LIST-05 | YES (supply_matrix 63-68%) | Need offline variety/SKU counts |
| SELL-01 | PARTIAL (christine.json 15, go_live 54.4%) | -- |
| SELL-02 | BLOCKED (no temporal GA4 data) | Need P0.2 temporal data |
| SELL-03 | YES (loop2_phase1v2_buyers 39.1%) | Limited coverage |
| SELL-04 | YES (buyer_concentration 56.6%) | Need Top 5, not Top 2 |
| SELL-05 | NO (no buyer payment method data) | Need buyer-level payment enrollment data |
| GROWTH-01 | NO (no Payments 2.0 criteria) | Need product team input |
| GROWTH-02 | YES (sell_by_channel 67.9%) | -- |
| GROWTH-03 | PARTIAL (5 WH_ESUITE accounts) | Small pool |
| GROWTH-04 | NO (no Standing Order count data) | Need new Snowflake query |

## Enablement Assets Referenced

- **9 LIST Challenger Messages** (from definitions.json `list_challenger`): referenced in LIST-01, LIST-02, LIST-03, LIST-04, LIST-05
- **6 BUY Discovery Questions** (from definitions.json `buy_discovery`): referenced in BUY-01, BUY-02
- **5 BUY Challenger Messages** (from definitions.json `buy_challenger`): referenced in BUY-02, BUY-04
- **Sell Buyer Onboarding** (from definitions.json `sell_buyer_onboarding`): referenced in SELL-01, SELL-04
- **Sell Sustainability Score** (from definitions.json `sell_sustainability_score`): referenced in SELL-03, SELL-04
- **Readiness Layers** (from definitions.json `readiness_layers`): referenced across CONFIG and LIST leads

## Next Steps

1. **Facu reviews each lead** -- moves from DRAFT to APPROVED or rejects with reason
2. **Rose generates lead lists** for leads with data available (CONFIG-01, CONFIG-02, CONFIG-05, BUY-02, LIST-05 = quick wins)
3. **Rose fills data gaps** for leads blocked by missing data (BUY-01 anticipation, BUY-04 leakage, SELL-02 temporal, GROWTH-04 Standing Orders)
4. **Case study documentation** for leads marked NEEDS CASE STUDY
5. **Enablement content development** for APPROVED leads (talk tracks, checklists, objection handling)
