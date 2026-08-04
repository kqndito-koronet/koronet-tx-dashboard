# TX Dashboard — decisiones de revisión de Facu

Este registro convierte la revisión pieza por pieza en criterios de aceptación. Una decisión aprobada no cambia la UI ni ejecuta trabajo externo por sí sola.

## Cómo aprendemos de aprobaciones

Los “sí”, “agreed” y “correcto” explícitos de Facu se registran como señales de aprobación para el tipo de decisión correspondiente. Cada señal conserva: propuesta, evidencia usada, riesgo/scope, decisión, resultado posterior y corrección si la hubiera.

La autonomía sólo puede proponerse por **clase acotada de decisión** después de evidencia repetida de que las propuestas fueron aprobadas y produjeron el resultado esperado. Ejemplo: primero proponer la estructura de una card; después, si las revisiones de esa clase se aprueban consistentemente y pasan validación, pedir permiso para implementar cambios de estructura equivalentes. Nunca se infiere autoridad para datos, prioridades, intervenciones, outreach, bridge, staging o promociones.

### Señales registradas

| ID | Señal de Facu | Clase de decisión | Propuesta aprobada | Evidencia / riesgo | Resultado a validar |
|---|---|---|---|---|---|
| R1 | “Agreed” | Portfolio selection | Lectura potencial/captura/online/cambio + SFDC/TX Opps; purchases con método visible | Auditoría de 19 columnas y est. BUY inconsistente; cambio de UI aún no autorizado | La futura vista permite seleccionar cuentas mejor que la actual. |
| R2 | “OK” | POTENTIAL structure | Primera capa de negocio; monetización por canal plegable dentro de la card | Auditoría Price's; cambio de UI aún no autorizado | La card permite entender el gap antes del detalle. |
| R3 | “me parece correcto” | OPPORTUNITIES structure | Máximo 3 hipótesis evidence-first; no tareas automáticas | Leads actuales se superponen y sobredeclaran confianza | La card lleva a una investigación/play correcto. |
| R3a | “debería link…” | Enablement navigation | Link de candidato a tabs de lead y enablement preservando contexto | No crea intervención ni task | CS/Implementation puede entender y trabajar el play sin perder la cuenta. |

## R1 — Portfolio para seleccionar una cuenta

Fecha: 2026-08-01
Decisión: **APPROVED**

La primera vista debe ayudar a elegir una cuenta por potencial, captura, adopción online y cambio; no ser un dump de diagnóstico.

Columnas requeridas:

1. Cuenta
2. Cohorte / owner
3. GMV de empresa
4. Koronet Sell
5. Sell online %
6. Koronet Buy
7. Buy online %
8. Cambio de las métricas anteriores, con periodo compatible visible
9. SFDC Opps: cantidad y etapa más alta
10. TX Opps: cantidad de oportunidades de optimización que pasaron su contrato de evidencia

`Est. Buy GMV` deja de ser el nombre/cálculo actual. La futura columna será **Est. total purchases**, con método visible: `Observed`, `Modeled` o `Hypothesis`.

Reglas:

- Observed Koronet procurement nunca puede presentarse como compra total de la empresa.
- Un modelo de compras debe estar calibrado por segmento/cuentas Core comparables, no por una constante universal; debe exponer inputs, periodo y confianza/rango.
- Los detalles de configuración, CVR, take rate y lead explanations viven al abrir la cuenta, no como columnas de selección inicial.

## R2 — Card 1: POTENTIAL

Fecha: 2026-08-01
Decisión: **APPROVED**

POTENTIAL debe responder primero tamaño, captura, digitalización y cambio; después permitir validar monetización sin convertir la card en un dump de canales.

Orden de lectura aprobado:

1. **Empresa:** GMV anual estimado, método, fecha y confianza; contexto fact-based de producto/reimplementación cuando exista evidencia.
2. **SELL:** total estimado → Koronet Sell → online/offline → cambio en periodo compatible.
3. **BUY:** total purchases (`Observed` / `Modeled` / `Hypothesis`) → Koronet Buy → online/offline → cambio.
4. **Economía Koronet:** fees realizados; escenarios sólo con hipótesis/tasa/base/horizonte visibles.
5. **Conclusión:** mayor gap verificable y la pregunta que OPPORTUNITIES debe resolver.

El detalle actual de monetización por `eCommerce`, `K2K`, `API`, `Offline` e indirect BUY se conserva **plegable dentro de POTENTIAL**, bajo “Ver desglose de monetización”; no ocupa la primera capa de lectura.

Reglas:

- No duplicar ORA y cálculo si representan la misma estimación.
- No usar etiquetas como “saturated”, “big opportunity” o un gap como conclusión si el periodo/denominador no es compatible.
- Si falta el total purchases o el SELL spine, la card muestra el hueco y bloquea el escenario, no lo rellena.

## R3 — Card 2: OPPORTUNITIES

Fecha: 2026-08-01
Decisión: **APPROVED**

OPPORTUNITIES es hypothesis-first: convierte el gap de POTENTIAL en pocas investigaciones candidatas, no en un feed de leads automáticos ni una lista de tareas ejecutables.

Estructura aprobada:

1. Contexto relevante que cambia la lectura (producto, reimplementation/historia sólo con fuente, SFDC Opps).
2. Decisión actual: qué no se puede decidir todavía y por qué.
3. Máximo tres candidatos BUY / LIST / SELL, cada uno con hipótesis, evidencia, incógnita, condición de confirmación y enlace a la evidencia de su card.
4. Configuración solamente si es prerrequisito del candidato actual.
5. Para un candidato confirmado: enlace a play/enablement con por qué importa, pitch, objeciones, casos y owner sugerido.

Cada candidato de OPPORTUNITIES debe enlazar a la tab correspondiente de **leads** y de **enablement**. El link conserva el contexto de cuenta/candidato para que CS o Implementation entiendan la lógica, cómo trabajarlo y los ejemplos; abrirlo no aprueba ni crea una tarea.

Reglas:

- No sumar un “total opportunity” de leads superpuestos.
- No llamar P1/P2 a un lead automático; la prioridad requiere evidencia comparable y decisión humana.
- No crear tarea, outreach ni intervención desde la card. Primero investigación; después play; después aprobación explícita.
- La falta de evidencia es un estado útil (`BLOCKED` / `needs evidence`), no un hueco que se rellena con confianza artificial.

## R4 — Card 3: BUY

Fecha: 2026-08-01
Decisión: **APPROVED**

BUY no repite el volumen/captura de POTENTIAL. Explica cómo aumentar la proporción de compras online: si el supply correcto está disponible, por qué no se usa y qué tipo de lead corresponde.

Estructura aprobada:

1. **Unit economics:** “+10 puntos de compra online = +$XX GMV online = +$YY indirect fees”, sólo con base/método/periodo de total purchases visibles y válidos.
2. **Cobertura y gap**, online hoy vs offline hoy, con estado de disponibilidad online y explicación del gap para: vendors activos, categorías, variedades, SKUs, y oferta de corto/medio/largo plazo.
3. **Lectura del gap:**
   - offline de algo disponible online → lead de migración/activación;
   - offline de algo no disponible online → supply gap, cuantificar cuentas/GMV y proponer a Sales conexión/supply;
   - no mezclar SKU con variedad.
4. **Horizonte y fulfillment:** usar consistencia de oferta por plazo como señal de capacidad de planificar y cumplir; no sólo como existencia de producto.
5. **Conexiones/lifecycle:** resumen compacto; mostrar por defecto sólo estados/gaps materiales (activados no activos, conectados no activados, churned, churned→offline, offline sin conexión). Detalle completo plegable.
6. Cada señal/candidato enlaza a lead y enablement contextualizados; no crea tarea.

Reglas:

- Que la mayoría de vendors/categorías estén offline es una hipótesis/gap importante, no una excepción que se oculte.
- “Leakage” requiere probar disponibilidad online de la misma necesidad; si no, se clasifica supply gap o `needs evidence`.
- Horizonte online sin comparador offline se muestra como evidencia parcial, nunca como comparación concluyente.

## R5 — Card 4: LIST

Fecha: 2026-08-01
Decisión: **APPROVED**

LIST explica si los clientes pueden encontrar online una oferta equivalente o mejor a la que el wholesaler ofrece offline. Si no la ven, clientes actuales y nuevos no tienen motivo para comprar online.

Estructura aprobada:

1. **Capacidad de publicar:** mostrar sólo configuración/integración que determine si supply comprado puede aparecer en el shop.
2. **Paridad online/offline:** comparar categorías, variedades, SKUs y oferta de corto/medio/largo plazo cuando la evidencia exista. La diferencia debe explicar qué no pueden encontrar los compradores online y qué hay que hacer para que la oferta sea parecida o mejor online —incluida la oferta sourceable a futuro.
3. **Best-in-class siempre:** mostrar la propia comparación online/offline cuando exista **y** un best-in-class para dimensionar cuánto mejor puede ser la cuenta. Prioridad de comparación: comparable local cuando exista; después comparable de mismo modelo/segmento; después benchmark más amplio, siempre con fuente, periodo y alcance visibles. No poner ceros cuando falta offline ni presentar el benchmark como si fuera su propio offline.
4. **Bunches visible y separado:** mostrar GMV/breadth online vs offline y configuración relevante. Si hay GMV offline de bunches sin visibilidad online, explicitar la parte material de oferta/GMV que el shop deja fuera. No usar un porcentaje de industria como si fuera el TAM exacto de la cuenta.
5. **Conclusión:** decidir si el siguiente cuello es configuración, paridad/visibilidad, supply disponible a futuro o SELL/demanda; enlazar al lead y enablement.

Reglas:

- LIST usa un proxy de ventas sólo si lo declara; no debe venderlo como catálogo actual.
- Ausencia de comparador offline = `no disponible`, no “sin gap”.
- Best-in-class es una referencia para aprender, no prueba causal ni sustituto silencioso del dato de cuenta.

## R6 — Card 5: SELL

Fecha: 2026-08-01
Decisión: **APPROVED**

SELL no repite el negocio/captura de POTENTIAL. Empieza por la base de clientes y la readiness del shop para decidir entre mejorar la experiencia, activar clientes offline existentes o traer tráfico.

Estructura aprobada:

1. **Clientes:** online/offline activos, nuevos, recurrentes/churned y AOV, sólo con misma población/periodo; una línea compacta de contexto de GMV online/offline es opcional, no un bloque repetido.
2. **Cohorte inicial:** identificar clientes offline de valor, activos y con fit/acceso al eShop antes de recomendar adquisición de tráfico.
3. **Calidad del shop:** estado de paridad LIST, checkout/config relevante, tráfico/CVR cuando sea atribuible, y best-in-class siempre visible con comparable local prioritario.
4. **Gate:** si clientes existentes no convierten o LIST no tiene paridad, bloquear tráfico nuevo y explicar qué debe mejorar primero.
5. **Unit economics:** escenario de conversión de una cohorte offline adecuada, con base/periodo/tasa visibles; escenario, no forecast.
6. **Conclusión:** shop/listing primero, activación de cohorte o tráfico, con link a lead y enablement.

Reglas:

- No mezclar L30D, total, Q2 o fuentes en una tabla sin contrato común.
- No mostrar buyer counts sin GMV de canal corroborado en la ventana seleccionada.
- No recomendar tráfico antes de verificar oferta, checkout y conversión de clientes existentes.

## Scope actual — Accounts primero

Fecha: 2026-08-01
Decisión: **APPROVED**

El trabajo activo se limita a hacer la tab **Accounts** muy buena y confiable: Portfolio y las cards POTENTIAL → OPPORTUNITIES → BUY → LIST → SELL, incluyendo la consistencia entre ellas, datos, fuentes, periodos y estados de evidencia.

Las tabs de leads/enablement no son trabajo activo. Cuando una card revele una necesidad de enablement, se puede registrar como sugerencia vinculada al lead, sin construir, modificar ni revisar la superficie de enablement hasta que Accounts apruebe el MVP.

## R7 — Confianza de métricas: fuente no equivale a cobertura de negocio

Fecha: 2026-08-01
Decisión: **APPROVED**

Una métrica `observed` significa únicamente que el sistema observó ese evento en una fuente definida. No significa que Koronet vea el total de compra/venta de la empresa. Si la cuenta no usa Core como ERP, una transacción observada en Koronet sólo representa transacciones dentro del scope Koronet.

Cada métrica material debe separar explícitamente:

1. **Qué medimos:** por ejemplo, `Koronet Buy YTD`, no “total purchases”.
2. **Scope/cobertura del negocio:** `Koronet transactions only`, `Core ERP purchase records`, `CRM estimate`, o `unknown`.
3. **Tipo de evidencia:** measured event, modeled estimate, hypothesis, partial o blocked.
4. **Fuente, definición y periodo.**

Ejemplo correcto: `Koronet Buy YTD $X · measured in PROCUREMENTS_SV · Koronet transactions only · as-of date`. El total purchases de la empresa es otro campo y sólo puede ser observado si la fuente realmente cubre el ERP/negocio, o modeled/hypothesis con método explícito.

## R8 — Base de purchases y estimador GMV

Fecha: 2026-08-01
Decisión: **APPROVED**

El ratio base actual para modelar `total purchases` es **54% del GMV de empresa**, salvo evidencia específica de cuenta o calibración segmentada que lo reemplace. Es una hipótesis/base de modelo, no compra observada.

El estimador de GMV requiere research externo por cuenta según `ops/frameworks/floral_gmv_estimator_prompt.md`: clasificación correcta, señales adecuadas al tipo de negocio, al menos dos métodos independientes cuando sea posible, rango, confianza, fuentes/fecha y una futura comparación con realidad. Un valor SFDC sin ese trail sigue siendo útil como estimación comercial, pero no es una validación del modelo.
