# Exploración de las tablas

Inspeccioné las primeras diez filas de ventas, productos, categorías, territorios y campañas para reconocer los campos disponibles.

```sql
-- AdventureWorks | Exploración de las tablas
-- Fuente: proyecto académico de TripleTen.
-- Tablas requeridas: ventas_2017, productos, productos_categorias, territorios y campanas.

-- Primeras diez filas de ventas_2017.
SELECT *
FROM ventas_2017
LIMIT 10;

-- Primeras diez filas de productos.
SELECT *
FROM productos
LIMIT 10;

-- Primeras diez filas de productos_categorias.
SELECT *
FROM productos_categorias
LIMIT 10;

-- Primeras diez filas de territorios.
SELECT *
FROM territorios
LIMIT 10;

-- Primeras diez filas de campanas.
SELECT *
FROM campanas
LIMIT 10;
```

[Volver a la presentación del proyecto](README.md)
