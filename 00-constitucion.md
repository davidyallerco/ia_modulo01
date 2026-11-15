# 00 — Constitución del proyecto LUQUITA

> **Lectura obligatoria.** Todo agente de IA y toda persona que trabaje en este repositorio debe leer este documento antes de hacer cualquier cambio. Estos principios no se negocian. Si una tarea exige violar uno, la tarea se detiene y se escala a un humano del equipo.

- **Proyecto:** Luquita, billetera digital infantil con tag NFC para la tienda del colegio, con consumo pospago en convenio con el colegio.
- **País de operación:** Perú.
- **Piloto:** colegio particular en Lurigancho-Chosica, Lima (unos 200 estudiantes de 8 a 14 años).
- **Versión:** 1.17 (**aprobada** el 2026-11-13 por David Yallerco, responsable humano del proyecto).

### Historial de cambios

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 2026-11-04 | Versión inicial con modelo de saldo prepagado y recargas manuales por Yape/Plin. |
| 1.1 | 2026-11-04 | Cambio a modelo pospago: suscripción mensual en convenio con el colegio y consumo cobrado en la boleta del colegio. Se eliminan las recargas, el saldo y el desembolso. |
| 1.2 | 2026-11-04 | Se agregan: suscripción de S/ 5 por familia en el consolidado, tope mensual de S/ 300, bloqueo de consumo por boleta impaga, PIN del alumno en cada compra, detalle de consumo para el padre a pedido y eliminación de datos 30 días después de terminar el año escolar. |
| 1.3 | 2026-11-04 | El detalle que recibe el padre es de compras (fecha, hora y monto), no de productos, y lo solicita desde la app móvil. |
| 1.4 | 2026-11-04 | El colegio registra las familias registradas desde un panel web (Angular 20 aprobado). Se elimina el bloqueo de compras por deuda. Cada compra puede llevar una descripción referencial. Los datos se conservan 5 años desde el cierre del año escolar. |
| 1.5 | 2026-11-04 | La familia se identifica por el DNI del apoderado. El Excel del colegio trae DNI, nombre, apellidos, correo y estado del apoderado. Al terminar el año escolar, todas las familias pasan a inactivas. |
| 1.6 | 2026-11-04 | El periodo mensual se cierra el día 30 de cada mes. La suscripción solo se cobra a familias activas al cierre. Los ajustes los registra la secretaria del colegio. |
| 1.7 | 2026-11-04 | El periodo mensual se cierra el último día de cada mes (30 o 31), de forma que coincide con el mes calendario. El año escolar empieza en marzo. El apoderado puede configurar un tope mensual menor a S/ 300. El administrador de Luquita registra los colegios, las secretarias y los dueños de tienda. |
| 1.8 | 2026-11-04 | La secretaria inicia sesión con su DNI y el dueño de la tienda con su correo, ambos con una clave aleatoria que reciben por correo. El tope personalizado aplica de inmediato y no puede ser negativo. |
| 1.9 | 2026-11-04 | El cambio de la clave recibida por correo es obligatorio en el primer inicio de sesión. Cada alumno nuevo tiene un tope mensual de S/ 300 por defecto, que solo el apoderado puede cambiar. |
| 1.10 | 2026-11-04 | Los ajustes no cuentan para los límites de consumo; solo cambian el monto que paga la familia en el consolidado. |
| 1.11 | 2026-11-05 | Un solo apoderado por familia. Solo la secretaria cambia el correo del apoderado. Las familias se registran cada año después de la matrícula. El Libro de Reclamaciones se responde en máximo 3 días hábiles. |
| 1.12 | 2026-11-05 | Sin conexión a internet no se registran compras. El colegio entrega los tags en blanco. Los reclamos los responde el administrador de Luquita. |
| 1.13 | 2026-11-05 | Los reclamos los responde el dueño de la tienda, y la respuesta se envía al correo del apoderado. |
| 1.14 | 2026-11-05 | Excepción del PRINCIPIO-007: al responder un reclamo, el dueño de la tienda ve el texto del reclamo y la compra disputada, sin DNI ni consumo acumulado. |
| 1.15 | 2026-11-05 | Unificación de términos: se usa siempre «apoderado» (no «padre») y un único modelo `Movimiento` con tipo `COMPRA`, `ANULACION` o `AJUSTE` (no «asiento»). |
| 1.16 | 2026-11-11 | Decisión del equipo para el piloto: los alumnos solo compran de marzo a noviembre, mientras la secretaria del colegio o el administrador de Luquita tengan habilitadas las ventas; el primer periodo es el mes de la primera habilitación y no hay periodos de diciembre a febrero. Se retiran el Libro de Reclamaciones y el detalle de protección de datos personales (consentimiento, plazo de conservación de 5 años y eliminación), por ser un piloto académico. Se mantienen los datos mínimos del menor y la prohibición de perfilamiento. |
| 1.17 | 2026-11-13 | **Aprobada por David Yallerco (2026-11-13).** Simplificación del piloto: se retira la suscripción de S/ 5.00; no hay temporada de ventas ni cierre mensual automático (el consolidado se calcula por mes calendario al consultarlo); los alumnos solo compran mientras la app del dueño de la tienda esté activa, y la activan o desactivan la secretaria del colegio o el administrador de Luquita; el Excel lo importan la secretaria o el administrador y toda familia importada queda activa; se retira el fin del año escolar; el PIN del alumno pasa a 3 dígitos. Reemplaza la temporada de marzo a noviembre de la versión 1.16. |

---

### PRINCIPIO-001: Español latinoamericano en todo el proyecto

El código (clases, métodos, variables, endpoints, tablas, columnas), los comentarios, la documentación, los mensajes de commit y todos los textos de la interfaz se escriben en español latinoamericano; la app no tiene otros idiomas. Los identificadores de código usan solo caracteres ASCII, sin tildes ni ñ (por ejemplo `anio`, `consumoMensual`). Las palabras reservadas del lenguaje y los nombres propios de los frameworks quedan como son.

Ejemplo de cumplimiento: `POST /api/v1/compras` atendido por `CompraService.registrarCompra(...)`, que persiste un `Movimiento` de tipo `COMPRA` en la tabla `movimiento`; la UI muestra "Consumo de noviembre: S/ 85.50".
Ejemplo de violación: un endpoint `POST /api/v1/purchases` con una clase `PurchaseService`, o un botón que dice "Submit".

---

### PRINCIPIO-002: Stack tecnológico cerrado

El stack es: backend en Java 21 con Spring Boot 3.x; base de datos PostgreSQL; apps móviles (apoderados y vendedores) en Flutter; panel web del colegio y del administrador de Luquita en Angular 20; infraestructura en Microsoft Azure. Para introducir otro lenguaje, framework, base de datos o proveedor de nube se necesita un ADR aprobado por un humano.

Ejemplo de cumplimiento: el módulo de notificaciones por correo se implementa como un servicio Spring Boot que se despliega en Azure.
Ejemplo de violación: un agente crea una función en Node.js en AWS Lambda para enviar los correos, sin ADR aprobado.

---

### PRINCIPIO-003: Luquita registra consumo pospago, no maneja dinero

Luquita registra el consumo de cada alumno en la tienda del colegio y lo resume por mes calendario, en hora de Lima, en un consolidado que se calcula al consultarlo, para que el colegio lo cobre en su boleta, según el convenio. No hay cierre mensual automático ni temporada de ventas: los alumnos solo pueden comprar mientras la app del dueño de la tienda esté activa, y la activan o desactivan la secretaria del colegio o el administrador de Luquita. Cada año escolar, después de la matrícula, la secretaria del colegio o el administrador de Luquita importan en el panel web un archivo Excel con las familias que se suman al servicio (la secretaria también puede registrarlas manualmente), con el DNI, nombre, apellidos y correo del apoderado; toda familia importada queda en estado activo. La secretaria puede pasar una familia a estado inactivo. El administrador de Luquita registra cada colegio, su secretaria (con su DNI y correo) y el dueño de su tienda (con su correo). La familia se identifica por el DNI del apoderado: el apoderado solo puede crear su cuenta en la app si su DNI pertenece a una familia activa registrada por el colegio. Las deudas de la boleta las gestiona el colegio fuera del sistema: Luquita no bloquea compras por deuda. Luquita no procesa pagos ni cobra: no recibe recargas, no guarda saldos, no se integra con pasarelas, Yape o Plin, no almacena datos de tarjetas o cuentas bancarias y no emite comprobantes SUNAT. Todos los pagos ocurren fuera del sistema: el cobro de la boleta y el pago mensual del colegio al dueño de la tienda por el consumo total. Luquita no cobra suscripción ni comisiones en el piloto. El dueño de la tienda solo registra compras con el tag NFC; nunca registra pagos ni recargas.

Ejemplo de cumplimiento: una familia con dos hijos aparece en el consolidado de noviembre como "Ana: S/ 142.00 — Luis: S/ 98.50 — Total familia: S/ 240.50"; un test verifica que el consolidado no agrega ningún cargo además del consumo.
Ejemplo de violación: agregar al consolidado un cargo de suscripción o comisión, crear una tabla `recarga` o `saldo`, integrar la API de un banco o de Yape, calcular o transferir pagos entre el colegio y la tienda dentro del sistema, o generar boletas electrónicas desde Luquita.

---

### PRINCIPIO-004: Integridad del consumo

Los montos se manejan en soles (PEN):

- En Java con `BigDecimal` a escala 2 y `RoundingMode.HALF_EVEN`.
- En PostgreSQL con `NUMERIC(12,2)`.
- En Flutter como enteros en céntimos.
- `float` y `double` están prohibidos para montos.

Cada compra es un movimiento inmutable en la tabla `movimiento`, de solo inserción, con un campo `tipo` que vale `COMPRA`, `ANULACION` o `AJUSTE`; el consumo mensual de cada alumno se calcula como la suma de sus movimientos del mes calendario. Cada operación lleva una clave de idempotencia. Una compra errónea no se edita ni se borra: se anula con un movimiento de tipo `ANULACION` por el mismo monto en signo contrario. Cada movimiento se registra con la fecha y hora del servidor, nunca con una fecha pasada, así que un mes ya entregado al colegio no cambia: cualquier corrección la registra la secretaria del colegio como `AJUSTE` en el mes en curso. Los `AJUSTE` no cuentan para los límites diario y mensual: solo cambian el monto del consolidado que paga la familia.

Ejemplo de cumplimiento: una compra de S/ 3.50 inserta un movimiento `COMPRA` de +3.50. Si el NFC se lee dos veces, la segunda petición con la misma `Idempotency-Key` devuelve el mismo resultado sin duplicar el consumo. Para corregirla se inserta `ANULACION` de −3.50.
Ejemplo de violación: crear tablas separadas `compra`, `anulacion` o `ajuste`, `UPDATE movimiento SET monto = 2.50`, un `DELETE` sobre la tabla `movimiento`, insertar un movimiento con una fecha pasada, o un `double precio` en una entidad.

---

### PRINCIPIO-005: Límites de consumo y controles parentales obligatorios

Cada alumno puede consumir como máximo S/ 30.00 por día calendario y S/ 300.00 por mes calendario, en hora de Lima (`America/Lima`). Cada alumno nuevo recibe un tope mensual de S/ 300.00 por defecto; solo su apoderado puede cambiarlo, a un valor entre S/ 0.00 y S/ 300.00, y el cambio aplica de inmediato. Ambos límites se validan en el servidor, dentro de la misma transacción que la compra. Si la app del vendedor no tiene conexión a internet, no se registra la compra: no hay cobro sin conexión ni compras guardadas para enviarse después. Cada compra confirmada genera un correo al apoderado con fecha, hora, tienda, monto y consumo acumulado del mes; si el envío falla, se reintenta y queda registrado. Cuando un apoderado bloquea un tag, la siguiente compra con ese tag se rechaza: el estado del tag no se guarda en caché.

Ejemplo de cumplimiento: un alumno que ya consumió S/ 27.00 hoy intenta comprar S/ 5.00 y recibe `422 LIMITE_DIARIO_EXCEDIDO`. Uno que acumula S/ 298.00 en el mes intenta comprar S/ 4.00 y recibe `422 LIMITE_MENSUAL_EXCEDIDO`.
Ejemplo de violación: validar los límites solo en la app Flutter, configurarlos con valores distintos en el código, o cachear el estado del tag por 5 minutos.

---

### PRINCIPIO-006: Tag NFC más PIN del alumno

Los tags NTAG213/NTAG215 guardan únicamente un token aleatorio de 128 bits (UUID v4) en un registro NDEF; el consumo y los datos viven solo en el servidor. El colegio entrega los tags en blanco y el apoderado los graba desde la app. Después de grabarse, el tag se bloquea contra escritura (lock bits). El tag nunca contiene DNI, nombre, montos ni identificadores secuenciales. Toda compra requiere los dos factores: el tag físico y el PIN que el apoderado crea para su hijo, que el alumno ingresa en el celular del vendedor. Sin PIN válido no se registra la compra.

Ejemplo de cumplimiento: el tag contiene `luquita:tag:9f1c2e7a-4b3d-4e8f-a1c6-2d5e7f9b0a13`; el servidor resuelve el alumno, verifica el PIN y recién entonces inserta la compra. Un test verifica que `POST /compras` sin PIN o con PIN incorrecto responde `401` y no crea ningún movimiento.
Ejemplo de violación: grabar en el tag `DNI=71234567;consumo=20.00`, usar el ID autoincremental del alumno como contenido del tag, o registrar una compra solo con el tag, sin PIN.

---

### PRINCIPIO-007: Datos mínimos de menores y ningún perfilamiento

Del alumno solo se guardan DNI y nombre. Cada familia tiene un solo apoderado, del que solo se guardan DNI, nombre, apellidos y correo; su correo solo lo puede cambiar la secretaria del colegio. La app del vendedor muestra solo el nombre del alumno: nunca el DNI ni el consumo acumulado. Al colegio solo se le entrega el consolidado mensual por familia (nombre y DNI de cada alumno y total de cada uno). El detalle de compras (fecha, hora, monto y descripción referencial de cada compra) se entrega únicamente al apoderado de ese alumno, cuando lo solicita desde la app móvil de Luquita. Está prohibida cualquier analítica, publicidad, perfilamiento de consumo o cesión de datos de menores a otros terceros. Por ser un piloto académico, el consentimiento formal, los plazos de conservación y la eliminación de datos quedan fuera del alcance.

Ejemplo de cumplimiento: la respuesta de `GET /vendedor/tags/{token}` devuelve `{ "nombreAlumno": "Ana" }` y nada más; el reporte para el colegio no lista compras individuales.
Ejemplo de violación: agregar campos como foto, alergias o grado sin un cambio aprobado de esta constitución, mostrar al vendedor el consumo del mes, enviar al colegio el detalle de compras de cada alumno, mostrar el detalle de un alumno a otro apoderado, o integrar Firebase Analytics o Google Ads.

---

### PRINCIPIO-008: Cifrado, secretos y PIN

Estos son los mínimos obligatorios:

- **Datos en tránsito:** TLS 1.2 como mínimo, preferiblemente 1.3.
- **Base de datos en reposo:** Azure Database for PostgreSQL con cifrado AES-256 usando llaves administradas por el cliente en Azure Key Vault.
- **DNI:** cifrado a nivel de columna con AES-256-GCM (nonce de 96 bits único por registro), más un índice ciego HMAC-SHA256 para búsquedas.
- **PIN de apoderados:** 6 dígitos, guardado como hash Argon2id (m=19456 KiB, t=2, p=1). Tras 5 intentos fallidos la cuenta se bloquea 15 minutos.
- **Clave de secretarias y dueños de tienda:** clave aleatoria que el sistema genera y envía a su correo al registrarlos, guardada como hash Argon2id con los mismos parámetros. La secretaria inicia sesión con su DNI y el dueño de la tienda con su correo. En su primer inicio de sesión deben cambiar obligatoriamente la clave recibida antes de hacer cualquier otra acción. Tras 5 intentos fallidos la cuenta se bloquea 15 minutos.
- **PIN del alumno:** 3 dígitos, definido y cambiable solo por su apoderado, y guardado como hash Argon2id con los mismos parámetros. En el celular del vendedor se ingresa enmascarado. Tras 3 intentos fallidos el tag se bloquea y solo el apoderado puede desbloquearlo.
- **Tokens:** JWT firmados con ES256 y expiración de 15 minutos.
- **Secretos:** solo en Azure Key Vault, accedidos mediante Managed Identity.
- **Logs:** nunca registran PIN, claves, DNI, tokens ni secretos.

Ejemplo de cumplimiento: la columna `alumno.dni_cifrado` es `bytea`, la búsqueda usa `alumno.dni_hmac`, `alumno.pin_hash` empieza con `$argon2id$`, y el CI ejecuta gitleaks sin hallazgos.
Ejemplo de violación: guardar un PIN con SHA-256 o en texto plano, mostrar el PIN del alumno en la pantalla del vendedor, escribir `spring.datasource.password=...` en `application.yml`, o un log como `Login PIN=1234`.

---

### PRINCIPIO-009: Lo que un agente de IA NO puede hacer

**Totalmente prohibido para agentes de IA:**

- Acceder o consultar datos reales de producción.
- Modificar código de autenticación, PIN o cifrado.
- Agregar dependencias nuevas.
- Modificar o desactivar tests existentes. Sí puede agregar tests nuevos.
- Ejecutar migraciones de base de datos.
- Usar datos reales de menores en prompts o fixtures.
- Hacer commit de secretos.
- Hacer merge sin revisión humana.

**Solo con aprobación humana explícita:**

- Desplegar a producción.
- Modificar la lógica de consumo, ledger, límites, activación de la app del dueño o consolidado mensual.

Los agentes pueden enviar código a servicios de IA externos siempre que no incluya datos sensibles: DNI, nombres reales, correos, PIN, tokens, secretos o datos de producción.

Ejemplo de cumplimiento: un agente propone un PR que agrega un test y usa el fixture `Alumno("Alumno Prueba", "00000000")`; un humano lo revisa y hace el merge.
Ejemplo de violación: un agente agrega `@Disabled` a un test que falla, añade una librería a `pom.xml`, ejecuta `flyway migrate`, cambia el tope mensual de S/ 300 sin aprobación, o pega un registro real de `alumno` en un prompt.

---

### PRINCIPIO-010: Calidad mínima verificable en CI

Todo PR debe pasar CI con pruebas unitarias obligatorias y una cobertura de líneas de al menos 80 % por proyecto. La cobertura se mide con JaCoCo en el backend, con `flutter test --coverage` en las apps y con el reporte de cobertura de Angular CLI en el panel. Si el CI falla, el merge queda bloqueado, y todo merge necesita al menos 1 aprobación humana.

Ejemplo de cumplimiento: un PR con 84 % de cobertura, tests en verde y una aprobación se integra a `main`.
Ejemplo de violación: bajar el umbral de JaCoCo a 70 %, excluir paquetes del reporte para inflar la cobertura, o hacer merge con el CI en rojo.

---

### PRINCIPIO-011: Accesibilidad WCAG 2.1 AA

Las apps Flutter y el panel Angular cumplen WCAG 2.1 nivel AA. En concreto:

- Contraste de al menos 4.5:1 para texto normal.
- Áreas táctiles de al menos 48×48 dp, incluido el teclado del PIN del alumno.
- Etiquetas `Semantics` o `aria-label` en todos los controles.
- Soporte para escalar el texto al 200 % sin perder contenido.

Ejemplo de cumplimiento: el botón "Bloquear tag" tiene `Semantics(label: 'Bloquear tag de Ana')` y pasa la verificación de contraste.
Ejemplo de violación: un botón que es solo un ícono, sin etiqueta, o un texto gris claro con contraste 2.8:1 sobre fondo blanco.

---

### PRINCIPIO-012: Documentación en el repositorio

Toda la documentación vive en el repositorio en Markdown. Cada endpoint está descrito en una especificación OpenAPI 3.x versionada. La arquitectura se documenta con diagramas C4 (al menos contexto y contenedores) y las decisiones con ADRs. Los commits siguen Conventional Commits en español. Un PR que agrega o cambia un endpoint debe actualizar la especificación OpenAPI en el mismo PR.

Ejemplo de cumplimiento: el commit `feat(consumo): generar consolidado mensual por familia para el colegio` incluye el cambio en `docs/api/openapi.yaml`.
Ejemplo de violación: un commit "cambios varios", un endpoint nuevo que no aparece en el OpenAPI, o documentación de arquitectura guardada solo en un Drive externo.

---

## Cómo se modifica esta constitución

Para cambiar esta constitución se necesita un PR dedicado, con la etiqueta `constitucion` y aprobación humana, y una nueva fila en el historial de cambios. Ningún agente de IA puede editar este archivo por iniciativa propia.
