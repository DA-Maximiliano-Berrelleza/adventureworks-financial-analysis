# AdventureWorks | Análisis del desempeño financiero internacional con SQL

Analicé las ventas de 2017 de AdventureWorks en seis mercados para comparar ingresos, costos, beneficio bruto, margen y ROI. El objetivo fue aportar evidencia para priorizar mercados y orientar la revisión del presupuesto de marketing.

Integré ventas, productos, categorías y territorios con SQL, calculé indicadores por país e incorporé el gasto de campañas. Presenté los resultados en Excel y elaboré recomendaciones con la estructura Contexto → Hallazgo → Implicación.

**Contexto:** Proyecto académico de bootcamp de análisis de datos.  
**Herramientas:** SQL y Excel. Las consultas utilizan conversiones de tipo como `::integer` y `::numeric`.

## Preguntas de negocio

- ¿Cuánto beneficio bruto genera cada país?
- ¿Cómo se comparan los mercados al relacionar ese beneficio con el gasto de campañas?

## Datos y proceso

La base de ventas tiene una fila por producto y pedido. Utilicé `ventas_2017`, `productos`, `productos_categorias`, `territorios` y `campanas`.

| Etapa | Trabajo realizado | Consultas |
|---|---|---|
| Exploración | Inspección de las tablas disponibles. | [01_exploracion.md](01_exploracion.md) |
| Preparación | Integración mediante `LEFT JOIN`, tratamiento de nulos con `COALESCE` y cálculo de ingreso y costo por línea. | [02_preparacion.md](02_preparacion.md) |
| Indicadores | Agregación por país, integración del gasto de campañas y cálculo de beneficio bruto, margen y ROI con protección frente a división por cero mediante `NULLIF`. | [03_indicadores.md](03_indicadores.md) |
| Validación | Comprobación de nulos en claves, cantidades no válidas y precios negativos. | [04_validaciones.md](04_validaciones.md) |

### Validaciones documentadas

Las consultas registraron **0 nulos** en `numero_pedido`, `clave_producto` y `clave_territorio`; **0 filas** con `cantidad_pedido <= 0`; y **0 productos** con `precio_producto < 0`.

### Indicadores utilizados

| Indicador | Fórmula del proyecto |
|---|---|
| Ingresos | Suma de precio del producto × cantidad pedida. |
| Costos | Suma de costo del producto × cantidad pedida. |
| Beneficio bruto | Ingresos − costos. |
| Margen (%) | Beneficio bruto ÷ ingresos × 100. |
| ROI (%) | Beneficio bruto ÷ costo de campañas × 100. |

## Resultados por país

La siguiente tabla reproduce los indicadores del archivo Excel. Los importes se muestran con dos decimales y las tasas como porcentajes.

| País | Ingresos | Costos | Campañas | Beneficio bruto | Margen | ROI |
|---|---:|---:|---:|---:|---:|---:|
| United States | 3,353,939.92 | 1,899,471.33 | 1,920,000.00 | 1,454,468.60 | 43.37 % | 75.75 % |
| Australia | 2,532,003.49 | 1,474,958.18 | 2,150,400.00 | 1,057,045.31 | 41.75 % | 49.16 % |
| United Kingdom | 1,189,636.78 | 681,508.55 | 2,304,000.00 | 508,128.24 | 42.71 % | 22.05 % |
| Germany | 1,071,460.42 | 611,295.36 | 2,265,600.00 | 460,165.06 | 42.95 % | 20.31 % |
| France | 924,316.93 | 527,797.14 | 2,208,000.00 | 396,519.79 | 42.90 % | 17.96 % |
| Canada | 710,205.18 | 392,325.91 | 1,824,000.00 | 317,879.27 | 44.76 % | 17.43 % |

- **Estados Unidos** registró los mayores ingresos y beneficio bruto, con el ROI más alto del análisis: **75.75 %**.
- **Australia** ocupó el segundo lugar en beneficio bruto, con **1,057,045.31**, y registró un ROI de **49.16 %**.
- **Canadá** presentó el mayor margen, **44.76 %**, junto con los menores ingresos y el menor ROI, **17.43 %**.
- **Reino Unido, Alemania y Francia** registraron márgenes entre **42.71 % y 42.95 %**, con ROI entre **17.96 % y 22.05 %**.

## Recomendaciones

1. Incrementar gradualmente la inversión publicitaria en Estados Unidos. Dado que este mercado presenta el mayor ROI del análisis, se recomienda realizar una prueba piloto con un aumento del presupuesto del 10–20 %. Monitorear el comportamiento del ROI y del beneficio bruto antes de ampliar la inversión permitirá evaluar si el incremento mantiene la eficiencia observada.

2. Auditar la estrategia publicitaria en Europa y Canadá. Se recomienda revisar la segmentación, los canales de marketing y el costo por adquisición en United Kingdom, Germany, France y Canada, considerando su menor ROI frente a Estados Unidos y Australia. Esta revisión permitirá identificar oportunidades de mejora y orientar los ajustes de campañas y presupuesto.

## Archivos de resultados

- [Excel con indicadores, resumen ejecutivo y respuestas del proyecto](Resultados_AdventureWorks.xlsx).
- [Tabla de indicadores en CSV](Resultados_AdventureWorks.csv).

El CSV contiene únicamente la tabla de seis países. `Margen_pct` y `ROI_pct` conservan la escala decimal del Excel: por ejemplo, `0.7575` equivale a `75.75 %`.

## Secuencia y dependencias de las consultas

El entorno de TripleTen guardaba resultados intermedios para utilizarlos en las etapas posteriores:

| Tabla intermedia | Resultado que representa | Dónde se utiliza |
|---|---|---|
| `ventas_clean` | Segunda consulta de preparación: detalle integrado con ingreso y costo por línea. | Primera y segunda consultas de indicadores. |
| `pais_ingreso_costo` | Primera consulta de indicadores: ingresos y costos por país y territorio. | Consulta final de indicadores. |
| `pais_campanas` | Segunda consulta de indicadores: ingresos, costos y gasto de campañas por territorio. | Consulta final de indicadores. |

Los archivos Markdown presentan las consultas dentro de bloques SQL y conservan los `SELECT` documentados en el curso. Para ejecutarlos fuera de ese entorno se necesitan las tablas originales y las intermedias con estructura y datos equivalentes. El repositorio reúne las consultas y los resultados exportados; la base de datos del curso no está incluida.

## Documentación de origen

- [Documento del proyecto: instrucciones, consultas, resultados y evaluación](https://docs.google.com/document/d/1kMlZ2qaPa2eE2hIZ4oMiO0PjxVw9AVRjn5KsbQ94yzE/edit).
- [Archivo de resultados y resumen ejecutivo](https://docs.google.com/spreadsheets/d/18KTDt9F5ICos_CMCmpIs4y6qaCVom6Rh/edit).
