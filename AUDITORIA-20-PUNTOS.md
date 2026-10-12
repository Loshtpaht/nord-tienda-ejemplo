# Auditoría de los 20 puntos del video — NÓRD

Fecha: 11 de octubre de 2026. Alcance: aplicación local de producción y copia estática; la aplicación completa aún no tiene URL pública funcional. «Comprobado» indica evidencia local, no certificación del futuro alojamiento.

| Punto exacto | Estado previo y corrección | Comprobación y pendiente |
|---|---|---|
| 1. Página 404 | Faltaba una respuesta propia. Se agregó una página con regreso al catálogo. | `/ruta-que-no-existe` devuelve HTTP 404 local. |
| 2. Diseño | Ya había diseño NÓRD y temas. Se pulieron controles táctiles y transiciones CSS con movimiento reducido. | Recorrido visual móvil, escritorio, claro y oscuro. Falta revisar la versión en dominio definitivo. |
| 3. Links OK | La copia estática apuntaba a una aplicación aún no publicada. Se sustituyó por aviso de pendiente; enlaces internos legales y navegación se comprobaron localmente. | Rutas públicas 200, ruta inexistente 404 y cero IDs duplicados en el inicio. Los enlaces de WhatsApp/redes de ejemplo no son contactos reales. |
| 4. Formularios | Se añadieron teléfono obligatorio, consentimiento sin premarcar, límites de cantidad y permisos de pedidos. | Recorrido local: datos ficticios, resumen con correo/teléfono, pedido `NRD-000009` y reserva; OTP probado con proveedor simulado. Falta entrega real de OTP. |
| 5. Loading states | Había carga de catálogo y acciones. Se agregó pantalla de carga de ruta y estados con controles bloqueados durante envíos. | Recorridos locales; falta medir conexión lenta real en alojamiento. |
| 6. Manejo de errores | El servidor ya validaba existencias y consentimiento. Se mejoraron mensajes de respuesta y 404. | Casos 409 de stock, inválidos y sesión anónima; fallos reales de correo/red quedan pendientes de proveedor. |
| 7. SEO básico | HTML semántico y encabezados existentes. Se añadieron rutas públicas explícitas y se evitó indexar la demo. | Respuestas HTML locales; URL canónica definitiva pendiente de dominio y contenido reales. |
| 8. Meta description | Descripción general anterior. Se añadieron descripciones por página legal y error. | Inspección de HTML local. |
| 9. Título por página | Se añadieron títulos específicos en legales, panel y 404. | Inspección de `<title>` local; 404 corregida en la última compilación. |
| 10. Sitemap.xml | No existía. Ahora enumera inicio y políticas, sin panel ni API. | `/sitemap.xml` devuelve XML local. El origen público definitivo se genera a partir de la solicitud; no se anuncia indexación de demo. |
| 11. Robots.txt | No existía. Bloquea panel/API y apunta al mapa; toda la demo lleva `noindex`. | `/robots.txt` y `X-Robots-Tag: noindex, nofollow` por HTTP local. |
| 12. Alt text | Las fichas tenían nombres alternativos; decorativas usan texto vacío donde corresponde. | Revisión de componentes y navegador. La redacción final debe seguir las fotos reales del operador. |
| 13. Open Graph | Se agregó imagen de 1200×630 y textos para inicio/legales. | Metadatos HTML locales. Previsualización real de redes pendiente de HTTPS público. |
| 14. Favicon | SVG NÓRD existente, declarado en metadatos. | Solicitud local `/favicon.svg` 200 y pestaña del navegador. |
| 15. HTTPS | Política HSTS cuando llega solicitud HTTPS en producción. | Solo HTTP loopback probado; redirección y certificado deben verificarse en host definitivo. |
| 16. Analytics | No hay analíticas ni píxeles, por privacidad y porque es demo. Las cookies técnicas y de preferencia se explican y la preferencia requiere consentimiento. | Código y solicitudes locales revisados; analizar cookies del host definitivo antes de activar analíticas. No se envían datos de cliente a un tercero. |
| 17. Imágenes | Las fotos grandes de portada, pantalón y fragancias se sirvieron como WebP; originales y créditos se conservaron. | Ningún archivo de imagen supera 8 MB. Verificación visual local. |
| 18. Accesibilidad | Se añadieron controles −/+ uniformes, límites y disponibilidad anunciada; estados de foco y movimiento reducido. | Recorrido local con teclado y contraste revisado visualmente. Queda pendiente auditoría instrumental completa de lector de pantalla/contraste en URL definitiva. |
| 19. Consola | En los recorridos locales previos no aparecieron errores de navegador relevantes. | Tras la última compilación, inicio y checkout devolvieron cero `warn/error` en consola. Falta hosting; ESLint aún señala deuda preexistente. |
| 20. Rendimiento | WebP, fuente local y sin vídeo/analíticas ni scripts de terceros. Se midió catálogo bajo carga local. | 400 lecturas loopback, hasta 50 concurrencia, cero errores; p95 máximo 2299 ms. No equivale a prueba de producción ni a miles simultáneos. |

Las fotografías, contactos y textos comerciales son ejemplos. Las políticas para El Salvador son borradores y requieren revisión con los datos y operación reales del negocio.
