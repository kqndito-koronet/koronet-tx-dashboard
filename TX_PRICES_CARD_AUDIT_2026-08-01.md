# Auditoría MVP — Price's (`816515`) por card

Auditoría contra [el feedback de aceptación de Price's](TX_PRICES_ACCEPTANCE_FEEDBACK_2026-08-01.md).
Regla: una card sólo aprueba si permite decidir qué investigar sin mezclar una estimación, una actividad observada, una ventana temporal o un proxy.

## Veredicto

**Price's no aprueba como Account Deep Dive MVP.** Hay datos útiles para investigar, pero la superficie no permite todavía una decisión confiable entre cobertura/leakage de BUY, paridad de LIST o conversión de SELL.

| Card | Estado | Evidencia útil hoy | Falla que impide decidir |
|---|---|---|---|
| 1. POTENTIAL | **FAIL P0** | `est_sell_gmv=$1.8M`, SFDC, confianza MEDIUM; BUY temporal hasta 2026-07-30 | `est_buy_gmv=$84,416`, fuente `CORE`, era actividad observada y se presentaba como potencial. La UI ahora lo bloquea, pero todavía falta un modelo de potencial BUY. |
| 2. OPPORTUNITIES | **FAIL P1** | Core + Procurement + eShop; métricas BUY/LIST/SELL existen en fragmentos | No retiene evidencia de reimplementación, compras online pre-Koronet, objetivo del cliente ni causalidad. Las oportunidades se generan desde métricas, no desde una hipótesis verificable de cuenta. |
| 3. BUY | **FAIL P0** | BUY YTD: $165,667 total / $100,275 online = 60.53%; mes actual: $81,251 / $17,993 = 22.15%; 26 vendors online, 3 offline | No hay GMV ni transición por vendor/categoría/variedad/SKU, ni conectividad, ni primer/último evento. No se puede explicar el 39.47% offline ni detectar leakage. |
| 4. LIST | **FAIL P0** | 8 categorías, 10 variedades, 15 SKUs; 1 categoría y 2 SKUs online; señal de 2 unidades 0–7 días | La propia UI declara que es un **proxy** (“no hay catalog data — mostramos lo que vendieron”). No hay comparación online/offline ni definición común de breadth, inventario, listados o horizonte. Los ceros no son evidencia de ausencia. |
| 5. SELL | **FAIL P0** | El V2 local dice YTD $140,054 total, $3,278 online, $136,776 offline (as-of 2026-07-30) | Otras fuentes dicen $0 sell GMV, 4 buyers/1 online/4 offline; `buyers_l30d` dice 5 online; phase V2 dice 4 online total y 1 active L30D. Es imposible saber qué significa el “6 clientes online” que Facu vio sin fuente/ventana en pantalla. |

## Reconciliación de la aparente contradicción de SELL

El feedback de Facu observó “sin ventas online” y “6 clientes online” en la superficie publicada. La data local actual contiene otra versión: V2 tiene $3,278 online y $140,054 total, mientras `metrics.json` sigue en $0. Eso **no invalida el feedback**: confirma el problema de MVP. La superficie/refresh puede cambiar de fuente sin hacer visible qué versión, periodo o definición se está usando.

Hasta que SELL use un único spine, el dashboard no debe concluir “traer tráfico”, “arreglar conversión” ni “activar buyers” para Price's.

## Qué sí se puede decir hoy, sin exagerar

- Price's tiene una estimación de tamaño de $1.8M, no una estimación de BUY validada.
- Hay adopción online de BUY, pero cambió mucho según ventana: 60.53% YTD, 22.15% en el mes actual y 97.47% en el trimestre previo. Eso amerita investigar, no declarar que Procurement aceleró ni que está sano.
- Hay señal de catálogo/listado reducido, pero no de paridad online/offline. Por ahora es una pregunta de investigación, no un lead de configuración.
- Hay ventas en el V2 local, pero no un contrato que reconcilie GMV, clientes, formato y tiempo. Por ahora no hay diagnóstico de SELL.

## Defectos reutilizables encontrados

El validador de contrato (`python3 -B scripts/validate_tx_card_contract.py`) bloquea el refresh actual con:

- **16** casos donde una actividad Core observada aparece como potencial BUY.
- **203** casos con buyers online y $0 de online SELL GMV en la métrica base.
- **209** casos que exponen buyer counts de fuentes/ventanas diferentes sin contrato compartido.

No son 428 cuentas independientes necesariamente: son violaciones de tres tipos que se superponen. Price's cae en los tres tipos y por eso es un buen acceptance account.

## Criterio de salida para Price's

La siguiente versión sólo aprueba si contiene un único extracto versionado/as-of que permita escoger una sola hipótesis inicial, con evidencia:

1. **BUY coverage/leakage:** qué vendor/categoría/SKU y cuánto GMV permanece offline, se desconectó o volvió offline.
2. **LIST parity:** qué existe y se vende online versus offline, y a qué horizonte; sin proxy presentado como inventario.
3. **SELL readiness/conversion:** GMV, clientes y actividad online/offline en la misma ventana antes de decidir tráfico, onboarding o configuración.

La solicitud de datos para producir ese extracto está en estado DRAFT en `data/ai_request_queue.json` como `AI-DATA-PRICES-ACCOUNT-SPINE`; no ejecuta un pull ni crea una intervención.
