# NÓRD · tienda de demostración

[Abrir la demo estática de GitHub Pages](https://loshtpaht.github.io/nord-tienda-ejemplo/). Es una muestra sin ventas ni cobros reales. Permite explorar los 13 productos de ejemplo, filtrar, elegir variantes y usar un carrito con límites de existencias **simuladas**. No recibe pedidos ni datos de clientes, no reserva inventario y no permite iniciar sesión. GitHub Pages no ejecuta la aplicación completa.

`nord-proyecto.zip` contiene el código actualizado de la aplicación React/Vinext con servidor, D1/R2, panel, inventario y pedidos. Descomprímelo y sigue `GUIA.md`. `index.html` es una copia HTML independiente que puede abrirse sin conexión. Las imágenes, precios, cantidades, contactos y políticas son ejemplos o borradores para El Salvador.

La aplicación local permite confirmar pedidos en el panel, ver el correo y teléfono/WhatsApp del cliente solo con permiso de pedidos, y contactarlo manualmente. Los mensajes automáticos se reservan **únicamente** para códigos temporales de acceso, cuando se configure un proveedor de correo. La llegada real de esos códigos no se ha probado. Los pedidos ficticios de prueba no generan correo.

**Todavía no existe una URL pública verificada de la aplicación completa.** El intento de despliegue en Sites falló durante la migración D1; no se alteró a ciegas el historial de migraciones. Vercel Hobby no proporciona directamente los bindings D1/R2 ni la identidad confiable de Sites. Consulta [ESTADO.md](ESTADO.md) para los bloqueos precisos, [AUDITORIA-20-PUNTOS.md](AUDITORIA-20-PUNTOS.md) para la lista del video, [RENDIMIENTO.md](RENDIMIENTO.md) para las mediciones y [THERMOS.md](THERMOS.md) para la revisión de código.

Se aprobaron 23 pruebas automatizadas, TypeScript y compilación local. El ZIP no contiene credenciales, sesiones ni bases de clientes. Antes de vender, hay que sustituir datos, contactos y productos, verificar licencias y revisar las políticas con la operación real. [Créditos de imágenes](image-credits.html).
