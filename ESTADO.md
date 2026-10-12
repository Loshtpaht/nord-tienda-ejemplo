# Estado de publicación de NÓRD

Actualizado el 11 de octubre de 2026. **La tienda completa todavía no tiene una URL pública verificada.**

- Demo estática de GitHub Pages: https://loshtpaht.github.io/nord-tienda-ejemplo/ . El contenido remoto debe sincronizarse con esta revisión antes de considerarlo actualizado.
- Repositorio público: https://github.com/Loshtpaht/nord-tienda-ejemplo . Incluye una copia ZIP del proyecto, también pendiente de sincronización con esta revisión.
- Copia HTML autónoma local: `outputs/nord-copia.html`.
- Aplicación completa local: http://127.0.0.1:8787/ y panel http://127.0.0.1:8787/admin, únicamente mientras el servidor de esta computadora esté activo.

GitHub Pages y el HTML permiten explorar el catálogo y el carrito; no registran pedidos, no reservan unidades, no autentican usuarios y no envían códigos. El servidor local usa D1/R2 locales y sí permite probar pedidos y permisos, pero no constituye publicación por internet.

## Bloqueos de publicación

El despliegue anterior en Sites falló al migrar D1 con `incomplete input: SQLITE_ERROR`. La consulta remota todavía no devuelve bindings ni tablas, de modo que no se puede saber qué parte de las migraciones se aplicó. **No se modificaron las migraciones 0000–0002** ni se reintentó a ciegas. La migración 0003 agrega dos índices y es nueva. Identificadores: Site `appgprj_6acbcb03f2608191a78d9efd2801c769`, despliegue fallido `appgdep_6acbe38f60d0819196a70782b7ddfb09`.

Vercel Hobby no ejecuta directamente los bindings D1 y R2 de esta aplicación ni su autenticación de alojamiento Sites. Una publicación allí exige migrar almacenamiento y adaptar la identidad confiable, preservando transacciones de inventario. No se hará pasar el frontend estático por solución. No hay base y almacén compatibles configurados ni despliegue HTTPS completo probado. Antes de escoger alojamiento definitivo también hay que comprobar términos y límites aplicables a esta demo sin ventas reales.

El correo se usa **solo para códigos temporales de acceso**: `magustin2007@gmail.com` como administrador y `agustinlunamarcos@gmail.com` como cuenta de inventario; el administrador puede otorgar aparte permiso de pedidos. No se generan correos automáticos por pedido ni al cliente ni a la tienda. El administrador o gestor de pedidos ve correo y teléfono/WhatsApp del cliente y lo contacta manualmente. Para OTP real faltan una cuenta y credenciales de Mailgun sandbox con destinatarios autorizados, o dominio/remitente verificado en Resend. No se ha probado llegada a ninguna bandeja.

## Evidencia local

Pasaron 23 pruebas de dominio/DB, TypeScript y compilación de producción. Incluyen carrera por última unidad, idempotencia, rollback, cancelación, vencimiento, permisos de inventario/pedidos y OTP simulado. La última unidad con dos solicitudes HTTP simultáneas tuvo 201/409 en la pasada previa; hay que repetir en hosting antes de producción. La ruta 404 devuelve 404 real; inicio, panel y páginas legales devuelven 200 y `noindex`. Se añadieron sitemap, robots, Open Graph, favicon, controles de cantidad y WebP.

La prueba de lectura local de catálogo (400 solicitudes, hasta 50 concurrentes) produjo cero errores y p95 de 48, 330, 1309 y 2299 ms según el escalón. No prueba capacidad de 1.000–3.000 usuarios simultáneos. Los planes de consulta justifican dos índices nuevos. Detalles en `outputs/nord/docs/auditoria-20-puntos.md`, `rendimiento-y-limites.md` y `thermos-review.md`.

El negocio, sus contactos, políticas definitivas, dominio, copias de seguridad remotas, mantenimiento programado, entrega OTP y revisión legal de El Salvador siguen pendientes. Es una demostración sin pagos ni ventas reales.
