# Facu Feedback — Aug 1, 2026

Feedback directo de Facu durante browser review. Organizado por CARD para tracking claro.

---

## PORTFOLIO (Tab 1 columns)

| # | Feedback | Applied? | Enablement note |
|---|---|---|---|
| P1 | Buy columns más importantes que config/buy/list/sell mini-scores | ✅ | — |
| P2 | Fees = una columna (direct+indirect sumados) | ✅ | — |
| P3 | Take Rate = (fees) / (sell + buy GMV) | ✅ | — |
| P4 | # Opps = APPROVED only | ❌ needs approval workflow | Para ENABLE: reps need to understand what APPROVED means and how leads get promoted |
| P5 | Trend obligatorio (growing/flat/declining) | ✅ | — |
| P6 | eShop CVR % con percentil/banda | partial (7 accounts only) | Para ENABLE: explain to CS why CVR matters — "calidad del shop de un vistazo" |
| P7 | Penetration annualized for short timeframes | ✅ | — |

## CARD 1: POTENTIAL

| # | Feedback | Applied? | Enablement note |
|---|---|---|---|
| POT1 | Est GMV = más importante del dashboard, descubre opps invisibles | ✅ | Para ENABLE: train reps to START with Est GMV → Koronet GMV → Online % → THEN drill |
| POT2 | Fee breakdown por canal como tabla con take rate | ✅ | Para ENABLE: "canales sin monetizar = revenue gap" — how to have the conversation |
| POT3 | Flag GMV > $10K con $0 fees (API gap) | ✅ | Para ENABLE: "API a 0% = acuerdo especial o gap de pricing. Verificar con account manager" |
| POT4 | Pre go-live label en azul | ✅ | — |
| POT5 | ORA source label missing | ✅ "Christine ORA" | — |
| POT6 | Indirect fee 1.5% hardcoded — hacer visible | ✅ labeled "[est.]" | Para ENABLE: reps should know this is an estimate, not invoiced |

## CARD 2: OPPORTUNITIES

| # | Feedback | Applied? | Enablement note |
|---|---|---|---|
| OPP1 | NO es solo config — es ALL opps priorizadas (config + BUY/LIST/SELL) | ✅ restructured + prioritized by impact | Para ENABLE: this card is "por qué deberías actuar en esta cuenta" — the executive summary. Opps sorted P1-P5 by revenue impact. |
| OPP2 | Para cada product type: potencial, best-in-class, known limitations | ✅ benchmarks.json wired per segment | Para ENABLE: "cuentas Core con bunches + time depth 90d+ tienen 22% CVR" — this IS the pitch. Product profile shows POTENTIAL (what the setup can achieve), BEST-IN-CLASS (segment medians + best account from benchmarks.json), and KNOWN LIMITATIONS (eSuite ~5% of Core TAM, etc.) |
| OPP3 | Opps sin $ cuantificado no son opps | ✅ $ impact on 7+ opp types | Para ENABLE: every opp now shows "if you fix X = $Y impact". Offline GMV at $0 fees, leakage recovery $, offline buyer conversion potential, indirect fee potential. Total $ at stake shown as banner. |
| OPP4 | LIST opps never generated | ✅ 5 types (variety coverage, time depth, bunches, open market, category gap) | — |
| OPP5 | Estructura invertida: opps primero, config después | ✅ SELL > BUY > LIST > CONFIG order | — |
| OPP6 | Links a los tabs correspondientes | ✅ clickable links scroll to card with highlight | Para ENABLE: click "see BUY card" → scrolls to and highlights BUY card within the same account |

## CARD 3: BUY

| # | Feedback | Applied? | Enablement note |
|---|---|---|---|
| BUY1 | Unit economics: "10% more online = $X proc = $Y fees" | ✅ | Para ENABLE: this is the ONE number to show the client — "tu oportunidad en $" |
| BUY2 | Vendor table: online vs offline con Vendors, Categories, Varieties, Procurement $ | ✅ restructured | Para ENABLE: "mirá cuántos vendors tenés offline que no usás online — ahí está el gap" |
| BUY3 | K2K lifecycle: no es infrastructure, requiere acuerdos y selling | ✅ as compact flow | Para ENABLE: "cada conexión K2K requirió convencer al vendor. Las dormant/churned = inversión perdida" |
| BUY4 | Bunches NO va en BUY | ✅ removed | — |
| BUY5 | Anticipación de COMPRAS: online Y offline para compararlas | ❌ BLOCKED (ORDER_DATE) | Para ENABLE: "¿comprás para mañana o planificás? Spot buying = más caro, más riesgo" |
| BUY6 | Vendor coverage: ¿puede comprar online lo que necesita a corto/medio/largo? | ❌ needs temporal vendor data | Para ENABLE: "si tu vendor de X categoría solo tiene producto a 3 días, no podés planificar" |
| BUY7 | Open Market + Prebook como temporal: PARA CUÁNDO compran | partial (GMV split exists) | Para ENABLE: BUY Discovery Questions — "¿Planificás compras a futuro o comprás para mañana?" |
| BUY8 | Anticipation data: es ONLINE procurement, no offline. Label claro | ✅ relabeled | — |
| BUY9 | Order Mix usa SELL-side data — misleading | ✅ labeled "(proxy)" | — |

## CARD 4: LIST

| # | Feedback | Applied? | Enablement note |
|---|---|---|---|
| LIST1 | Time depth: quiero ONLINE Y OFFLINE de lo que VENDEN | ✅ 3 bars | Para ENABLE: "mirá cuánto vendés offline que no mostrás online — eso es plata que dejás en la mesa" |
| LIST2 | Full comparison online vs offline (categorías, variedades, SKUs) | partial (cats ✅, vars partial, SKUs ❌) | Para ENABLE: 9 Challenger Messages — "lo que no está en tu eShop no existe para tus clientes" |
| LIST3 | Time depth = recency of last sale, NOT listing depth. Label honesto | ✅ "Variety Freshness" | Para ENABLE: "si una variedad no se vendió en 90 días, ¿sigue en tu eShop? ¿Debería?" |
| LIST4 | LIST data = proxy from SALES. No hay data de lo que LISTAN | ✅ documented | Para ENABLE: acknowledge to client "estamos viendo lo que vendiste, no lo que mostrás — si mostrás más, vas a vender más" |
| LIST5 | Open Market = lo que SUBEN, no lo que venden. Mislabeled | ✅ "(proxy)" | — |
| LIST6 | TAM perdido por bunches/categorías/variedades/SKUs/timeframe | partial (bunches $, rest ❌) | Para ENABLE: "si no habilitás bunches, el 95% de tu mercado retail no puede comprarte online" |
| LIST7 | Best-in-class benchmark (Kennicott) en time depth | ✅ third bar | Para ENABLE: "Kennicott tiene 84% de variedades vendidas en los últimos 90 días — vos tenés X%" |
| LIST8 | Anticipación de lo que VENDEN (online vs offline) | ❌ BLOCKED (CREATED_DATE) | Para ENABLE: "¿vendés para hoy o para la semana que viene? Los mejores venden a 30+ días" |
| LIST9 | Config features para vender a largo plazo (MaxAge, sold_as_future, SOs, Bunches) | partial | Para ENABLE: "5-min fix: subir MaxAge de 10 a 30 = 3x más inventario visible" |

## CARD 5: SELL

| # | Feedback | Applied? | Enablement note |
|---|---|---|---|
| SELL1 | Unit economics: "X% más buyers = $Y GMV = $Z fees" | ✅ | Para ENABLE: the revenue case — "cada buyer online vale $X/año en fees" |
| SELL2 | Offline buyers/AOV/L30D — no más dashes | ✅ from metrics_v2_buyers_full | Para ENABLE: "tenés X buyers offline que nunca compraron online — cada uno vale $Y" |
| SELL3 | Concentration Top 5, sin nombres | ✅ | Para ENABLE: "si tu top 5 es >50% de tu GMV online, un churn te mata" |
| SELL4 | Hardgoods visibility | ❌ no data | Para ENABLE: hardgoods = recurring revenue, different seasonality, different buyer profile |
| SELL5 | GA4 conversion rate — perdiste la métrica | exists but 93% excluded | Para ENABLE: "CVR = calidad del shop. Si tenés tráfico pero no convertís, el problema es inventario/pricing" |
| SELL6 | Best-in-class examples con links a leads | partial (Kennicott 22.8% CVR) | Para ENABLE: show the client "Kennicott tiene 22.8% conversion — vos tenés X%. La diferencia es inventario." |
| SELL7 | Retention over time (not just point-in-time) | ❌ needs temporal data | Para ENABLE: "si tu retention baja, estás perdiendo clientes — ¿qué cambió?" |
| SELL8 | Standing orders son OFFLINE, no presentar como online | ✅ corrected | — |
| SELL9 | Si una tabla no tiene un field, preguntate por qué | ✅ cleaned up | — |

## CROSS-CARD

| # | Feedback | Applied? | Enablement note |
|---|---|---|---|
| X1 | Data con timeframes (WoW/MoM/QoQ/YoY) en TODO | partial (sell/buy ✅, fees ✅, rest ❌) | — |
| X2 | Coverage 28% = inaceptable | documented, not solved | Para ENABLE: every missing account = a blind spot in prioritization |
| X3 | Config vs reality: MaxAge/Future controlan eShop display pero ventas bypasean | ✅ documented | Para ENABLE: explain to reps that config ≠ reality — check BOTH |
| X4 | La tesis: ofrecé todo → confirmá rápido → deliver con calidad | documented in scope v2 | Para ENABLE: this IS the pitch. Every card should connect back to this. |
| X5 | Trust labels: DRAFT y APPROVED only | ❌ no workflow | — |
| X6 | El loop tiene que hacer que todo evolucione TODOS LOS DÍAS | in progress | — |

## BLOCKED ITEMS (need Snowflake schema changes or new data sources)

| Item | What's needed | Who resolves |
|---|---|---|
| True anticipation (BUY) | ORDER_DATE or CREATED_DATE in PROCUREMENTS_SV | Product/Data team |
| True anticipation (LIST/SELL) | ORDER_DATE or CREATED_DATE in SALES_SV | Product/Data team |
| eShop CVR per company | GA4 data by company_id, not hostname (93% use shared domain) | Rose + GA4 setup |
| What's LISTED (not sold) | No CATALOG_SV exists | Product/Data team |
| Offline SKUs | No offline SKU count in any data source | Rose query |
| New User CVR | GA4 new vs returning segmentation | Rose + GA4 |

## LEARNINGS FROM THIS REVIEW

1. **Time depth ≠ listing depth.** It measures recency of last sale per variety. High 90d+ = stale, not forward-planned.
2. **Standing orders are offline.** Don't present them as online channel data.
3. **LIST data is a proxy.** All "listing" data comes from SALES_SV. No catalog/listing data exists.
4. **Config vs reality:** MaxAge/Future config controls eShop display, but sales happen through K2K/prebooks/SOs that bypass config limits.
5. **Empty columns = questions to answer.** If Offline shows dashes, either get the data or remove the column. Don't show meaningless comparisons.

---

## CARD 1 POTENTIAL — ENABLEMENT NOTES (Aug 1 improvement cycle)

### What a CS rep needs to know about this card

**Reading order (Facu flow):** Est GMV (how big is the prize?) -> Koronet GMV (how much do we capture?) -> Penetration % (what share?) -> Online % (how digital?) -> THEN drill into the fee table.

**Key concepts:**
- **Penetration qualifiers:** <20% = "big opportunity" (most of their business is outside Koronet). 20-50% = "room to grow." 50-80% = "majority captured." >80% = "saturated" (focus on monetization, not volume).
- **Online % qualifiers:** 0% = "no online" (need activation). <20% = "mostly offline" (biggest lever is moving offline volume online). 20-50% = "healthy." >50% = "digital-first" (focus on growing the pie, not the share).
- **Est Buy uses a 3-tier cascade:** (1) direct estimate if available, (2) actual buy/sell ratio x est sell, (3) 54% default. Source is shown -- if it says "avg ratio 54%", the estimate is rough.
- **Indirect fees are estimated at 1.5% of Buy Online.** This is NOT from invoices -- it's a calculation. The "[est.]" label makes this clear. If the actual rate changes, the dashboard must be updated.
- **Take rate = (direct fees + indirect fees) / (sell GMV + buy GMV).** This is the monetization efficiency. Higher = better. If take rate is low but GMV is high, there's a pricing or monetization gap.

### Discovery questions the rep should ask based on what they see

1. **Sell penetration <20%:** "We see your total sales are ~$XM but only $Y goes through Koronet. Where is the rest going? Direct orders? Other platforms?"
2. **Buy penetration <20%:** "Your estimated procurement is ~$XM but only $Y comes through Koronet. Are you buying from vendors not connected to the platform?"
3. **Online % <20%:** "Most of your Koronet volume is offline. What prevents your clients from ordering online? Is the eShop inventory complete?"
4. **Channel NOT MONETIZED:** "You have $X in [channel] GMV but $0 in fees. Is this a special agreement, or should we review the pricing setup?"
5. **API take rate <0.5%:** "Your API sales show a very low fee rate. Is the fee schedule configured correctly?"
6. **ORA/calc diverge >30%:** "Our two estimates of your business size are far apart. Can you help us calibrate -- is your annual sales volume closer to $X or $Y?"
7. **Large offline GMV ($100K+):** "You have $X in offline sales at $0 fees. If even 10% of that moved online, that's $Y in new platform volume."
8. **Pre go-live (blue banner):** "You haven't gone live yet. Once you do, there's $X in sell potential based on your estimated size."

### What changed in this improvement cycle (for tracking)

| Fix | What | Why it matters |
|---|---|---|
| Take rate formula | Fixed from fees/sell_online to (fees+indirect)/(sell+buy) | Old formula inflated rate by dividing by online-only sell |
| Sell penetration fallback | Falls back to calc estimate when ORA is null | 60% of accounts have no ORA; now they can show penetration |
| Buy gap banner | Shows "$XM of estimated procurement not through Koronet" | Buy side now has same SO WHAT as sell side |
| Penetration qualifiers | Inline text: big opportunity / room to grow / majority captured / saturated | Numbers mean nothing without context |
| Online % qualifiers | Inline text: no online / mostly offline / healthy / digital-first | Same as above |
| Offline row V2 wired | Uses temporal sell_offline from V2 with delta arrow | Offline value now updates when timeframe changes |
| Indirect fee display fix | Shows V2-aware buy online value in formula, not stale static | Formula display matches calculation |
| Take rate on TOTAL row | Shows overall take rate next to total fees | Monetization efficiency visible at a glance |
| Offline SO WHAT | "move 10% online = $XK GMV" when offline >$100K | Quantifies the offline opportunity |
| NOT MONETIZED guidance | "verify pricing agreement" instead of just flag | Rep knows what to DO, not just what's wrong |
| ORA divergence guidance | "ask account manager which estimate is current" | Actionable, not just a warning |
| Focus summary | Bottom of card: biggest levers listed | Operator sees priority at a glance |

### Remaining gaps (documented, not invented)

| Gap | Why it can't be fixed now | What would fix it |
|---|---|---|
| Channel fees not temporal | No V2 per-channel fee breakdown exists | Rose: query fees by channel with temporal structure |
| Channel GMV not temporal | No V2 sell_by_channel exists | Rose: query channel GMV with temporal structure |
| Channel fees use company_name match | sell_by_channel.json has no company_id | Rose: add company_id to sell_by_channel query |
| Indirect fee rate hardcoded 1.5% | No config source for actual rate | Facu decision: where does the rate live? |
| No benchmarks by product type | No cross-account benchmark data | Rose: compute network averages per ct_id |

---

## CARD 2 OPPORTUNITIES — ENABLEMENT NOTES (Aug 1 improvement cycle)

### What a CS rep needs to know about this card

**Purpose:** This is the executive summary of why you should act on this account. It answers: "What are the biggest opportunities, prioritized by revenue impact?" Every opportunity has a SO WHAT with $ impact where data exists.

**Reading order:**
1. **Total $ at stake** (green banner at top) -- the sum of all quantified opportunities
2. **Revenue opportunities** (SELL > BUY > LIST, sorted by priority P1/P2/P3) -- what to DO
3. **Config opportunities** (amber section below) -- enablers that unlock revenue
4. **Bottleneck + Next Action** -- the single most important thing to address first
5. **Product profile** -- what this account type CAN achieve, benchmarks, and known limitations

**Key concepts:**
- **P1 = highest priority, P2 = medium, P3 = lower.** Opps are sorted by priority first, then by $ impact within each priority level.
- **Every $ amount is either calculated or estimated.** Offline GMV at $0 fees is real. "If 10% moves online = $X" is a projection using conservative take rates.
- **Revenue opps link to the detailed cards.** Click "see BUY card" to scroll directly to the BUY analysis for this account.
- **Product profile is NOT just what the account has -- it's what the account CAN achieve.** The POTENTIAL section shows the ceiling. BEST-IN-CLASS shows actual benchmarks from similar accounts. KNOWN LIMITATIONS shows structural constraints.
- **Config issues are blockers or limiters.** A blocker (red) prevents online sales entirely (MaxAge=0, no eShop). A limiter (amber) reduces potential but doesn't block (low MaxAge, bunches OFF).

### Discovery questions the rep should ask based on what they see

1. **Large offline GMV ($100K+):** "You have $X in offline sales generating $0 in platform fees. What would it take to move even 10% online? That's $Y in new fee revenue."
2. **Many offline buyers (5+):** "You have X buyers who only purchase offline. Are they aware of the eShop? What prevents them from ordering online?"
3. **Low repeat rate (<40%):** "Your repeat rate is X%. The network median for your segment is Y%. Buyers are trying your eShop but not coming back -- what's their experience?"
4. **High concentration (top 5 >50%):** "Your top 5 buyers represent X% of your online GMV. If one churns, the impact is significant. How can we diversify?"
5. **K2K leakage:** "You have X vendors connected via K2K who still buy offline. That's $Y recoverable today -- they already have the connection, they just need to use it."
6. **Offline procurement ($50K+):** "You have $X in offline procurement generating $0 indirect fees. Each vendor you move online generates 1.5% in indirect revenue."
7. **Low variety count (below median):** "You have X varieties online vs the segment median of Y. More varieties = more search results for buyers = more sales."
8. **No long-term inventory:** "Your eShop shows 0% inventory at 30+ days. Buyers planning ahead can't find you."
9. **Bunches OFF:** "Your bunches flag is OFF. This means 95% of retail buyers can't see bunch-level inventory. That's a 5-minute config fix."
10. **eSuite account with low activity:** "As an eSuite account, your addressable market is ~5% of what a Core account can reach. The primary value driver is K2K buying."

### What changed in this improvement cycle (for tracking)

| Fix | What | Why it matters |
|---|---|---|
| Opportunity generator rewritten | 15+ opportunity types (was 9), each with text/type/gmv_impact/priority/action | Opps are now prioritized by impact and have actionable next steps |
| $ impact on all viable opps | Offline GMV, leakage cost, indirect fee potential, offline buyer conversion all quantified | Reps see dollars, not just descriptions |
| Product profile with 3 layers | POTENTIAL + BEST-IN-CLASS (from benchmarks.json) + KNOWN LIMITATIONS per ct_id | Explains what the setup CAN achieve, not just what it has |
| Best-in-class benchmarks wired | Online sell %, repeat rate, variety count, catalog freshness from benchmarks.json per segment | "Your segment median is X, best is Y" -- concrete targets |
| Revenue opps sorted SELL > BUY > LIST | SELL opportunities shown first (highest direct revenue impact) | What matters most appears first |
| Clickable card links | "see BUY card" scrolls to and highlights BUY card within the same row | No more manual scrolling to find the detail |
| Total $ at stake banner | Green banner showing sum of all quantified opportunities | Operator sees the total prize at a glance |
| Next Action uses best action | Uses the action field from highest-priority opp (not just first opp text) | More actionable guidance |
| SELL opps enriched | Offline GMV at $0 fees, low repeat rate with benchmark, high concentration, offline buyers with conversion potential | SELL opps were minimal (2 types), now 5 types |
| BUY opps enriched | Leakage recovery with $, offline procurement at $0 indirect fees | BUY opps were minimal (2 types), now 4 types |
| LIST opps enriched | Category gap detection, below-median variety detection with benchmarks | LIST opps were 4 types, now 5 types with benchmark comparisons |
| eSuite limitations quantified | "~5% of Core TAM", "only 6 of 79 sold in 30 days" | Known limitations are data-backed, not generic |

### Remaining gaps (documented, not invented)

| Gap | Why it can't be fixed now | What would fix it |
|---|---|---|
| Take rate not available per segment | benchmarks.json metric 9_take_rate is NOT_COMPUTABLE (fees not in source files) | Rose: load fee-by-account data from Snowflake |
| CVR benchmarks not shown in profile | GA4 CVR only available for 8 accounts with own hostname | Fix GA4 per-company attribution |
| Hardgoods visibility | No product_type segmentation in any data source | Rose: new query splitting perishable vs hardgoods |
| Temporal benchmarks | All benchmarks are point-in-time | Rose: compute benchmarks per period for trending |
| APPROVED vs DRAFT workflow | All opps shown, no approval gate | Facu decision: define approval workflow |
