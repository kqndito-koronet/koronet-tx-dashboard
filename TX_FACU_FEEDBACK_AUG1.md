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
| OPP1 | NO es solo config — es ALL opps priorizadas (config + BUY/LIST/SELL) | ✅ restructured | Para ENABLE: this card is "por qué deberías actuar en esta cuenta" — the executive summary |
| OPP2 | Para cada product type: potencial, best-in-class, known limitations | ❌ no benchmarks per type | Para ENABLE: "cuentas Core con bunches + time depth 90d+ tienen 22% CVR" — this IS the pitch |
| OPP3 | Opps sin $ cuantificado no son opps | partial (bunches has $) | Para ENABLE: every opp = "if you fix X → $Y impact" — that's what makes CS act |
| OPP4 | LIST opps never generated | ✅ 4 types added | — |
| OPP5 | Estructura invertida: opps primero, config después | ✅ | — |
| OPP6 | Links a los tabs correspondientes | partial | Para ENABLE: click → goes to the playbook for that opp type |

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
