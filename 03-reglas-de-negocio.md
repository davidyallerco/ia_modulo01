# 03 — Reglas de negocio: LUQUITA

- **Producto:** Luquita, billetera digital infantil con tag NFC para la tienda del colegio.
- **Notación:** EARS (Easy Approach to Requirements Syntax).
- **Documentos base:** `00-constitucion.md` (versión 1.17), `01-project-brief.md` y `02-alcance.md`.
- **Fecha:** 2026-11-04. **Actualización:** 2026-11-11 (RN-088 ajustada; RN-096 y RN-097 nuevas, por decisión del equipo sobre el primer periodo).
- **Segunda actualización (2026-11-11, decisiones del equipo tras la auditoría `09`):**
  - **A-01:** RN-054 se limita a los DNI que ya existían antes de la importación y se agrega RN-098 (DNI duplicado dentro del mismo archivo).
  - **A-02:** las compras solo se permiten de marzo a noviembre, mientras la secretaria o el administrador tengan habilitadas las ventas. Se reescriben RN-096 y RN-097 y se agregan RN-099 a RN-103. Los ejemplos con compras o cierres de diciembre a febrero se cambiaron a meses de la temporada de ventas.
  - **M-01 (idempotencia de compras rechazadas):** RN-015 reescrita: el resultado original se devuelve también cuando la compra fue rechazada, sin volver a verificar el PIN ni sumar intentos fallidos.
  - **Retiradas:** RN-043, RN-044 y RN-092 a RN-095 (Libro de Reclamaciones), RN-002 (consentimiento) y RN-039 (eliminación a los 5 años). Sus números no se reutilizan.
- **Tercera actualización (2026-11-13, simplificación del piloto, constitución 1.17):**
  - **Sin suscripción:** se retiran RN-032 y RN-057.
  - **Sin cierre mensual automático ni temporada de ventas:** se retiran RN-062, RN-096, RN-099 y RN-100. El consolidado se calcula al consultarlo (RN-031 y RN-063 reescritas) y los movimientos siempre llevan la fecha del servidor (RN-034 reescrita). Las referencias a «periodo» pasan a «mes calendario». Reemplaza la temporada de la segunda actualización.
  - **App del dueño activa:** RN-097, RN-101, RN-102 y RN-103 se reescriben: para comprar, la app del dueño de la tienda tiene que estar activa, y la activan o desactivan la secretaria o el administrador.
  - **Excel:** lo importan la secretaria o el administrador (RN-045), sin columna `estado` (RN-051), y toda familia importada queda `ACTIVO` (RN-053 y RN-054 reescritas). Nueva RN-104: solo la secretaria cambia el estado de una familia. Se retira RN-055 (fin del año escolar).
  - **PIN del alumno de 3 dígitos:** RN-004 reescrita; los ejemplos usan el PIN `730`.
- **Cuarta actualización (2026-11-13, cierre de hallazgos de la auditoría `09` y de `05`):** RN-005 queda solo para «definir» (no hay cambio de PIN en el piloto); RN-007, RN-015, RN-027, RN-028, RN-029, RN-030, RN-071 y RN-073 se precisan; nueva RN-105 (política de la clave nueva del personal).

**Convenciones:**
- Montos en soles con dos decimales (S/ 30.00).
- Fechas y horas en ISO 8601, hora de Lima (`America/Lima`, UTC-05:00).
- Todos los ejemplos usan datos ficticios.
- **Fuente "Propuesta (por confirmar)":** reglas que la documentación no define explícitamente. El equipo debe validarlas. Al 2026-11-05, todas las reglas propuestas fueron confirmadas.

---

## Registro

---
**RN-001** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M1 | Fuente: Constitución PRINCIPIO-007
Patrón EARS: Ubiquitous
El sistema DEBE almacenar, como datos personales del alumno, únicamente su DNI y su nombre.
✅ **Ejemplo:** el registro del alumno contiene `nombre = "Alumno Prueba"` y `dni = "00000000"`, sin ningún otro dato personal.
❌ **Contraejemplo:** el registro del alumno incluye `grado = "5to primaria"` o `alergias = "maní"`.

---
**RN-003** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M1 | Fuente: Constitución PRINCIPIO-008
Patrón EARS: Ubiquitous
El PIN de acceso del apoderado DEBE tener exactamente 6 dígitos numéricos.
✅ **Ejemplo:** el apoderado define el PIN `482915` y el sistema lo acepta.
❌ **Contraejemplo:** el sistema acepta el PIN `4829` (4 dígitos) o `48a915` (contiene una letra).

---
**RN-004** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M1 | Fuente: Constitución PRINCIPIO-008
Patrón EARS: Ubiquitous
El PIN del alumno DEBE tener exactamente 3 dígitos numéricos.
✅ **Ejemplo:** el apoderado define para su hijo el PIN `730` y el sistema lo acepta.
❌ **Contraejemplo:** el sistema acepta como PIN del alumno `73` (2 dígitos) o `7302` (4 dígitos).

---
**RN-005** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M1 | Fuente: Constitución PRINCIPIO-008
Patrón EARS: Unwanted
Si un usuario distinto del apoderado del alumno intenta definir el PIN de ese alumno, el sistema NO DEBE aplicarlo. En el piloto el PIN solo se define al registrar al hijo; no hay operación para cambiarlo (riesgo R5 aceptado: el apoderado conoce el PIN que definió).
✅ **Ejemplo:** el vendedor intenta registrar a "Alumno Prueba" con un PIN y el sistema responde `403` sin guardar nada.
❌ **Contraejemplo:** el apoderado de otra familia define un PIN para "Alumno Prueba" y se guarda.

---

## Tags

---
**RN-006** | Categoría: Tags | Prioridad: MUST | Funcionalidad: M2 | Fuente: Usuario
Patrón EARS: State-driven
Mientras la familia no esté registrada por el colegio en estado `ACTIVO`, el sistema NO DEBE permitir vincular un tag a sus hijos.
✅ **Ejemplo:** la secretaria registró a "Familia Prueba" como registrada el 2027-03-01; su apoderado vincula un tag a su hijo y el sistema lo acepta.
❌ **Contraejemplo:** una familia que el colegio no registró vincula un tag a su hijo y el sistema lo acepta.

---
**RN-007** | Categoría: Tags | Prioridad: MUST | Funcionalidad: M2 | Fuente: Constitución PRINCIPIO-006
Patrón EARS: Ubiquitous
El tag DEBE contener únicamente un token aleatorio en formato UUID v4 (128 bits, de los cuales 122 son aleatorios).
✅ **Ejemplo:** el tag contiene `luquita:tag:9f1c2e7a-4b3d-4e8f-a1c6-2d5e7f9b0a13`.
❌ **Contraejemplo:** el tag contiene `DNI=00000000;nombre=Alumno Prueba` o el identificador secuencial `alumno:152`.

---
**RN-008** | Categoría: Tags | Prioridad: MUST | Funcionalidad: M2 | Fuente: Constitución PRINCIPIO-006
Patrón EARS: Event-driven
Cuando la app del apoderado termine de grabar el token en el tag, la app DEBE bloquear el tag contra escritura.
✅ **Ejemplo:** el apoderado graba el token y, al intentar reescribir el tag con otra app NFC, la escritura falla.
❌ **Contraejemplo:** después de la vinculación, otra app NFC puede sobrescribir el token del tag.

---
**RN-009** | Categoría: Tags | Prioridad: MUST | Funcionalidad: M2 | Fuente: Constitución PRINCIPIO-006
Patrón EARS: Unwanted
Si el tag que se intenta vincular no es de tipo NTAG213 ni NTAG215, la app DEBE rechazar la vinculación.
✅ **Ejemplo:** el apoderado acerca un tag MIFARE Classic y la app muestra "Este tag no es compatible con Luquita" sin vincularlo.
❌ **Contraejemplo:** la app vincula un tag MIFARE Classic al alumno.

---
**RN-091** | Categoría: Tags | Prioridad: MUST | Funcionalidad: M2 | Fuente: Usuario
Patrón EARS: Unwanted
Si el tag que se intenta vincular ya contiene datos grabados, la app DEBE rechazar la vinculación.
✅ **Ejemplo:** el apoderado acerca un tag en blanco entregado por el colegio y la app lo graba y lo vincula.
❌ **Contraejemplo:** la app vincula un tag que ya tenía grabado otro token o datos de otra aplicación.

---
**RN-010** | Categoría: Tags | Prioridad: SHOULD | Funcionalidad: S3 | Fuente: Alcance
Patrón EARS: Ubiquitous
Cada alumno DEBE tener como máximo un tag activo.
✅ **Ejemplo:** el alumno "Alumno Prueba" tiene un tag activo; el apoderado vincula uno nuevo y el anterior pasa a estado desvinculado.
❌ **Contraejemplo:** el alumno "Alumno Prueba" tiene dos tags activos con los que se puede comprar.

---
**RN-011** | Categoría: Tags | Prioridad: SHOULD | Funcionalidad: S3 | Fuente: Alcance
Patrón EARS: Event-driven
Cuando el apoderado desvincule un tag, el sistema DEBE rechazar toda compra posterior hecha con ese tag.
✅ **Ejemplo:** el tag se desvincula a las 2026-11-04T08:00:00-05:00; una compra con ese tag a las 08:00:05 es rechazada.
❌ **Contraejemplo:** el tag desvinculado registra una compra de S/ 3.50 una hora después.

---

## Compras

---
**RN-012** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-006
Patrón EARS: Ubiquitous
Toda solicitud de compra DEBE incluir el token del tag y el PIN del alumno.
✅ **Ejemplo:** la solicitud incluye `token = 9f1c2e7a-…` y `pin = 730`, y el sistema la procesa.
❌ **Contraejemplo:** el sistema registra una compra que solo incluye el token del tag.

---
**RN-013** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-006
Patrón EARS: Unwanted
Si el PIN del alumno es incorrecto, el sistema NO DEBE registrar la compra.
✅ **Ejemplo:** el PIN correcto es `730`, el alumno digita `731` y el sistema responde `401` sin crear ningún movimiento.
❌ **Contraejemplo:** con el PIN `731` se registra una compra de S/ 2.00.

---
**RN-014** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-004
Patrón EARS: Ubiquitous
El monto de una compra DEBE ser mayor que S/ 0.00.
✅ **Ejemplo:** el vendedor ingresa S/ 3.50 y la compra se procesa.
❌ **Contraejemplo:** el sistema acepta una compra de S/ 0.00.

---
**RN-082** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-004
Patrón EARS: Ubiquitous
El monto de una compra DEBE tener como máximo dos decimales.
✅ **Ejemplo:** el vendedor ingresa S/ 3.50 y la compra se procesa.
❌ **Contraejemplo:** el sistema acepta una compra de S/ 3.505.

---
**RN-015** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-004
Patrón EARS: Event-driven
La app del vendedor DEBE generar la clave de idempotencia al iniciar cada intento de cobro y reutilizarla solo para reintentar ese mismo intento. Cuando llegue una solicitud de compra con una clave de idempotencia ya procesada y el mismo contenido, el sistema DEBE devolver el resultado original, sea una compra registrada o un rechazo, sin crear un movimiento nuevo, sin volver a verificar el PIN y sin sumar intentos fallidos. *(Ampliada el 2026-11-11, auditoría M-01.)*
✅ **Ejemplo:** dos solicitudes con `Idempotency-Key = a1b2c3` por S/ 3.50 llegan con 1 segundo de diferencia; el consumo del día sube solo S/ 3.50. Y si la primera solicitud con `Idempotency-Key = d4e5f6` respondió `401 PIN_INVALIDO` (contador en 1) y el vendedor la reintenta con la misma clave, recibe el mismo `401` y el contador sigue en 1.
❌ **Contraejemplo:** la doble lectura del tag con la misma clave registra S/ 7.00 de consumo, o el reintento de un `401` perdido vuelve a verificar el PIN y deja el contador en 2.

---
**RN-016** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: State-driven
Mientras el tag esté bloqueado, el sistema DEBE rechazar toda compra hecha con ese tag.
✅ **Ejemplo:** el tag está bloqueado desde las 2026-11-04T09:00:00-05:00; una compra a las 10:15:00 es rechazada.
❌ **Contraejemplo:** un tag bloqueado registra una compra de S/ 5.00.

---
**RN-089** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario
Patrón EARS: Unwanted
Si la app del vendedor no tiene conexión a internet, la app NO DEBE registrar la compra.
✅ **Ejemplo:** sin conexión, el vendedor acerca el tag y la app muestra "Sin conexión: no se puede cobrar con Luquita" sin registrar nada.
❌ **Contraejemplo:** sin conexión, la app acepta la compra de S/ 3.50 y la muestra como cobrada.

---
**RN-090** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario
Patrón EARS: Ubiquitous
La app del vendedor NO DEBE guardar compras para enviarlas al servidor después.
✅ **Ejemplo:** al volver la conexión no se envía ninguna compra pendiente, porque la app no guardó ninguna.
❌ **Contraejemplo:** la app guarda 5 compras hechas sin conexión y las envía al servidor cuando vuelve internet.

---

---
**RN-048** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario
Patrón EARS: Optional
Donde el vendedor ingrese una descripción referencial de la compra, el sistema DEBE guardarla junto con la compra.
✅ **Ejemplo:** el vendedor registra S/ 3.50 con la descripción "Jugo de naranja" y la compra queda guardada con esa descripción.
❌ **Contraejemplo:** el vendedor escribe "Jugo de naranja" y la compra se guarda sin descripción.

---
**RN-049** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario
Patrón EARS: Unwanted
Si el vendedor no ingresa una descripción referencial, el sistema DEBE registrar la compra igualmente.
✅ **Ejemplo:** el vendedor registra S/ 2.00 sin descripción y la compra queda registrada.
❌ **Contraejemplo:** la app no deja confirmar la compra hasta que el vendedor escriba una descripción.

---
**RN-050** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario
Patrón EARS: Ubiquitous
La descripción referencial de una compra DEBE tener como máximo 60 caracteres.
✅ **Ejemplo:** "Empanada de pollo y chicha morada" (33 caracteres) se acepta.
❌ **Contraejemplo:** se acepta una descripción de 120 caracteres.

---

## Límites

---
**RN-017** | Categoría: Límites | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Unwanted
Si el consumo del alumno en el día más el monto de la nueva compra supera S/ 30.00, el sistema DEBE rechazar la compra con el código `LIMITE_DIARIO_EXCEDIDO`.
✅ **Ejemplo:** el alumno consumió S/ 27.00 el 2026-11-04; una compra de S/ 5.00 ese día se rechaza con `422 LIMITE_DIARIO_EXCEDIDO`.
❌ **Contraejemplo:** con S/ 27.00 consumidos el mismo día, una compra de S/ 5.00 se registra y el consumo del día llega a S/ 32.00.

---
**RN-018** | Categoría: Límites | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Unwanted
Si el consumo del alumno en el mes más el monto de la nueva compra supera S/ 300.00, el sistema DEBE rechazar la compra con el código `LIMITE_MENSUAL_EXCEDIDO`.
✅ **Ejemplo:** el alumno acumula S/ 298.00 en noviembre de 2026; una compra de S/ 4.00 se rechaza con `422 LIMITE_MENSUAL_EXCEDIDO`.
❌ **Contraejemplo:** con S/ 298.00 acumulados, una compra de S/ 4.00 se registra y el mes llega a S/ 302.00.

---
**RN-019** | Categoría: Límites | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Ubiquitous
El sistema DEBE calcular el día y el mes calendario de los límites en hora de Lima (`America/Lima`).
✅ **Ejemplo:** una compra a las 2026-11-04T23:59:00-05:00 cuenta para el 2026-11-04, y una a las 2026-11-05T00:01:00-05:00 cuenta para el 2026-11-05.
❌ **Contraejemplo:** una compra a las 2026-11-04T20:00:00-05:00 se cuenta en el 2026-11-05 porque el sistema usa UTC.

---
**RN-020** | Categoría: Límites | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Ubiquitous
El sistema DEBE validar los límites diario y mensual en el servidor.
✅ **Ejemplo:** el servidor rechaza con `LIMITE_DIARIO_EXCEDIDO` una compra aunque la app del vendedor no haya hecho ninguna validación.
❌ **Contraejemplo:** solo la app del vendedor valida el límite y una app modificada registra una compra que deja el día en S/ 33.00.

---
**RN-083** | Categoría: Límites | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Ubiquitous
El sistema DEBE validar los límites dentro de la misma transacción que registra la compra.
✅ **Ejemplo:** dos compras simultáneas de S/ 4.00 con S/ 25.00 ya consumidos en el día: la primera se registra y la segunda se rechaza.
❌ **Contraejemplo:** ambas compras simultáneas se registran y el día queda en S/ 33.00.

---

---
**RN-069** | Categoría: Límites | Prioridad: COULD | Funcionalidad: C3 | Fuente: Usuario
Patrón EARS: Optional
Donde el apoderado haya configurado un tope mensual personalizado para su hijo, el sistema DEBE rechazar con el código `LIMITE_MENSUAL_EXCEDIDO` la compra que lo supere.
✅ **Ejemplo:** el apoderado configuró un tope de S/ 150.00; con S/ 148.00 consumidos en el mes, una compra de S/ 3.00 se rechaza.
❌ **Contraejemplo:** con tope personalizado de S/ 150.00, el alumno llega a S/ 151.00 en el mes.

---
**RN-070** | Categoría: Límites | Prioridad: COULD | Funcionalidad: C3 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Unwanted
Si el apoderado intenta configurar un tope mensual mayor a S/ 300.00, el sistema NO DEBE guardarlo.
✅ **Ejemplo:** el apoderado ingresa S/ 350.00 y la app muestra "El tope máximo es S/ 300.00" sin guardar el cambio.
❌ **Contraejemplo:** se guarda un tope mensual personalizado de S/ 350.00.

## Familias y deuda


---
**RN-076** | Categoría: Límites | Prioridad: COULD | Funcionalidad: C3 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando el apoderado cambie el tope mensual personalizado, el sistema DEBE aplicarlo a la siguiente compra de su hijo.
✅ **Ejemplo:** el alumno consumió S/ 120.00 en el mes; a las 10:00:00 el apoderado baja el tope a S/ 100.00 y una compra de S/ 2.00 a las 10:00:30 se rechaza con `LIMITE_MENSUAL_EXCEDIDO`.
❌ **Contraejemplo:** el nuevo tope de S/ 100.00 recién se aplica desde el mes siguiente.

---
**RN-077** | Categoría: Límites | Prioridad: COULD | Funcionalidad: C3 | Fuente: Usuario
Patrón EARS: Unwanted
Si el apoderado intenta configurar un tope mensual menor a S/ 0.00, el sistema NO DEBE guardarlo.
✅ **Ejemplo:** el apoderado ingresa −S/ 10.00 y la app muestra "El tope no puede ser negativo" sin guardar el cambio.
❌ **Contraejemplo:** se guarda un tope mensual de −S/ 10.00.

---
**RN-078** | Categoría: Límites | Prioridad: MUST | Funcionalidad: M1 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando se registre un alumno nuevo, el sistema DEBE asignarle un tope mensual de S/ 300.00.
✅ **Ejemplo:** el apoderado registra a "Alumno Prueba" y su tope mensual queda en S/ 300.00.
❌ **Contraejemplo:** el alumno nuevo queda sin tope mensual o con un tope de S/ 0.00.

---
**RN-079** | Categoría: Límites | Prioridad: COULD | Funcionalidad: C3 | Fuente: Usuario
Patrón EARS: Unwanted
Si un usuario distinto del apoderado del alumno intenta cambiar su tope mensual, el sistema NO DEBE aplicar el cambio.
✅ **Ejemplo:** la secretaria intenta cambiar el tope de "Alumno Prueba" y el sistema responde `403`.
❌ **Contraejemplo:** el dueño de la tienda sube el tope de un alumno de S/ 150.00 a S/ 300.00.
---
**RN-021** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario; Constitución PRINCIPIO-003
Patrón EARS: Ubiquitous
El sistema NO DEBE rechazar una compra por deudas de la familia con el colegio.
✅ **Ejemplo:** "Familia Prueba" tiene impaga la boleta de octubre de 2026; en noviembre su hijo compra S/ 2.00 con tag y PIN válidos y la compra se registra.
❌ **Contraejemplo:** la compra de S/ 2.00 se rechaza con el código `FAMILIA_CON_DEUDA`.

---
**RN-022** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando la secretaria del colegio registre manualmente una familia en el panel web, el sistema DEBE marcar a esa familia como registrada desde la fecha de registro.
✅ **Ejemplo:** la secretaria registra a "Familia Prueba" el 2027-03-01T09:00:00-05:00 y la familia queda registrada desde 2027-03-01.
❌ **Contraejemplo:** la familia registrada el 2027-03-01 queda sin registrar hasta que el apoderado la active desde la app.

---
**RN-045** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando la secretaria del colegio o el administrador de Luquita importen un archivo Excel de familias en el panel web, el sistema DEBE registrar cada familia de una fila válida.
✅ **Ejemplo:** el administrador importa `familias-2027.xlsx` con 150 filas válidas para el "Colegio Prueba" y quedan 150 familias registradas.
❌ **Contraejemplo:** el archivo tiene 150 filas válidas y solo quedan registradas las primeras 100.

---
**RN-046** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Unwanted
Si una fila del archivo Excel no cumple el formato definido, el sistema NO DEBE registrar la familia de esa fila.
✅ **Ejemplo:** la fila 12 no tiene DNI del apoderado; el sistema la omite y la informa como "Fila 12: falta el DNI del apoderado".
❌ **Contraejemplo:** la fila 12 sin DNI del apoderado se registra como una familia registrada.

---
**RN-047** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando termine la importación de un archivo Excel, el sistema DEBE mostrar a quien importó el archivo la cantidad de filas registradas y la lista de filas rechazadas con su motivo.
✅ **Ejemplo:** al terminar, el panel muestra "148 familias registradas — 2 filas rechazadas: fila 12 (falta DNI), fila 87 (DNI duplicado)".
❌ **Contraejemplo:** el panel solo muestra "Importación terminada" sin indicar qué filas se rechazaron.

---
**RN-051** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Ubiquitous
El archivo Excel de familias DEBE tener las columnas `dni_apoderado`, `nombre`, `apellidos` y `correo`.
✅ **Ejemplo:** el archivo tiene las cuatro columnas y la fila `00000000 | Apoderado | Prueba Uno | apoderado@ejemplo.pe`.
❌ **Contraejemplo:** el archivo no trae la columna `correo` y el sistema lo importa igual.

---
**RN-052** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Unwanted
Si el DNI del apoderado de una fila no tiene exactamente 8 dígitos numéricos, el sistema NO DEBE registrar esa fila.
✅ **Ejemplo:** la fila con DNI `0000000` (7 dígitos) se rechaza con el motivo "DNI inválido".
❌ **Contraejemplo:** se registra una familia con DNI `00A00000`.

---
**RN-053** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario (reescrita el 2026-11-13)
Patrón EARS: Event-driven
Cuando se importe una fila válida del archivo Excel, el sistema DEBE dejar a esa familia en estado `ACTIVO`.
✅ **Ejemplo:** se importa la fila de "Apoderado Prueba Uno" y la familia queda `ACTIVO`.
❌ **Contraejemplo:** la familia importada queda `INACTIVO` o sin estado.

---
**RN-054** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando una fila del Excel tenga un DNI de apoderado de una familia que ya existía en el sistema antes de iniciar esa importación, el sistema DEBE actualizar los datos de esa familia y dejarla en estado `ACTIVO`, sin crear una familia nueva.
✅ **Ejemplo:** en febrero de 2028 se importa la fila del DNI `00000000`; la familia registrada en 2027 se actualiza, queda `ACTIVO` y conserva a sus hijos y sus tags.
❌ **Contraejemplo:** la importación de 2028 crea una segunda familia con el DNI `00000000`.

---
**RN-098** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario (2026-11-11, auditoría A-01)
Patrón EARS: Unwanted
Si dos o más filas del mismo archivo Excel tienen el mismo `dni_apoderado`, el sistema DEBE procesar solo la primera de esas filas y rechazar las siguientes con el motivo "DNI duplicado".
✅ **Ejemplo:** las filas 5 y 87 tienen el DNI `00000003`; la fila 5 registra la familia (o la actualiza, si ya existía antes de la importación) y el resultado informa "fila 87 (DNI duplicado)".
❌ **Contraejemplo:** la fila 87 sobrescribe el nombre o el correo que acababa de registrar la fila 5.

---
**RN-056** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario
Patrón EARS: State-driven
Mientras la familia esté en estado `INACTIVO`, el sistema DEBE rechazar las compras de sus hijos con el código `FAMILIA_INACTIVA`.
✅ **Ejemplo:** el 2027-05-03 la secretaria pasa la familia a `INACTIVO`; el 2027-05-10, con la app del dueño activa, su hijo intenta comprar S/ 2.00 y recibe `422 FAMILIA_INACTIVA`.
❌ **Contraejemplo:** con la familia `INACTIVO`, el alumno compra S/ 2.00 y la compra se registra.

---
**RN-104** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario (2026-11-13)
Patrón EARS: Unwanted
Si un usuario distinto de la secretaria del colegio intenta cambiar el estado de una familia entre `ACTIVO` e `INACTIVO`, el sistema NO DEBE aplicar el cambio.
✅ **Ejemplo:** la secretaria pasa "Familia Prueba" a `INACTIVO` el 2027-05-03 desde el panel web y, desde ese momento, sus hijos no pueden comprar.
❌ **Contraejemplo:** el dueño de la tienda o el apoderado pasan una familia a `INACTIVO` o a `ACTIVO`.

---
**RN-058** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M1 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando el apoderado cree su cuenta en la app, el sistema DEBE asociarla a la familia registrada por el colegio con el mismo DNI de apoderado.
✅ **Ejemplo:** el apoderado crea su cuenta con DNI `00000000` y queda asociado a "Familia Prueba", importada con ese DNI.
❌ **Contraejemplo:** la cuenta creada con DNI `00000000` queda asociada a otra familia o a ninguna.

---
**RN-059** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M1 | Fuente: Usuario
Patrón EARS: Unwanted
Si el DNI ingresado no pertenece a una familia en estado `ACTIVO` registrada por el colegio, el sistema NO DEBE crear la cuenta del apoderado.
✅ **Ejemplo:** el DNI `11111111` no está en la lista del colegio y la app muestra "Tu DNI no está registrado en Luquita. Consulta con la secretaría del colegio."
❌ **Contraejemplo:** se crea una cuenta con el DNI `11111111`, que el colegio no registró.

---
**RN-060** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M1 | Fuente: Usuario
Patrón EARS: Ubiquitous
El apoderado DEBE iniciar sesión en la app con su DNI y su PIN de acceso.
✅ **Ejemplo:** el apoderado ingresa DNI `00000000` y PIN `482915` y accede a su cuenta.
❌ **Contraejemplo:** la app permite el acceso solo con el PIN, sin pedir el DNI.

---
**RN-061** | Categoría: Notificaciones | Prioridad: MUST | Funcionalidad: M4 | Fuente: Usuario
Patrón EARS: Ubiquitous
El sistema DEBE enviar las notificaciones al correo del apoderado registrado por el colegio.
✅ **Ejemplo:** el Excel trae `apoderado@ejemplo.pe` y los correos de cada compra llegan a esa dirección.
❌ **Contraejemplo:** los correos se envían a una dirección distinta, que nadie registró en el sistema.

---
**RN-065** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Unwanted
Si un usuario distinto del administrador de Luquita intenta registrar un colegio, el sistema NO DEBE registrarlo.
✅ **Ejemplo:** el administrador de Luquita registra "Colegio Prueba" y queda registrado.
❌ **Contraejemplo:** la secretaria de un colegio registra un colegio nuevo desde el panel web.

---
**RN-066** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Unwanted
Si un usuario distinto del administrador de Luquita intenta registrar una secretaria o un dueño de tienda, el sistema NO DEBE registrarlo.
✅ **Ejemplo:** el administrador registra a la secretaria `secretaria@colegioprueba.pe` y al dueño `tienda@colegioprueba.pe`.
❌ **Contraejemplo:** el dueño de la tienda registra a un segundo vendedor desde su app.

---
**RN-067** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Ubiquitous
El registro de una secretaria o de un dueño de tienda DEBE incluir su correo.
✅ **Ejemplo:** la secretaria se registra con el correo `secretaria@colegioprueba.pe`.
❌ **Contraejemplo:** se registra un dueño de tienda sin correo.


---
**RN-075** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Ubiquitous
El registro de una secretaria DEBE incluir su DNI.
✅ **Ejemplo:** la secretaria se registra con DNI `00000001` y correo `secretaria@colegioprueba.pe`.
❌ **Contraejemplo:** se registra una secretaria solo con su correo.
---
**RN-068** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Ubiquitous
La secretaria DEBE iniciar sesión en el panel web con su DNI como usuario y su clave.
✅ **Ejemplo:** la secretaria ingresa el usuario `00000001` y su clave, y accede al panel web.
❌ **Contraejemplo:** la secretaria accede con su correo como usuario.

---
**RN-071** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario
Patrón EARS: Ubiquitous
El dueño de la tienda y el administrador de Luquita DEBEN iniciar sesión con su correo como usuario (sin distinguir mayúsculas) y su clave.
✅ **Ejemplo:** el dueño ingresa `tienda@colegioprueba.pe` y su clave, y accede a la app de cobro.
❌ **Contraejemplo:** el dueño accede con un usuario distinto del correo que registró el administrador.

---
**RN-072** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando el administrador de Luquita registre a una secretaria o a un dueño de tienda, el sistema DEBE enviar a su correo una clave aleatoria de acceso.
✅ **Ejemplo:** el administrador registra a `tienda@colegioprueba.pe` el 2027-02-15T10:00:00-05:00 y ese correo recibe su clave de acceso.
❌ **Contraejemplo:** el administrador define la clave a mano y se la comunica por teléfono.

---
**RN-073** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M3, M5 | Fuente: Usuario
Patrón EARS: State-driven
Mientras la secretaria, el dueño de la tienda o el administrador de Luquita no hayan cambiado la clave recibida por correo, el sistema NO DEBE permitirles ninguna acción distinta de cambiar esa clave.
✅ **Ejemplo:** en su primer inicio de sesión, la secretaria solo ve la pantalla "Cambia tu clave"; después de cambiarla accede al panel web.
❌ **Contraejemplo:** el dueño de la tienda registra una compra sin haber cambiado la clave que le llegó por correo.

---
**RN-074** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Ubiquitous
La clave aleatoria generada por el sistema DEBE tener 12 caracteres, con letras mayúsculas, minúsculas y números.
✅ **Ejemplo:** el sistema genera la clave `k7Rm2pXq9Tb4`.
❌ **Contraejemplo:** el sistema genera la clave `123456`.

---
**RN-105** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M3, M5 | Fuente: Usuario (2026-11-13, auditoría M-07)
Patrón EARS: Unwanted
Si la clave nueva de una secretaria, de un dueño de tienda o del administrador de Luquita tiene menos de 12 caracteres, no combina mayúsculas, minúsculas y números, o es igual a la clave recibida, el sistema NO DEBE aceptarla.
✅ **Ejemplo:** la secretaria cambia `k7Rm2pXq9Tb4` por `Colegio2027Ok` y el sistema la acepta.
❌ **Contraejemplo:** el sistema acepta `123456` o la misma `k7Rm2pXq9Tb4` como clave nueva.

---
**RN-086** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M1 | Fuente: Usuario
Patrón EARS: Ubiquitous
Cada familia DEBE tener exactamente un apoderado.
✅ **Ejemplo:** "Familia Prueba" tiene como único apoderado al DNI `00000000`.
❌ **Contraejemplo:** "Familia Prueba" tiene dos cuentas de apoderado, una del padre y otra de la madre.

---
**RN-087** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Unwanted
Si un usuario distinto de la secretaria del colegio intenta cambiar el correo de un apoderado, el sistema NO DEBE aplicar el cambio.
✅ **Ejemplo:** la secretaria cambia en el panel web el correo del apoderado `00000000` a `nuevo@ejemplo.pe`.
❌ **Contraejemplo:** el apoderado cambia su propio correo desde la app.

---
**RN-088** | Categoría: Registro | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario
Patrón EARS: Event-driven
Cuando la secretaria registre las familias de un año escolar, después de la matrícula, el sistema DEBE asociar esas familias a ese año escolar.
✅ **Ejemplo:** el 2027-02-20, después de la matrícula, la secretaria importa el Excel y las familias quedan asociadas al año escolar 2027, que empieza en marzo.
❌ **Contraejemplo:** las familias importadas el 2027-02-20 quedan sin año escolar o asociadas a 2026.

## Notificaciones

---
**RN-023** | Categoría: Notificaciones | Prioridad: MUST | Funcionalidad: M4 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Event-driven
Cuando una compra quede registrada, el sistema DEBE enviar un correo al apoderado con la fecha, la hora, la tienda, el monto y el consumo acumulado del mes.
✅ **Ejemplo:** tras una compra de S/ 3.50 el 2026-11-04T10:15:00-05:00, el apoderado recibe: "Compra en Tienda del colegio — 2026-11-04 10:15 — S/ 3.50 — Consumo de noviembre: S/ 88.50".
❌ **Contraejemplo:** el correo no incluye el consumo acumulado del mes, o no se envía ningún correo por esa compra.

---
**RN-024** | Categoría: Notificaciones | Prioridad: MUST | Funcionalidad: M4 | Fuente: Usuario
Patrón EARS: Unwanted
Si el envío del correo de una compra falla, el sistema DEBE reintentarlo hasta 3 veces, con 60 segundos entre cada intento.
✅ **Ejemplo:** el primer envío falla a las 10:15:02 y el sistema reintenta a las 10:16:02.
❌ **Contraejemplo:** el envío falla una vez y el sistema no vuelve a intentarlo.

---
**RN-084** | Categoría: Notificaciones | Prioridad: MUST | Funcionalidad: M4 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Event-driven
Cuando termine cada intento de envío del correo de una compra, el sistema DEBE registrar su resultado.
✅ **Ejemplo:** queda registrado "intento 1: fallido; intento 2: enviado".
❌ **Contraejemplo:** el correo se envía en el intento 2 y no queda ningún registro de los intentos.

---
**RN-025** | Categoría: Notificaciones | Prioridad: MUST | Funcionalidad: M4 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Unwanted
Si el envío del correo de una compra falla, el sistema NO DEBE anular la compra.
✅ **Ejemplo:** la compra de S/ 3.50 sigue registrada aunque los 3 reintentos del correo fallen.
❌ **Contraejemplo:** el fallo del correo hace que la compra de S/ 3.50 se anule.

---

## Seguridad

---
**RN-026** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M4 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Event-driven
Cuando el apoderado bloquee el tag de su hijo, el sistema DEBE rechazar la siguiente compra con ese tag.
✅ **Ejemplo:** el apoderado bloquea el tag a las 2026-11-04T10:00:00-05:00 y una compra a las 10:00:01 es rechazada.
❌ **Contraejemplo:** después del bloqueo, una compra con ese tag se registra.

---
**RN-085** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M4 | Fuente: Constitución PRINCIPIO-005
Patrón EARS: Ubiquitous
El sistema NO DEBE usar un estado del tag guardado en caché para validar una compra.
✅ **Ejemplo:** cada compra consulta el estado actual del tag en la base de datos.
❌ **Contraejemplo:** una compra a las 10:02:00 se registra porque el estado del tag estaba en caché por 5 minutos.

---
**RN-027** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-008
Patrón EARS: Unwanted
Si el PIN del alumno se ingresa mal 3 veces consecutivas, sin importar el tiempo entre intentos, el sistema DEBE bloquear el tag. El contador vuelve a cero con un PIN correcto o cuando el apoderado desbloquea el tag.
✅ **Ejemplo:** el alumno digita `111`, `222` y `333` seguidos; tras el tercer intento el tag queda bloqueado y la cuarta solicitud es rechazada aunque tenga el PIN correcto.
❌ **Contraejemplo:** el alumno falla el PIN 5 veces seguidas y el tag sigue activo.

---
**RN-028** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M4 | Fuente: Constitución PRINCIPIO-008
Patrón EARS: State-driven
Mientras el tag esté bloqueado, por PIN fallidos o por decisión del apoderado, el sistema NO DEBE permitir que un usuario distinto del apoderado del alumno lo desbloquee.
✅ **Ejemplo:** el apoderado desbloquea el tag desde su app y el alumno vuelve a comprar.
❌ **Contraejemplo:** el vendedor desbloquea el tag desde su app.

---
**RN-029** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M1 | Fuente: Constitución PRINCIPIO-008
Patrón EARS: Unwanted
Si el PIN de un apoderado o la clave de una secretaria, de un dueño de tienda o del administrador de Luquita se ingresa mal 5 veces consecutivas, el sistema DEBE bloquear esa cuenta durante 900 segundos. El acceso vuelve a estar permitido desde el segundo 900, y el contador vuelve a cero con un acceso exitoso o al terminar el bloqueo.
✅ **Ejemplo:** tras el quinto intento fallido a las 2026-11-04T08:00:00-05:00, el acceso queda bloqueado hasta las 08:15:00.
❌ **Contraejemplo:** tras el quinto intento fallido, el sexto intento con la credencial correcta da acceso a las 08:01:00.

---
**RN-030** | Categoría: Seguridad | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-008
Patrón EARS: Ubiquitous
Los registros (logs) del sistema NO DEBEN contener ningún PIN, DNI, clave, token (JWT o del tag completo) ni secreto.
✅ **Ejemplo:** el log de una compra registra `tagId=9f1c… resultado=APROBADA monto=3.50`.
❌ **Contraejemplo:** el log registra `pin=730 dni=00000000`.

---

## Consolidado mensual y app del dueño

---
**RN-063** | Categoría: Consolidado mensual | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario (reescrita el 2026-11-13)
Patrón EARS: Ubiquitous
Cada compra DEBE contarse en el consolidado del mes calendario en que se registró, en hora de Lima.
✅ **Ejemplo:** una compra del 2027-03-31T10:00:00-05:00 aparece en el consolidado de marzo, y una del 2027-04-01T00:00:01-05:00 en el de abril.
❌ **Contraejemplo:** la compra del 2027-03-31T23:30:00-05:00 aparece en el consolidado de abril porque el sistema usa la hora UTC (2027-04-01T04:30:00Z).

---
**RN-031** | Categoría: Consolidado mensual | Prioridad: MUST | Funcionalidad: M5 | Fuente: Constitución PRINCIPIO-003 (reescrita el 2026-11-13)
Patrón EARS: Event-driven
Cuando la secretaria del colegio consulte el consolidado de un mes, el sistema DEBE mostrar, por cada familia, el consumo neto de cada uno de sus hijos en ese mes calendario, calculado en ese momento a partir de los movimientos.
✅ **Ejemplo:** la secretaria consulta noviembre de 2026 y ve para "Familia Prueba": "Alumno Prueba 1: S/ 142.00 — Alumno Prueba 2: S/ 98.50 — Total familia: S/ 240.50".
❌ **Contraejemplo:** el consolidado de noviembre solo muestra el total de la familia, sin separar el consumo de cada hijo, o agrega un cargo de suscripción.

---
**RN-033** | Categoría: Consolidado mensual | Prioridad: MUST | Funcionalidad: M5 | Fuente: Constitución PRINCIPIO-007
Patrón EARS: Ubiquitous
El consolidado que recibe el colegio NO DEBE incluir compras individuales.
✅ **Ejemplo:** el consolidado muestra "Alumno Prueba: S/ 142.00" como total del mes.
❌ **Contraejemplo:** el consolidado lista "2026-11-04 10:15 — S/ 3.50; 2026-11-05 10:20 — S/ 2.00…".

---
**RN-034** | Categoría: Consolidado mensual | Prioridad: MUST | Funcionalidad: M5 | Fuente: Constitución PRINCIPIO-004 (reescrita el 2026-11-13)
Patrón EARS: Ubiquitous
El sistema DEBE registrar cada movimiento con la fecha y hora del servidor y NO DEBE aceptar una fecha enviada por el cliente.
✅ **Ejemplo:** el 2026-11-04T10:15:00-05:00 una app envía una compra con `registradaEn = 2026-10-15`; la compra se registra con la fecha del 2026-11-04 y el consolidado de octubre no cambia.
❌ **Contraejemplo:** la compra se registra con fecha 2026-10-15 y cambia el total de octubre ya entregado al colegio.

---
**RN-097** | Categoría: Compras | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario (reescrita el 2026-11-13)
Patrón EARS: Unwanted
Si llega una compra mientras la app del dueño de la tienda no está activa, el sistema NO DEBE registrarla, NO DEBE verificar el PIN del alumno y DEBE responder con el código `VENTAS_NO_HABILITADAS`.
✅ **Ejemplo:** la secretaria activa la app del dueño recién el 2027-03-01; una compra de S/ 2.00 el 2027-02-25 se rechaza con `409 VENTAS_NO_HABILITADAS`, sin crear ningún movimiento y sin sumar un PIN fallido.
❌ **Contraejemplo:** con la app del dueño desactivada, la compra del 2027-02-25 se registra.

---
**RN-101** | Categoría: App del dueño | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario (reescrita el 2026-11-13)
Patrón EARS: Unwanted
Si un usuario distinto de la secretaria del colegio o del administrador de Luquita intenta activar o desactivar la app del dueño de la tienda, el sistema NO DEBE aplicar el cambio.
✅ **Ejemplo:** el administrador activa la app del dueño del "Colegio Prueba" el 2027-03-01 y queda activa.
❌ **Contraejemplo:** el dueño de la tienda activa su propia app.

---
**RN-102** | Categoría: App del dueño | Prioridad: MUST | Funcionalidad: M5 | Fuente: Usuario (reescrita el 2026-11-13)
Patrón EARS: Event-driven
Cuando la secretaria del colegio o el administrador de Luquita desactiven la app del dueño de la tienda, el sistema DEBE rechazar las compras siguientes.
✅ **Ejemplo:** la secretaria desactiva la app del dueño el 2027-07-12T08:00:00-05:00 por las vacaciones de medio año; una compra a las 08:00:05 se rechaza con `409 VENTAS_NO_HABILITADAS`.
❌ **Contraejemplo:** después de desactivar la app, una compra se registra.

---

## Anulaciones

---
**RN-035** | Categoría: Compras | Prioridad: SHOULD | Funcionalidad: S1 | Fuente: Constitución PRINCIPIO-004
Patrón EARS: Event-driven
Cuando el vendedor anule una compra del mes en curso, el sistema DEBE insertar un movimiento `ANULACION` por el mismo monto con signo contrario.
✅ **Ejemplo:** la compra de +S/ 3.50 del 2026-11-04 se anula con un movimiento `ANULACION` de −S/ 3.50, y el consumo del mes baja S/ 3.50.
❌ **Contraejemplo:** la anulación modifica la compra original a S/ 0.00 o la elimina de la base de datos.

---
**RN-036** | Categoría: Compras | Prioridad: SHOULD | Funcionalidad: S1 | Fuente: Constitución PRINCIPIO-004
Patrón EARS: Unwanted
Si se necesita corregir un consumo de un mes anterior, el sistema DEBE registrar la corrección como un movimiento `AJUSTE` en el mes en curso.
✅ **Ejemplo:** una compra errónea de S/ 4.00 del 2026-10-20, con el consolidado de octubre ya entregado al colegio, se corrige con un `AJUSTE` de −S/ 4.00 en noviembre de 2026.
❌ **Contraejemplo:** la corrección se inserta con fecha 2026-10-20 y cambia el consolidado de octubre ya entregado.


---
**RN-064** | Categoría: Compras | Prioridad: SHOULD | Funcionalidad: S1 | Fuente: Usuario
Patrón EARS: Unwanted
Si un usuario distinto de la secretaria del colegio intenta registrar un `AJUSTE`, el sistema NO DEBE registrarlo.
✅ **Ejemplo:** la secretaria registra desde el panel web un `AJUSTE` de −S/ 4.00 y queda registrado.
❌ **Contraejemplo:** el vendedor registra un `AJUSTE` de −S/ 4.00 desde su app.


---
**RN-080** | Categoría: Límites | Prioridad: SHOULD | Funcionalidad: S1 | Fuente: Usuario
Patrón EARS: Ubiquitous
El sistema NO DEBE incluir los movimientos `AJUSTE` en el consumo usado para validar los límites diario y mensual.
✅ **Ejemplo:** en noviembre de 2026 hay un `AJUSTE` de −S/ 4.00; el alumno puede consumir hasta S/ 300.00 en compras, y no S/ 304.00.
❌ **Contraejemplo:** el `AJUSTE` de −S/ 4.00 libera cupo y el alumno llega a S/ 304.00 en compras en noviembre.

---
**RN-081** | Categoría: Consolidado mensual | Prioridad: SHOULD | Funcionalidad: S1 | Fuente: Usuario
Patrón EARS: Ubiquitous
El monto de cada alumno en el consolidado DEBE ser la suma de sus movimientos `COMPRA`, `ANULACION` y `AJUSTE` del mes.
✅ **Ejemplo:** en noviembre de 2026 el alumno consume S/ 300.00 y tiene un `AJUSTE` de −S/ 4.00; su consolidado de noviembre es S/ 296.00.
❌ **Contraejemplo:** el consolidado de noviembre muestra S/ 300.00 e ignora el `AJUSTE`.
---

## Datos

---
**RN-037** | Categoría: Datos | Prioridad: SHOULD | Funcionalidad: S2 | Fuente: Constitución PRINCIPIO-007
Patrón EARS: Event-driven
Cuando el apoderado solicite desde la app móvil el detalle de consumo de su hijo, el sistema DEBE entregarle la lista de compras del mes con la fecha, la hora, el monto y la descripción referencial de cada una.
✅ **Ejemplo:** el apoderado pide el detalle de noviembre de 2026 y ve "2026-11-04 10:15 — S/ 3.50 — Jugo de naranja; 2026-11-05 10:20 — S/ 2.00 — (sin descripción)".
❌ **Contraejemplo:** el detalle solo muestra el total del mes, sin la lista de compras.

---
**RN-038** | Categoría: Datos | Prioridad: SHOULD | Funcionalidad: S2 | Fuente: Constitución PRINCIPIO-007
Patrón EARS: Unwanted
Si un apoderado solicita el detalle de consumo de un alumno que no es su hijo, el sistema NO DEBE entregarlo.
✅ **Ejemplo:** el apoderado de "Familia Prueba" pide el detalle de un alumno de otra familia y recibe `403`.
❌ **Contraejemplo:** el apoderado de "Familia Prueba" ve las compras de un alumno de otra familia.

---

## UX

---
**RN-040** | Categoría: UX | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-007
Patrón EARS: Ubiquitous
La app del vendedor DEBE mostrar únicamente el nombre del alumno al leer su tag.
✅ **Ejemplo:** al acercar el tag, la pantalla del vendedor muestra "Alumno Prueba" y el campo para el PIN.
❌ **Contraejemplo:** la pantalla del vendedor muestra el DNI `00000000` o "Consumo de noviembre: S/ 88.50".

---
**RN-041** | Categoría: UX | Prioridad: MUST | Funcionalidad: M3 | Fuente: Constitución PRINCIPIO-008
Patrón EARS: Ubiquitous
La app del vendedor DEBE mostrar enmascarado cada dígito del PIN que ingresa el alumno.
✅ **Ejemplo:** el alumno digita `730` y la pantalla muestra `•••`.
❌ **Contraejemplo:** la pantalla muestra `730` o muestra cada dígito por 1 segundo antes de ocultarlo.

---
**RN-103** | Categoría: UX | Prioridad: MUST | Funcionalidad: M3 | Fuente: Usuario (2026-11-11)
Patrón EARS: State-driven
Mientras la app del dueño de la tienda no esté activa, la app DEBE mostrar "La app de cobro no está activa. Consulta con la secretaría del colegio." y NO DEBE permitir iniciar un cobro.
✅ **Ejemplo:** el 2027-07-13 el dueño abre su app desactivada y ve "La app de cobro no está activa. Consulta con la secretaría del colegio.", sin el botón "Cobrar".
❌ **Contraejemplo:** con la app desactivada, la app deja ingresar el monto y leer el tag, y recién el servidor rechaza la compra.

---
**RN-042** | Categoría: UX | Prioridad: SHOULD | Funcionalidad: S5 | Fuente: Alcance
Patrón EARS: State-driven
Cuando un mes haya terminado, el dueño de la tienda PUEDE consultar el total del consumo de ese mes que el colegio debe pagarle.
✅ **Ejemplo:** terminado noviembre de 2026, el dueño consulta "Consumo total de noviembre: S/ 18 450.00".
❌ **Contraejemplo:** el dueño ve el consumo detallado de cada alumno.

---

## Preguntas abiertas

No quedan preguntas abiertas en este catálogo. Los hallazgos que siguen abiertos están en `05-preguntas-abiertas.md`. Los hallazgos de severidad media que siguen abiertos están en `05-preguntas-abiertas.md`.
