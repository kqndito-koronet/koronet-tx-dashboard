# TX Dashboard — Guía Completa

**Fecha:** 2026-08-01
**Para:** Facu (y eventualmente Cata, Christine, Mauro)

---

## QUÉ ES

El centro de operaciones de Transactions. No es un reporte ni un dashboard de BI. Es la herramienta con la que operamos TX: entender qué clientes son prioridad, reconocer oportunidades, y saber exactamente qué hacer con cada cuenta.

**URL:** https://kqndito-koronet.github.io/koronet-scorecard/transactions/
**Objetivo:** que cuando llega Labs → https://tx-scorecard.koronetlabs.com (solo Koronet, detrás de Cloudflare Access)

**Quién lo usa:**
- **Facu** — priorizar cuentas, preparar Chapter TX, review de portfolio
- **Cata/Christine** — entender dónde está la oportunidad de cada cuenta, qué hacer
- **Mauro** — ver trending, entender el estado del negocio
- **CS reps** — (eventualmente) preparar calls con clientes, enablement

---

## LA TESIS DETRÁS DE TODO

Creemos que los wholesalers van a tener más clientes — existentes y nuevos — comprando online con más share of wallet SI:

1. Le ofrecen **TODO lo que quieren** (inventario completo, bunches, a largo plazo, a precios competitivos)
2. O una **manera de pedir lo que les falta**, de manera digital, y le confirman rápido
3. Y después **deliver en las promesas**, fulfilling con calidad

Cada card, cada métrica, cada lead existe para medir si esto está pasando y ayudar a que pase.

---

## QUÉ MUESTRA

### Timeframe selector
Arriba de la tabla. Permite cambiar entre:
- **YTD 2026** (default) — acumulado enero-julio
- **This Month** — solo julio
- **Last Month** — solo junio
- **This Quarter** — Q3 (julio)
- **Last Quarter** — Q2 (abril-junio)

Cuando cambiás el timeframe, los números de Koronet Sell, Koronet Buy, Fees y sus deltas se actualizan. Penetration se anualiza automáticamente para períodos cortos (× 12 para meses, × 4 para quarters).

**Data as of:** 2026-07-30 (se muestra en el banner azul arriba).

### Portfolio view (la tabla antes de abrir una cuenta)

| Columna | Qué muestra | Cómo usarlo |
|---|---|---|
| Account | Nombre + tipo de producto | Identidad |
| Owner | CS owner (de SFDC o Christine) | ¿Quién actúa? |
| Priority | IMPL/P1/CS_P2/TA/WATCH | Filtro principal — arrancar por las de arriba |
| Est Sell GMV | Estimado total de venta del cliente | ¿Cuán grande es el prize? |
| Koronet Sell | GMV sell que pasa por Koronet | Lo que capturamos hoy |
| Penetr. % | Koronet / Est Sell | ¿Cuánto del pie tenemos? |
| Online % | Online / Total Koronet Sell | ¿Cuánto es digital? Color: verde >20%, amber 1-19%, rojo 0% |
| Est Buy GMV | Estimado total de compra | ¿Cuánto sourcing hay? |
| Koronet Buy | GMV buy que pasa por Koronet | Lo que capturamos del lado compra |
| Buy Pen. % | Koronet Buy / Est Buy | ¿Cuánto del sourcing tenemos? |
| Online Buy % | Online / Total Koronet Buy | ¿Cuánto compra digital? |
| Fees | Direct + Indirect sumados | Revenue total de esta cuenta |
| Take Rate | Fees / (Sell + Buy GMV) | Eficiencia de monetización |
| Trend | YoY delta del sell GMV | ▲ verde = creciendo, ▼ rojo = cayendo, — gris = flat |
| eShop CVR | Orders / Visits (GA4) | Calidad del shop. ⚠ solo ~7 cuentas con hostname propio |
| Opps | Cantidad de oportunidades detectadas | ¿Cuántas cosas hay para hacer? |
| SFDC Opp | Salesforce opportunity stage | Contexto comercial |

**Deltas:** Cuando hay data temporal (435 sell, 417 buy companies), cada valor muestra ▲ +X% o ▼ -X% comparando el período seleccionado vs el anterior.

**Filtros:** Priority, Owner, Product type, Config status, Buy %, List %, Sell %, Implementation stage, Search, Has-data-only.

### Al abrir una cuenta — 5 cards expandibles

---

### Card 1: POTENTIAL — "¿Dónde estamos y dónde está el potencial?"

**Objetivo:** Responder en 10 segundos: ¿cuán grande es esta cuenta y cuánto estamos capturando?

**Sell side:**
- Est. Sell (ORA — Christine) — el estimado externo
- Est. Sell (calc) — estimado calculado, con source y confidence
- Koronet Sell — lo que pasa por la plataforma, con penetración %
- Online Sell — lo que es digital, con online %

**Buy side:**
- Est. Buy — calculado del ratio buy/sell real (54% fallback, NO 60-70% flat)
- Koronet Buy — procurement que pasa por Koronet
- Online Buy — procurement digital

**Fees por canal (tabla):**

| Canal | GMV | Fees | Take Rate |
|---|---|---|---|
| eCommerce | $X | $Y | Z% |
| K2K | $X | $Y | Z% |
| API | $X | $Y | Z% ← flag si <0.5% |
| Offline | $X | $0 | 0% |
| Indirect (1.5% × Buy Online) [est.] | — | $Y | — |
| **TOTAL** | — | $Y | — |

🔴 **NOT MONETIZED** flag cuando GMV > $10K pero $0 fees.

**Pre go-live:** Banner azul con "Pre go-live — $XM potential (stage)" para cuentas en implementación.

**SO WHAT:** El gap entre Est GMV y Koronet GMV = la oportunidad que no estamos capturando. Si Est Sell = $50M y Koronet Sell = $5M → 90% del negocio está fuera de la plataforma.

---

### Card 2: OPPORTUNITIES — "¿Cuáles son las oportunidades priorizadas?"

**Objetivo:** Resumen de TODAS las oportunidades de la cuenta — config + revenue — priorizadas por impacto.

**Estructura:**
1. **Revenue opportunities (arriba, prominentes):** BUY, LIST, SELL opps con $ impact cuando hay data. Links a los tabs correspondientes.
2. **Config opportunities (abajo):** blockers (🛑), limiting (⚠️), info (ℹ️)
3. **Bottleneck + Next Action:** el primer obstáculo y qué hacer

**Product profile:** Explica qué significa el tipo de producto de esta cuenta (Core, eSuite, K2K, Proc). Qué puede hacer, qué no puede.

**Lo que se agregó (Aug 1 improvement cycle):**
- ✅ Best-in-class benchmarks por tipo de setup (from benchmarks.json per segment — online sell %, repeat rate, variety count, catalog freshness)
- ✅ Known limitations cuantificadas per product type (eSuite ~5% Core TAM, K2K vendor dependency, etc.)
- ✅ $ impact en 7+ tipos de oportunidades (offline GMV, leakage, indirect fees, buyer conversion potential)
- ✅ 15+ opportunity types (was 9), sorted by priority P1-P3
- ✅ Clickable links to BUY/LIST/SELL cards with scroll + highlight
- ✅ Total $ at stake banner
- ✅ Each opp has actionable next step for reps

---

### Card 3: BUY — "¿Compra por la plataforma?"

**Objetivo:** ¿Este wholesaler está comprando online? ¿Qué le falta para comprar más? ¿Tiene los vendors que necesita?

**Unit economics:**
```
For every 10% more online procurement:
  = $X in procurement
  = $Y in indirect fees (1.5%)
Currently: Z% online → $W in indirect fees
```

**SOURCING — Online vs Offline:**

| | Online | Offline | Gap |
|---|---|---|---|
| Vendors | X | Y | Z sin K2K ($) |
| Categories | X | Y | Z not online |
| Varieties | X | Y | Z not online |
| Procurement $ | $X | $Y | $Y at $0 indirect fees |

**K2K lifecycle (compact):**
```
36 connected → 32 activated (89%) → 28 active L30D · 2 dormant · 2 churned
```
K2K = requiere acuerdo + selling, no es solo infraestructura.

**Leakage:** Vendors CON K2K pero que compran offline → $ recuperable hoy.

**Online Procurement Lead Time:** Bar chart mostrando distribución de anticipación (0-3d / 4-7d / 8-14d / 15-30d / 30d+). Solo online — offline no disponible (falta ORDER_DATE en Snowflake).

**SO WHAT:** Si >60% de las compras son a 0-3 días → ⚠ reactive buying (spot). Si >30% a 15d+ → ✓ forward-planned.

**Order Mix:** Open Market vs Prebook (proxy — viene de SALES_SV, no de procurement).

---

### Card 4: LIST — "¿Qué vende online vs offline?"

**Objetivo:** ¿Lo que esta cuenta vende online es suficiente para atraer y retener buyers? ¿Cuánto se pierde por lo que NO vende online?

**IMPORTANTE — data rule:** No hay data de lo que LISTAN en el eShop (no existe CATALOG_SV). Todo viene de SALES_SV — lo que VENDIERON. Si lo vendieron online, estaba listado. Es un proxy (undercount — lo listado sin ventas no aparece).

**Online vs Offline (lo que vendieron):**

| | Online | Offline | Gap |
|---|---|---|---|
| Categories | X | Y | Z not online |
| Varieties | X | Y | Z not online |
| SKUs | X | — | needs data |

**Variety Freshness (recency of last online sale per variety):**
3 barras horizontales:
- **ONLINE** — distribución de freshness de variedades vendidas online
- **OFFLINE** — distribución de freshness de variedades vendidas offline
- **BEST (Kennicott)** — benchmark

Colores: 🔴 0-3d | 🟠 4-7d | 🔵 8-14d | 🟣 15-30d | 🟢 31-90d | 🟢 90d+

**CUIDADO:** Esto mide CUÁNDO fue la ÚLTIMA VENTA de cada variedad, no la profundidad del inventario. Alto % en 90d+ = variedades que no se venden hace mucho (stale), NO planificación a futuro. Con MaxAge=10 y Future=OFF, las ventas a 90d+ vienen de K2K/prebooks, no del eShop.

**Config features:**
- MaxAge (controla cuántos días muestra en eShop)
- Bunches flag (controla tab de bunches en eShop)

**TAM perdido:** Si bunches OFF → "95% TAM blocked — $X offline bunch GMV not visible online"

**Open Market Sales (proxy):** Cuánto se vendió como Open Market (proxy — viene de SALES_SV, no de uploads reales).

---

### Card 5: SELL — "¿Los clientes compran online?"

**Objetivo:** ¿Los clientes de esta cuenta compran online? ¿Está creciendo, repitiendo o churning?

**RESUMEN:**
```
Online: $X (Y%) | Offline: $Z
Offline: N total buyers, N active L30D | AOV: $X | Churned: N
```

**SO WHAT:** $Z vendiendo offline a $0 direct fees.

**Unit economics:**
```
For every 10% more online buyers (+N buyers):
  = $X in sell GMV
  = $Y in fees (Z% take rate)
Currently: W% online → $V in direct fees
```

**Buyers table:**

| Metric | Online | Offline | Total |
|---|---|---|---|
| Buyers | X | Y | Z |
| Active L30D | X | Y | — |
| AOV | $X | $Y | — |

- **New (this month):** N new buyers (total)
- **Churned:** N buyers (total)
- **Repeat Rate:** X% (online) — con benchmark de red (median, p75)
- **Concentration Top 5:** X% — con $ breakdown (#1: $X · #2: $Y · ...)

**Channel mix:** eCommerce X% · K2K Y% · API Z%

**Format:** Boxes X% · Bunches Y% · Short Z% / Forward W%

**GA4 eShop:** Sessions → Transactions = X% conversion. Best-in-class: Kennicott ~22.8%. Solo funciona para ~7 cuentas con hostname propio (93% usa app.kometsales.com compartido).

**Retention:** N active L30D / total online buyers = X%

---

## TABS 2-6 (estructura de 4 capas)

Cada tab temático (CONFIG, BUY, LIST, SELL, GROWTH) tiene:

1. **Leads + reasoning** — qué es la oportunidad y POR QUÉ
2. **Lead list** — qué cuentas aplican (DRAFT o APPROVED)
3. **Enablement** — cómo ayudar al rep (qué decir, qué hacer)
4. **Case studies** — prueba de que funciona con cuentas reales

**Estado actual:** 23 leads definidos (todos DRAFT), parcialmente conectados. Enablement y case studies en progreso.

---

## DATA

### Sources (22+ JSON files)

| Categoría | Archivos | Companies | Freshness |
|---|---|---|---|
| **Temporal sell/buy** | metrics_v2_sell.json, metrics_v2_buy.json | 435/417 | Jul 30, 2026 |
| **Temporal fees** | metrics_v2_fees.json | 244 (network + top 15) | Jul 30, 2026 |
| **Snapshot metrics** | metrics.json | 112 (28%) | H1 2026 |
| **Config** | config.json | 397 (99.5%) | Jul 30, 2026 |
| **Buyers full** | metrics_v2_buyers_full.json | 342 | Jul 31, 2026 |
| **Concentration Top 5** | buyer_concentration_v2.json | 284 | Jul 30, 2026 |
| **Time depth offline** | time_depth_offline.json | 205 + Kennicott benchmark | H1 2026 |
| **Benchmarks** | benchmarks.json | 9 metrics, by segment | Jul 30, 2026 |
| **Accounts** | accounts.json | 399 | Jul 30, 2026 |
| **Vendors** | vendor_detail.json | 336 | H1 2026 |
| **GA4** | ga4_eshop.json | 29 hostnames | Jul 2026 |
| **+ 11 más** | supply_matrix, bunches, sfdc, etc. | varies | H1 2026 |

### Data rules
- Todo viene de Snowflake (PRODUCTION.ANALYTICS.*)
- ks_flag=TRUE siempre
- Dump accounts excluidos (R12)
- Online = eCommerce + K2K + API
- Buy usa total_cost (no extended_cost)
- Fees = TRANSACTION_FEES (histórico) + EXPECTED (mes actual)
- Axerrio se suma aparte (~$54K H1)

### Limitaciones conocidas
- **28% coverage** en metrics.json (112/399 cuentas) — inaceptable, en progreso
- **No hay data de lo que LISTAN** — solo de lo que vendieron (proxy)
- **GA4 CVR solo para ~7 cuentas** — 93% usa hostname compartido
- **No hay ORDER_DATE** — no podemos medir anticipación real
- **No hay New User CVR** — necesita GA4 segmentado
- **No hay hardgoods** — sin product_type segmentation
- **Ninfa Flowers** tiene $562M artifact en buy 2025 — YoY buy inflado

---

## EL LOOP

### Qué es
Un ciclo continuo que hace que el dashboard mejore TODOS LOS DÍAS. No es un cron — es un sistema que PIENSA, CONSIGUE DATA, CONSTRUYE, VERIFICA, y APRENDE.

### 3 capas que corren juntas

**INCORPORATE (el sistema nervioso):**
- Lee feedback de Facu (Slack DM, chat, TX_FACU_FEEDBACK_AUG1.md)
- Lee calls nuevas (Fathom)
- Lee Slack threads relevantes
- Incorpora deep dives, learnings, señales de stakeholders
- Checkea si hay data nueva disponible

**THINK (el cerebro):**
- Lee TODO el scope v2 (objetivo del dashboard + objetivo de cada card)
- Audita UNA card contra scope + feedback + audits anteriores
- ¿Cada número tiene SO WHAT? ¿Lo usaría Cata?
- ¿Es consistente con otras cards?
- ¿Los labels son honestos? ¿La data mide lo que dice?
- ¿Hay data cargada pero no mostrada?
- Flag discrepancias (como MaxAge vs time depth)
- Genera enablement notes para esta card

**SHIP (el motor):**
- Si falta data → Rose microagent contra Snowflake
- Si la data existe → Codex verifica → builder integra
- Audit post-build: ¿lo que buildeé matchea scope?
- Push → verify deployed → reportar

### Cadencia

**Un ciclo por card (2-3 horas cada uno):**
```
POTENTIAL → OPPORTUNITIES → BUY → LIST → SELL → PORTFOLIO → META REVIEW
```

**Meta review (después de cada ronda completa):**
- ¿Los números son consistentes entre cards?
- ¿El dashboard cumple su objetivo?
- ¿Qué le falta al dashboard?
- ¿Qué aprendió el loop que mejora el próximo ciclo?
- Priorizar gaps para la próxima ronda

**Después de la meta review → volver a empezar.**
Pass 1 arregla lo obvio. Pass 2 atrapa lo que pass 1 no vio. Pass 3 mejora lo que pass 2 construyó. Espiral, no checklist.

### Cómo mejora el loop MISMO

Cada pasada completa genera:
- Errores que cometió → reglas nuevas para no repetir
- Data que faltó → Rose backlog para la próxima pasada
- Feedback de Facu → se aplica Y se generaliza
- Enablement notes → alimentan las tabs 2-6
- Discrepancias → se documentan como data rules

El loop de la pasada 3 es MEJOR que el de la pasada 1 porque aprendió de los errores.

### Autonomía progresiva

```
AHORA (L0): Facu triggerea → agentes ejecutan → Facu revisa
SEMANA 2 (L1): Loop corre automático → Facu revisa outputs
SEMANA 4 (L2): Pre-meeting briefs automáticos → Post-call capture
MES 2+ (L3): Loop propone mejoras → Facu aprueba excepciones
```

Cada nivel se gana con runs limpios + Facu confirmando que sirve.

---

## ESTADO ACTUAL (Aug 1, 2026)

### Lo que funciona
- ✅ 399 cuentas con filtros
- ✅ Data temporal sell/buy (435/417 companies, as of Jul 30)
- ✅ Timeframe selector (YTD/Month/Quarter) con deltas
- ✅ Buy columns en portfolio
- ✅ Trend column con YoY real
- ✅ 5 cards expandibles con data
- ✅ Fees por canal con NOT MONETIZED flags
- ✅ Unit economics en BUY y SELL
- ✅ Vendor table restructurada (sourcing + K2K lifecycle)
- ✅ Time depth con online/offline/best-in-class
- ✅ Offline buyers, AOV, L30D, churn (342 companies)
- ✅ Concentration Top 5 (284 companies)
- ✅ Benchmarks por métrica y segmento
- ✅ 23 leads definidos (DRAFT)
- ✅ Labels honestos sobre proxy data y limitaciones

### Lo que falta
- ❌ Labs publish (rol groot:labs-user pendiente)
- ❌ eShop CVR per company (93% excluidas por hostname compartido)
- ❌ New User CVR
- ❌ Anticipación real (ORDER_DATE no existe)
- ❌ Hardgoods visibility
- ✅ Best-in-class benchmarks en OPPORTUNITIES card (wired from benchmarks.json per segment, Aug 1)
- ❌ Trust labels (DRAFT/APPROVED workflow)
- ❌ Coverage >80% (hoy 28% en metrics snapshot)
- ❌ Data refresh automatizado
- ❌ Meeting prep automático
- ❌ Post-call capture de Fathom
- ❌ Daily TX Change Report

### El loop está corriendo
36 horas non-stop: card por card, pasada tras pasada. Cada ciclo lee todo el scope, piensa, consigue data, buildea, pushea, aprende. Te reporto con cada push.

---

## ARCHIVOS CLAVE

| Archivo | Qué es |
|---|---|
| `docs/transactions/index.html` | EL dashboard |
| `docs/transactions/TX_DASHBOARD_SCOPE_V2.md` | Spec autoritativo (feedback de Facu) |
| `docs/transactions/TX_FACU_FEEDBACK_AUG1.md` | Feedback por card con enablement notes |
| `docs/transactions/TX_DASHBOARD_P0_NO_LOSS_INVENTORY.md` | Inventario de 181 campos + 53 gaps |
| `docs/transactions/TX_LEAD_DEFINITIONS_DRAFT.md` | 23 leads con 4-layer structure |
| `docs/transactions/TX_LOOP_STATE.md` | Estado del loop (qué card, qué pass) |
| `docs/transactions/TX_SESSION_ERRORS_AND_LOOP_DESIGN.md` | Errores + diseño del loop |
| `docs/transactions/TX_DASHBOARD_COMPLETE_GUIDE.md` | Este archivo |
| `docs/transactions/data/*.json` | 28 archivos de data |

---

## CÓMO DAR FEEDBACK

1. **En este chat** — lo aplico en el ciclo actual
2. **En Slack DM** — lo leo en el próximo ciclo del loop
3. **Abrí el dashboard y decime qué no tiene sentido** — "esta cuenta muestra X pero debería mostrar Y"

Todo el feedback se loguea en `TX_FACU_FEEDBACK_AUG1.md` organizado por card, con enablement notes para las tabs 2-6.
