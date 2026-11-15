# 04 — Glosario (lenguaje ubicuo): LUQUITA

Términos del dominio que deben usarse igual en la documentación, en las conversaciones del equipo y en el código. El nombre en código sigue el PRINCIPIO-001: español, solo caracteres ASCII, sin tildes ni ñ.

- **Documentos base:** `00-constitucion.md` (versión 1.17), `02-alcance.md` y `03-reglas-de-negocio.md`.
- **Fecha:** 2026-11-04. **Actualización:** 2026-11-11 (se retiran Reclamo, Consentimiento y Libro de Reclamaciones) y 2026-11-13 (se retiran Suscripción, Periodo, Cierre mensual, Fin del año escolar y Temporada de ventas; Habilitación de ventas pasa a «Activación de la app del dueño»; PIN del alumno de 3 dígitos).

---

| # | Término | Nombre en código | Definición | Reglas relacionadas |
|---|---|---|---|---|
| 1 | **Familia** | `Familia` | Grupo formado por exactamente un apoderado y uno o más alumnos del colegio. Es la unidad a la que el colegio cobra el consumo de sus hijos. | RN-006, RN-022, RN-086 |
| 2 | **Apoderado** | `Apoderado` | Padre, madre o tutor responsable de la familia (único por familia), identificado por su DNI. De él se guardan DNI, nombre, apellidos y correo. Crea la cuenta, registra a sus hijos, define sus PIN, vincula y bloquea los tags y recibe los correos. Es el único término para este actor: no usar «padre» en el código ni en la documentación. | RN-005, RN-028, RN-058, RN-060 |
| 3 | **Alumno** | `Alumno` | Estudiante de 8 a 14 años que compra en la tienda. Solo se guardan su DNI y su nombre. | RN-001 |
| 4 | **Vendedor** | `Vendedor` | Dueño o encargado de la tienda del colegio que cobra las compras desde su celular con NFC, solo mientras su app esté activa. | RN-040, RN-041, RN-097, RN-103 |
| 5 | **Tienda** | `Tienda` | Quiosco o cafetería del colegio piloto, único lugar donde se puede usar Luquita. | RN-023, RN-042 |
| 6 | **Colegio** | `Colegio` | Institución que firma el convenio, registra las familias, cobra el consolidado en su boleta y paga al dueño de la tienda. Gestiona las deudas fuera del sistema. | RN-021, RN-022, RN-031 |
| 7 | ~~**Suscripción**~~ | — | Retirada el 2026-11-13: Luquita no cobra suscripción en el piloto. El antiguo término «familia suscrita» pasó a ser «familia registrada» (registrada por el colegio). | — |
| 8 | **Tag** | `Tag` | Sticker o llavero NFC de tipo NTAG213 o NTAG215 que el alumno usa para comprar. Lo entrega en blanco el colegio o lo compra el apoderado. | RN-007, RN-009, RN-091 |
| 9 | **Token del tag** | `tokenTag` | Identificador UUID v4 (128 bits, 122 aleatorios) grabado en el tag. Es lo único que contiene el tag. | RN-007 |
| 10 | **Vinculación** | `Vinculacion` | Acción del apoderado que, desde la app móvil, graba el token en el tag, lo bloquea contra escritura y lo asocia a uno de sus hijos. | RN-006, RN-008 |
| 11 | **Desvinculación** | `Desvinculacion` | Acción del apoderado que retira un tag del alumno, por ejemplo al perderlo. El tag desvinculado ya no permite comprar. | RN-010, RN-011 |
| 12 | **Bloqueo de tag** | `BloqueoTag` | Estado en que el tag no permite compras. Se produce cuando el apoderado lo bloquea o tras 3 PIN fallidos consecutivos. Solo el apoderado puede desbloquearlo, sea cual sea el motivo del bloqueo. | RN-016, RN-026, RN-027, RN-028 |
| 13 | **PIN del alumno** | `pinAlumno` | Clave de 3 dígitos que el apoderado define para su hijo y que el alumno digita en cada compra. | RN-004, RN-013, RN-027 |
| 14 | **PIN de acceso** | `pinAcceso` | Clave de 6 dígitos con la que el apoderado ingresa a su app, junto con su DNI. | RN-003, RN-029, RN-060 |
| 15 | **Compra** | `TipoMovimiento.COMPRA` | Consumo de un alumno en la tienda, registrado con tag y PIN válidos. Es un `Movimiento` de tipo `COMPRA` con monto positivo; no es una entidad aparte. | RN-012, RN-014 |
| 16 | **Movimiento** | `Movimiento` | Registro inmutable de la tabla `movimiento`. Su campo `tipo` vale `COMPRA`, `ANULACION` o `AJUSTE`. Nunca se edita ni se borra. | RN-034, RN-035, RN-036 |
| 17 | **Ledger** | tabla `movimiento` | Conjunto de solo inserción de todos los movimientos, del que se calcula el consumo. | RN-035 |
| 18 | **Anulación** | `TipoMovimiento.ANULACION` | Movimiento con el mismo monto de una compra errónea en signo contrario, dentro del mes en curso. | RN-035 |
| 19 | **Ajuste** | `TipoMovimiento.AJUSTE` | Movimiento que corrige un consumo de un mes anterior. Lo registra solo la secretaria del colegio, en el mes en curso. No cuenta para los límites; solo cambia el monto del consolidado. | RN-036, RN-064, RN-080, RN-081 |
| 20 | **Clave de idempotencia** | `claveIdempotencia` | Identificador único de cada solicitud de compra que evita registrarla dos veces, por ejemplo por una doble lectura del tag. | RN-015 |
| 21 | **Consumo diario** | `consumoDiario` | Suma de los movimientos `COMPRA` y `ANULACION` del alumno en un día calendario en hora de Lima. No incluye los `AJUSTE`. | RN-017, RN-019, RN-080 |
| 22 | **Consumo mensual** | `consumoMensual` | Suma de los movimientos `COMPRA` y `ANULACION` del alumno en un mes calendario en hora de Lima. No incluye los `AJUSTE`. | RN-018, RN-023, RN-080 |
| 23 | **Límite diario** | `LIMITE_DIARIO` | Consumo máximo de S/ 30.00 por alumno en un día calendario. | RN-017 |
| 24 | **Tope mensual** | `LIMITE_MENSUAL` | Consumo máximo de S/ 300.00 por alumno en un mes calendario. El apoderado puede configurar uno menor (tope mensual personalizado). | RN-018, RN-069, RN-070 |
| 25 | **Mes de consumo** | `mes` (año y mes) | Mes calendario en hora de Lima al que pertenece cada compra según su fecha de registro en el servidor. Reemplaza al antiguo «periodo»: no se abre ni se cierra. | RN-034, RN-063 |
| 26 | ~~**Cierre mensual**~~ | — | Retirado el 2026-11-13: el consolidado se calcula al consultarlo. | — |
| 27 | **Consolidado mensual** | `ConsolidadoMensual` | Resumen por familia de un mes que recibe el colegio: consumo neto de cada hijo y total de la familia, calculado al consultarlo. No incluye compras individuales ni cargos adicionales. | RN-031, RN-033, RN-063 |
| 28 | **Boleta del colegio** | `Boleta` | Cobro mensual que el colegio hace a la familia y en el que incluye el consolidado. Luquita no la emite y su deuda no bloquea compras. | RN-021 |
| 29 | **Secretaria del colegio** | `SecretariaColegio` | Usuaria del colegio que, desde el panel web, importa o registra las familias, cambia su estado, activa o desactiva la app del dueño y obtiene el consolidado mensual. | RN-022, RN-045, RN-047, RN-101, RN-104 |
| 30 | **Detalle de compras** | `DetalleCompras` | Lista de compras de un alumno en el mes (fecha, hora, monto y descripción referencial) que solo su apoderado puede solicitar desde la app móvil. | RN-037, RN-038 |
| 31 | **Año escolar** | `AnioEscolar` | Periodo lectivo del colegio; empieza en marzo. Las familias se asocian al año escolar del año calendario de su importación. | RN-088 |
| 32 | **Activación de la app del dueño** | `ventasHabilitadas` | Interruptor que la secretaria del colegio o el administrador de Luquita encienden o apagan. Solo con la app del dueño activa los alumnos pueden comprar. No tiene fechas fijas. | RN-097, RN-101, RN-102, RN-103 |
| 33 | **Convenio** | `Convenio` | Acuerdo entre el colegio y Luquita que define el cobro en la boleta, y el pago a la tienda. | — |
| 34 | **Familia registrada** | `FamiliaRegistrada` | Familia que el colegio registró en Luquita (importada por la secretaria o el administrador, o registrada a mano por la secretaria), identificada por el DNI del apoderado y con un estado. No implica ningún cobro de suscripción. | RN-006, RN-022, RN-045, RN-054 |
| 35 | **Importación de familias** | `ImportacionFamilias` | Carga en el panel web, por la secretaria o el administrador, de un archivo Excel con las columnas `dni_apoderado`, `nombre`, `apellidos` y `correo`. Toda familia importada queda `ACTIVO`. Informa las filas registradas y las rechazadas. | RN-045 a RN-047, RN-051 a RN-054, RN-098 |
| 36 | **Descripción referencial** | `descripcionReferencial` | Texto opcional de hasta 60 caracteres que el vendedor agrega a una compra (por ejemplo, "Jugo de naranja"). No es un catálogo de productos. | RN-048, RN-049, RN-050 |
| 37 | **Panel web del colegio** | `PanelColegio` | Aplicación web en Angular 20 que usa la secretaria del colegio. | RN-022, RN-045 |
| 38 | **Estado de la familia** | `EstadoFamilia` | `ACTIVO` (al importarla o registrarla) o `INACTIVO` (cuando la secretaria lo cambia). Una familia inactiva no puede comprar ni vincular tags. El código de error `FAMILIA_INACTIVA` conserva su nombre. | RN-006, RN-053, RN-056, RN-104 |
| 39 | ~~**Fin del año escolar**~~ | — | Retirado el 2026-11-13. | — |
| 40 | **Administrador de Luquita** | `AdministradorLuquita` | Usuario de Luquita que, desde el panel web, registra los colegios, sus secretarias y los dueños de tienda con su correo, importa el Excel de familias y activa o desactiva la app del dueño. | RN-045, RN-065, RN-066, RN-067, RN-101 |
| 41 | **Tope mensual personalizado** | `topeMensualPersonalizado` | Tope mensual de cada alumno: S/ 300.00 por defecto al registrarlo; solo su apoderado puede cambiarlo, entre S/ 0.00 y S/ 300.00, y aplica de inmediato. | RN-069, RN-070 |
| 42 | **Clave de acceso** | `claveAcceso` | Clave aleatoria que el sistema envía al correo de la secretaria o del dueño de la tienda al registrarlos. La secretaria la usa con su DNI y el dueño con su correo; deben cambiarla obligatoriamente en su primer inicio de sesión por una de al menos 12 caracteres con mayúsculas, minúsculas y números. El administrador también la usa con su correo. | RN-029, RN-068, RN-071 a RN-074, RN-105 |
| 43 | ~~**Temporada de ventas**~~ | — | Retirada el 2026-11-13: las compras dependen solo de que la app del dueño esté activa. | — |
| 44 | **Desbloqueo de tag** | `DesbloqueoTag` | Acción del apoderado que devuelve un tag bloqueado a estado activo. Solo él puede hacerla. | RN-028 |
| 45 | **App del dueño** | `AppDueno` | App móvil Flutter que usa el dueño de la tienda para cobrar con el tag del alumno y su PIN. En las pantallas se muestra como «app de cobro»; ambos nombres designan la misma aplicación. Solo funciona cuando la secretaria o el administrador la tienen activa. |
| 46 | **NDEF** | — | Formato estándar de mensajes que se graban en un tag NFC. Luquita graba en el tag solo el token opaco del tag. |
| 47 | **Token opaco** | `tokenTag` | Identificador aleatorio que no contiene ningún dato del alumno ni de la familia; es lo único que se graba en el tag (ver «Token del tag»). |
| 48 | **JWT** | — | Token firmado que el servidor entrega al iniciar sesión y que la app o el panel envían en cada solicitud para identificar al usuario y su rol. |
| 49 | **HMAC** | — | Código calculado con una llave secreta que permite buscar un DNI cifrado sin descifrarlo (índice ciego) y firmar la huella de una solicitud idempotente. |
| 50 | **Outbox (correo saliente)** | tabla `correo_saliente` | Cola en base de datos donde se guarda cada correo antes de enviarse, para reintentarlo si falla sin perder la compra. |

---

## Términos que NO forman parte del dominio

Estos términos no deben aparecer en el código ni en la documentación del producto, porque describen algo que Luquita no hace (PRINCIPIO-003):

| Término prohibido | Por qué | Usar en su lugar |
|---|---|---|
| Saldo | Luquita no guarda saldos. | Consumo mensual |
| Recarga | Luquita no recibe recargas. | — |
| Pago (como acción de Luquita) | Luquita no procesa pagos; la familia paga la boleta al colegio. | Compra (en la tienda) |
| Comprobante o boleta electrónica | Luquita no emite comprobantes SUNAT. | Consolidado mensual |
| Familia con deuda o bloqueo por deuda | La deuda de la boleta no bloquea compras; la gestiona el colegio fuera del sistema. | — |
| Padre (como nombre del actor) | El actor se llama siempre «apoderado». | Apoderado |
| Asiento | El registro se llama siempre «movimiento». | Movimiento |
