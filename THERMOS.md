# Revisión Thermos

Se ejecutaron dos pases independientes en paralelo con las skills locales `thermo-nuclear-review` y `thermo-nuclear-code-quality-review`, coordinados por `thermos`. Se les proporcionó el mismo alcance: diferencias del proyecto NÓRD, código de inventario, pedidos, permisos, OTP, exportación estática, migraciones y pruebas. Es una adaptación de las instrucciones de Cursor a subagentes de Codex; no implica tener instalado el plugin nativo de Cursor.

| Pase | Hallazgo | Acción |
|---|---|---|
| Seguridad/corrección | Una actualización de imágenes de ejemplo podía sobrescribir campos de producto editados por la tienda. | Se limitó a migrar referencias de PNG predeterminadas a WebP y se añadió prueba de conservación de cambios. |
| Seguridad/corrección | El procesador de correo previo podía dejar trabajos en cola al cambiar el modo demo. | El requisito actual desactiva todos los correos de pedido y deja solo OTP; se retiró su activación. No se envía cola antigua. |
| Seguridad/corrección | Documentación de correo y privacidad obsoleta. | README, políticas y estado se actualizaron a OTP exclusivo y contacto manual. |
| Calidad | Migración de datos de ejemplo durante lectura de catálogo y lógica compacta. | Se simplificó y se hizo monótona; queda como deuda moverla a una migración operativa cuando se conozca el estado remoto. |
| Calidad | Exportador HTML basado en sustituciones de texto y copia de 2,38 MB con imágenes embebidas. | Se mantiene el HTML autónomo solicitado; antes de evolucionar el exportador conviene transformarlo con estructura DOM y separar recursos en Pages. |

Se volvieron a ejecutar 23 pruebas, tipos y compilación de producción. Además se revisaron UX, accesibilidad, SEO, rendimiento y despliegue por separado en `auditoria-20-puntos.md`. La imposibilidad de verificar la base remota de Sites y el correo real son límites del resultado, no aprobaciones implícitas.
