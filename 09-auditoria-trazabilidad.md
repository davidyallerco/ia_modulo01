# 09 — Auditoría de trazabilidad del Spec Pack: LUQUITA

- **Producto:** Luquita, billetera digital infantil pospago con tag NFC para la tienda del colegio.
- **Revisión:** QA Architect Senior (Agente QA de Backlog, BMAD).
- **Documentos auditados:** `03-reglas-de-negocio.md` (act. 2026-11-11), `06-user-stories-refinadas.md` (2026-11-11), `07-architecture.md` v0.3.2, `08-backlog.md` v0.2, `02-alcance.md` (2026-11-04) y `05-preguntas-abiertas.md` (seguimiento hasta constitución 1.15).
- **Fecha:** 2026-11-11. **Actualizaciones:** 2026-11-11 (decisiones sobre A-01, A-02 y M-01; retiro del Libro de Reclamaciones y del detalle de protección de datos) y 2026-11-13 (simplificación del piloto, constitución 1.17, y cierre de todos los hallazgos). Ver las secciones 0c, 0b y 0.

> **Límite de la revisión:** `00-constitucion.md`, `01-project-brief.md` y `04-glosario.md` no venían en el paquete. La consistencia con la constitución se verificó contra lo que citan de ella `02`, `06` y `07` (stack, principios y valores), no contra su texto.

---

## 0c. Cierre de hallazgos (2026-11-13)

Por pedido del responsable del proyecto, se tomaron decisiones para cerrar todos los hallazgos abiertos sin agregar historias de usuario. **Estado: 0 críticos, 0 mayores y 0 sugerencias abiertos.** Las decisiones, los ADR y las dependencias fueron **aprobados el 2026-11-13 por David Yallerco** (T-001, T-002 y T-003).

| Hallazgo | Decisión | Dónde |
|---|---|---|
| M-03 HU-012A/B y HU-018A/B inexistentes | No se dividen las HU: el backlog traza a HU-012 y HU-018 completas | `08` |
| M-04 Inicio de sesión del administrador sin regla | RN-071, RN-073 y RN-029 incluyen al administrador; criterio nuevo en HU-001 | `03`, `06`, T-035 |
| M-05 Cambio del PIN del alumno inexistente | Riesgo aceptado: no hay cambio de PIN en el piloto; RN-005 queda para «definir» | `03`, `06` HU-008, `07` §6.7 |
| M-06 Sin recuperación de acceso | Riesgo aceptado: procedimiento manual del equipo de Luquita con el script de arranque | `02`, `07` §4.3.2 y §6.7, HU-002 |
| M-07 Sin política de clave nueva | Nueva RN-105 (12 caracteres, mayúsculas, minúsculas y números, distinta de la recibida) | `03`, `06` HU-003, `07` §4.2.3, T-035 |
| M-11 Token de renovación del panel | Cookie `HttpOnly`, `Secure` y `SameSite=Strict` limitada a la renovación | `07` §4.2.4, T-038 |
| M-12 Distribución de apps e iOS | Solo Android; APK firmado por prueba interna de Google Play | `02`, `07` C1 y §6.7, T-079, T-080 |
| M-13 T-003 como cuello de botella | Los pendientes ya están decididos; T-003 queda como revisión y firma del PO (M) y es dependencia de T-052 y T-077 | `06`, T-003 |
| M-14 Pruebas de autorización | Nueva T-094 (`AutorizacionIT.java`); T-058 prueba la medianoche y T-069 la ausencia de compras individuales | `08` |
| S-01 RN-030 incompleta | RN-030 incluye claves, tokens y secretos | `03` |
| S-02 Bits del UUID v4 | RN-007 y el glosario indican 122 bits aleatorios | `03`, `04` |
| S-03 `TAG_YA_GRABADO` con dos significados | Nuevo `TAG_YA_ACTIVO` para la activación | `07` §4.1 y §4.4.4 |
| S-06 RN-097 sin HU-012 | Ya trazada a HU-012 | `06` |
| S-07 Procesos sin tarea | Alertas y prueba de restauración en T-016; limpieza de tokens en T-030 | `07` §6.7, `08` |
| S-08 Ejecutor de T-065 | Pasa a Humano | `08` |
| S-09 Alcance desactualizado | `02` menciona ACS, la distribución y las decisiones vigentes | `02` |
| S-10 Errores de formato | Corregidos (§6.1 BouncyCastle, orden de §7.3) | `07` |
| S-11 Ejemplo `GET /vendedor/tags/{token}` en la constitución | Aceptado: ADR-007 documenta la variante `POST /tags/resolucion`; se ajustará en la próxima versión de la constitución | `07` §4.5.1 |
| S-12 Varios tags `ACTIVO` por alumno | Riesgo aceptado: es el mecanismo de reemplazo del piloto | `02` |
| S-13 Correcciones de QA sin aplicar | Aplicadas (DNI del alumno en HU-008 y precondición de HU-015) | `06` |
| S-14 H-024 y H-027 | H-024 fuera de alcance (W17); H-027 resuelto con RN-104 | `02`, `05` |

Además: los 49 criterios `[POR CONFIRMAR]` de `06` pasaron a `[CONFIRMADO]`, los códigos ✱ de `07` §4.1 quedaron confirmados y los hallazgos H abiertos de `05` se cerraron (puntaje 5 de 5).

---

## 0b. Seguimiento de la simplificación del piloto (2026-11-13)

El equipo simplificó el piloto sin agregar historias de usuario: se retira la suscripción de S/ 5.00, no hay cierre mensual automático ni temporada de ventas (el consolidado se calcula por mes al consultarlo), los alumnos compran solo con la app del dueño activa (la activan la secretaria o el administrador), el Excel lo importan la secretaria o el administrador y toda familia queda `ACTIVO`, se retira el fin del año escolar y el PIN del alumno pasa a 3 dígitos.

| Hallazgo | Estado | Efecto |
|---|---|---|
| A-02 | ✅ Resuelto (con otra solución, de nuevo) | Sin periodos no existe "primer periodo": el consolidado se calcula por mes calendario y las compras dependen solo de la app del dueño activa. Reemplaza la temporada de marzo a noviembre. |
| M-02 Prioridad del fin del año escolar | ⚪ **Retirado** | HU-025, RN-055 y RN-057 retiradas; la baja de una familia la hace la secretaria cambiando su estado (RN-104, HU-005). |
| S-05 Cierre a las 23:59:59.000 | ⚪ **Retirado** | No hay cierre: cada compra cuenta en el mes de su `fecha_lima` (RN-063). |

**Cambios en los documentos:** constitución 1.17; reglas (se retiran RN-032, RN-055, RN-057, RN-062, RN-096, RN-099 y RN-100; se reescriben RN-004, RN-031, RN-034, RN-045, RN-051, RN-053, RN-054, RN-063, RN-097 y RN-101 a RN-103; nueva RN-104); HU (HU-018 y HU-033 simplificadas, HU-025 retirada: 19 HU y 54 puntos); arquitectura v0.5 (sin tabla `periodo`, sin C5, ADR-005 y ADR-010 reescritos); backlog v0.4 (84 tareas y 105 h; se retiran T-067, T-068, T-071 y T-072).

**Quedaban abiertos:** 0 críticos, 9 mayores y 12 sugerencias; se cerraron en la sección 0c.

---

## 0. Seguimiento de las decisiones del equipo (2026-11-11)

Los documentos `00` (v1.16), `01`, `02`, `03`, `04`, `05`, `06`, `07` (v0.4) y `08` (v0.3) se actualizaron con estas decisiones:

| Hallazgo | Estado | Qué se hizo |
|---|---|---|
| **A-01** DNI duplicado en el Excel | ✅ **Resuelto** | Se aplicó la acción propuesta: nueva RN-098 (se procesa la primera fila y se rechazan las siguientes con "DNI duplicado"); RN-054 limitada a familias que ya existían antes de la importación; criterios [CONFIRMADO] en HU-004; contrato §4.3.3 y criterio de T-040 actualizados. |
| **A-02** Primer periodo después de marzo | ✅ **Resuelto** (con otra solución) | El equipo decidió una temporada de ventas: los alumnos solo compran del 1 de marzo al 30 de noviembre, mientras la secretaria o el administrador tengan habilitadas las ventas. RN-096 y RN-097 reescritas y RN-099 a RN-103 nuevas; el primer periodo es el mes de la primera habilitación (nunca uno anterior); el cierre de noviembre deshabilita las ventas y no hay periodos de diciembre a febrero. `VENTAS_NO_HABILITADAS` reemplaza a `SIN_PERIODO_ABIERTO`. Nueva HU-033, ADR-010, §4.3.6 y tareas T-091 a T-093; T-067 ya no crea periodos pasados en la recuperación; T-084 incluye "primera habilitación en mayo". Los ejemplos de diciembre a febrero se cambiaron a meses de la temporada, y se fijó la convención de fixtures ("Colegio Prueba" con ventas habilitadas desde el 2026-03-02). |
| **M-01** Idempotencia de compras rechazadas | ✅ **Resuelto** | Se aplicó la acción propuesta: RN-015 ampliada a los rechazos; el servidor guarda en `operacion_idempotente` el resultado de cada intento de compra (también `401`, `422` y `409`) en la misma transacción, y el reintento lo devuelve sin volver a verificar el PIN; estado `RECHAZADA` en `GET /compras/operaciones/{clave}`; `hash_solicitud` con HMAC; criterios [CONFIRMADO] en HU-012; T-056, T-057, T-060, T-064 y T-082 actualizadas (T-082 prueba que un `401` perdido y reintentado deja el contador en 1). |
| M-08 `codigoCompra` | ⚪ **Retirado** | Sin Libro de Reclamaciones no se necesita `codigoCompra` (columna, correo y contrato eliminados). |
| M-09 Descripción libre con datos del menor | ⚪ **Retirado** | El detalle de protección de datos salió del alcance (H-038 retirado). |
| M-10 Texto del consentimiento | ⚪ **Retirado** | El consentimiento salió del alcance (RN-002 retirada). |
| M-14 Pruebas de autorización | 🟡 Sigue abierto | Ya no incluye los casos de reclamos (RN-092, RN-095); sí los de tag, alumno y habilitación de ventas de otro colegio. |
| M-02 Prioridad del fin del año escolar | 🟡 Sigue abierto | Sin periodo de diciembre, el riesgo pasa a ser que las familias no reimportadas sigan `ACTIVO` en la temporada siguiente. |
| S-04 Instante de la eliminación (RN-039) | ⚪ **Retirado** | RN-039 retirada. |
| S-15 DNI del alumno en el consolidado | ⚪ **Retirado** | Detalle de minimización fuera del alcance del piloto. |
| S-09 Alcance desactualizado | 🟡 Parcial | `02` ya no tiene los trámites legales y refleja la temporada y la importación de febrero; falta mencionar Azure Communication Services como proveedor de correo. |
| S-13 Correcciones de QA sin aplicar | 🟡 Parcial | Aplicado el DNI `00000010` del alumno en HU-008 c1; falta la precondición de HU-015 c1. |

**Quedan abiertos:** 0 críticos, 10 mayores (M-02 a M-07 y M-11 a M-14) y 13 sugerencias. Los tres hallazgos que bloqueaban la Fase 3 (A-01, A-02 y M-01) están resueltos. Se agrega una decisión nueva para el PO: si el mes de la primera habilitación y los meses con las ventas pausadas cobran la suscripción completa (Pendiente 17 de las HU).

> El resto de este documento es la auditoría original del 2026-11-11. La matriz de la sección 3 está actualizada con las decisiones.

---

## 1. Resumen ejecutivo

La trazabilidad está **casi completa y es aritméticamente correcta**: las 80 reglas MUST tienen HU, las 20 HU Must tienen componente y endpoint, los 8 componentes tienen tareas y ninguna tarea queda sin HU ni justificación de infraestructura. Los conteos del backlog (90 tareas, 115.5 h, distribución por componente, tamaño y ejecutor) y la ruta crítica de 20 h se recalcularon y cuadran.

Los problemas no están en la cobertura, sino en **reglas que se contradicen o no cubren un caso que la arquitectura ya implementa**, y en **decisiones del equipo que llegaron a la arquitectura y al backlog sin volver a las reglas ni a las HU**.

| Nivel | Resultado | Estado |
|---|---|---|
| Reglas MUST → HU | 80 / 80 | ✅ (RN-005 sin flujo implementable) |
| HU Must → componente / API | 20 / 20 (+ HU-025) | ✅ |
| Componente → tareas | 8 / 8 | ✅ |
| Tareas → HU | 66 con HU, 24 de infraestructura, 0 huérfanas | ⚠️ 4 identificadores de HU (HU-012A/B, HU-018A/B) no existen en `06` |
| Conteos y ruta crítica del backlog | Recalculados | ✅ |
| Valores numéricos (30, 300, 5, 4/6 dígitos, 3/5 fallos, 900 s, 1+3 reintentos, 60 caracteres, 3 días hábiles, 5 años) | Consistentes en los 5 documentos | ✅ (detalles en S-02 y S-04) |
| Stack frente a la constitución (vía `02` §5 y `07` §6) | Java 21 + Spring Boot 3, PostgreSQL, Flutter, Angular 20, Azure | ✅ con 7 dependencias pendientes de aprobación (T-002) |

| Severidad | Cantidad |
|---|---|
| 🔴 Crítico | 2 |
| 🟡 Mayor | 14 |
| 🟢 Sugerencia | 15 |
| **Total** | **31** |

**Qué resolver antes de empezar la Fase 3:** A-01 (DNI duplicado en el Excel), A-02 (primer periodo cuando el colegio se registra después de marzo) y M-01 (idempotencia de compras rechazadas). Los tres cambian código de la ruta crítica. *(Actualización: A-01 y A-02 resueltos; ver sección 0.)*

---

## 2. Hallazgos

### 🔴 Críticos

| ID | Ubicación | Problema | Acción concreta |
|---|---|---|---|
| A-01 ✅ Resuelto | `03` RN-047 (ejemplo) frente a RN-054; `06` HU-004 c6 y criterio `[POR CONFIRMAR]`; `07` §4.3.3; `08` T-040 | **Dos reglas MUST dan resultados opuestos para el mismo dato.** El ejemplo de RN-047 rechaza la fila 87 por "DNI duplicado", y el ejemplo de respuesta de `07` §4.3.3 también. Pero RN-054 dice que un DNI que ya existe **actualiza** la familia, y el texto de §4.3.3 lo repite. Si la fila 5 crea la familia `00000003`, la fila 87 con el mismo DNI se rechaza (RN-047) o la sobrescribe (RN-054). T-040 no puede implementarse sin elegir, y su aprobación (Pendiente 10) sigue abierta. | Decidir y escribir una regla nueva, por ejemplo: "Si dos filas del mismo archivo tienen el mismo `dni_apoderado`, el sistema DEBE registrar la primera y rechazar las siguientes con el motivo 'DNI duplicado'". Limitar RN-054 a DNI que ya existían **antes** de la importación. Agregar ese caso al criterio de T-040. |
| A-02 ✅ Resuelto | `03` RN-096 y RN-097; `07` ADR-005 y §4.3.3; `08` T-040, T-067 y T-084; ejemplos de RN-017, RN-031, RN-043, HU-018 c1, T-085 | **RN-096 no define el primer periodo cuando el colegio registra su primera familia después de marzo.** La regla siempre abre "marzo de ese año". Si la secretaria importa el Excel el 2026-11-15, el año escolar es 2026 y el primer periodo es **marzo de 2026, ya pasado**. Las compras de noviembre no tienen periodo (`409 SIN_PERIODO_ABIERTO`), y la recuperación de T-067 ("al arrancar, cierra los periodos vencidos y abre los siguientes") generaría los consolidados de marzo a octubre de 2026, con S/ 5.00 de suscripción por mes a cada familia `ACTIVO`: hasta S/ 40.00 por familia por meses en que no existía. Además, casi todos los ejemplos de reglas, HU y tareas usan compras, cierres y reclamos de **noviembre de 2026**, que son imposibles para un colegio cuyo primer año es 2027 (el caso de los ejemplos de RN-096). | Reescribir RN-096: el primer periodo es el **mayor entre marzo y el mes de la primera importación**, y nunca se crean periodos anteriores a la fecha de registro. Limitar la recuperación de T-067 a periodos que el sistema abrió y que vencieron mientras estaba detenido. Fijar una convención de fixtures, por ejemplo "Colegio Prueba: primer año escolar 2026, periodo de noviembre de 2026 abierto", y agregar a T-084 el escenario "primera importación en mayo". |

### 🟡 Mayores

| ID | Ubicación | Problema | Acción concreta |
|---|---|---|---|
| M-01 ✅ Resuelto | `03` RN-015; `07` §2.1 (paso g), §3.2 (`operacion_idempotente`) y §4.5.3; `08` T-056, T-060, T-082 | **Una compra rechazada no es idempotente, y el reintento suma otro PIN fallido.** RN-015 exige devolver el resultado original de **toda** clave ya procesada. La arquitectura solo guarda las compras exitosas (en `movimiento`); `operacion_idempotente` excluye las compras. Si el alumno digita mal el PIN, la respuesta `401` se pierde y el vendedor toca "Reintentar" (ADR-002, misma clave y mismo cuerpo), el servidor vuelve a verificar el mismo PIN y el contador sube a 2. Con dos reintentos, el tag queda bloqueado por un solo error del alumno. Además, `GET /compras/operaciones/{clave}` solo distingue `REGISTRADA` o `404`. | Guardar el resultado de **cada** solicitud de compra, incluidas las 4xx, por (`registrado_por`, `idempotency_key`), y devolverlo en los reintentos. Agregar el estado `RECHAZADA` con su `codigo` a §4.5.3. Agregar a T-082 el caso "401 perdido + reintento: el contador queda en 1". |
| M-02 | `03` RN-055 (SHOULD, M5) y RN-057 (MUST); `02` M5; `06` HU-025 (Should) y trazabilidad M5; `07` §4.3.4b; `08` T-071 y T-072 (Must) | **La prioridad del fin del año escolar no coincide entre documentos.** El alcance lo pone en M5 (Must), la regla y la HU lo dejan como Should y el backlog lo implementa como Must. Sin RN-055, ninguna familia pasa a `INACTIVO` en el MVP salvo por reimportación, así que RN-057 (MUST) no se cumple en diciembre. | Subir RN-055 a MUST. Pasar HU-025 al MVP en `06`, con estimación, y agregarla a la tabla "funcionalidades Must → HU" (hoy M5 suma 20 puntos sin ella). |
| M-03 | `06` HU-012 y HU-018; `07` §7.1 y §7.2; `08` convenciones y trazabilidad | **El backlog traza a HU que no existen.** 13 tareas apuntan a HU-012A, HU-012B, HU-018A o HU-018B. En `06` solo son una recomendación de QA: HU-012 sigue con 5 puntos y 16 criterios, y la arquitectura traza a HU-012 y HU-018 sin dividir. Un agente que valide "criterios de la HU → tests de la tarea" no encuentra la HU. | Formalizar la división en `06`, con criterios asignados y estimación por HU, y actualizar §7.1 y §7.2 de la arquitectura. |
| M-04 | `03` RN-029, RN-068, RN-071 y RN-073; `07` §4.2.2; `08` T-035 | **El inicio de sesión del administrador de Luquita no tiene regla.** El equipo decidió "correo y clave, con cambio obligatorio y bloqueo por fallos", pero solo quedó en la arquitectura y en T-035. RN-029 y RN-073 mencionan a la secretaria y al dueño, no al administrador, y ninguna HU tiene un criterio de su acceso (HU-001 empieza con "el administrador inició sesión"). | Agregar una regla de inicio de sesión del administrador, extender RN-029 y RN-073 al administrador y añadir a HU-001 un criterio de acceso y otro de bloqueo. |
| M-05 | `03` RN-005; `06` HU-008 c5 y HU-021; `07` §4.4; `08` "Fuera de este backlog" | **RN-005 prohíbe que otro usuario "cambie" el PIN del alumno, pero no existe la operación de cambiarlo.** No hay endpoint ni tarea; el criterio c5 de HU-008 ("el vendedor intenta cambiar su PIN → 403") no se puede probar. HU-021 queda fuera aunque QA la sugirió como Must por el riesgo R5 (alumno que olvida su PIN). | Incluir HU-021 en el MVP: un endpoint de cambio de PIN del alumno y una tarea con ejecutor Humano (código de PIN, PRINCIPIO-009). Si se descarta, reescribir RN-005 y HU-008 c5 solo para "definir", y registrar R5 como riesgo aceptado. |
| M-06 | `06` HU-002 (brecha), HU-022 y HU-023; `07` §4.3.2; `08` T-037 | **No hay forma de recuperar el acceso.** Si el correo `CLAVE_INICIAL` falla, el panel muestra `FALLIDO` y no ofrece nada más: la secretaria o el dueño quedan sin acceso. Un apoderado que olvida su PIN no puede desbloquear el tag de su hijo. La única salida es tocar la base de datos a mano. | Llevar al MVP al menos HU-023 (el administrador regenera la clave, que también sirve de reenvío) con su tarea Humana. Decidir si HU-022 entra al piloto o se acepta el riesgo. |
| M-07 | `06` HU-003 y HU-011 (brecha), Pendiente 11; `07` §4.2.3; `08` T-035 | **No hay política para la clave nueva.** T-035 solo rechaza repetir la clave recibida, así que la secretaria puede cambiar `k7Rm2pXq9Tb4` por `123456`, que es justo el contraejemplo de RN-074. | Escribir la regla, por ejemplo "al menos 12 caracteres con mayúsculas, minúsculas y números, distinta de la recibida", y agregar `123456` como caso negativo en T-035. |
| M-08 ⚪ Retirado | `03` RN-023; `06` HU-015 c1 y HU-019; `07` §4.4.7 y §4.5.2; `08` T-055 y T-073 | **`codigoCompra` no tiene regla y contradice el criterio de HU-015.** El equipo lo confirmó (Pendiente 9), y la arquitectura y T-055 lo incluyen en el correo, pero RN-023 no se actualizó y el texto esperado de HU-015 c1 no lo contiene: un test escrito desde la HU falla contra T-055. HU-019 tampoco tiene un criterio para un código inválido o de otra familia. | Actualizar RN-023 y el texto de HU-015 c1. Escribir una regla que diga que el código del reclamo debe ser de una compra de la familia, y agregar a HU-019 sus criterios positivo y negativo. |
| M-09 ⚪ Retirado | `05` H-038 (abierto); `03` RN-048 a RN-050; `07` §3.2 (`movimiento.descripcion`); `08` T-062 | **La descripción libre puede guardar datos del menor durante 5 años.** El hallazgo H-038 sigue abierto y ninguna tarea lo mitiga. La descripción aparece en reclamos y, en el futuro, en el detalle S2. | Decidir entre una lista corta de descripciones o texto libre con advertencia. Como mínimo, agregar a T-062 un aviso visible ("No escribas datos personales del alumno") con su criterio. |
| M-10 ⚪ Retirado | `05` H-039 (abierto); `03` RN-002; `07` §4.4.2 (`versionConsentimiento: "2026-11"`); `08` T-046 y T-049 | **Nadie redacta el texto del consentimiento.** La arquitectura guarda una versión, pero ninguna tarea produce el texto ni incluye el plazo de 5 años y su motivo (H-039). Sin texto no hay qué versionar. | Agregar una tarea Humana: "Redactar el consentimiento v2026-11 con finalidad y plazo de conservación". Hacer que T-049 lo muestre completo, con un criterio que verifique la versión guardada. |
| M-11 | `07` §4.2.4 y §6.4; `08` T-038 | **No se define dónde guarda el panel Angular el token de renovación**, que vale 12 horas. Para las apps se exige `flutter_secure_storage`; para el panel, T-038 solo dice que "renueva en silencio". Guardado en `localStorage`, un XSS lo expone. | Decidir el mecanismo, por ejemplo una cookie `HttpOnly`, `Secure` y `SameSite=Strict` limitada a `/autenticacion/renovacion`, o solo memoria. Documentarlo en §4.2.4 y agregarlo como criterio de T-038. |
| M-12 | `02` §6 ("Tiendas de aplicaciones"); `07` C1 (iOS 15+); `08` Fase 4, T-079 y T-080 | **Falta la tarea de distribuir las apps, y nada prueba iOS.** T-080 necesita las apps instaladas en celulares reales, pero ninguna tarea las firma ni las publica. C1 declara soporte para iOS 15+, pero T-079 solo prueba Android. | Agregar una tarea Humana en la Fase 4 (APK firmado o prueba interna en Google Play, más TestFlight si se mantiene iOS) como dependencia de T-079 y T-080. Agregar la prueba NFC en iOS o quitar iOS del alcance de C1. |
| M-13 | `08` T-003, "Tareas bloqueadas" y ruta crítica | **Una sola tarea de 2 horas bloquea 38 tareas.** T-003 debe resolver 13 códigos ✱, 12 pendientes y las propuestas de §7.3. La ruta crítica le asigna 2 horas, pero depende de la agenda del PO. Además, T-052 y T-077 declaran una aprobación previa (Pendientes 7 y 5) sin depender de T-003, así que podrían empezar antes de la decisión. | Dividir T-003 por tema (credenciales, Excel, tags, cobro, cierre, reclamos), cada parte con responsable y fecha. Declarar la parte correspondiente como dependencia de T-052 y T-077, y medir la ruta crítica en días calendario además de horas. |
| M-14 | `08` Fase 5; `03` RN-019, RN-033, RN-087, RN-092 y RN-095 | **Faltan pruebas de autorización y de bordes de reglas MUST.** No hay un test de integración que recorra la matriz rol × recurso × familia (tag, alumno, reclamo y `codigoCompra` de otra familia; secretaria o administrador que responden un reclamo; apoderado que cambia su correo). T-074 no verifica la ausencia de DNI y consumo (RN-095). T-058 no prueba la medianoche de Lima (RN-019). T-069 no verifica que no haya compras individuales (RN-033). | Agregar `AutorizacionIT.java` en la Fase 5 y ampliar los criterios de T-058 (23:59:59 frente a 00:00:00), T-069 (RN-033) y T-074 (RN-095). |

### 🟢 Sugerencias

| ID | Ubicación | Problema | Acción concreta |
|---|---|---|---|
| S-01 | `03` RN-030; `06` brecha 1 | RN-030 sigue prohibiendo solo PIN y DNI en los logs; el PRINCIPIO-008 también prohíbe claves, tokens y secretos. Hoy lo cubren el criterio transversal, T-026 y T-086. | Ampliar el texto de RN-030. |
| S-02 | `03` RN-007; `07` §3.4.3 | RN-007 habla de un token "de 128 bits" en UUID v4; un UUID v4 tiene 122 bits aleatorios, como dice la arquitectura. | Escribir "UUID v4 (122 bits aleatorios)". |
| S-03 | `07` §4.1 y §4.4.4 | `TAG_YA_GRABADO` tiene dos significados: "el chip ya tiene datos o lock bits" (catálogo) y "el tag ya está `ACTIVO`" (activación). | Crear `TAG_YA_ACTIVO` para el segundo caso. |
| S-04 ⚪ Retirado | `07` §4.3.4b; `03` RN-039; `06` HU-030 | La respuesta del fin de año muestra la eliminación prevista para el 2032-12-20; el ejemplo de RN-039 la da por hecha el 2032-12-21, y HU-030 solo prueba el 2032-12-19. Falta el instante exacto. | Fijar "a partir de las 00:00:00 del día siguiente al aniversario, hora de Lima". |
| S-05 | `03` RN-062 y RN-063; `07` ADR-005 | El cierre a las 23:59:59.000 deja fuera las compras del último segundo del mes, que según RN-063 pertenecen a ese mes. Ya está `[POR CONFIRMAR]` en HU-018. | Resolverlo en T-003. Alternativa: cerrar a las 00:00:00 del día 1 con el criterio `registrado_en < inicio del periodo siguiente`. |
| S-06 | `06` HU-012 y trazabilidad | RN-097 es de categoría Compras (M3), pero solo está trazada a HU-018. | Agregarla a las reglas relacionadas de HU-012 (o de HU-012A). |
| S-07 | `07` §3.2 y §1.3; `08` | Hay procesos sin tarea: la limpieza diaria de `token_renovacion`, las alertas de Application Insights (cierre fallido, correos `FALLIDO`, sobre todo `CLAVE_INICIAL`) y la verificación de respaldo y restauración de PostgreSQL. | Agregar tres tareas pequeñas o extender T-016 y T-030. |
| S-08 | `08` T-065 | T-065 reinicia el contador de PIN fallidos con ejecutor "Agente IA con revisión humana". El PRINCIPIO-009 reserva a humanos el código de PIN. | Confirmar si reiniciar el contador cuenta como código de PIN; si cuenta, cambiar el ejecutor a Humano. |
| S-09 | `02` (2026-11-04) | El alcance no recoge decisiones posteriores: mantiene los trámites legales (registro ante la ANPD) que se retiraron, el correo "por definir" (ahora ACS), "a inicio del año escolar" (el Excel se sube en febrero) y no menciona los periodos de enero y febrero. | Actualizar `02` con las decisiones del 2026-11-11. |
| S-10 | `07` §6.1, §7.2 y §7.3; `08` T-001 | Hay errores de formato: la tabla §7.2 se corta por una línea en blanco antes de HU-025; §7.3 salta el #7 y pone el #15 antes del #14; T-001 cita la arquitectura 0.3.1 (la vigente es la 0.3.2); §6.1 invierte la relación ("BouncyCastle, que necesita `Argon2PasswordEncoder`": es al revés). | Corregir. |
| S-11 | Constitución PRINCIPIO-007 (según `07` §4.5.1) | La constitución da como ejemplo `GET /vendedor/tags/{token}`; la arquitectura usa `POST /tags/resolucion` (ADR-007). Como la constitución prevalece, un agente podría seguir el ejemplo literal. | Actualizar el ejemplo de la constitución o registrar la excepción en ella. |
| S-12 | `07` §3.3; `06` HU-009 (riesgo) | Sin RN-010 (S3), el MVP permite varios tags `ACTIVO` por alumno. De hecho, es la única forma de reemplazar un tag perdido. | Declararlo en `02` y `06` como riesgo aceptado y mecanismo de reemplazo del MVP. |
| S-13 | `06` HU-008 c1 y HU-015 c1 | Las correcciones de QA no se aplicaron al texto original: el alumno sigue con el DNI `00000000` (el mismo del apoderado), lo que invalida T-086, y a HU-015 c1 le falta la precondición "S/ 85.00 consumidos". | Aplicar las correcciones en el texto de los criterios. |
| S-14 | `05` H-024 y H-027 | Dos hallazgos abiertos no tienen tratamiento posterior: el apoderado sin celular NFC y la baja de la suscripción a mitad de año (hoy solo se puede reimportando el Excel como `INACTIVO`). | Decidir o declararlos fuera de alcance en `02`. |
| S-15 ⚪ Retirado | `06` HU-018 c3; `07` §4.3.4 y §4.3.5 | El consolidado muestra el DNI del alumno a la secretaria, mientras que los demás endpoints del panel nunca devuelven un DNI completo. | Confirmar que el colegio necesita el DNI para la boleta (minimización, PRINCIPIO-007); si no, mostrar solo el nombre. |

---

## 3. Matriz de trazabilidad: reglas → HU → componentes → tareas

Componentes: C1 App Apoderados · C2 App Vendedor · C3 Panel Web · C4 Luquita API · C5 Cierre mensual · C6 Correo (outbox) · C7 PostgreSQL · C8 Key Vault. En la columna Tareas se listan las que implementan o prueban la regla; las transversales (T-021, T-022, T-024, T-031 y T-032) se omiten. Estado: ✅ completo · ⚠️ con hallazgo · ❌ bloqueado o sin flujo.

### 3.1 Reglas MUST (75, actualizado el 2026-11-13)

| Regla | HU | Componentes | Tareas | Estado |
|---|---|---|---|---|
| RN-001 | HU-008 | C1, C4, C7 | T-046, T-049 | ✅ |
| RN-003 | HU-006 | C1, C4 | T-044, T-048 | ✅ |
| RN-004 | HU-008 | C1, C4 | T-046, T-049 | ✅ (3 dígitos desde el 2026-11-13) |
| RN-005 | HU-008 | C4 | T-046, T-094 | ✅ (M-05 cerrado el 2026-11-13) |
| RN-006 | HU-009 | C1, C4 | T-050, T-053 | ✅ |
| RN-007 | HU-009 | C1, C4, C8 | T-027, T-050, T-053 | ✅ (S-02 cerrado el 2026-11-13) |
| RN-008 | HU-009 | C1, C4 | T-051, T-053, T-079 | ✅ |
| RN-009 | HU-010 | C1, C4 | T-050, T-052, T-079 | ✅ |
| RN-091 | HU-010 | C1 | T-052, T-079 | ✅ |
| RN-012 | HU-012 | C2, C4 | T-056, T-057, T-062 | ✅ |
| RN-013 | HU-012, HU-014 | C4 | T-057, T-083 | ✅ |
| RN-014 | HU-012 | C2, C4 | T-025, T-056, T-062 | ✅ |
| RN-082 | HU-012 | C2, C4 | T-025, T-056, T-062 | ✅ |
| RN-015 | HU-012 | C2, C4, C7 | T-056, T-060, T-064, T-082 | ✅ (M-01 resuelto) |
| RN-016 | HU-016 | C4 | T-056, T-065 | ✅ |
| RN-089 | HU-012 | C2 | T-008, T-063 | ✅ |
| RN-090 | HU-012 | C2 | T-008, T-063, T-064 | ✅ |
| RN-048 | HU-012 | C2, C4 | T-056, T-062 | ✅ |
| RN-049 | HU-012 | C2, C4 | T-056, T-062 | ✅ |
| RN-050 | HU-012 | C2, C4 | T-056, T-062 | ✅ |
| RN-017 | HU-013 | C4 | T-023, T-058, T-081 | ✅ |
| RN-018 | HU-013 | C4 | T-023, T-058 | ✅ |
| RN-019 | HU-013 | C4 | T-058, T-059 | ✅ (M-14 cerrado el 2026-11-13) |
| RN-020 | HU-013 | C4 | T-058, T-081 | ✅ |
| RN-083 | HU-013 | C4, C7 | T-058, T-081 | ✅ |
| RN-078 | HU-008, HU-013 | C4, C7 | T-018, T-046, T-058 | ✅ |
| RN-021 | HU-012 | C4 | T-056 | ✅ |
| RN-022 | HU-005 | C3, C4 | T-041, T-043 | ✅ |
| RN-045 | HU-004 | C3, C4 | T-040, T-042 | ✅ |
| RN-046 | HU-004 | C3, C4 | T-040, T-042 | ✅ |
| RN-047 | HU-004 | C3, C4 | T-040, T-042 | ✅ (A-01 resuelto) |
| RN-051 | HU-004 | C4 | T-040 | ✅ |
| RN-052 | HU-004, HU-005 | C3, C4 | T-040, T-041, T-043 | ✅ |
| RN-053 | HU-004 | C4 | T-040 | ✅ |
| RN-054 | HU-004, HU-005 | C4 | T-040 | ✅ (A-01 resuelto) |
| RN-098 | HU-004 | C3, C4 | T-040, T-042 | ✅ (nueva) |
| RN-056 | HU-012 | C4 | T-056 | ✅ |
| RN-058 | HU-006 | C1, C4 | T-044, T-048 | ✅ |
| RN-059 | HU-006 | C1, C4 | T-044, T-048 | ✅ |
| RN-060 | HU-007 | C1, C4, C8 | T-045, T-048 | ✅ |
| RN-061 | HU-015, HU-005 | C4, C6 | T-041, T-055 | ✅ |
| RN-065 | HU-001 | C3, C4 | T-036, T-039 | ✅ |
| RN-066 | HU-002 | C3, C4 | T-037, T-039 | ✅ |
| RN-067 | HU-002 | C3, C4 | T-037, T-039 | ✅ |
| RN-075 | HU-002 | C3, C4, C8 | T-027, T-037, T-039 | ✅ |
| RN-068 | HU-003 | C3, C4 | T-035, T-038 | ✅ |
| RN-071 | HU-011 | C2, C4 | T-035, T-061 | ✅ |
| RN-072 | HU-002 | C4, C6 | T-033, T-034, T-037 | ✅ (M-06 cerrado el 2026-11-13) |
| RN-073 | HU-003, HU-011 | C2, C3, C4 | T-029, T-035, T-038, T-061 | ✅ (M-04, M-07 cerrado el 2026-11-13) |
| RN-074 | HU-002 | C4 | T-037 | ✅ |
| RN-105 | HU-001, HU-003, HU-011 | C2, C3, C4 | T-035, T-038, T-061 | ✅ (nueva, 2026-11-13) |
| RN-086 | HU-006 | C4, C7 | T-017, T-044 | ✅ |
| RN-087 | HU-005 | C3, C4 | T-041, T-043 | ✅ (M-14 cerrado el 2026-11-13) |
| RN-088 | HU-004, HU-005 | C4 | T-040, T-041 | ✅ |
| RN-023 | HU-015 | C4, C6 | T-034, T-055, T-056 | ✅ |
| RN-024 | HU-015 | C6 | T-034 | ✅ |
| RN-084 | HU-015 | C6, C7 | T-034 | ✅ |
| RN-025 | HU-015 | C6 | T-033, T-034 | ✅ |
| RN-026 | HU-016 | C1, C4 | T-065, T-066 | ✅ |
| RN-085 | HU-016 | C4 | T-054, T-056, T-065 | ✅ |
| RN-027 | HU-014, HU-017 | C4 | T-057, T-082, T-083 | ✅ (M-01 resuelto) |
| RN-028 | HU-017 | C1, C4 | T-065, T-066, T-083 | ✅ |
| RN-029 | HU-003, HU-007, HU-011 | C1, C2, C3, C4 | T-035, T-045 | ✅ (M-04 cerrado el 2026-11-13) |
| RN-030 | HU-007, HU-014 | C4 | T-026, T-086 | ✅ (S-01 cerrado el 2026-11-13) |
| RN-063 | HU-018 | C4 | T-059, T-069, T-084 | ✅ (reescrita el 2026-11-13) |
| RN-097 | HU-012, HU-033 | C2, C4 | T-059, T-084, T-093 | ✅ (S-06 cerrado el 2026-11-13) |
| RN-101 | HU-033 | C3, C4 | T-091, T-092 | ✅ |
| RN-102 | HU-033 | C4 | T-059, T-091, T-084 | ✅ |
| RN-104 | HU-005 | C3, C4 | T-041, T-043 | ✅ (nueva, 2026-11-13) |
| RN-031 | HU-018 | C3, C4 | T-069, T-070, T-084 | ✅ (reescrita el 2026-11-13) |
| RN-033 | HU-018 | C4 | T-069 | ✅ (M-14 cerrado el 2026-11-13) |
| RN-034 | HU-018 | C4, C7 | T-059, T-084, T-087 | ✅ (reescrita el 2026-11-13) |
| RN-040 | HU-012 | C2, C4 | T-054, T-062 | ✅ |
| RN-041 | HU-012 | C2 | T-062, T-089 | ✅ |
| RN-103 | HU-033 | C2 | T-093, T-089 | ✅ |

**Resumen (actualizado el 2026-11-13, tras el cierre):** 75 ✅ · 0 ⚠️ · 0 ❌ sobre 75 reglas MUST. Retiradas el 2026-11-13: RN-032, RN-057, RN-062, RN-096, RN-099 y RN-100; nueva: RN-104. Retiradas: RN-002, RN-043, RN-044 y RN-092 a RN-095. Nuevas: RN-098 a RN-103. *(Original: 57 ✅ · 19 ⚠️ · 4 ❌ sobre 80.)*

### 3.2 Reglas SHOULD y COULD (15)

| Reglas | HU | Componentes | Tareas | Estado |
|---|---|---|---|---|
| RN-035, RN-036, RN-064, RN-080, RN-081 (S1) | HU-026, HU-027 (sugeridas) | — | — (T-058 ya excluye `AJUSTE` de los límites) | Fuera del MVP |
| RN-037, RN-038 (S2) | HU-028 (sugerida) | — | — | Fuera del MVP |
| RN-010, RN-011 (S3) | HU-029 (sugerida) | — | — | Fuera del MVP (S-12) |
| RN-042 (S5) | HU-031 (sugerida) | — | — | Fuera del MVP |
| RN-069, RN-070, RN-076, RN-077, RN-079 (C3) | HU-032 (sugerida) | — | — (el check 0.00–300.00 ya está en T-018) | Fuera del MVP |

### 3.3 Trazabilidad backward: decisiones implementadas sin regla

Ninguna tarea es huérfana, pero estas tareas implementan decisiones que viven solo en la arquitectura o en el backlog:

| Tarea | Qué implementa | Falta | Hallazgo |
|---|---|---|---|
| T-035 | Inicio de sesión, bloqueo y cambio de clave del administrador | Regla y criterio de HU | M-04 |
| T-031 | Idempotencia en todo `POST` de negocio | Confirmar el alcance de "operación" (brecha 4 de `06`) | T-003 |
| 13 tareas | HU-012A, HU-012B, HU-018A y HU-018B | Definir esas HU en `06` | M-03 |
