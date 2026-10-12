# NÓRD · tienda de demostración

Tienda responsive con modo oscuro, catálogo editable, variantes, carrito sin pagos, pedidos con reservas atómicas y panel privado. Ropa, precios, cantidades, contactos y fotografías son ejemplos. No es una oferta comercial real.

## Entregables distintos

- La aplicación React/Vinext funciona con servidor Cloudflare Workers, base D1 y archivos R2. Contiene panel, inventario, auditoría y pedidos. El correo automático se usa únicamente para códigos de entrada al panel cuando se configura un proveedor.
- `docs/index.html` es la demostración para GitHub Pages. Permite explorar, filtrar y probar el carrito. No recibe pedidos ni datos personales y no autentica administradores.
- `docs/inventario-demo.html` es un panel público de muestra. Permite buscar, filtrar, ajustar cantidades y precios, crear un producto ficticio y revisar una propuesta de cambio por frase. Guarda esos cambios únicamente en el navegador del visitante; no modifica D1/R2, no solicita cuentas ni recibe pedidos. `../nord-inventario-demo.html` es su copia independiente.
- `../nord-copia.html` es la copia visual independiente, con fotografías, fuente y políticas incorporadas para abrirla sin conexión. El código fuente completo es este repositorio.

GitHub Pages sirve archivos estáticos: no ejecuta el servidor ni las reservas. Los pedidos formales enlazan a la aplicación completa cuando exista una URL verificada. La vista de Sites se mantiene privada durante la preparación; un permiso de proveedor dentro de la aplicación no cambia por sí solo la audiencia del alojamiento.

## Iniciar en tu computadora

Requiere Node 22.13 o superior y pnpm. Ejecuta `pnpm install`, `pnpm build` y aplica las migraciones D1 locales pendientes, en orden, siguiendo `drizzle/meta/_journal.json`:

```sh
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0000_lazy_trauma.sql
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0001_hard_jimmy_woo.sql
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0002_early_hawkeye.sql
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0003_aberrant_bulldozer.sql
pnpm dev
```

No repitas migraciones ya aplicadas. Local y producción usan bases separadas. El inicio simulado local `seedy@sites.test` solo funciona en desarrollo y loopback; está deshabilitado en el paquete de producción. La autenticación ChatGPT requiere el alojamiento Sites y sus cabeceras confiables. Para otro alojamiento se debe adaptar esa integración, nunca confiar en cabeceras de identidad enviadas directamente por Internet.

## Administrar sin editar código

En `/admin`, Inventario permite crear productos, editar nombre/descripción/precio, subir JPG/PNG/WebP de hasta 5 MB, destacar u ocultar productos y añadir variantes. Para cambiar cantidades utiliza Actualizar, revisa anterior/nueva y confirma. Los números del catálogo se consultan desde D1; no son texto fijo.

La frase `el CHANEL ahora tiene 60 unidades` prepara una propuesta. Si hay varios productos o variantes compatibles, exige elegir uno. La propuesta vence en diez minutos, pertenece a quien la creó y se rechaza si el inventario cambió entre la revisión y la aplicación. Sin servicio de IA utiliza un intérprete local de frases absolutas; con la clave opcional de OpenAI acepta más formas de escribir. No envía datos de clientes a la IA.

Configuración permite sustituir marca, logotipo, portada, bienvenida, contactos, redes, categorías y plazo de reserva (5 a 10080 minutos). Los datos del operador están pendientes. Antes de quitar el modo de ejemplo exige información comercial. Las imágenes se sirven localmente, con fuentes y licencias en `public/image-credits.html`.

## Acceso y entrega a otro dueño

- Administrador: maneja catálogo, pedidos, configuración, registro y accesos. Este rol corresponde a quien opera la tienda; no exclusivamente al desarrollador.
- Cuenta de inventario: solo ve y ajusta los productos asignados. Por defecto no ve clientes ni pedidos.
- Permiso adicional de pedidos: el administrador puede concederlo por separado. Esa cuenta puede ver el nombre, correo, teléfono/WhatsApp y zona del cliente; confirmar, cancelar y cambiar estados. No puede ver configuración ni gestionar otros accesos. Los límites se verifican en el servidor.
- En Accesos, un administrador asigna productos, concede o retira el permiso de pedidos y revoca cuentas. No se crea una contraseña compartida.

El acceso por correo usa un código aleatorio de seis cifras, válido diez minutos y de un solo uso. Cinco errores bloquean el intento. Se limitan las solicitudes por correo/IP, se guardan códigos con HMAC y sesiones mediante hash, y se usa una cookie HttpOnly/Secure/SameSite=Strict de una hora. Los códigos nunca se muestran en el panel ni en logs. Configura correo y `AUTH_SECRET` para usarlo de verdad.

Un código de correo no es automáticamente más seguro que una buena contraseña con MFA: depende de proteger la cuenta de correo y sigue siendo susceptible a phishing. Activa MFA en las cuentas utilizadas; el acceso por correo no se anuncia como MFA independiente. Referencia: [OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html).

Al vender/transferir el proyecto: el comprador debe controlar alojamiento, dominio, correo y datos. Sustituye `ADMIN_EMAILS` por sus cuentas, revoca proveedores que no continúen, retira el acceso del creador si no existe autorización de mantenimiento y rota los secretos. No entregues una copia con base de clientes ni credenciales.

## Pedidos e inventario

El cliente proporciona nombre, correo, teléfono/WhatsApp y zona/dirección. Acepta políticas y procesamiento necesario mediante casillas sin premarcar. Revisa el resumen antes del envío formal. La consulta pública por WhatsApp es un canal separado.

El servidor vuelve a comprobar precio, producto activo y cantidades. Pedido, líneas y reserva se guardan en un batch D1 atómico. Los triggers comprueban las unidades en el momento de la escritura: dos solicitudes no pueden apartar la misma última unidad. Una clave de idempotencia evita duplicar una solicitud repetida. Los pedidos aparecen en el panel y no generan correos automáticos.

Estados: recibido → confirmado → en preparación → en camino → entregado; cancelación permitida antes de entregado. Cancelar devuelve las unidades una sola vez. Vencer cancela únicamente los recibidos pendientes; confirmar conserva la reserva. En Pedidos hay enlaces para abrir un correo o una conversación de WhatsApp con el número proporcionado por el cliente, sin enviar nada automáticamente. La confirmación interna no sustituye acordar con el cliente cargos, pago, fecha y entrega.

El catálogo y el panel liberan reservas vencidas al consultarse. Configura un programador externo cada minuto para `POST /api/admin/maintenance`, con `Authorization: Bearer <MAINTENANCE_TOKEN>`, para procesar vencimientos sin tráfico. En un Site privado también necesita el acceso de servicio del alojamiento. No publiques esos valores en URLs ni en GitHub.

## Servicios por configurar

1. Códigos de acceso: puedes usar Mailgun sandbox sin dominio propio (`OTP_MAIL_PROVIDER=mailgun`, `MAILGUN_API_KEY`, `MAILGUN_SANDBOX_DOMAIN`, `OTP_MAIL_FROM`) si verificas las dos direcciones autorizadas, o Resend con dominio/remitente verificados (`RESEND_API_KEY`, `MAIL_FROM`). Se envía solo el código OTP. La aceptación de API no demuestra llegada a la bandeja; hay que probar ambas cuentas realmente. No introduzcas claves en el chat.
2. OpenAI es opcional: `OPENAI_API_KEY`, `OPENAI_MODEL`. Sin clave funciona la actualización manual y el intérprete local. Obtén/configura claves por el mecanismo seguro del proveedor, no por el chat ni código del navegador.
3. Mantenimiento: `MAINTENANCE_TOKEN` y programador; copias de seguridad, recuperación, vigilancia de fallos y retención.
4. Operador real, contactos, registro de comercio electrónico, productos reales y condiciones de entrega. Configura dominio/HTTPS y revisa audiencia y cookies del alojamiento antes de abrir al público.

Los secretos se guardan en la configuración del servidor. `.env.example` contiene únicamente nombres y ejemplos vacíos. No subas `.env`, `.dev.vars`, `.wrangler`, sesiones, pedidos ni tokens al repositorio.

## Privacidad y preparación legal

Hay páginas separadas de privacidad, términos, cookies, reembolsos, datos del negocio y créditos. El formulario pide únicamente datos para coordinar; no hay tarjetas, publicidad, analíticas ni píxeles sociales. WhatsApp/redes solo se abren al pulsar enlaces. La fuente e imágenes se alojan localmente.

El almacenamiento técnico del carrito tiene vencimiento de siete días; la preferencia de tema solo persiste al permitir preferencias. Consentimiento de almacenamiento: 180 días, revocable desde el pie. No se cargan analíticas que requieran un consentimiento adicional. Falta auditar las cookies de autenticación/plataforma en el dominio definitivo.

La política propone 15 días de cambios voluntarios para prendas/accesorios sin uso y respeta el retracto legal aplicable de ocho días y el plazo de reintegro de quince días. Un perfume abierto puede excluirse del cambio voluntario por gusto, pero no elimina garantías ni otros derechos. El operador debe confirmar estas condiciones antes de vender.

Las solicitudes canceladas que nunca se confirmaron se anonimizan tras 90 días mediante mantenimiento. La evidencia de relaciones comerciales confirmadas no se borra automáticamente: la obligación de conservación de comercio electrónico es de diez años. Los plazos de códigos OTP y copias de seguridad requieren el calendario del operador.

Documentos de referencia: [Ley de Protección al Consumidor, reformas 2024](https://www.defensoria.gob.sv/wp-content/uploads/download-manager-files/Reformas%202024.pdf), [Registro de comercio electrónico](https://www.defensoria.gob.sv/registrocomercioelectronico/), [Ley de datos personales](https://agenciadeciberseguridad.gob.sv/documentos/decretos/decreto_144_proteccion_datos.pdf), [reforma anunciada el 17 de septiembre de 2026](https://www.asamblea.gob.sv/node/14116). El texto íntegro vigente, sus requisitos y la operación final deben revisarse localmente. No es una garantía de ausencia de reclamos ni una certificación jurídica o de seguridad.

Las licencias de fotografías no conceden automáticamente todos los derechos de marcas/personas. Las recreaciones masculinas con IA están identificadas y conservan atribución y licencias de sus referencias. Antes de ventas reales, utiliza fotos y fichas autorizadas del proveedor y verifica autenticidad/especificaciones. No se afirma afiliación con las marcas.

## Pruebas

`pnpm test`: 23 pruebas de dominio/base de datos, retiro de los perfumes antiguos del inventario, reservas simultáneas, rollback, cancelación, vencimiento, idempotencia, auditoría, consentimientos, conservación, códigos temporales y permisos separados de inventario/pedidos. El proveedor OTP se simuló; no se probó entrega real a una bandeja.

Además se verificaron peticiones HTTP contra D1 local: última unidad con dos pedidos simultáneos (201/409), cancelación una vez, rechazo de cabeceras falsificadas/origen ajeno, panel anónimo, consentimiento inválido y eliminación de fragancias antiguas. Evidencia en `docs/http-check.json`. Recorrido de checkout con teclado, confirmación pendiente y número asignado; imágenes cargadas y diseños móvil/escritorio/claro/oscuro.

## Movimiento y Scroll World

Se usan transiciones CSS cortas, interacción de fotos y un movimiento sutil de portada, respetando movimiento reducido. No hay videos ni cargos de Scroll World. El brief para una futura exploración se conserva en `docs/scroll-world-brief.md`.

Para regenerar las copias: `node scripts/export-inventory-demo.mjs` y `node scripts/export-static.mjs`. Cambiar esas copias no cambia el inventario guardado de la tienda completa.
