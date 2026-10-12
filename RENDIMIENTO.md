# Rendimiento, consultas y crecimiento

La prueba de carga del 11 de octubre de 2026 se limitó a `GET /api/catalog` en `127.0.0.1:8787` con D1 local, 100 solicitudes por escalón. No afectó servicios externos ni generó cargos.

| Concurrencia | Solicitudes | p50 | p95 | Errores |
|---:|---:|---:|---:|---:|
| 1 | 100 | 34 ms | 48 ms | 0 |
| 10 | 100 | 268 ms | 330 ms | 0 |
| 25 | 100 | 624 ms | 1309 ms | 0 |
| 50 | 100 | 697 ms | 2299 ms | 0 |

Son lecturas de catálogo, no sesiones completas ni escrituras de pedidos. **1.000–3.000 visitas al día** distribuidas en horas y **1.000–3.000 usuarios simultáneos** representan cargas muy distintas. Estos números no prueban capacidad para ninguno de esos volúmenes en producción. Para una promesa de capacidad faltan entorno publicado, límites reales del plan, datos representativos, métricas de D1/R2, mezcla de lecturas y escrituras, y ensayo gradual autorizado.

Se usó `EXPLAIN QUERY PLAN` sobre las migraciones locales. La búsqueda de pedidos recibidos vencidos pasaba de `SCAN orders` a `SEARCH orders USING COVERING INDEX orders_status_expiry_idx (status=? AND expires<?)`. La búsqueda de variantes por producto pasaba de `SCAN variants` a `SEARCH variants USING INDEX variants_product_idx (product_id=?)`. Esos dos índices están en la migración **nueva** `0003_aberrant_bulldozer.sql`; las migraciones previas no se tocaron. `order_lines.order_id`, `orders.order_key` y `auth_sessions.token_hash` ya tenían índices o claves. La consulta reciente de pedidos/auditoría usa orden por ID con límite 300. Se agruparon variantes en memoria con un `Map` para evitar un filtro por cada producto.

La reserva de última unidad, clave de idempotencia, cancelación y vencimiento tienen pruebas de base de datos. El vencimiento automático requiere programador externo y token secreto. El respaldo y la restauración de D1/R2 todavía no se han probado en alojamiento; deben hacerse antes de usar datos reales. Las sesiones y auditoría deben monitorizarse en el proveedor final.
