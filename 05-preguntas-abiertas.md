# 05 — Auditoría del Spec Pack: LUQUITA

- **Producto:** Luquita, billetera digital infantil con tag NFC para la tienda del colegio.
- **Revisión:** Agente QA (BMAD), en rol de abogado del diablo.
- **Documentos auditados:** `00-constitucion.md` (auditoría inicial sobre la versión 1.3; seguimiento hasta la versión 1.15), `01-project-brief.md`, `02-alcance.md`, `03-reglas-de-negocio.md` y `04-glosario.md`.
- **Fecha:** 2026-11-04.

> Este reporte señala defectos; no decide reglas de negocio. Cada sugerencia indica qué debe decidir el equipo.

## Resumen

| Severidad | Cantidad | Qué significa |
|---|---|---|
| CRÍTICA | 3 | Bloquea la implementación del MVP: un agente no puede construirlo sin inventar reglas. |
| ALTA | 9 | Un agente implementaría algo incorrecto o incompleto en un flujo principal. |
| MEDIA | 15 | Casos borde o flujos alternativos sin cubrir, o ambigüedades acotadas. |
| BAJA | 8 | Formato, redacción o glosario. |
| **Total** | **35** | |

---

## CRÍTICA

| ID | Ubicación | Problema | Sugerencia |
|---|---|---|---|
| H-001 | `03-reglas-de-negocio.md` RN-006; `02-alcance.md` M1 y M2 | **Nadie registra la suscripción.** RN-006 exige que la familia tenga una "suscripción registrada" para vincular un tag, pero ningún documento dice quién la registra, cuándo ni cómo. Sin eso, ningún tag puede vincularse y el MVP completo queda bloqueado. Es la pregunta abierta 2. | Decidir si la suscripción la registra el colegio (por ejemplo, con la lista de familias inscritas) o el apoderado al crear su cuenta. Escribir una regla para el alta de la suscripción y otra para su fecha de inicio. |
| H-002 | `02-alcance.md` M5 y "Decisión pendiente"; `03-reglas-de-negocio.md` RN-021, RN-022 y RN-031 | **El colegio no tiene canal en el sistema.** El colegio debe obtener el consolidado, marcar familias con deuda y registrar sus pagos, pero no está definido si usa un panel web, si recibe un archivo o cómo se autentica. Tres reglas MUST dependen de esto. | Decidir el canal (por ejemplo, panel Angular 20 con aprobación, según el PRINCIPIO-002) y escribir reglas de acceso del usuario del colegio: cómo inicia sesión y qué puede ver. |
| H-003 | `00-constitucion.md` PRINCIPIO-004; `03-reglas-de-negocio.md` RN-031 y RN-034 | **No está definido el momento exacto del cierre mensual.** RN-031 genera el consolidado "cuando termine un mes calendario", pero la constitución habla de un periodo "cerrado y entregado al colegio". No se sabe si el periodo se cierra a las 00:00:00 del día 1, cuando se genera el consolidado o cuando el colegio lo recibe. Esto decide qué compras y anulaciones entran en lo que el colegio cobra. | Definir el instante de cierre en hora de Lima (por ejemplo, `AAAA-MM-01T00:00:00-05:00`) y quién lo dispara (automático o manual). Separar en reglas distintas el cierre del periodo y la entrega al colegio. |

---

## ALTA

| ID | Ubicación | Problema | Sugerencia |
|---|---|---|---|
| H-004 | `00-constitucion.md` PRINCIPIO-005 frente a `03-reglas-de-negocio.md` RN-021 | **Contradicción sobre cuándo empieza el bloqueo por deuda.** La constitución dice que la familia no puede consumir "en el mes siguiente" a la boleta impaga; RN-021 bloquea desde el momento en que el colegio la marca. Si el colegio marca la deuda a mitad de mes, no está claro si el bloqueo es inmediato o desde el día 1 del mes siguiente. | Decidir el inicio del bloqueo y alinear la constitución y RN-021 con la misma redacción. |
| H-005 | `03-reglas-de-negocio.md` RN-032; pregunta abierta 3 | **Suscripción en meses sin consumo.** No se sabe si los S/ 5.00 se cobran en meses sin compras ni en vacaciones (enero y febrero). Afecta el consolidado y los ingresos proyectados del brief. | Definir en qué meses se cobra la suscripción y escribir una regla para el mes sin consumo. |
| H-006 | `00-constitucion.md` PRINCIPIO-004; `03-reglas-de-negocio.md` RN-036 | **El `AJUSTE` no tiene dueño ni efecto definido.** No se dice quién puede registrarlo (vendedor, colegio o Luquita), si requiere aprobación, si tiene un monto máximo, ni si cuenta para los límites diario y mensual del periodo abierto. Un ajuste de −S/ 4.00 en diciembre podría liberar cupo del tope mensual. | Decidir quién registra ajustes, con qué aprobación y si suman o no al consumo usado para los límites. |
| H-007 | `02-alcance.md` M3 y S1 | **El MVP no permite corregir compras.** La anulación (S1) y el ajuste quedan fuera del MVP, pero el riesgo R2 (disputas) es de prioridad alta. En el MVP, una compra mal digitada (S/ 35.00 en vez de S/ 3.50) se cobraría en la boleta sin forma de corregirla. | Reevaluar si la anulación debe ser MUST o definir un procedimiento manual documentado para el MVP. |
| H-008 | `01-project-brief.md` R2; `03-reglas-de-negocio.md` | **No hay flujo para disputas.** El riesgo R2 ("mi hijo no compró eso") solo se mitiga con el PIN. No hay regla sobre cómo el apoderado disputa una compra, quién la resuelve, en qué plazo y qué pasa con la compra mientras se resuelve. El Libro de Reclamaciones es un mecanismo legal, no el flujo operativo de la disputa. | Decidir si las disputas pasan por el Libro de Reclamaciones o por un flujo propio, y escribir las reglas correspondientes. |
| H-009 | `03-reglas-de-negocio.md` RN-023 y M1 | **No se exige el correo del apoderado.** Todas las notificaciones dependen del correo del apoderado (M4), pero ninguna regla exige registrarlo, validar su formato ni verificar que le pertenezca. | Agregar reglas de registro del correo, de su verificación y de lo que ocurre si cambia. |
| H-010 | `00-constitucion.md` PRINCIPIO-007; `03-reglas-de-negocio.md` RN-039 | **La eliminación de datos choca con la deuda y con los reclamos.** Si una familia termina el año con deuda (mitigación de R1: "debe pagar para pasar de año"), sus datos se eliminan 30 días después del cierre, y se pierde el registro de la deuda. Además, la normativa del Libro de Reclamaciones exige conservar los reclamos por un plazo mínimo (por verificar), que podría superar los 30 días. | Decidir qué se conserva más allá de los 30 días y con qué base legal (deudas abiertas, reclamos) y reflejarlo como excepción en la constitución. |
| H-011 | `00-constitucion.md` PRINCIPIO-008; `03-reglas-de-negocio.md` RN-003 y RN-029 | **No se sabe con qué se identifica el usuario.** El acceso es con PIN de 6 dígitos, pero no se define el identificador (correo, celular o DNI del apoderado). Un PIN sin identificador no permite autenticar. | Definir el identificador de inicio de sesión del apoderado, del vendedor y del colegio. |
| H-012 | `02-alcance.md` M3; `03-reglas-de-negocio.md` | **No se define cómo se crea la cuenta del vendedor.** No hay regla sobre quién la crea, cuántos vendedores puede haber por tienda ni cómo se asocia a la tienda. | Decidir el alta del vendedor (por Luquita o por el colegio) y escribir su regla. |

---

## MEDIA

| ID | Ubicación | Problema | Sugerencia |
|---|---|---|---|
| H-013 | `04-glosario.md` Consumo diario; `03-reglas-de-negocio.md` RN-017 y RN-035 | **Las anulaciones afectan el límite de forma ambigua.** El consumo diario es "suma neta", así que una anulación libera cupo. Si se anula el día 10 una compra del día 4, no está claro a qué día se resta. | Decidir si la anulación cuenta en el día de la compra original o en el día en que se anula. |
| H-014 | `00-constitucion.md` PRINCIPIO-008; `03-reglas-de-negocio.md` RN-027 | **El conteo de PIN fallidos del alumno no está definido.** La constitución dice "3 intentos fallidos"; RN-027 agrega "consecutivos". No se dice cuándo se reinicia el contador (al acertar, a la medianoche, al desbloquear). | Alinear la constitución y la regla, y escribir una regla para el reinicio del contador. |
| H-015 | `03-reglas-de-negocio.md` RN-029 | **Mismo problema para el bloqueo de cuentas.** "5 intentos" no indica si son consecutivos, en qué ventana de tiempo, ni si el contador se reinicia tras los 900 segundos. | Definir la ventana y el reinicio del contador. |
| H-016 | `04-glosario.md` Familia; `03-reglas-de-negocio.md` RN-023 | **Una familia tiene un solo apoderado.** El glosario dice "un apoderado", pero padre y madre podrían querer ambos recibir correos o bloquear el tag. No se dice si es posible ni qué ve cada uno. | Decidir si una familia puede tener más de un apoderado y con qué permisos. |
| H-017 | `02-alcance.md` M4; `03-reglas-de-negocio.md` RN-028 | **El desbloqueo de un tag bloqueado por el apoderado no tiene regla.** RN-028 solo cubre el bloqueo por PIN fallidos. Bloquear y desbloquear son dos casos con reglas distintas. | Escribir una regla para el desbloqueo de un tag que el apoderado bloqueó. |
| H-018 | `03-reglas-de-negocio.md` RN-043 y RN-044 | **Nadie responde los reclamos.** No se dice quién responde (Luquita, el colegio o la tienda) ni cómo llega la respuesta al apoderado. El plazo de 15 días hábiles está marcado "por verificar". | Definir el responsable de responder y verificar el plazo con la norma vigente. |
| H-019 | `03-reglas-de-negocio.md` RN-039 | **No se define la continuidad entre años escolares.** Si los datos se eliminan al cerrar el año, no está claro qué pasa con las familias que siguen el año siguiente: si se vuelven a registrar y vincular los tags, o si los tags siguen siendo válidos. Tampoco quién registra la fecha de cierre (pregunta abierta 5). | Definir el flujo de reinicio anual y quién registra la fecha de cierre del año escolar. |
| H-020 | `02-alcance.md` W9; `03-reglas-de-negocio.md` | **No hay regla para la falta de conexión.** Las compras sin conexión están fuera de alcance, pero no hay regla sobre qué hace la app del vendedor cuando no hay internet: qué mensaje muestra y si guarda la compra o no. | Escribir una regla tipo Unwanted para la compra sin conexión. |
| H-021 | `03-reglas-de-negocio.md` RN-015 | **No se define quién genera la clave de idempotencia ni cuándo.** RN-015 asume que la app la reutiliza en los reintentos, pero no dice que la app la genera una vez por intento de compra. Si genera una nueva en cada reintento, la regla no protege. | Agregar una regla sobre la generación de la clave en la app del vendedor y su vigencia. |
| H-022 | `02-alcance.md` M2; `03-reglas-de-negocio.md` RN-008 | **No se sabe en qué estado llegan los tags del colegio.** Si el colegio los entrega ya grabados o bloqueados, el apoderado no podrá grabarlos como exige RN-008. | Decidir si los tags se entregan en blanco o ya grabados, y ajustar M2 y RN-008. |
| H-023 | `03-reglas-de-negocio.md` RN-010 y RN-011 | **No se dice si un tag desvinculado puede volver a usarse.** Como queda bloqueado contra escritura, no puede regrabarse. No se dice si puede volver a vincularse con el mismo token si aparece. | Decidir si un tag desvinculado es descartable o puede revincularse. |
| H-024 | `02-alcance.md` supuestos; `03-reglas-de-negocio.md` | **No hay alternativa para el apoderado sin celular NFC.** Si el apoderado no tiene un celular capaz de grabar tags, no puede vincularlos. | Decidir el flujo alternativo (por ejemplo, la vinculación asistida por el colegio) o dejarlo explícitamente fuera de alcance. |
| H-025 | `04-glosario.md`; `00-constitucion.md`; `02-alcance.md` | **"Padre" y "apoderado" se usan para lo mismo.** La constitución, el brief y el alcance dicen "padre"; las reglas y el glosario dicen "apoderado". Un agente podría crear dos entidades distintas. | Elegir un término, actualizarlo en todos los documentos y dejar el otro como sinónimo en el glosario. |
| H-026 | `04-glosario.md` Asiento y Ledger; `00-constitucion.md` PRINCIPIO-004 | **Tres nombres para el mismo concepto.** La constitución usa las tablas `compra` y `movimiento`; el glosario define `Asiento` y llama al ledger `Movimiento`. No queda claro si `Compra` es una entidad propia o un tipo de `Asiento`. | Fijar un único modelo de nombres (por ejemplo, `Movimiento` con tipo `COMPRA`, `ANULACION` o `AJUSTE`) y corregir los ejemplos de la constitución. |
| H-027 | `02-alcance.md`; `03-reglas-de-negocio.md` | **No se define la baja de la suscripción.** No se dice qué pasa si una familia cancela la suscripción, cuándo deja de cobrarse ni qué pasa con sus tags y su consumo pendiente. | Decidir el flujo de baja y escribir sus reglas. |

---

## BAJA

| ID | Ubicación | Problema | Sugerencia |
|---|---|---|---|
| H-028 | `03-reglas-de-negocio.md` RN-014 | **Regla compuesta:** "mayor que S/ 0.00 **y** como máximo dos decimales". | Dividir en dos reglas. |
| H-029 | `03-reglas-de-negocio.md` RN-020, RN-024 y RN-026 | **Reglas compuestas.** RN-020: validar en el servidor y en la misma transacción. RN-024: reintentar y registrar el resultado. RN-026: rechazar y no usar caché. | Dividir cada una en reglas atómicas. |
| H-030 | `03-reglas-de-negocio.md` RN-042 | **Patrón EARS mal usado.** "Optional" se usa para funcionalidades opcionales ("Donde se incluya la funcionalidad X"), no para estados. "Donde el periodo esté cerrado" es State-driven. | Cambiar a "Mientras el periodo esté cerrado…". |
| H-031 | `03-reglas-de-negocio.md` RN-028 | **Mezcla de palabras normativas.** "Solo el apoderado PUEDE" expresa una prohibición para los demás actores, no una opción. | Reescribir como "el sistema NO DEBE permitir que un usuario distinto del apoderado desbloquee el tag". |
| H-032 | `04-glosario.md` | **Términos usados en las reglas pero no definidos:** familia registrada o suscripción registrada (RN-006), consentimiento (RN-002), Libro de Reclamaciones (RN-043) y desbloqueo (RN-028). | Agregar estos términos al glosario. |
| H-033 | `04-glosario.md` | **Glosario mezclado con códigos.** `Convenio` se define pero no se usa en ninguna regla. `LIMITE_DIARIO`, `LIMITE_MENSUAL` y `FAMILIA_CON_DEUDA` mezclan conceptos con códigos de error; los códigos `LIMITE_DIARIO_EXCEDIDO` y `LIMITE_MENSUAL_EXCEDIDO` no están en el glosario. | Separar en una tabla de códigos de error y usar nombres de concepto en el glosario. |
| H-034 | `01-project-brief.md` secciones 4 y 5 | **Se confunden alumnos y familias.** El KPI 2 habla de 200 alumnos activos, pero la proyección de ingresos supone 200 familias. Como la suscripción es por familia, el ingreso real puede ser menor. | Separar la meta de alumnos de la de familias y recalcular la proyección. |
| H-035 | `01-project-brief.md` sección 2; `03-reglas-de-negocio.md` | **El rango de edad no se puede verificar.** El segmento es de alumnos de 8 a 14 años, pero el sistema solo guarda DNI y nombre (PRINCIPIO-007), así que no puede validar la edad. | Dejar explícito que la edad no la valida el sistema, o decidir cómo se controla fuera de él. |

---

## Cobertura del alcance por las reglas

| Funcionalidad | Reglas | Estado |
|---|---|---|
| M1 Registro de familia y alumnos | RN-001 a RN-005, RN-029 | Parcial: falta el correo y el identificador de inicio de sesión (H-009, H-011). |
| M2 Vinculación del tag | RN-006 a RN-009 | Bloqueada por H-001. |
| M3 Cobro con tag + PIN | RN-012 a RN-021, RN-027, RN-030, RN-040, RN-041 | Parcial: falta el alta del vendedor y la falta de conexión (H-012, H-020). |
| M4 Control parental | RN-023 a RN-026, RN-028 | Parcial: falta el desbloqueo manual (H-017). |
| M5 Cierre mensual | RN-022, RN-031 a RN-034 | Bloqueada por H-002 y H-003. |
| M6 Libro de Reclamaciones | RN-043, RN-044 | Parcial: falta el responsable de responder (H-018). |
| S1 Anulación | RN-035, RN-036 | Parcial (H-006). |
| S2 Detalle de compras | RN-037, RN-038 | Cubierta. |
| S3 Reemplazo de tag | RN-010, RN-011 | Parcial (H-023). |
| S4 Eliminación anual | RN-039 | Parcial (H-010, H-019). |
| S5 Reporte para el dueño | RN-042 | Cubierta, con formato a corregir (H-030). |

---

## Puntaje de madurez: 3 de 5

**Justificación:** la constitución es concreta y verificable, el alcance está bien acotado y las reglas cubren casi todas las funcionalidades con ejemplos y contraejemplos reales. Pero tres decisiones del negocio (quién registra la suscripción, cómo opera el colegio y cuándo se cierra el mes) bloquean el MVP. Además, hay contradicciones entre la constitución y las reglas en la morosidad y en la eliminación de datos. Un agente hoy tendría que inventar reglas para construir M2 y M5.

**Para subir a 4 de 5**, hay que resolver: H-001, H-002, H-003, H-004, H-005, H-006, H-007, H-009, H-010 y H-011.

**Para llegar a 5 de 5**, además hay que resolver todos los hallazgos de severidad MEDIA y corregir los de formato (H-028 a H-031).

---

## Seguimiento de decisiones (2026-11-04, constitución versión 1.4)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-001 | **Resuelto** | La secretaria del colegio registra a inicio de año las familias registradas en el panel web, importando un Excel o manualmente (RN-022, RN-045 a RN-047). |
| H-002 | **Parcial** | El colegio usa un panel web en Angular 20. Ya no necesita marcar familias con deuda. Sigue pendiente cómo inicia sesión la secretaria (ver H-011). |
| H-004 | **Resuelto** | La deuda de la boleta no bloquea compras: si el alumno asiste al colegio, puede comprar (RN-021, W12). |
| H-010 | **Resuelto** | Los datos se conservan 5 años desde el cierre del año escolar, lo que cubre las deudas pendientes (RN-039). |
| H-019 | **Parcial** | La continuidad anual depende del registro de familias a inicio de cada año; falta definir qué pasa con los tags y cuentas del año anterior. |

### Nuevos hallazgos

| ID | Severidad | Ubicación | Problema | Sugerencia |
|---|---|---|---|---|
| H-036 | CRÍTICA | `03-reglas-de-negocio.md` RN-006, RN-022 y RN-045 | **No se sabe cómo se reconoce a una familia registrada en la app.** El colegio registra familias en el panel y el apoderado crea su cuenta en la app, pero no hay un dato común que permita saber que esa cuenta pertenece a una familia registrada. | Definir el dato de enlace (por ejemplo, DNI o correo del apoderado) y escribir la regla de vinculación de la cuenta. |
| H-037 | ALTA | `03-reglas-de-negocio.md` RN-045 a RN-047 | **El formato del Excel no está definido.** No se conocen las columnas ni cuáles son obligatorias, así que RN-046 no es verificable. | Definir una plantilla de Excel con sus columnas y reglas de validación. |
| H-038 | MEDIA | `00-constitucion.md` PRINCIPIO-007; `03-reglas-de-negocio.md` RN-048 | **La descripción referencial es texto libre.** El vendedor podría escribir datos personales o información sensible del alumno, que luego se conservarían 5 años. | Decidir si la descripción se elige de una lista corta o se mantiene libre con una advertencia, y escribir la regla. |
| H-039 | MEDIA | `00-constitucion.md` PRINCIPIO-007 | **El plazo de 5 años necesita sustento ante la Ley 29733.** Conservar datos de menores por 5 años debe estar justificado por la finalidad (respaldar deudas) e informado en el consentimiento. | Incluir el plazo y su motivo en el texto del consentimiento (RN-002). |

**Puntaje actualizado: 3 de 5.** Se resolvieron tres hallazgos críticos o altos, pero aparece un nuevo crítico (H-036). Para subir a 4 de 5, falta resolver: H-003, H-005, H-006, H-007, H-009, H-011, H-036 y H-037.

---

## Seguimiento de decisiones (2026-11-04, constitución versión 1.5)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-036 | **Resuelto** | La familia se identifica por el DNI del apoderado; la cuenta de la app se asocia a la familia con ese DNI (RN-058, RN-059). |
| H-037 | **Resuelto** | El Excel trae `dni_apoderado`, `nombre`, `apellidos`, `correo` y `estado`, con `ACTIVO` por defecto (RN-051, RN-053). |
| H-005 | **Parcial** | Al terminar el año todas las familias pasan a inactivas; se propone no cobrar la suscripción a una familia inactiva todo el mes (RN-057, por confirmar). Falta definir el mes parcial. |
| H-009 | **Parcial** | El correo del apoderado viene del Excel del colegio y se propone usarlo para las notificaciones (RN-061, por confirmar). Falta definir si el apoderado puede cambiarlo. |
| H-011 | **Propuesto** | Inicio de sesión del apoderado con DNI y PIN (RN-060, por confirmar). Falta el inicio de sesión de la secretaria y del vendedor. |
| H-019 | **Resuelto** | La continuidad anual se da por el estado: al terminar el año las familias pasan a inactivas y, al reimportarlas, vuelven a activas conservando hijos y tags (RN-054, RN-055). |

**Puntaje actualizado: 3 de 5.** Ya no hay hallazgos críticos de identificación ni de registro. Para subir a 4 de 5 falta resolver H-003 (momento del cierre mensual), H-006 (ajustes), H-007 (corrección de compras en el MVP) y confirmar las reglas marcadas como propuesta en `03-reglas-de-negocio.md`.

---

## Seguimiento de decisiones (2026-11-04, constitución versión 1.6)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-003 | **Resuelto** | El periodo se cierra el día 30 de cada mes a las 23:59:59 en hora de Lima; las compras del día 31 pasan al periodo siguiente (RN-062, RN-063). |
| H-005 | **Resuelto** | Una familia en estado inactivo al cierre del periodo no paga suscripción (RN-057). |
| H-006 | **Parcial** | Los ajustes los registra solo la secretaria del colegio (RN-064). Falta definir si un ajuste libera cupo del tope mensual. |
| H-007 | **Resuelto (riesgo aceptado)** | La anulación y los ajustes quedan fuera del MVP. En el MVP no se pueden corregir compras erróneas. |
| H-009 | **Resuelto** | Las notificaciones van al correo del Excel del colegio (RN-061). Falta decidir si el apoderado puede cambiarlo. |
| H-011 | **Parcial** | El apoderado inicia sesión con DNI y PIN (RN-060). Falta el inicio de sesión de la secretaria y del vendedor. |

### Nuevos hallazgos

| ID | Severidad | Ubicación | Problema | Sugerencia |
|---|---|---|---|---|
| H-040 | ALTA | `03-reglas-de-negocio.md` RN-062 | **Febrero no tiene día 30.** La regla de cierre no se puede cumplir en febrero (28 o 29 días). Aunque sea mes de vacaciones, el sistema necesita una fecha de cierre. | Definir el cierre de febrero, por ejemplo el último día del mes. |
| H-041 | ALTA | `00-constitucion.md` PRINCIPIO-005; `03-reglas-de-negocio.md` RN-018, RN-019 y RN-062 | **El tope mensual y el periodo no coinciden.** El tope de S/ 300.00 se calcula por mes calendario (del 1 al 31), pero el consolidado va del 31 de un mes al 30 del siguiente. Un alumno podría consumir más de S/ 300.00 en un mismo periodo de cobro. | Decidir si el tope se calcula por mes calendario o por periodo, y alinear la constitución y las reglas. |

**Puntaje actualizado: 4 de 5.** Ya no hay hallazgos críticos abiertos y las decisiones del flujo principal (suscripción, cuenta, cobro, cierre y ajustes) están definidas. Para llegar a 5 de 5 falta resolver H-040, H-041, el efecto del ajuste en el tope (H-006), el inicio de sesión de la secretaria y del vendedor (H-011), los hallazgos de severidad MEDIA y las correcciones de formato (H-028 a H-031).

---

## Seguimiento de decisiones (2026-11-04, constitución versión 1.7)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-040 | **Resuelto** | El periodo se cierra el último día de cada mes (30 o 31) y el año escolar empieza en marzo (RN-062, RN-063). |
| H-041 | **Resuelto** | El periodo coincide con el mes calendario, igual que el tope mensual. |
| H-011 | **Propuesto** | La secretaria y el dueño de la tienda inician sesión con correo y PIN (RN-068, por confirmar). |
| H-012 | **Resuelto** | El administrador de Luquita registra el colegio, la secretaria y el dueño de la tienda con su correo (RN-065 a RN-067). |

Además, el apoderado puede configurar un tope mensual personalizado de hasta S/ 300.00 (C3; RN-069 y RN-070).

**Puntaje actualizado: 4 de 5.** Para llegar a 5 de 5 falta: confirmar las reglas propuestas, decidir el efecto del ajuste en el tope (H-006), el momento en que aplica un nuevo tope personalizado, los hallazgos de severidad MEDIA y las correcciones de formato (H-028 a H-031).

---

## Seguimiento de decisiones (2026-11-04, constitución versión 1.8)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-011 | **Resuelto** | Apoderado: DNI y PIN. Secretaria: DNI y clave. Dueño de la tienda: correo y clave. Las claves son aleatorias y llegan por correo al registrarlos (RN-068, RN-071, RN-072). |

Además: el tope mensual personalizado aplica de inmediato (RN-076) y no puede ser negativo (RN-077).

### Nuevos hallazgos

| ID | Severidad | Ubicación | Problema | Sugerencia |
|---|---|---|---|---|
| H-042 | MEDIA | `03-reglas-de-negocio.md` RN-072 y RN-073 | **La clave enviada por correo puede seguir en uso indefinidamente.** El cambio en el primer inicio de sesión es opcional para el dueño y no está definido para la secretaria. Si alguien accede al correo, puede usar esa clave. | Evaluar si el cambio de la clave en el primer inicio debe ser obligatorio y si la clave enviada debe vencer en un plazo. |
| H-043 | BAJA | `03-reglas-de-negocio.md` RN-077 | **Un tope de S/ 0.00 equivale a bloquear las compras.** No está claro si es un uso esperado o si debería usarse el bloqueo del tag. | Confirmar si se permite un tope de S/ 0.00. |

**Puntaje actualizado: 4 de 5.** Para llegar a 5 de 5 falta: el efecto del ajuste en el tope (H-006), confirmar las reglas propuestas (RN-024, RN-046, RN-047, RN-050, RN-074), H-042, los hallazgos de severidad MEDIA y las correcciones de formato (H-028 a H-031).

---

## Seguimiento de decisiones (2026-11-04, constitución versión 1.9)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-042 | **Resuelto** | El cambio de la clave recibida por correo es obligatorio en el primer inicio de sesión, para la secretaria y para el dueño de la tienda (RN-073). La clave aleatoria tiene 12 caracteres (RN-074). |

Además: cada alumno nuevo recibe un tope mensual de S/ 300.00 por defecto (RN-078) y solo su apoderado puede cambiarlo (RN-079).

**Puntaje actualizado: 4 de 5.** Sigue abierto el efecto del ajuste en el tope (H-006). Para llegar a 5 de 5 falta resolverlo, confirmar las reglas propuestas que quedan (RN-024, RN-046, RN-047, RN-050), los hallazgos de severidad MEDIA y las correcciones de formato (H-028 a H-031).

---

## Seguimiento de decisiones (2026-11-04, constitución versión 1.10)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-006 | **Resuelto** | Los ajustes los registra solo la secretaria (RN-064), no cuentan para los límites de consumo (RN-080) y solo cambian el monto del consolidado (RN-081). |

**Puntaje actualizado: 4 de 5.** El flujo principal ya no tiene decisiones abiertas. Para llegar a 5 de 5 falta confirmar las reglas propuestas que quedan (RN-024, RN-046, RN-047, RN-050), resolver los hallazgos de severidad MEDIA aún abiertos (H-013 a H-018, H-020 a H-027, H-038, H-039) y corregir el formato (H-028 a H-031).

---

## Seguimiento de decisiones (2026-11-05, constitución versión 1.11)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-016 | **Resuelto** | Cada familia tiene exactamente un apoderado (RN-086). |
| H-009 | **Resuelto** | Solo la secretaria puede cambiar el correo del apoderado (RN-087). |
| H-018 | **Parcial** | El reclamo se responde en máximo 3 días hábiles (RN-044), dentro del máximo legal de 15 días hábiles (Ley 31435 y DS 101-2022-PCM). Falta definir quién lo responde: Luquita, el colegio o la tienda. |
| H-019 | **Resuelto** | Las familias se registran cada año escolar después de la matrícula (RN-088), y el plazo de 5 años cuenta desde que la secretaria registra el fin del año (RN-039). |
| H-028 | **Resuelto** | RN-014 se dividió en RN-014 y RN-082. |
| H-029 | **Resuelto** | RN-020, RN-024 y RN-026 se dividieron (RN-083, RN-084 y RN-085). |
| H-030 | **Resuelto** | RN-042 usa ahora el patrón State-driven. |
| H-031 | **Resuelto** | RN-028 se reescribió con NO DEBE. |
| H-032 | **Resuelto** | Se agregaron al glosario Consentimiento, Libro de Reclamaciones y Desbloqueo de tag. |

Todas las reglas marcadas como propuesta (RN-024, RN-046, RN-047, RN-050) quedaron confirmadas.

### Hallazgos que siguen abiertos

| ID | Severidad | Tema |
|---|---|---|
| H-013 | MEDIA | Día al que se resta una anulación para el límite diario. |
| H-014 | MEDIA | Reinicio del contador de PIN fallidos del alumno. |
| H-015 | MEDIA | Ventana y reinicio del contador de intentos fallidos de las cuentas. |
| H-017 | MEDIA | Regla de desbloqueo de un tag que bloqueó el apoderado. |
| H-018 | MEDIA | Quién responde los reclamos. |
| H-020 | MEDIA | Qué hace la app del vendedor sin conexión a internet. |
| H-021 | MEDIA | Quién genera la clave de idempotencia y cuándo. |
| H-022 | MEDIA | Estado en que el colegio entrega los tags (en blanco o grabados). |
| H-023 | MEDIA | Si un tag desvinculado puede volver a vincularse. |
| H-024 | MEDIA | Alternativa para el apoderado sin celular con NFC. |
| H-025 | MEDIA | Unificar "padre" y "apoderado" en todos los documentos. |
| H-026 | MEDIA | Unificar `Compra`, `Asiento` y `Movimiento`. |
| H-027 | MEDIA | Baja de la suscripción a mitad de año. |
| H-038 | MEDIA | Riesgo de datos personales en la descripción libre de la compra. |
| H-039 | MEDIA | Informar el plazo de 5 años en el consentimiento. |
| H-033 | BAJA | Separar los códigos de error del glosario. |
| H-034 | BAJA | Separar la meta de alumnos de la de familias en el brief. |
| H-035 | BAJA | El sistema no valida el rango de edad de 8 a 14 años. |
| H-043 | BAJA | Confirmar si se permite un tope de S/ 0.00. |

**Puntaje actualizado: 4 de 5.** El flujo principal, las reglas confirmadas y el formato ya están listos para que un agente implemente el MVP. Para llegar a 5 de 5 falta cerrar los 15 hallazgos de severidad MEDIA de la tabla anterior.

---

## Seguimiento de decisiones (2026-11-05, constitución versión 1.12)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-020 | **Resuelto** | Sin conexión no se registran compras ni se guardan para después (RN-089, RN-090). |
| H-022 | **Resuelto** | El colegio entrega los tags en blanco; la app rechaza tags que ya tienen datos (RN-091). |
| H-018 | **Resuelto** | El administrador de Luquita responde los reclamos (RN-092). Se propone enviar la respuesta al correo del apoderado (RN-093, por confirmar). |

**Puntaje actualizado: 4 de 5.** Quedan 13 hallazgos de severidad MEDIA (H-013 a H-015, H-017, H-021, H-023 a H-027, H-038, H-039) y 4 de severidad BAJA (H-033 a H-035, H-043).

---

## Seguimiento de decisiones (2026-11-05, constitución versión 1.13)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-018 | **Resuelto** | El dueño de la tienda responde los reclamos (RN-092) y la respuesta se envía al correo del apoderado (RN-093). |

### Nuevo hallazgo

| ID | Severidad | Ubicación | Problema | Sugerencia |
|---|---|---|---|---|
| H-044 | MEDIA | `00-constitucion.md` PRINCIPIO-007; `03-reglas-de-negocio.md` RN-040 y RN-092 | **El dueño de la tienda podría ver datos que la constitución le prohíbe.** Para responder un reclamo sobre una compra, el dueño necesita ver ese reclamo y quizá la compra disputada. Pero el PRINCIPIO-007 dice que el vendedor solo ve el nombre del alumno, nunca su DNI ni su consumo. | Definir qué datos ve el dueño al responder un reclamo (por ejemplo, el texto del reclamo y la compra disputada, sin DNI ni consumo acumulado) y agregarlo como excepción en el PRINCIPIO-007. |

**Puntaje actualizado: 4 de 5.** Quedan 14 hallazgos de severidad MEDIA (H-013 a H-015, H-017, H-021, H-023 a H-027, H-038, H-039, H-044) y 4 de severidad BAJA (H-033 a H-035, H-043).

---

## Seguimiento de decisiones (2026-11-05, constitución versión 1.14)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-044 | **Resuelto** | Al responder un reclamo, el dueño de la tienda ve el texto del reclamo y la compra disputada, sin DNI ni consumo acumulado. Se agregó como excepción en el PRINCIPIO-007 (RN-094, RN-095). |

**Puntaje actualizado: 4 de 5.** Quedan 13 hallazgos de severidad MEDIA (H-013 a H-015, H-017, H-021, H-023 a H-027, H-038, H-039) y 4 de severidad BAJA (H-033 a H-035, H-043).

---

## Seguimiento de decisiones (2026-11-05, constitución versión 1.15)

| Hallazgo | Estado | Decisión del equipo |
|---|---|---|
| H-025 | **Resuelto** | El actor se llama siempre «apoderado» en la constitución, el alcance, las reglas y el glosario. «Padre» queda en la lista de términos que no se usan. |
| H-026 | **Resuelto** | Un único modelo `Movimiento` (tabla `movimiento`) con tipo `COMPRA`, `ANULACION` o `AJUSTE`. «Asiento» queda en la lista de términos que no se usan. |

**Puntaje final de esta fase: 4 de 5, listo para pasar a arquitectura.** Quedan 11 hallazgos de severidad MEDIA (H-013 a H-015, H-017, H-021, H-023, H-024, H-027, H-038, H-039) y 4 de severidad BAJA (H-033 a H-035, H-043), todos casos borde que no bloquean el MVP.

---

## Seguimiento de decisiones (2026-11-11, constitución versión 1.16)

Decisiones del equipo tomadas sobre los hallazgos críticos de la auditoría de trazabilidad (`09-auditoria-trazabilidad.md`).

| Hallazgo | Estado | Decisión |
|---|---|---|
| A-01 (auditoría 09) | **Resuelto** | DNI duplicado dentro del mismo Excel: se procesa la primera fila y se rechazan las siguientes con "DNI duplicado" (RN-098). RN-054 solo actualiza familias que ya existían antes de la importación. |
| A-02 (auditoría 09) | **Resuelto** | Los alumnos solo compran del 1 de marzo al 30 de noviembre, mientras la secretaria o el administrador tengan habilitadas las ventas (RN-096, RN-097, RN-099 a RN-103). El primer periodo es el mes de la primera habilitación; el cierre de noviembre deshabilita las ventas y no hay periodos de diciembre a febrero. |
| H-005 | **Resuelto** | La suscripción solo se cobra en los periodos de la temporada de ventas (marzo a noviembre). Falta confirmar el mes de la primera habilitación y los meses pausados (Pendiente 17 de las HU). |
| H-018, H-044 | **Retirado** | El Libro de Reclamaciones sale del alcance del piloto (RN-043, RN-044 y RN-092 a RN-095 retiradas). |
| H-038 | **Retirado** | Deja de aplicar el plazo de conservación de 5 años; la descripción referencial sigue siendo texto libre de hasta 60 caracteres. |
| H-039 | **Retirado** | El consentimiento y el plazo de conservación salen del alcance del piloto (RN-002 y RN-039 retiradas). |
| H-040 | **Sin efecto** | Ya no hay periodo de febrero. |

**Puntaje: 4 de 5.** Siguen abiertos los hallazgos de severidad MEDIA H-013 a H-015, H-017, H-021, H-023, H-024 y H-027, y los de severidad BAJA H-033 a H-035 y H-043.

---

## Seguimiento de decisiones (2026-11-13, constitución versión 1.17)

El equipo simplificó el piloto para no agregar historias de usuario nuevas.

| Tema | Estado | Decisión |
|---|---|---|
| Suscripción de S/ 5.00 | **Retirada** | Luquita no cobra suscripción en el piloto (RN-032 y RN-057 retiradas). H-005 queda sin efecto. |
| Temporada de ventas (A-02) | **Reemplazada** | Ya no hay rango de marzo a noviembre: los alumnos compran mientras la app del dueño de la tienda esté activa, y la activan o desactivan la secretaria o el administrador (RN-097, RN-101 a RN-103; RN-099 y RN-100 retiradas). |
| Cierre mensual y periodos | **Retirados** | El consolidado se calcula por mes calendario al consultarlo (RN-031 y RN-063 reescritas); cada movimiento lleva la fecha del servidor, así que un mes ya entregado no cambia (RN-034). RN-062 y RN-096 retiradas. H-040 y H-041 quedan sin efecto. |
| Importación del Excel | **Simplificada** | La importan la secretaria o el administrador; se quita la columna `estado` y toda familia importada queda `ACTIVO` (RN-045, RN-051, RN-053 y RN-054). Solo la secretaria cambia el estado de una familia (RN-104 nueva). |
| Fin del año escolar | **Retirado** | RN-055 retirada; la baja de una familia se hace cambiando su estado (cubre en parte H-027). |
| PIN del alumno | **Cambiado** | 3 dígitos en lugar de 4 (RN-004); el tag se sigue bloqueando tras 3 intentos fallidos. |

**Puntaje: 4 de 5.** Siguen abiertos los hallazgos de severidad MEDIA H-013 a H-015, H-017, H-021, H-023 y H-024, y los de severidad BAJA H-033 a H-035 y H-043.

---

## Cierre de hallazgos (2026-11-13)

Decisiones tomadas para cerrar los hallazgos que seguían abiertos, sin agregar historias de usuario.

| Hallazgo | Estado | Decisión |
|---|---|---|
| H-013 | **Resuelto** | Una anulación (S1, fuera del MVP) cuenta en el día y el mes en que se registra, con la fecha del servidor (RN-034). |
| H-014 | **Resuelto** | 3 PIN fallidos consecutivos sin ventana de tiempo; el contador vuelve a cero con un PIN correcto o al desbloquear el tag (RN-027). |
| H-015 | **Resuelto** | 5 fallos consecutivos; el contador vuelve a cero con un acceso exitoso o al terminar los 900 s, y el acceso se permite desde el segundo 900 (RN-029). |
| H-017 | **Resuelto** | Solo el apoderado desbloquea un tag, cualquiera sea el motivo del bloqueo (RN-028). |
| H-021 | **Resuelto** | La app del vendedor genera la clave de idempotencia al iniciar cada intento y la reutiliza solo para reintentarlo (RN-015, ADR-002). |
| H-023 | **Resuelto** | Un tag desvinculado (S3, fuera del MVP) se descarta y no se vuelve a vincular. |
| H-024 | **Fuera de alcance** | El apoderado sin celular con NFC puede iniciar sesión en otro celular para grabar el tag (`02` W17). |
| H-033 | **Aceptado** | El glosario se mantiene; el catálogo de códigos de error vive en la arquitectura §4.1. |
| H-034 | **Sin efecto** | Sin suscripción ya no hay proyección por familias; el KPI de uso es por alumno. |
| H-035 | **Resuelto** | El sistema no valida la edad; lo controla el colegio (`02`, decisiones). |
| H-043 | **Resuelto** | Se permite un tope de S/ 0.00 (C3, fuera del MVP). |

**Puntaje final: 5 de 5.** No quedan hallazgos abiertos.

---

## Hallazgos del Spec Finalizer (2026-11-14)

Auditoría final del Spec Pack v1.0 (`10_2-reporte-spec-finalizer.md`): **0 críticos, 2 mayores y 6 menores.** Score: Operatividad 4/5, Utilidad 4/5, Orden 4/5. Los mayores no bloquean la implementación.

| # | Hallazgo | Severidad | Estado |
|---|---|---|---|
| SF-M1 | Restos del modelo de suscripción: código `DNI_NO_SUSCRITO`, su mensaje y «familias suscritas» en los documentos | Mayor | **Resuelto (2026-11-14):** el código pasó a `DNI_NO_REGISTRADO`, el mensaje a «Tu DNI no está registrado en Luquita…» y «suscrita» a «registrada» (incluidos `FamiliaRegistrada` y `registrada_desde`) |
| SF-M2 | «App de cobro» (`09`) frente a «app del dueño» (resto) sin equivalencia en el glosario | Mayor | **Resuelto (2026-11-14):** el glosario define «App del dueño» e indica que en pantalla se muestra como «app de cobro» |
| SF-m1 | Glosario sin NDEF, JWT, HMAC, outbox ni token opaco | Menor | **Resuelto (2026-11-14):** 5 entradas nuevas |
| SF-m2 | `07` §6 dice «constitución 1.16» | Menor | **Resuelto (2026-11-14)** |
| SF-m3 | Notación `{id}` en `09` frente a `{colegioId}` en `07` | Menor | **Resuelto (2026-11-14)** |
| SF-m4 | Q-05, Q-07 y Q-08 abiertas en `09` §7 | Menor | Abierto (no bloquean) |
| SF-m5 | `08` no cita reglas por ID | Menor | Abierto |
| SF-m6 | P-028, P-110, P-111 y P-112 sin screen spec | Menor | Aceptado (Won't y Could) |
