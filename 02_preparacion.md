# Preparación de la base de análisis

Integré ventas, productos, categorías y territorios mediante LEFT JOIN. Utilicé COALESCE para tratar los nulos y calculé el ingreso y el costo por línea de pedido.

```sql
-- AdventureWorks | Preparación de la base de análisis
-- Fuente: proyecto académico de TripleTen.
-- Tablas requeridas: ventas_2017, productos, productos_categorias y territorios.
-- En TripleTen, la salida de la segunda consulta se guardaba como ventas_clean.
-- Estas consultas SELECT conservan la lógica del documento; no crean esa tabla.

-- Integración de ventas, productos, categorías y territorios; tratamiento de nulos.
SELECT
    v.numero_pedido,
    v.clave_producto,
    p.nombre_producto,
    pc.clave_categoria,
    COALESCE(p.precio_producto, 0) AS precio_producto,
    COALESCE(v.cantidad_pedido, 0) AS cantidad_pedido,
    COALESCE(p.costo_producto, 0) AS costo_producto,
    t.pais,
    t.continente,
    v.clave_territorio
FROM ventas_2017 AS v
LEFT JOIN productos AS p
    ON v.clave_producto = p.clave_producto
LEFT JOIN productos_categorias AS pc
    ON p.clave_subcategoria = pc.clave_subcategoria
LEFT JOIN territorios AS t
    ON v.clave_territorio = t.clave_territorio;

-- Cálculo del ingreso y costo por línea de pedido.
SELECT
    v.numero_pedido,
    v.clave_producto,
    p.nombre_producto,
    pc.clave_categoria,
    COALESCE(p.precio_producto, 0)  AS precio_producto,
    COALESCE(v.cantidad_pedido, 0)  AS cantidad_pedido,
     COALESCE(p.costo_producto, 0)   AS costo_producto,
    t.pais,
    t.continente,
    v.clave_territorio,
    COALESCE(p.precio_producto, 0) * COALESCE(v.cantidad_pedido, 0) AS ingreso_total,
    COALESCE(p.costo_producto, 0) * COALESCE(v.cantidad_pedido, 0) AS costo_total
FROM ventas_2017 AS v
LEFT JOIN productos AS p
    ON v.clave_producto = p.clave_producto
LEFT JOIN productos_categorias AS pc
    ON p.clave_subcategoria = pc.clave_subcategoria
LEFT JOIN territorios AS t
    ON v.clave_territorio = t.clave_territorio;
```

[Volver a la presentación del proyecto](README.md)
