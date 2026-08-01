# TX Dashboard — Scope v2

**Date:** 2026-07-31
**Author:** Facu (direct feedback), captured verbatim with context
**Status:** AUTHORITATIVE — this file governs all dashboard work until superseded
**Loop rule:** Every build session reads this file first. Every feedback session updates it. The dashboard improves because this document improves.

---

## What this dashboard IS

Centro de operaciones para Cata, Facu y Christine. No es un reporte, no es un dashboard de BI, no es un proyecto de data. Es la herramienta con la que operamos Transactions.

### Tres usos concretos:

1. **Entender qué clientes son prioridad** — antes de abrir cualquier cuenta, ver el potencial y las oportunidades a nivel portafolio
2. **Reconocer nuevas oportunidades** — detectar cuentas donde hay potencial que no estamos capturando, para marcarlas como prioridad
3. **Entender del cliente dónde está la oportunidad** — una vez que abrís la cuenta, ver exactamente dónde está el gap y qué hacer

### Navegación desde la cuenta:
- Links a **oportunidades específicas** en las tabs de CONFIG/BUY/LIST/SELL/GROWTH
- Link al **full Account Deep Dive** (más profundidad, historia, diagnóstico completo)
- **Surfaces para mostrar a los clientes** — para influenciarlos con data de su propia cuenta

### Qué NO es:
- No reemplaza el pacing dashboard
- No es board/TAM opportunity sizing
- No toma decisiones estratégicas — muestra evidencia, Facu/Cata/Christine deciden

---

## Tab 1: ACCOUNTS (Cockpit)

### Objetivo
La vista de portafolio. Antes de abrir una cuenta, tenés que entender muy bien:
- El **potencial de buy y de sell** de cada cuenta
- **Dónde hay oportunidades y CUÁNTAS** — pero no necesariamente cuáles (eso está en las cards y las tabs)

Esto permite decidir en qué cuentas enfocarse y por qué.

### Columnas del portfolio view (ANTES de abrir una cuenta)

La tabla principal muestra estas columnas. El objetivo es que sin abrir la cuenta ya entiendas potencial, captura, y si está creciendo o no.

| Columna | Qué muestra | Por qué importa |
|---|---|---|
| Account | Nombre + product type tag | Identidad |
| Owner | CS owner | Quién actúa |
| Priority | IMPL/P1/CS_P2/TA/WATCH/etc | Filtro principal |
| Est Sell GMV | Estimado total de venta del cliente | Tamaño del prize |
| Koronet Sell | GMV sell que pasa por Koronet | Lo que capturamos hoy |
| Sell Penetration % | Koronet Sell / Est Sell | ¿Cuánto del pie tenemos? |
| Online Sell % | Online / Total Koronet Sell | ¿Cuánto es digital? |
| **Est Buy GMV** | Estimado total de compra del cliente | **MÁS IMPORTANTE que el detalle de config/buy/list/sell** |
| **Koronet Buy** | GMV buy que pasa por Koronet | Lo que capturamos del lado compra |
| **Buy Penetration %** | Koronet Buy / Est Buy | ¿Cuánto del sourcing tenemos? |
| **Online Buy %** | Online / Total Koronet Buy | ¿Cuánto compra digital? |
| Fees (Total) | Direct + Indirect fees sumados | Revenue total de esta cuenta |
| Take Rate | (Direct Fees + Indirect Fees) / (Sold GMV + Bought GMV) | Eficiencia de monetización |
| # Opps (approved) | Cantidad de oportunidades APPROVED (no DRAFT) | ¿Cuántas oportunidades confirmadas tiene? |
| SFDC Opps | Link a Salesforce opportunities | Contexto comercial |
| **Trend** | Alguna noción de growing/flat/declining | **¿Está mejorando o no?** Sin esto no priorizás bien |
| **eShop CVR %** | Orders / Visits (de GA4) | **Calidad del shop.** Con percentil o banda (bueno/mediano/malo). Ver nota abajo. |

**Cambios clave vs versión anterior:**
- **BUY columns son más importantes** que el detalle inline de config/buy/list/sell scores. El detalle está en las cards.
- **Sacar** las columnas de config/buy/list/sell mini-scores del portfolio view — eso se ve al abrir la cuenta
- **Fees = una sola columna** (direct + indirect sumados), no separados
- **Take Rate = (fees + indirect) / (sold + bought GMV)** — la métrica de eficiencia
- **# Opps = solo APPROVED** — no mostrar drafts en el portfolio, eso es ruido
- **Trend = obligatorio** — growing/flat/declining en las métricas principales. Sin esto no sabés si una cuenta está mejorando o empeorando.
- **eShop CVR % = conversion rate del eShop** (orders / visits, de GA4). Muestra claramente la calidad del shop.

### eShop Conversion Rate — detail

**La tesis detrás de todo esto:**
Creemos que los wholesalers van a tener más clientes — existentes y nuevos — comprando online con más share of wallet SI:
1. Le ofrecen **TODO lo que quieren** (inventario completo, bunches, a largo plazo, a precios competitivos)
2. O una **manera de pedir lo que les falta**, de manera digital, y le confirman rápido
3. Y después **deliver en las promesas**, fulfilling con calidad

La CVR es el termómetro de esta tesis. Si ofrecés todo lo que quieren → más conversion. Si cumplís → más repeat. Si crecés el catálogo y la experiencia → más new users que convierten.

**Por qué es tan importante operacionalmente:** La conversion rate te muestra la CALIDAD del shop de un vistazo. Si tienen tráfico pero no convierten, el problema es el inventario/pricing/UX, no el marketing.

**Qué mostrar:**
- **CVR % total** (orders / visits)
- **New User CVR %** (conversión de visitantes nuevos — critical para growth)
- **Percentil o banda:** bueno / mediano / malo. Benchmarks del network:
  - Kennicott: ~22.8% (best in class)
  - Sole Farms: ~47% (outlier alto)
  - Promedio network: TBD (Rose needs to compute)

**Trending es CRÍTICO acá:** La CVR y la New User CVR tienen que mostrar si MEJORAN. Un shop que mejora su CVR está haciendo algo bien (mejor inventario, mejor pricing, más SKUs, más variedades, más time depth). Un shop con CVR flat o declining tiene un problema.

**Lo que todavía no estamos teniendo en cuenta (pero es el goal):**
- Los SKUs y varieties importan, pero dependen de QUÉ SKUs y A QUÉ PRECIOS
- Un eShop con 1000 SKUs pero todos al mismo precio y sin variedad relevante no convierte
- El goal es entender que SKUs × Variedades × Pricing × Time Depth = la calidad del shop
- Hoy medimos cantidad. Eventualmente tenemos que medir calidad (relevancia, competitividad de precio, seasonal coverage)
- Esto alimenta los leads de LIST y SELL

**Caveat GA4:** hostname ≠ eShop performance. Solo Kennicott usa su propio dominio. Para 117 de 150 companies, los buyers entran por app.kometsales.com. La CVR por hostname puede ser misleading — hay que computar por company, no por host.

### Filtros necesarios
Prioridad (IMPL, P1, CS_P2, TA, WATCH, etc.), producto (Core/eSuite/K2K/Proc), owner, estado de implementación.

### Al abrir una cuenta — 5 cards expandibles

---

### Card 1: POTENTIAL

**Objetivo:** Responder "¿dónde estamos ahora y dónde está el potencial?"

**Por qué es fundamental:** El Est Sell GMV y el Est Buy GMV son las métricas más importantes del dashboard. Nos ayudan a descubrir oportunidades DONDE NO LAS VEMOS — porque no tenemos todo el ecosistema del cliente en la plataforma. Sin Est GMV, no podemos ni empezar.

**Facu flow:** Est GMV → Koronet GMV → Online % → ENTONCES drill into cards

**Lo que muestra:**
- Est Sell GMV (ORA + calc) — con source label
- Est Buy GMV (del ratio real buy/sell de la cuenta, 54% fallback — NO 60-70% flat)
- Koronet Sell/Buy actual
- Penetración % (denominador = total buying/selling estimado, no solo Koronet)
- Online %
- Fees breakdown por canal con take rate por canal (para detectar canales sin monetizar)
- Para pre-go-live: "Pre go-live — $XM potential" en azul, no como si fuera actividad fallida
- Para IMPL: mostrar implementation stage

**Feedback aplicado (Jul 31):**
- Si ORA y calc son la misma fuente → mostrar una vez con label
- Fee breakdown por canal como tabla: Channel | GMV | Fees | Take Rate | flags NOT MONETIZED
- Flag canales con GMV > $10K pero $0 fees (gap de API)

---

### Card 2: OPPORTUNITIES (antes era CONFIG)

**Objetivo:** Responder "¿cuáles son las oportunidades de esta cuenta, priorizadas?"

**Cambio fundamental (Jul 31):** Esta card NO es solo config. Es el resumen priorizado de TODAS las oportunidades de la cuenta — tanto de configuración como de revenue, BUY, LIST, SELL. Priorizadas por impacto.

**Lo que debería mostrar:**
- Para cada **tipo de setup** (combinación de productos: Core, eSuite, K2K, Proc, IMPL):
  - Explicar el **potencial** de ese tipo de setup
  - **Best in class** de cuentas similares (ej: "cuentas Core con bunches + time depth 90d+ tienen 22% conversión")
  - **Known limitations** del setup (ej: "eSuite = addressable market ~5% de Core, solo boxes, no standing orders")
- Oportunidades de CONFIG: blockers, settings que faltan, features no activados
- Oportunidades de revenue: BUY, LIST, SELL — un resumen con link al tab correspondiente
- Bottleneck principal y next action

**Esto tiene trabajo por hacerse.** La lógica de priorización, los benchmarks por tipo de setup, y las limitations conocidas necesitan ser definidos como leads.

**STATUS:** DRAFT — no tiene la estructura completa todavía

---

### Card 3: BUY — ¿Compra por la plataforma?

**Objetivo:** Responder "¿este wholesaler está comprando online? ¿Qué le falta para comprar más? ¿Tiene los vendors que necesita?"

**Unit economics:** Le gusta. "10% más online = $X procurement = $Y fees" — cuantifica el impacto de mover volumen online.

**Comparativa online vs offline — filas que FALTAN en la tabla actual:**

| Fila | Online | Offline | Gap | Por qué importa |
|---|---|---|---|---|
| Total vendors | X | Y | Z | ¿Cuántos proveedores tiene en total? |
| Connected (not activated) | X | — | — | ¿Creó la conexión K2K pero nunca activó? |
| Active (L30D) | X | Y | Z | ¿Cuántos proveedores usa regularmente? |
| Churned/Lapsed (L30D) | X | Y | — | ¿Perdió proveedores online recientemente? |
| Categories | X | Y | Z | GAP = oportunidad de nuevas categorías online |
| Varieties | X | Y | Z | |
| SKUs | X | Y | Z | |

**El GAP entre online y offline muestra la oportunidad.** K2K connections deberían ser obvias si aumentás las filas de vendors.

**Bunches NO va en BUY.** Los wholesalers venden bunches pero no compran bunches — bunches es un concepto de LIST/SELL. En BUY comprás boxes de importers/farms.

**LO QUE FALTA — cobertura de vendors:**
- ¿Tiene vendors que le permiten cubrir el % de TAM de categorías a corto/largo plazo ONLINE?
- Objetivo: mostrar claramente DÓNDE NO tiene vendors buenos
- Ejemplo: "Necesita un vendor de X categoría a corto plazo y solo tiene uno"
- En el deep dive o leads: más depth → seasonal products (se quedan sin), exclusive products, brands
- Objetivo claro: **online y offline, ¿qué % de tus compras son a corto, medio y largo plazo? ¿PODÉS comprar online de todo lo que necesitás o necesitás más vendors?**

**Open Market + Prebook:** No como conceptos separados sino dentro del nuance de **PARA CUÁNDO COMPRAN** — online y offline. ¿Compra para mañana (spot)? ¿Para la semana que viene? ¿Para el mes que viene? ¿Para la temporada?

**Anticipation data (7/14/30/+30D):** Bloqueado porque falta `CREATED_DATE` en el semantic view. Cuando esté disponible, esto completa la vista temporal.

---

### Card 4: LIST — ¿Lo muestra online?

**Objetivo:** Responder "¿lo que esta cuenta muestra online es suficiente para atraer y retener buyers? ¿Cuánto TAM se pierde por lo que NO muestra?"

**Resumen y SO WHAT — lo que falta:**
- ¿Se muestra a **largo plazo**? (no solo on-hand / spot)
- ¿Se muestra en **bunches**? (si no → solo 3-5% del TAM de retail es addressable)
- Estimar el **TAM que se pierden** — desde el offline breakdown si usan Core, o desde best in class

**Time Depth bar chart — ENCANTA:**
```
10% ■ 0-3d
13% ■ 4-7d
13% ■ 8-14d
18% ■ 15-30d
31% ■ 31-90d
16% ■ 90d+
· 5600 varieties total
```
Pero el actual es casi seguro **offline**. Hay que tener TRES versiones:
1. **Online** — lo que muestra en el eShop
2. **Offline** — lo que tiene en el cooler/sistema pero NO muestra
3. **Best in class** — cómo se ve una cuenta que lo hace bien (Kennicott)

La diferencia entre online y offline = la oportunidad. La diferencia con best in class = el target.

**Config features que ayudan a vender a largo plazo:**
- MaxAge (10 días bloquea 47% del volumen de Kennicott)
- sold_as_future
- Standing Orders (solo Core — decisión Jul 31: no va a eSuite)
- Bunches flag

**Open Market — CLARIFICACIÓN IMPORTANTE:**
Open Market en LIST es lo que el wholesaler **SUBE** (publica como disponible), no lo que compra. Confundir esto con BUY es un error conceptual. En LIST = "¿está publicando inventario futuro en Open Market?"

**Tabla de TAM perdido:**
Muestra el TAM que se pierden por no enable bunches → hacer **lo mismo por categorías, variedades, SKUs y timeframe**. Cada dimensión que no está online = volumen invisible para los buyers.

**9 Challenger Messages (existen en definitions.json):**
Estos son enablement para CS — cómo explicarle al wholesaler POR QUÉ importa listar más. Ejemplo: "Lo que no está en tu eShop no existe para tus clientes online." Van en la capa de enablement del tab LIST.

---

### Card 5: SELL — ¿Los clientes compran online?

**Objetivo:** Responder "¿los clientes de esta cuenta compran online? ¿Está creciendo, repitiendo o churning? ¿Cuánto falta para capturar el potencial?"

**Unit economics como BUY:** Cuantificar el impacto. "X% más buyers online = $Y GMV = $Z fees"

**Algunos ejemplos con link a leads de best in class:** Mostrar qué hacen las cuentas que mejor convierten (Kennicott 22.8%, Sole 47%) y linkear a los leads correspondientes.

**Tabla de metrics — ENCANTA:**
- Online vs Offline: Buyers, GMV, AOV
- L30D activity
- New Q2, Churned Q2
- Repeat Rate
- **Concentration: Top 5** — y FALTA todo lo offline (no tenés % conversión offline, obvio)
- Format: Boxes % / Bunches % / Short / Forward

**Channel mix:** En chiquito, dejarlo — especialmente porque si venden por API hay algo que analizar (Mayesh API a 0%, BySpeaks 3.5%)

**Hardgoods:** Alguna idea debería dar visibilidad a hardgoods. No es flowers pero es volumen.

**Retention:** Le gusta, se queda. Online retention over time.

**GA4 conversion:** Cuidado — hostname ≠ eShop performance. Solo Kennicott usa su dominio. Para los demás, el tráfico entra por app.kometsales.com. Los metrics de GA4 por hostname pueden ser misleading.

---

## Tabs 2-6: Estructura de 4 capas

Cada tab temático (CONFIG, BUY, LIST, SELL, GROWTH) tiene la misma estructura:

### Capa 1: Define leads + reasoning
Qué es la oportunidad y POR QUÉ es una oportunidad. No es una lista de métricas — es un argumento con evidencia.

### Capa 2: List for review
Qué cuentas aplican para este lead. Cada lead marcado como **DRAFT** o **APPROVED**. DRAFT = propuesta, no actuar. APPROVED = Facu revisó y confirmó, el team puede actuar.

### Capa 3: Enable reps
Cómo ayudar al rep a influenciar/activar al cliente. Talk tracks, discovery questions, checklists, objeciones comunes. El rep necesita saber QUÉ DECIR y QUÉ HACER, no solo ver data.

### Capa 4: Case studies / examples
Prueba de que funciona, con cuentas reales. "Kennicott hace X y logra Y." "Maple Grove habilitó Z y el resultado fue W." Esto es para que el rep SE LO CREA y pueda mostrar al cliente.

---

## CRITICAL REQUIREMENTS (non-negotiable)

### 1. Data actual con timeframes dinámicos y deltas

**El problema:** Los stakeholders ven números de junio en agosto. No confían en el dashboard porque la data no está up to date. Sin data actual, sin timeframes seleccionables, y sin deltas que muestren dirección, el dashboard es un poster, no una herramienta operativa.

**Lo que necesita:**

#### A. Estructura temporal en CADA métrica (no snapshots)

```json
{
  "sell_total": {
    "current_month": 45000,
    "prior_month": 42000,
    "current_quarter": 130000,
    "prior_quarter": 118000,
    "ytd": 259192,
    "prior_ytd": 195000,
    "as_of": "2026-07-29",
    "deltas": {
      "mom": 7.1,
      "qoq": 10.2,
      "yoy": 32.8
    }
  }
}
```

Esto aplica a TODAS las métricas de TODAS las cards:
- **POTENTIAL:** Sell GMV, Buy GMV, Penetration, Online %, Fees por canal
- **BUY:** Proc online, vendors activos, categories online, anticipation
- **LIST:** Time depth, varieties, SKUs, Open Market uploads
- **SELL:** Buyers, CVR, New User CVR, Repeat Rate, Concentration, AOV, Retention
- **Portfolio:** Trend column = resumen de todo lo anterior

#### B. Selector de timeframe en la UI

```
[ This Month ▾ ]  [ vs Prior Month ▾ ]

Sell GMV:  $45K  ▲ +7.1% MoM
Buy GMV:   $24K  ▲ +12% MoM
Fees:      $680  ▼ -2% MoM
CVR:       18%   ▲ +3pp MoM
```

El usuario elige el período base y el período de comparación. Los deltas se recalculan. No es un número fijo — es una vista dinámica. Color coding: verde sube, rojo baja, gris flat.

#### C. Data refresh pipeline

```
Hoy (roto):     Rose query manual → JSON snapshot → git commit → deploy manual
Necesario:       Rose query scheduled → JSON con períodos → auto-refresh → dashboard actualizado

Frecuencia: mínimo semanal, ideal diario (martes-viernes post fee data)
Constraint: TX fee process corre los lunes, data en Snowflake los martes.
```

#### D. Lo que esto requiere

**De Rose (P0.2):**
- Cada query trae data POR PERÍODO (mes, quarter, año)
- El JSON tiene la estructura temporal de arriba
- Los deltas se pre-computan en el JSON
- Cada valor lleva as_of date para que el dashboard muestre cuán fresca es la data

**Del builder:**
- Selector de timeframe en la UI (dropdown o tabs)
- Lógica para mostrar el delta según la comparación elegida
- Color coding automático
- Badge de freshness: "Data as of Jul 29" visible en todo momento

**De Nahua:**
- Wire la pipeline: Rose query → JSON → deploy a koronetlabs.com
- Scheduling (cron o trigger) para que corra sin que nadie lo pida

**Esto es lo que va a hacer que los stakeholders confíen.** Sin data actual con deltas, no podemos operar.

### 2. Trust labels
Solo dos: **DRAFT** y **APPROVED**. Simple. Un lead está en borrador o Facu lo aprobó para que el team actúe. Cuando el sistema madure y haya más gente operando, se puede agregar granularidad.

### 3. Data coverage audit (hard requirement)

112 de 399 cuentas tienen data = 28% coverage. Esto es inaceptable como superficie de decisión.

**Regla dura:** El loop tiene que auditar y llenar data ANTES de cada build.

```
ANTES de cada build session:
  1. ¿Cuántas cuentas tienen data? → target: 100% de priority accounts, >80% total
  2. ¿Qué métricas faltan por cuenta? → priorizar por prioridad de cuenta
  3. Rose llena gaps PRIMERO — no se construye sobre data incompleta
  4. NO presentar el dashboard como superficie de decisión con <80% cobertura
  5. Cada cuenta sin data = un ticket para Rose, no un "N/A" en el dashboard
```

**El loop tiene que asegurar que eventualmente tenemos ALL THE DATA.** Cada ciclo cierra gaps. Si una métrica no existe en Snowflake, se documenta como gap de producto — no se ignora.

### 4. El loop (cómo el dashboard mejora)

```
FEEDBACK LOOP:
  Cada review → feedback capturado en este doc (sección Feedback Log)
  Cada build session → lee este doc primero, aplica feedback
  Cada call con clientes → insights alimentan leads + enablement
  Cada ciclo 2 semanas → tracking: qué cuentas fueron prioridad, qué pasó, qué aprendimos, qué cambió
  Learnings → se aplican al siguiente ciclo

IMPROVEMENT LOOP:
  Dashboard muestra data → Facu/Cata/Christine revisan
  → Detectan gaps/errores/oportunidades nuevas
  → Feedback se captura
  → Build session aplica feedback
  → Dashboard mejora
  → Repeat
```

El dashboard no tiene que ser perfecto. Tiene que ser correcto donde importa y mejorar cada semana.

---

## Daily TX Change Report (the loop that ties everything together)

### Qué es
Una superficie diaria publicada en koronetlabs.com que muestra qué cambió, qué se aprendió, y qué proponen los agentes. No es un reporte pasivo — es el motor de evolución del sistema.

### Consumers
Facu, Cata, Christine, Mauro, CS reps

### Lo que produce CADA DÍA (martes a viernes, post-data refresh)

#### A. Account changes (Rose)
- Por cada target account: métricas de hoy vs ayer/semana pasada
- Red flags: 0TX accounts, declining CVR, lost vendors, inactive eSuite
- Nuevas oportunidades detectadas por data

#### B. Call intelligence (Pablito + Mercurio)
- Insights de las calls del día anterior (Fathom) por cuenta
- Commitments hechos, follow-ups pendientes, blockers surfaceados
- Señales de cliente: qué pidieron, qué rechazaron, qué les frustra

#### C. Play proposals & evolution (Mercurio)
- **Propuestas de plays NUEVAS** basadas en lo que se ve en la data y las calls
- **Evolución de plays existentes** — qué funcionó, qué no, qué ajustar
- **Enablement proposals** — talk tracks nuevos, discovery questions, objeciones
- Cada propuesta como DRAFT → Facu aprueba o rechaza → el sistema aprende
- **ESTO TIENE QUE ESTAR CONSTANTEMENTE EVOLUCIONANDO** — no es un one-time. Cada día el sistema propone algo mejor.

#### D. Deep dive execution & feedback (Mercurio + Rose)
- Client deep dives ejecutadas: qué se encontró, qué se aprendió
- **Mercurio aprende de cada deep dive** — patterns across accounts, qué funciona en qué tipo de cuenta, qué preguntas revelan más, qué data falta siempre. Cada deep dive hace a Mercurio mejor para la siguiente.
- **Feedback de cómo hacer deep dives mejor** — el proceso mismo mejora
- Queue de deep dives pendientes priorizada por oportunidad

#### E. Stakeholder influence (Socrates)
- **¿Qué hacen los stakeholders (CS, Impl, Product) que NO hacen?**
- ¿Cómo convencerlos? Argumentos, data, framing
- ¿Qué decisiones necesitan de quién para desbloquear cuentas?
- ¿Qué change management es necesario? (no solo herramientas — personas)

#### F. Leads, plays & enablement evolution
- Status de cada lead definition: DRAFT / APPROVED / cuántas cuentas aplican
- Nuevos leads propuestos vs leads que no funcionaron
- Enablement content: qué se agregó, qué falta, qué se mejoró
- Case studies nuevos: qué cuenta probó qué play con qué resultado
- **Esto está muy lejos todavía — pero el loop tiene que hacer que evolucione TODOS LOS DÍAS.** Cada call, cada interacción, cada dato nuevo alimenta la mejora de leads y enablement.

### El loop de evolución continua

```
CADA DÍA:
  Rose detecta cambios en data
  Pablito captura insights de calls y Slack
  Mercurio propone plays, mejoras a plays, enablement
  Socrates analiza stakeholder behavior + change management
  → TODO converge en el Daily TX Change Report
  → Facu/Cata/Christine revisan
  → Aprueban/rechazan/ajustan propuestas
  → El sistema incorpora el feedback
  → Al día siguiente el reporte es MEJOR

CADA SEMANA:
  Plays que funcionaron → case studies
  Plays que fallaron → anti-patterns
  Leads que se validaron → APPROVED
  Leads que no aplican → archived con razón
  Deep dives que revelaron algo → learnings compartidos
  Stakeholder moves que funcionaron → playbook de influencia

CADA 2 SEMANAS:
  Ciclo completo: qué cuentas fueron prioridad, qué pasó,
  qué aprendimos, qué cambió en el sistema
  → El dashboard refleja los cambios
  → Los leads y enablement están actualizados
  → Las próximas 2 semanas arrancan con mejor información
```

### Por qué esto es diferente a los loops que ya existen

Los 42 scripts que corren hoy producen HTMLs que nadie consume. Este loop es diferente porque:
1. **Produce UNA superficie visible** para todos los consumers
2. **Incluye propuestas activas** (Mercurio + Socrates), no solo data
3. **Tiene un feedback mechanism** — Facu aprueba/rechaza, el sistema aprende
4. **Evoluciona leads y enablement** cada día, no cada quarter
5. **Conecta calls + data + strategy** en un solo lugar

---

## What comes next (in order)

### P0.0 (NOW): No-loss inventory
- Inventario completo de cada campo de Tab 1 → source, coverage, disposition vs scope v2
- EN PROGRESO — 7 microagentes completaron, assembler armando el archivo final

### P0.1: Structural shell
- Portfolio columns nuevas (Buy columns, Fees sumadas, Take Rate, Trend, CVR)
- Card 2 renombrada a OPPORTUNITIES
- Coverage/freshness notice visible
- Placeholders seguros para valores que Rose no trajo todavía
- NO inventa valores, NO cambia data — solo estructura

### P0.2: Data contract con estructura temporal
- Rose pull con CADA métrica en formato temporal (current_month, prior_month, current_quarter, prior_quarter, ytd, prior_ytd, as_of, deltas)
- Aplica a TODAS las cards: POTENTIAL, BUY, LIST, SELL, Portfolio
- Coverage audit: target 100% priority accounts, >80% total
- Codex verifica data contract

### P0.3: Dashboard con timeframes dinámicos
- Selector de timeframe en la UI (período base + período de comparación)
- Deltas que se recalculan según la selección
- Color coding: verde sube, rojo baja, gris flat
- Badge de freshness: "Data as of [date]"
- 5 product profiles representativos pasan todos los checks

### P0.4: Facu browser review
- Facu acepta o corrige con data real y timeframes funcionando
- Feedback → scope v2 feedback log
- Publish a koronetlabs.com (detrás de Cloudflare Access)

### P1: Lead tabs + operating loop
- Tabs 2-6 con estructura de 4 capas y lead definitions
- Daily TX Change Report surface
- Meeting integration (pre/post meeting loops)
- Enablement content, case studies, learnings
- Data refresh pipeline automatizado (Nahua)

---

## Previous content to review and recover (not blindly restore)

These existed in previous versions and need to be ANALYZED against current reality before integrating:

- **Trust Journey** (GATE → BREADTH → DEPTH → FORMAT → BUYERS → COMPETITIVE) — 6-layer model. Needs rework: MaxAge=10 is functionally a GATE fail, not DEPTH. Standing orders not coming to eSuite eliminates Path B.
- **Outcome Chain** — useful as reasoning framework IF it helps answer SO WHATs. Not useful as a standalone tab.
- **Categories A-D** (Config, Activation, Intel, Inventory Supply) — lost in migration to CONFIG-BUY-LIST-SELL. Intel Framework and Activation Blockers need recovery.
- **Inventory Supply Decision Flow** (Paths A/B/C/Fallback) — operationally useful, should live in LIST enablement. But Path B (Standing Orders) doesn't apply to eSuite.
- **Product Capability Tree** — reference content, location TBD.

---

## Contradictions to resolve

1. **3 canonical docs say different things** about tab structure (9 vs 8 vs 7 tabs). Build plan's 8 tabs is what's implemented. Other docs need annotation.
2. **scorecard_vs_artifact_comparison.md** says "don't merge" but they ARE merged. Archive or annotate.
3. **tx_dashboard_system.md** describes 6 dashboards and 3 HTML files. Reality is 1 file. Update.
4. **Two priority taxonomies** (IMPL/P1/CS_P2/TA/WATCH vs P0/P1/P2/P3). accounts_registry system is what Facu uses.

---

## Feedback Log

### Jul 31, 2026 — Session 1 (Scope definition)

**Source:** Facu direct feedback in chat after reviewing Pablito's scope analysis cross-referencing 3 canonical docs, 3 dashboard state docs, and Jul 31 call transcripts (23 calls) + Slack threads.

**Key shifts from previous version:**
1. Card 2 changed from CONFIG to OPPORTUNITIES — mixes config + revenue + BUY/LIST/SELL, prioritized
2. BUY: missing vendor rows (Total → Connected → Active → Churned). Bunches does NOT belong in BUY.
3. BUY: missing vendor coverage analysis — can they buy online what they need at short/medium/long term?
4. LIST: time depth needs 3 versions (online vs offline vs best in class)
5. LIST: Open Market is what they UPLOAD, not what they buy
6. SELL: add unit economics + hardgoods visibility
7. Data with timeframe is non-negotiable prerequisite
8. Trust labels: only DRAFT and APPROVED
9. Outcome Chain: only if it adds SO WHAT value; not for its own sake

**Build approach feedback (Jul 31 late session):**
10. Router was proposing timelines — Facu: "tu timing es malísimo". RULE: no time estimates, focus on what and how.
11. Router wrote a plan without reading canonical build docs (WORKFLOW_CONTRACT, agent_launch_governance, session_closure_governance, build plan, system design). RULE: read ALL canonicals before proposing anything.
12. Router proposed one mega-agent for P0.0 inventory. RULE: microagentes per card/section, each with FULL context pasted (not "read this file"), forced to read ALL feedback and structure.
13. Router skipped the ORIENT phase — tried to decompose without understanding index.html structure first. RULE: map first, decompose second.
14. Router's plan didn't connect fields to scope v2 objectives. RULE: every inventory item maps to the scope requirement it serves, and GAPS show what scope v2 needs that doesn't exist.
15. Router proposed parallel streams without understanding dependencies properly. RULE: structural work (P0.1) can start, data-driven work (P0.3) must wait for verified data (P0.2).
16. Router's execution guide was already written properly in outbox — router should have checked outbox first. RULE: check outbox_for_claude/ before creating execution plans.

**What router did RIGHT (Jul 31):**
- Identified the security issue (public dashboard) and acted immediately
- Found grootctl skill, traced the install path, got repo access, installed tooling
- Spawned 3 parallel agents for call/Slack research — produced comprehensive synthesis
- Asked clarifying questions about SO WHAT per tab instead of assuming
- Captured Facu's feedback in full detail (not summarized) when corrected
- Built the scope v2 doc iteratively with Facu's direct input

**Context from today's calls:**
- eSuite addressable market = ~5% of Core's (only boxes)
- 79 eSuite enabled, only 6 sold in 30 days — inventory problem
- Max age 10 days blocks 47% of Kennicott's sales volume
- Order editing rigidity causes TX fee leakage (CAOSCA mark lost)
- Price parity broken (landed cost missing components)
- Standing orders NOT coming to eSuite (decision made today)
- Kennicott = north star: 22.8% conversion, 90+ day time depth, bunches
- Christine's IMPL wrap: 14 post-go-live accounts, several at 0TX after 30-86 days
