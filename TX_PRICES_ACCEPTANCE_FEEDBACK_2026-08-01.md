# Price's — feedback de aceptación del Dashboard TX

Fecha: 2026-08-01
Cuenta: Price's Floral Wholesale, LLC (`816515`)
Estado: **P0 — no usar esta card para decidir una intervención hasta reconciliar definiciones, ventanas y fuentes.**

Este documento preserva el feedback de Facu y lo convierte en condiciones verificables del MVP. No altera los datos ni asume que una cifra local es la verdad sobre la cuenta.

## Qué debería permitir entender la card

Price's es una cuenta estimada de **$1.8M GMV anual**. La card debe dejar claro cuánto potencial tiene, cuánto de su compra y venta ya pasa por Koronet, cuánto es online/offline y cómo cambian esas métricas. Debe permitir decidir qué investigar: completar compras online, resolver leakage hacia offline, mejorar el surtido/listado o validar primero si el shop convierte.

Contexto que la card debe poder conservar con fuente: es una cuenta Core con Procurement y shop; fue una **reimplementación**, y antes ya compraba online en e-commerces individuales. Por eso no se puede atribuir automáticamente a Procurement una aceleración: hay que comparar contra esa base histórica.

## Feedback de Facu, preservado

- $1.8M de GMV implica que una hipótesis de 10% de sell sería ~$180k y ~$2.7k en fees. Falta mostrar el unit economics de sell con definición explícita, no como resultado observado.
- En BUY, 60.5% online parece muy bueno para una Core account y debería disparar la pregunta: **qué de Procurement les funciona**. Pero $84k como `estimated buy potential` es incorrecto. Nota de corrección posterior de Facu: el ratio base a usar para modelar purchases es **54%**, salvo evidencia específica de cuenta; la aritmética previa `1.8M × 46%` quedó como referencia conversacional y no como baseline. Es una hipótesis a validar, no un dato que la card pueda inventar.
- Debe responder qué vendor, categoría, variedad o producto falta; cuáles se compran offline porque no están conectados; cuáles se conectaron y luego volvieron a offline; y si el leakage se repite por vendor.
- “26 vendors, 67 categories y 10 varieties” no es interpretable: una taxonomía donde variedades parecen menores que categorías necesita definición o está mal. Que haya casi nada offline cuando el dashboard dice ~40% offline también es inconsistente.
- LIST debe mostrar qué venden (breadth y horizonte/length in time) y las diferencias online/offline. Una métrica proxy no puede pasar por inventario, surtido o venta real sin decirlo.
- SELL no puede mostrar 6 clientes online si no hay ventas online. Los 107 clientes offline deben aparecer con la misma ventana y definición en LIST/Sell/on-time, o declararse no disponibles. Hoy la vista confunde y no permite decidir nada.

## Contradicciones verificadas en la data local

| Área | Lo que hoy aparece en la data | Por qué falla |
|---|---:|---|
| Potencial de BUY | `est_buy_gmv = $84,416`, fuente `CORE`, confianza `HIGH` | Es exactamente el BUY observado de la fuente Core; no es una estimación de potencial. No puede rotularse como potencial. |
| BUY temporal | YTD online = 60.53% al 2026-07-30; mes actual = 22.15%; trimestre previo = 97.47% | La cifra 60.5% de Facu es YTD. Sin ventana visible, puede interpretarse erróneamente como estado actual. |
| BUY acumulado previo | `proc_total = $84,416`; `proc_online = $82,282` | Esa ventana distinta da 97.47% online, no 60.5%. La card debe identificar ventana y no mezclarla con YTD. |
| Vendors | 26 online, 3 offline, 29 total | Útil como cobertura, pero no prueba que el 40% offline esté explicado ni muestra quién se desconectó o repite leakage. |
| LIST | 8 categorías, 10 variedades, 15 SKUs; online 1 categoría y 2 SKUs | No coincide con el número que Facu vio en la superficie publicada y no tiene comparador offline ni definición de cobertura. |
| Horizonte | 2 unidades vendidas en 0–7 días; cero en las demás bandas | No está claro si representa inventario, listados, órdenes o un proxy. No se puede usar para inferir surtido/availability. |
| SELL / buyers | `sell_gmv = $0`; buyers: 4 total, 1 online, 4 offline; otro archivo dice 5 buyers online L30D | Los conteos se contradicen y no comparten fuente/ventana. Ninguno debe presentarse como “clientes online” junto a $0 de ventas hasta reconciliación. |

Fuentes locales revisadas: `metrics_v2_buy.json` (as-of 2026-07-30), `metrics.json`, `est_gmv.json`, `vendor_detail.json`, `listing_detail.json`, `buyers.json`, `repeat_rate.json`, `sell_detail.json` y `loop2_list_usage_v2.json`.

## Condiciones de aceptación antes de promover la card

1. **Potencial ≠ actividad observada.** Un campo `estimated_buy_gmv` sólo puede aparecer si tiene método, inputs, fecha y nivel de confianza. El valor Core de $84,416 debe llamarse `observed_koronet_buy_gmv` con su ventana, o quedar fuera del bloque de potencial.
2. **Cada métrica lleva semántica.** La UI debe mostrar definición, fuente, `as_of`, ventana y cobertura. Si online/offline usa ventanas distintas, no se comparan ni se usan para un porcentaje.
3. **BUY responde cobertura y leakage.** Por vendor/categoría/variedad/SKU: volumen online, offline, conectividad, primer/último evento y transición online→offline cuando exista. Si no existe el dato, la card pide esa investigación; no muestra ceros engañosos.
4. **LIST separa inventario, listado y ventas.** Debe ofrecer breadth y horizonte para online y offline con una definición común; de lo contrario debe decir “no disponible / proxy”, no insinuar paridad.
5. **SELL usa un único spine.** GMV y clientes online/offline deben provenir de la misma población y ventana. Si GMV online es $0, un conteo de clientes online queda bloqueado hasta explicar la discrepancia.
6. **Escenarios de fees son escenarios.** Los $2.7k y $7.4k citados arriba son hipótesis de Facu y requieren una tasa, base y horizonte visibles; nunca son fees realizados.
7. **El contexto comercial es evidencia.** Core/Procurement/shop, reimplementación, e-commerce previo y base histórica deben tener fuente y fecha. Si falta, la card crea una solicitud de investigación, no una narrativa automática.

## Salida operativa esperada de Price's

Después de reconciliar lo anterior, la card debe producir una de estas conclusiones accionables, con evidencia: (a) expandir cobertura de BUY con vendors/categorías concretos; (b) investigar leakage recurrente; (c) mejorar inventory/listing para que el shop muestre lo que ya compra; (d) mejorar conversión del shop antes de llevar tráfico; o (e) no intervenir aún porque falta el spine de medición. Ninguna debe elegirse ahora con los datos contradictorios.
