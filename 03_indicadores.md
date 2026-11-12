# Indicadores financieros por país

Agrupé los ingresos y costos por país, incorporé el gasto de campañas y calculé beneficio bruto, margen y ROI según las fórmulas del proyecto.

```sql
-- AdventureWorks | Indicadores financieros por país
-- Fuente: proyecto académico de TripleTen.
-- Tablas requeridas: ventas_clean, campanas, pais_ingreso_costo y pais_campanas.
-- En TripleTen, la primera salida se guardaba como pais_ingreso_costo y la segunda
-- como pais_campanas. La tercera consulta utiliza esas tablas intermedias.
-- Los SELECT no materializan las tablas. Las conversiones y fórmulas son las originales.

-- Ingresos y costos por país y territorio, a partir de ventas_clean.
SELECT
    pais,
    clave_territorio,
    SUM(ingreso_total)::integer AS ingresos,
    SUM(costo_total)::integer AS costos
FROM ventas_clean
GROUP BY
    pais,
    clave_territorio
ORDER BY ingresos DESC;

-- Integración del gasto de campañas por territorio.
SELECT
    v.pais,
    v.clave_territorio,
    SUM(v.ingreso_total)::integer AS ingresos,
    SUM(v.costo_total)::integer   AS costos,
    COALESCE(c.costo_campana::numeric, 0)::integer AS costo_campana
FROM ventas_clean AS v
LEFT JOIN campanas AS c
    ON v.clave_territorio = c.clave_territorio::integer
GROUP BY
    v.pais,
    v.clave_territorio,
    c.costo_campana
ORDER BY
    ingresos DESC;

-- Beneficio bruto, margen y ROI según las fórmulas del proyecto.
SELECT
    p.pais,
    p.clave_territorio,
    SUM(p.ingresos)::integer AS ingresos,
    SUM(p.costos)::integer AS costos,
    COALESCE(SUM(c.costo_campana), 0)::integer AS costo_campana,
    (SUM(p.ingresos) - SUM(p.costos))::integer AS beneficio_bruto,
    (
        (SUM(p.ingresos) - SUM(p.costos)) * 100.0
        / NULLIF(SUM(p.ingresos), 0)
    ) AS margen_pct,
    (
        (SUM(p.ingresos) - SUM(p.costos)) * 100.0
        / NULLIF(COALESCE(SUM(c.costo_campana), 0), 0)
    ) AS roi_pct
FROM pais_ingreso_costo AS p
LEFT JOIN pais_campanas AS c
    ON p.clave_territorio = c.clave_territorio
GROUP BY
    p.pais,
    p.clave_territorio
ORDER BY
    p.clave_territorio,
    ingresos,
    costos;
```

[Volver a la presentación del proyecto](README.md)
