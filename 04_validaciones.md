# Validaciones de calidad de datos

Comprobé los nulos en las claves, las cantidades menores o iguales a cero y los precios negativos. El documento del proyecto registra cero casos en estas comprobaciones.

```sql
-- AdventureWorks | Validaciones de calidad de datos
-- Fuente: proyecto académico de TripleTen.
-- Tablas requeridas: ventas_2017 y productos.
-- El documento registra cero casos en cada una de estas comprobaciones.

-- Conteo de nulos en número de pedido, clave de producto y clave de territorio.
SELECT
    SUM(
        CASE
            WHEN numero_pedido IS NULL THEN 1
            ELSE 0
        END
    ) AS nulos_numero_pedido,
    SUM(
        CASE
            WHEN clave_producto IS NULL THEN 1
            ELSE 0
        END
    ) AS nulos_clave_producto,
    SUM(
        CASE
            WHEN clave_territorio IS NULL THEN 1
            ELSE 0
        END
    ) AS nulos_clave_territorio
FROM ventas_2017;

-- Conteo de cantidades menores o iguales a cero.
SELECT
    COUNT(*) AS filas_cantidad_no_valida
FROM ventas_2017
WHERE cantidad_pedido <= 0;

-- Conteo de precios negativos.
SELECT
    COUNT(*) AS productos_precio_no_valido
FROM productos
WHERE precio_producto < 0;
```

[Volver a la presentación del proyecto](README.md)
