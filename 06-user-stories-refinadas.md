# 06 — Historias de Usuario refinadas: LUQUITA

- **Producto:** Luquita, billetera digital infantil con tag NFC para la tienda del colegio.
- **Fase:** planificación. Revisión QA funcional (Agente QA Funcional, BMAD) sobre `06-user-stories.md` del 2026-11-11 (Agente PO).
- **Alcance de esta entrega:** solo el MVP (funcionalidades Must M1 a M5 de `02-alcance.md`).
- **Documentos base:** `00-constitucion.md` (versión 1.17), `01-project-brief.md`, `02-alcance.md`, `03-reglas-de-negocio.md` y `04-glosario.md`.
- **Fecha:** 2026-11-11.
- **Actualización del equipo (2026-11-11, después del backlog; `codigoCompra` y "periodos todos los meses" quedan reemplazados por la segunda actualización):** se confirman `codigoCompra` en el correo de cada compra (Pendiente 9), el año escolar de una importación en febrero (Pendiente 10, en parte) y el primer periodo en marzo, con periodos todos los meses desde ahí (RN-096 y RN-097). Se retiran del alcance de este piloto académico los trámites y la revisión legal (antes Pendiente 15 y brecha 7).
- **Segunda actualización del equipo (2026-11-11, tras la auditoría `09`):**
  - **DNI duplicado en el Excel (A-01):** se procesa la primera fila y se rechazan las siguientes (RN-098); RN-054 solo aplica a familias que ya existían antes de la importación. HU-004 actualizada.
  - **Temporada de ventas (A-02):** los alumnos solo compran de marzo a noviembre, mientras la secretaria o el administrador tengan habilitadas las ventas (RN-096, RN-097, RN-099 a RN-103). El primer periodo es el mes de la primera habilitación; no hay periodos de diciembre a febrero. Nueva HU-033; HU-012 y HU-018 actualizadas. Reemplaza la decisión anterior de "primer periodo en marzo, con periodos todos los meses".
  - **Retirados:** el Libro de Reclamaciones (HU-019, HU-020, épica 8 y `codigoCompra`) y el detalle de protección de datos (consentimiento en HU-008 y eliminación a los 5 años, HU-030).
  - **Idempotencia de compras rechazadas (M-01):** RN-015 ampliada; el servidor guarda el resultado de cada intento de compra, también los rechazos, y el reintento con la misma clave lo devuelve sin volver a verificar el PIN. Nuevos criterios [CONFIRMADO] en HU-012.
  - **Convención de fixtures (actualizada el 2026-11-13):** salvo que el criterio diga otra cosa, el "Colegio Prueba" tiene la app del dueño activa.
- **Tercera actualización del equipo (2026-11-13, simplificación del piloto, constitución 1.17; no agrega HU):**
  - **Sin suscripción, sin cierre mensual automático y sin temporada:** HU-018 se reduce a consultar el consolidado de un mes, calculado al consultarlo (5 → 3 puntos). Reemplaza la temporada de ventas de la segunda actualización.
  - **App del dueño activa:** HU-033 pasa a «Activar y desactivar la app del dueño de la tienda», sin fechas ni periodos (3 → 2 puntos).
  - **Excel:** lo importan la secretaria o el administrador, sin columna `estado`, y toda familia queda `ACTIVO` (HU-004). La secretaria cambia el estado de una familia en HU-005 (RN-104).
  - **Retirada:** HU-025 (fin del año escolar).
  - **PIN del alumno de 3 dígitos:** los ejemplos usan `730` (correcto) y `731` (incorrecto).
- **Cierre de pendientes (2026-11-13):** todos los criterios `[POR CONFIRMAR]` pasan a `[CONFIRMADO]` tal como los propuso QA (salvo el de HU-002 sobre el correo fallido, ajustado al procedimiento manual). Los pendientes del PO y las brechas quedan decididos o como riesgo aceptado. No se agregan HU.

**Convenciones:**
- Montos en soles con dos decimales (S/ 30.00).
- Fechas y horas en ISO 8601, hora de Lima (`America/Lima`, UTC-05:00).
- Solo datos ficticios: "Alumno Prueba", "Familia Prueba", DNI `00000000` (PRINCIPIO-009).
- Se usa el lenguaje del glosario: «apoderado» (nunca «padre») y «movimiento» de tipo `COMPRA`, `ANULACION` o `AJUSTE` (nunca «asiento»). «Vendedor» y «dueño de la tienda» designan al mismo actor.
- **`[NUEVO]`:** criterio agregado por QA. Indica su tipo (**Positivo**, **Negativo** o **Borde**) y, entre paréntesis, la regla o el principio del que se deduce.
- **`[CONFIRMADO]`:** criterio que estaba `[POR CONFIRMAR]` o que nace de una decisión del equipo del 2026-11-11.
- **`[POR CONFIRMAR]`:** ya no queda ninguno: el 2026-11-13 todos pasaron a `[CONFIRMADO]`.
- Los criterios originales del PO se conservan sin cambios. Las correcciones sugeridas van en el bloque **Observaciones QA** de cada HU.

---

## Resumen de la revisión QA

- **HU revisadas:** 20 (ninguna eliminada). **Puntos:** 60, sin reestimar.
- **Criterios nuevos:** 96 en las 20 HU (más 6 criterios transversales), de los cuales 41 están `[POR CONFIRMAR]` y 3 `[CONFIRMADO]`.
- **Cobertura:** las 20 HU terminan con al menos un criterio positivo, uno negativo y uno de borde.
- **Contradicciones con la constitución:** ningún criterio la contradice. Hay 7 brechas entre reglas y constitución (ver sección "Contradicciones y brechas").
- **HUs sugeridas:** 12 (HU-021 a HU-032): 4 por principios sin regla y 8 por reglas SHOULD o COULD sin HU.

**Hallazgos principales (revisar primero):**

1. **HU-009: un tag puede quedar inutilizable.** RN-008 bloquea el tag contra escritura apenas se graba. Si el servidor falla antes de registrar el token, el tag queda bloqueado con un token desconocido, y RN-091 impide volver a usarlo. Hay que definir el orden: registrar en el servidor → grabar → confirmar → bloquear.
2. **HU-012: riesgo de doble cobro por una respuesta perdida.** Si el servidor registra la compra pero la app pierde la conexión antes de recibir la respuesta, el vendedor cree que no cobró y vuelve a cobrar con otra clave. RN-090 prohíbe guardar compras pendientes, y RN-015 solo protege si se reusa la misma `Idempotency-Key`.
3. ~~**HU-019 depende de una funcionalidad fuera del MVP.**~~ **Retirado (2026-11-11):** el Libro de Reclamaciones salió del alcance.
4. **Logs:** RN-030 solo prohíbe PIN y DNI; el PRINCIPIO-008 prohíbe también claves, tokens y secretos.
5. **Faltan HU para principios de la constitución:** cambio del PIN del alumno (PRINCIPIO-008) y recuperación de un PIN o una clave olvidados.
6. **Importación de Excel ambigua:** ~~DNI duplicado dentro del mismo archivo~~ (resuelto con RN-098), ~~año escolar si se importa antes de marzo~~ (resuelto) y ceros iniciales del DNI que Excel elimina.
7. **INVEST:** con los criterios nuevos, HU-012 deja de ser pequeña; se recomienda dividirla. HU-018 también conviene dividirla.
8. **Fixtures:** el apoderado y el alumno usan el mismo DNI `00000000`. Así, un test del tipo "el log no contiene `00000000`" no distingue de quién es el dato.

---

## Criterios transversales (aplican a todas las HU)

Para no repetirlos en cada HU, estos criterios forman parte de la Definición de Hecho de todas las HU:

- [NUEVO] **Negativo (logs):** DADO QUE se ejecuta cualquier flujo de la HU, CUANDO se revisan los logs, ENTONCES no contienen ningún PIN, clave, DNI, JWT ni secreto. El token del tag solo aparece truncado, como en RN-030 (`tagId=9f1c…`). (RN-030, PRINCIPIO-008)
- [NUEVO] **Negativo (sesión):** DADO QUE un endpoint protegido recibe una solicitud sin JWT, o con un JWT emitido hace más de 15 minutos, CUANDO la procesa, ENTONCES responde `401` y no produce ningún efecto. (PRINCIPIO-008)
- [NUEVO] **Borde (accesibilidad):** DADO QUE la pantalla de la HU se muestra con el texto escalado al 200 %, CUANDO se recorre con un lector de pantalla, ENTONCES no se pierde contenido, cada control tiene `Semantics` o `aria-label`, las áreas táctiles miden al menos 48×48 dp y el contraste del texto es de al menos 4.5:1. (PRINCIPIO-011)
- [NUEVO] **Negativo (montos):** DADO QUE una HU suma montos, CUANDO se suman S/ 0.10 y S/ 0.20, ENTONCES el resultado es exactamente S/ 0.30, sin errores de redondeo, porque no se usa `float` ni `double`. (PRINCIPIO-004)
- [NUEVO] **Positivo (documentación):** DADO QUE la HU agrega o cambia un endpoint, CUANDO se revisa el PR, ENTONCES `docs/api/openapi.yaml` incluye el endpoint con sus códigos de error. (PRINCIPIO-012)
- [NUEVO] **Positivo (datos de prueba):** DADO QUE se escriben fixtures, CUANDO se revisan, ENTONCES solo usan datos ficticios y DNI distintos por actor: apoderado `00000000`, secretaria `00000001`, alumno `00000010`. (PRINCIPIO-009)

---

**Resumen (actualizado el 2026-11-13):** 19 HU, todas Must, 54 puntos en total. Se retiraron HU-019 y HU-020 (2026-11-11), se agregó HU-033 y se simplificaron HU-018 y HU-033 (2026-11-13).

| Épica | HU | Funcionalidad | Puntos |
|---|---|---|---|
| 1. Administración del colegio | HU-001 a HU-003 | M5 | 7 |
| 2. Registro de familias registradas | HU-004 y HU-005 | M5 | 8 |
| 3. Cuenta del apoderado y alumnos | HU-006 a HU-008 | M1 | 8 |
| 4. Tags | HU-009 y HU-010 | M2 | 7 |
| 5. Cobro | HU-011 a HU-014 | M3 | 12 |
| 6. Control parental | HU-015 a HU-017 | M4 | 7 |
| 7. Consolidado mensual y app del dueño | HU-018 y HU-033 | M5 | 5 |
| ~~8. Reclamaciones~~ | ~~HU-019 y HU-020~~ (retiradas) | ~~M6~~ | 0 |

---

## Épica 1 — Administración del colegio

---

### HU-001: Registrar el colegio piloto

**Prioridad:** Must
**Funcionalidad del alcance:** M5
**Estimación:** 2 puntos

Como **administrador de Luquita**, quiero registrar el colegio en el panel web, para habilitarlo y luego registrar a su secretaria y al dueño de su tienda.

**Criterios de aceptación:**
- DADO QUE el administrador de Luquita inició sesión en el panel web, CUANDO registra el colegio "Colegio Prueba", ENTONCES el colegio queda registrado y aparece en la lista de colegios del panel.
- DADO QUE la secretaria `00000001` inició sesión en el panel web, CUANDO intenta registrar el colegio "Colegio Nuevo", ENTONCES el sistema responde `403` y no registra el colegio.
- DADO QUE el dueño de la tienda `tienda@colegioprueba.pe` está autenticado en su app, CUANDO envía una solicitud para registrar un colegio, ENTONCES el sistema responde `403` y no registra nada.
- [NUEVO] **Borde:** DADO QUE la lista de colegios del panel está vacía, CUANDO el administrador registra "Colegio San José de Chosica", ENTONCES la lista muestra exactamente 1 colegio y el nombre se guarda y se muestra con su tilde, sin alterarlo. (RN-065; PRINCIPIO-001: solo los identificadores de código son ASCII, no los datos)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE "Colegio Prueba" ya está registrado, CUANDO el administrador intenta registrar otro colegio con el nombre "Colegio Prueba", ENTONCES el sistema no crea un segundo colegio e indica que ya existe.
- [NUEVO] [CONFIRMADO] **Positivo:** DADO QUE el administrador de Luquita recibió su clave inicial, CUANDO inicia sesión con su correo y esa clave, ENTONCES solo puede cambiarla y, después de cambiarla, accede al panel; con 5 claves incorrectas seguidas su cuenta se bloquea 900 segundos. (RN-029, RN-071, RN-073, RN-105; auditoría M-04)

**Reglas de negocio relacionadas:** RN-065

**Notas:** el piloto tiene un solo colegio (W10), pero el registro lo hace siempre el administrador. Panel web en Angular 20 (PRINCIPIO-002), con `aria-label` en todos los controles (PRINCIPIO-011).

**Observaciones QA:**
- **INVEST:** cumple.
- **Valores concretos:** sí.
- **Constitución:** sin contradicciones.
- **Brecha:** las reglas solo definen el nombre del colegio. Confirmar si hay otros campos obligatorios y si el nombre debe ser único.
- **Cobertura:** positivo (c1), negativo (c2, c3) y borde [NUEVO].

---

### HU-002: Registrar a la secretaria y al dueño de la tienda

**Prioridad:** Must
**Funcionalidad del alcance:** M5
**Estimación:** 3 puntos

Como **administrador de Luquita**, quiero registrar a la secretaria y al dueño de la tienda de un colegio con su correo, para que reciban una clave de acceso y puedan usar el panel web y la app de cobro.

**Criterios de aceptación:**
- DADO QUE "Colegio Prueba" está registrado, CUANDO el administrador registra a la secretaria con DNI `00000001` y correo `secretaria@colegioprueba.pe` el 2027-02-15T10:00:00-05:00, ENTONCES la secretaria queda registrada y ese correo recibe una clave aleatoria de 12 caracteres con mayúsculas, minúsculas y números (por ejemplo, `k7Rm2pXq9Tb4`).
- DADO QUE "Colegio Prueba" está registrado, CUANDO el administrador registra al dueño de la tienda con el correo `tienda@colegioprueba.pe`, ENTONCES el dueño queda registrado y recibe su clave aleatoria en ese correo.
- DADO QUE el administrador registra a una secretaria, CUANDO envía el formulario solo con el correo `secretaria@colegioprueba.pe`, sin DNI, ENTONCES el sistema no la registra e indica que falta el DNI.
- DADO QUE el administrador registra a un dueño de tienda, CUANDO envía el formulario sin correo, ENTONCES el sistema no lo registra e indica que falta el correo.
- DADO QUE el dueño de la tienda está autenticado en su app, CUANDO intenta registrar a un segundo vendedor, ENTONCES el sistema responde `403` y no lo registra.
- [NUEVO] **Negativo:** DADO QUE la secretaria `00000001` inició sesión en el panel web, CUANDO intenta registrar al dueño de tienda `otro@colegioprueba.pe`, ENTONCES el sistema responde `403` y no lo registra. (RN-066)
- [NUEVO] **Borde:** DADO QUE una prueba automatizada genera 1 000 claves aleatorias, CUANDO se revisa cada una, ENTONCES todas tienen exactamente 12 caracteres e incluyen al menos una mayúscula, una minúscula y un número. (RN-074)
- [NUEVO] **Negativo:** DADO QUE el administrador registró al dueño `tienda@colegioprueba.pe`, CUANDO se revisan los logs del registro y del envío del correo, ENTONCES no contienen la clave generada. (PRINCIPIO-008: los logs nunca registran claves)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE el administrador registra a una secretaria, CUANDO ingresa el DNI `0000001` (7 dígitos), ENTONCES el sistema no la registra e indica "DNI inválido".
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el servicio de correo no responde, CUANDO el administrador registra al dueño `tienda@colegioprueba.pe`, ENTONCES el usuario queda creado, el panel muestra «Clave no enviada» (`estadoEnvioClave: FALLIDO`) y la clave se regenera con el procedimiento manual del equipo de Luquita (arquitectura §6.7).
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el correo `tienda@colegioprueba.pe` ya pertenece al dueño registrado, CUANDO el administrador intenta registrar a otro usuario con ese correo, ENTONCES el sistema no lo registra e indica que el correo ya está en uso.

**Reglas de negocio relacionadas:** RN-066, RN-067, RN-072, RN-074, RN-075

**Notas:** el administrador nunca define ni ve la clave: el sistema la genera y la envía por correo. La clave se guarda como hash Argon2id (m=19456 KiB, t=2, p=1) y el DNI se cifra con AES-256-GCM con índice ciego HMAC-SHA256 (PRINCIPIO-008). Depende del servicio de envío de correo (proveedor por definir).

**Observaciones QA:**
- **INVEST:** cumple.
- **Valores concretos:** c3 y c4 dicen "indica que falta…" sin texto exacto. Fijar los mensajes (por ejemplo, "Falta el DNI de la secretaria").
- **Brechas:**
  - No hay regla de formato para el DNI de la secretaria, aunque es su usuario de acceso (RN-068). Se sugiere aplicar los 8 dígitos de RN-052.
  - No hay regla para el caso en que falla el envío de la clave, ni para un reenvío.
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c1, c2), negativo (c3, c4, c5 y nuevos) y borde [NUEVO].

---

### HU-003: Iniciar sesión como secretaria y cambiar la clave inicial

**Prioridad:** Must
**Funcionalidad del alcance:** M5
**Estimación:** 2 puntos

Como **secretaria del colegio**, quiero iniciar sesión en el panel web con mi DNI y cambiar la clave que recibí por correo, para acceder al panel con una clave que solo yo conozco.

**Criterios de aceptación:**
- DADO QUE la secretaria `00000001` recibió su clave por correo y nunca inició sesión, CUANDO ingresa el usuario `00000001` y esa clave, ENTONCES el panel solo le muestra la pantalla "Cambia tu clave".
- DADO QUE la secretaria todavía no cambió la clave recibida, CUANDO intenta abrir la importación de familias, ENTONCES el sistema rechaza la acción y la devuelve a la pantalla "Cambia tu clave".
- DADO QUE la secretaria cambió su clave, CUANDO vuelve a iniciar sesión con `00000001` y su nueva clave, ENTONCES accede al panel web.
- DADO QUE la secretaria intenta iniciar sesión, CUANDO usa su correo `secretaria@colegioprueba.pe` como usuario, ENTONCES el sistema no le da acceso.
- DADO QUE la secretaria ingresó mal su clave 5 veces y el quinto intento fue a las 2026-11-04T08:00:00-05:00, CUANDO ingresa la clave correcta a las 08:01:00, ENTONCES el acceso sigue bloqueado hasta las 08:15:00.
- [NUEVO] **Borde:** DADO QUE la secretaria ingresó mal su clave 4 veces, CUANDO en el quinto intento ingresa la clave correcta, ENTONCES accede al panel web. (RN-029: el bloqueo ocurre recién con el quinto fallo)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE la cuenta se bloqueó por el quinto fallo a las 2026-11-04T08:00:00-05:00, CUANDO la secretaria ingresa la clave correcta a las 08:14:59 y luego a las 08:15:00, ENTONCES el primer intento se rechaza y el segundo accede. (RN-029: 900 segundos; confirmar si el segundo 08:15:00 ya está libre)
- [NUEVO] **Negativo:** DADO QUE la secretaria cambia su clave inicial, CUANDO se revisan los logs del inicio de sesión y del cambio, ENTONCES no contienen la clave recibida, la nueva clave ni el DNI `00000001`. (RN-030, PRINCIPIO-008)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la secretaria está en la pantalla "Cambia tu clave", CUANDO define como nueva clave la misma `k7Rm2pXq9Tb4` que recibió o `123456`, ENTONCES el sistema rechaza el cambio. (RN-105)

**Reglas de negocio relacionadas:** RN-029, RN-068, RN-073

**Notas:** JWT firmados con ES256 y expiración de 15 minutos (PRINCIPIO-008). Los logs no registran el DNI ni la clave (RN-030). El código de error para "cuenta bloqueada" y para "clave pendiente de cambio" no está definido en las reglas; se debe fijar en la especificación OpenAPI.

**Observaciones QA:**
- **INVEST:** cumple.
- **Brechas:**
  - No hay política para la nueva clave (longitud mínima, complejidad, no repetir la recibida). Sin ella, la secretaria podría cambiar `k7Rm2pXq9Tb4` por `123456`, que es justo el contraejemplo de RN-074.
  - Falta definir si el contador de fallos vuelve a cero después de un acceso exitoso.
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c1, c3), negativo (c2, c4 y nuevos) y borde (c5 y nuevos).

---

## Épica 2 — Registro de familias registradas

---

### HU-004: Importar el Excel de familias registradas

**Prioridad:** Must
**Funcionalidad del alcance:** M5
**Estimación:** 5 puntos

Como **secretaria del colegio o administrador de Luquita**, quiero importar en el panel web un archivo Excel con las familias después de la matrícula, para registrarlas todas de una vez sin digitarlas una por una.

**Criterios de aceptación:**
- DADO QUE el archivo `familias-2027.xlsx` tiene las columnas `dni_apoderado`, `nombre`, `apellidos` y `correo` y 150 filas válidas, CUANDO la secretaria lo importa en marzo de 2027, ENTONCES quedan 150 familias registradas en estado `ACTIVO` y asociadas al año escolar 2027. *(Actualizado el 2026-11-13: sin columna `estado`.)*
- [CONFIRMADO] **Positivo:** DADO QUE el administrador de Luquita inició sesión, CUANDO importa `familias-2027.xlsx` para el "Colegio Prueba", ENTONCES las familias quedan registradas en ese colegio en estado `ACTIVO`. (RN-045, RN-053; reemplaza el criterio de la columna `estado` vacía, 2026-11-13)
- DADO QUE la fila 12 no tiene DNI del apoderado y la fila 30 tiene el DNI `0000000` (7 dígitos), CUANDO se importa el archivo, ENTONCES ninguna de las dos familias se registra y se informan como "Fila 12: falta el DNI del apoderado" y "Fila 30: DNI inválido".
- DADO QUE el archivo no trae la columna `correo`, CUANDO la secretaria lo importa, ENTONCES el sistema no registra ninguna familia e indica que falta la columna `correo`.
- DADO QUE la familia con DNI `00000000` se registró en 2027, tiene un hijo con un tag vinculado y la secretaria la pasó a `INACTIVO`, CUANDO en marzo de 2028 se importa una fila con ese DNI, ENTONCES el sistema actualiza sus datos, la familia vuelve a `ACTIVO`, conserva a su hijo y su tag, y no se crea una segunda familia.
- DADO QUE la importación terminó con 148 filas válidas y 2 rechazadas, CUANDO el panel muestra el resultado, ENTONCES indica "148 familias registradas — 2 filas rechazadas: fila 12 (falta DNI), fila 87 (DNI duplicado)".
- ~~[NUEVO] **Borde:** DADO QUE la fila del DNI `00000002` trae estado `INACTIVO`…~~ *(Retirado el 2026-11-13: el Excel ya no tiene columna `estado`.)*
- [NUEVO] **Borde:** DADO QUE la celda `dni_apoderado` de la fila 45 es numérica y vale `5`, porque Excel quitó los ceros iniciales del DNI ficticio `00000005`, CUANDO se importa, ENTONCES la fila 45 se rechaza con "Fila 45: DNI inválido" y el resto del archivo se procesa. (RN-052: exactamente 8 dígitos)
- [NUEVO] **Negativo:** DADO QUE el dueño de la tienda está autenticado, CUANDO envía una solicitud para importar `familias-2027.xlsx`, ENTONCES el sistema responde `403` y no registra ninguna familia. (RN-045: solo la secretaria o el administrador importan el Excel)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE las filas 5 y 87 del mismo archivo tienen el DNI `00000003`, CUANDO se importa, ENTONCES la fila 5 se registra y la fila 87 se rechaza con "Fila 87: DNI duplicado", sin cambiar los datos que registró la fila 5. (RN-047, RN-098; decisión del equipo 2026-11-11)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE la familia `00000000` existía antes de la importación y el archivo trae ese DNI en las filas 3 y 40, CUANDO se importa, ENTONCES la fila 3 actualiza la familia (`actualizadas: 1`) y la fila 40 se rechaza con "Fila 40: DNI duplicado". (RN-054, RN-098)
- ~~[NUEVO] [POR CONFIRMAR] **Negativo:** DADO QUE la fila 20 trae estado `SUSPENDIDO`…~~ *(Retirado el 2026-11-13: sin columna `estado`.)*
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE el archivo solo tiene la fila de encabezados, CUANDO se importa, ENTONCES el panel indica "0 familias registradas — 0 filas rechazadas" y no muestra un error técnico.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la secretaria elige el archivo `familias-2027.csv`, CUANDO intenta importarlo, ENTONCES el sistema no lo procesa e indica que solo acepta archivos `.xlsx`.
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE la matrícula de 2028 terminó en febrero, CUANDO la secretaria importa el Excel el 2028-02-20, ENTONCES las familias quedan asociadas al año escolar 2028, y no a 2027. (RN-088, decisión del equipo 2026-11-11)

**Reglas de negocio relacionadas:** RN-045, RN-046, RN-047, RN-051, RN-052, RN-053, RN-054, RN-088, RN-098

**Notas:** las filas válidas se registran aunque otras se rechacen y todas quedan `ACTIVO`. El archivo de prueba solo puede contener datos ficticios (PRINCIPIO-009). Valida el supuesto S4 del brief con la lista real del colegio, sin usarla en fixtures.

**Observaciones QA:**
- **INVEST:** E ⚠️. Hay tres ambigüedades que afectan la estimación:
  - **DNI duplicado dentro del archivo:** el ejemplo de RN-047 rechaza la fila ("DNI duplicado"), pero RN-054 diría que la segunda fila actualiza la familia creada por la primera. **Resuelto (2026-11-11):** RN-098 procesa la primera fila y rechaza las siguientes; RN-054 solo aplica a familias que ya existían antes de la importación.
  - **Año escolar:** el año escolar empieza en marzo (PRINCIPIO-003), pero RN-088 asocia las familias "después de la matrícula", que suele terminar en febrero. **Resuelto (2026-11-11):** el Excel se sube en febrero y las familias van al año escolar del año calendario de la importación.
  - **Ceros iniciales:** decidir si se rechaza la fila o si se completan los ceros. Se recomienda entregar una plantilla con la columna `dni_apoderado` en formato texto.
- **Valores concretos:** c6 usa "fila 87 (DNI duplicado)"; ahora lo respaldan RN-098 y los criterios [CONFIRMADO].
- **Brecha:** no hay regla de formato para `correo`.
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c1), negativo (c3, c4 y nuevos) y borde (c2, c5 y nuevos).

---

### HU-005: Registrar manualmente una familia registrada

**Prioridad:** Must
**Funcionalidad del alcance:** M5
**Estimación:** 3 puntos

Como **secretaria del colegio**, quiero registrar o actualizar manualmente en el panel web una familia registrada, para incluir a las familias que se registran después de la importación o corregir el correo de un apoderado.

**Criterios de aceptación:**
- DADO QUE la secretaria inició sesión en marzo de 2027, CUANDO registra el 2027-03-01T09:00:00-05:00 a la familia del apoderado `00000000` (nombre "Apoderado", apellidos "Prueba Uno", correo `apoderado@ejemplo.pe`), ENTONCES la familia queda en estado `ACTIVO`, registrada desde 2027-03-01 y asociada al año escolar 2027.
- DADO QUE la secretaria registra una familia, CUANDO ingresa el DNI `00A00000`, ENTONCES el sistema no la registra e indica "DNI inválido".
- DADO QUE la familia `00000000` ya existe, CUANDO la secretaria cambia su correo a `nuevo@ejemplo.pe`, ENTONCES el sistema actualiza el correo de esa misma familia, sin crear una familia nueva, y los siguientes correos se envían a `nuevo@ejemplo.pe`.
- DADO QUE el apoderado `00000000` está autenticado en la app, CUANDO intenta cambiar su propio correo, ENTONCES el sistema responde `403` y no aplica el cambio.
- [NUEVO] **Negativo:** DADO QUE el dueño de la tienda está autenticado, CUANDO envía una solicitud para registrar la familia `00000004`, ENTONCES el sistema responde `403` y no la registra. (PRINCIPIO-003: la secretaria registra las familias)
- [NUEVO] **Borde:** DADO QUE la secretaria registra una familia, CUANDO ingresa el DNI `000000001` (9 dígitos), ENTONCES el sistema no la registra e indica "DNI inválido". (RN-052)
- [NUEVO] **Borde:** DADO QUE la secretaria cambió el correo de la familia `00000000` a `nuevo@ejemplo.pe` a las 2026-11-04T10:00:00-05:00, CUANDO su hijo compra S/ 2.00 a las 10:00:30, ENTONCES el correo de esa compra llega a `nuevo@ejemplo.pe` y no a `apoderado@ejemplo.pe`. (RN-061, RN-087)
- [NUEVO] **Negativo:** DADO QUE la familia `00000000` ya existe, CUANDO la secretaria intenta registrarla otra vez con el mismo DNI, ENTONCES el sistema no crea una segunda familia. (RN-086; PRINCIPIO-003: la familia se identifica por el DNI del apoderado)
- [CONFIRMADO] **Positivo:** DADO QUE "Familia Prueba" está `ACTIVO`, CUANDO la secretaria la pasa a `INACTIVO` el 2027-05-03, ENTONCES sus hijos ya no pueden comprar (`422 FAMILIA_INACTIVA`) y, si la vuelve a `ACTIVO`, pueden comprar de nuevo. (RN-056, RN-104; decisión del equipo 2026-11-13)
- [CONFIRMADO] **Negativo:** DADO QUE el dueño de la tienda o el administrador están autenticados, CUANDO intentan cambiar el estado de "Familia Prueba", ENTONCES el sistema responde `403` y no aplica el cambio. (RN-104)

**Reglas de negocio relacionadas:** RN-022, RN-052, RN-054, RN-056, RN-087, RN-088, RN-104

**Notas:** del apoderado solo se guardan DNI, nombre, apellidos y correo (PRINCIPIO-007). El fin del año escolar se retiró el 2026-11-13: la baja de una familia se hace cambiando su estado.

**Observaciones QA:**
- **INVEST:** cumple.
- **Brechas:**
  - ~~Cambio manual del estado de una familia.~~ **Resuelto (2026-11-13):** lo hace la secretaria (RN-104).
  - Ante un DNI repetido, confirmar si el sistema rechaza el registro o actualiza la familia (como RN-054 en el Excel).
  - No hay reglas de longitud para nombre y apellidos ni de formato del correo.
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c1, c3), negativo (c2, c4 y nuevos) y borde [NUEVO].

---

## Épica 3 — Cuenta del apoderado y alumnos

---

### HU-006: Crear la cuenta del apoderado

**Prioridad:** Must
**Funcionalidad del alcance:** M1
**Estimación:** 3 puntos

Como **apoderado**, quiero crear mi cuenta en la app móvil con mi DNI y un PIN de acceso, para gestionar a mis hijos en Luquita.

**Criterios de aceptación:**
- DADO QUE la secretaria registró a "Familia Prueba" con el DNI `00000000` en estado `ACTIVO`, CUANDO el apoderado crea su cuenta con DNI `00000000` y PIN `482915`, ENTONCES la cuenta queda creada y asociada a "Familia Prueba".
- DADO QUE el DNI `11111111` no está en la lista del colegio, CUANDO el apoderado intenta crear su cuenta con ese DNI, ENTONCES la app muestra "Tu DNI no está registrado en Luquita. Consulta con la secretaría del colegio." y no crea la cuenta.
- DADO QUE la familia del DNI `00000002` está en estado `INACTIVO`, CUANDO el apoderado intenta crear su cuenta con ese DNI, ENTONCES el sistema no crea la cuenta.
- DADO QUE el apoderado crea su cuenta, CUANDO define el PIN `4829` (4 dígitos) o `48a915` (contiene una letra), ENTONCES el sistema rechaza el PIN y no crea la cuenta.
- DADO QUE "Familia Prueba" ya tiene la cuenta del apoderado `00000000`, CUANDO se intenta crear una segunda cuenta de apoderado para esa familia, ENTONCES el sistema no la crea.
- [NUEVO] **Negativo:** DADO QUE la familia del DNI `00000002` está en estado `INACTIVO`, CUANDO el apoderado intenta crear su cuenta, ENTONCES la app muestra "Tu DNI no está registrado en Luquita. Consulta con la secretaría del colegio." (RN-059: el mismo caso que un DNI que no pertenece a una familia `ACTIVO`)
- [NUEVO] **Borde:** DADO QUE el apoderado crea su cuenta, CUANDO define el PIN `000000`, ENTONCES el sistema lo acepta, porque tiene exactamente 6 dígitos numéricos. (RN-003)
- [NUEVO] **Borde:** DADO QUE el apoderado crea su cuenta, CUANDO define el PIN `4829150` (7 dígitos), ENTONCES el sistema rechaza el PIN y no crea la cuenta. (RN-003)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el apoderado `00000000` envió el formulario y la red se cortó antes de recibir la respuesta, CUANDO vuelve a enviarlo, ENTONCES queda una sola cuenta asociada a "Familia Prueba" y la app le indica que ya puede iniciar sesión.

**Reglas de negocio relacionadas:** RN-003, RN-058, RN-059, RN-086

**Notas:** el PIN de acceso se guarda como hash Argon2id (PRINCIPIO-008). App en Flutter (PRINCIPIO-002), con teclado accesible de áreas táctiles de 48×48 dp (PRINCIPIO-011).

**Observaciones QA:**
- **INVEST:** cumple.
- **Valores concretos:** c3 y c5 no fijan el mensaje que ve el apoderado. Para c3 se propone el mensaje de RN-059 (criterio nuevo); para c5 falta definirlo.
- **Brechas:**
  - Las reglas aceptan PIN triviales como `000000` o `123456`. Si el equipo quiere rechazarlos, necesita una regla nueva aprobada; QA no la asume.
  - No está definido qué pasa con una cuenta existente cuando su familia pasa a `INACTIVO`: ¿puede iniciar sesión, ver a sus hijos o desbloquear tags?
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c1), negativo (c2 a c5 y nuevos) y borde [NUEVO].

---

### HU-007: Iniciar sesión como apoderado

**Prioridad:** Must
**Funcionalidad del alcance:** M1
**Estimación:** 2 puntos

Como **apoderado**, quiero iniciar sesión en la app con mi DNI y mi PIN de acceso, para usar mi cuenta de forma segura.

**Criterios de aceptación:**
- DADO QUE el apoderado tiene cuenta con DNI `00000000` y PIN `482915`, CUANDO ingresa ese DNI y ese PIN, ENTONCES accede a su cuenta.
- DADO QUE el apoderado abre la pantalla de inicio de sesión, CUANDO intenta ingresar solo con el PIN `482915`, sin DNI, ENTONCES la app no le da acceso.
- DADO QUE el apoderado ingresó mal su PIN 5 veces y el quinto intento fue a las 2026-11-04T08:00:00-05:00, CUANDO ingresa el PIN correcto a las 08:01:00, ENTONCES el acceso sigue bloqueado hasta las 08:15:00.
- DADO QUE ocurrió un intento de inicio de sesión fallido, CUANDO se revisa el log, ENTONCES el registro no contiene el DNI `00000000` ni el PIN ingresado.
- [NUEVO] **Borde:** DADO QUE el apoderado ingresó mal su PIN 4 veces, CUANDO en el quinto intento ingresa `482915`, ENTONCES accede a su cuenta. (RN-029)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE la cuenta se bloqueó a las 2026-11-04T08:00:00-05:00, CUANDO el apoderado ingresa el PIN correcto a las 08:14:59 y luego a las 08:15:00, ENTONCES el primer intento se rechaza y el segundo accede. (RN-029)
- [NUEVO] **Negativo:** DADO QUE el apoderado inició sesión a las 2026-11-04T10:00:00-05:00, CUANDO la app usa ese mismo JWT a las 10:15:01 sin renovarlo, ENTONCES el servidor responde `401`. (PRINCIPIO-008: expiración de 15 minutos)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el DNI `11111111` no tiene cuenta, CUANDO alguien intenta iniciar sesión con ese DNI y el PIN `482915`, ENTONCES la app muestra el mismo mensaje que para un PIN incorrecto, sin revelar si el DNI tiene cuenta.

**Reglas de negocio relacionadas:** RN-029, RN-030, RN-060

**Notas:** JWT firmados con ES256 y expiración de 15 minutos (PRINCIPIO-008). Por el PRINCIPIO-009, ningún agente de IA puede modificar el código de autenticación ni de PIN; su implementación la hace y revisa un humano.

**Observaciones QA:**
- **INVEST:** cumple.
- **Brecha:** RN-029 dice "5 veces", sin "consecutivas" ni ventana de tiempo. Falta definir si un acceso exitoso reinicia el contador y si los fallos de días distintos se acumulan.
- **Fixtures:** en c4, si el alumno también tiene el DNI `00000000`, el test no distingue de quién es el dato filtrado. Usar los DNI de los criterios transversales.
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c1), negativo (c2, c4 y nuevos) y borde (c3 y nuevos).

---

### HU-008: Registrar a un hijo y definir su PIN

**Prioridad:** Must
**Funcionalidad del alcance:** M1
**Estimación:** 3 puntos

Como **apoderado**, quiero registrar a mi hijo con su DNI y nombre y definir su PIN de 3 dígitos, para que pueda comprar en la tienda del colegio.

**Criterios de aceptación:**
- DADO QUE el apoderado de "Familia Prueba" inició sesión, CUANDO el 2026-11-04T19:30:00-05:00 registra a "Alumno Prueba" con DNI `00000010` y define el PIN `730`, ENTONCES el alumno queda registrado con un tope mensual de S/ 300.00. *(Actualizado el 2026-11-11: se retiró el consentimiento y se aplicó la corrección del DNI del alumno.)*
- ~~DADO QUE el apoderado completa el registro de su hijo, CUANDO no marca la aceptación del consentimiento, ENTONCES el sistema no guarda el registro.~~ *(Retirado el 2026-11-11: el consentimiento salió del alcance del piloto.)*
- DADO QUE el apoderado define el PIN de su hijo, CUANDO ingresa `73` (2 dígitos) o `7302` (4 dígitos), ENTONCES el sistema rechaza el PIN y no guarda el registro.
- DADO QUE la solicitud de registro incluye el campo `grado = "5to primaria"`, CUANDO el sistema guarda al alumno, ENTONCES solo almacena su DNI y su nombre.
- DADO QUE "Alumno Prueba" pertenece a "Familia Prueba", CUANDO el apoderado de otra familia o el vendedor intentan definir su PIN, ENTONCES el sistema responde `403` y no guarda nada. *(Actualizado el 2026-11-13: en el piloto no hay operación para cambiar el PIN.)*
- [NUEVO] **Borde:** DADO QUE el apoderado define el PIN de su hijo, CUANDO ingresa `000`, ENTONCES el sistema lo acepta, porque tiene exactamente 3 dígitos numéricos. (RN-004)
- [NUEVO] **Negativo:** DADO QUE el apoderado define el PIN de su hijo, CUANDO ingresa `7a2`, ENTONCES el sistema rechaza el PIN y no guarda el registro. (RN-004: solo dígitos numéricos)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el apoderado registra a su hijo, CUANDO ingresa el DNI `0000001` (7 dígitos), ENTONCES el sistema no guarda el registro e indica "DNI inválido".
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el DNI `00000010` ya está registrado como alumno de otra familia, CUANDO el apoderado de "Familia Prueba" intenta registrar a un hijo con ese DNI, ENTONCES el sistema no lo registra e indica que consulte con la secretaría del colegio.

**Reglas de negocio relacionadas:** RN-001, RN-004, RN-005, RN-078

**Notas:** DNI del alumno cifrado con AES-256-GCM e índice ciego HMAC-SHA256; PIN del alumno como hash Argon2id (PRINCIPIO-008). El público objetivo es de 8 a 14 años, pero las reglas no piden validar la edad, porque no se guarda la fecha de nacimiento (RN-001).

**Observaciones QA:**
- **INVEST:** cumple.
- **Corrección sugerida a c1:** usar para el alumno un DNI distinto del de su apoderado (por ejemplo, `00000010`). **Aplicada el 2026-11-11.**
- **Brechas:**
  - No hay regla de formato para el DNI del alumno (RN-052 solo aplica al apoderado).
  - No está definido si un DNI de alumno puede repetirse entre familias.
  - No hay un máximo de hijos por familia.
  - No está definido si una familia `INACTIVO` puede registrar hijos.
- **Constitución:** c4 cumple el PRINCIPIO-007.
- **Cobertura:** positivo (c1), negativo (c2, c4, c5 y nuevos) y borde (c3 y nuevos).

---

## Épica 4 — Tags

---

### HU-009: Grabar y vincular un tag a un hijo

**Prioridad:** Must
**Funcionalidad del alcance:** M2
**Estimación:** 5 puntos

Como **apoderado**, quiero grabar desde la app un tag NFC en blanco y vincularlo a mi hijo, para que pueda comprar en la tienda sin efectivo ni celular.

**Criterios de aceptación:**
- DADO QUE la secretaria registró a "Familia Prueba" como `ACTIVO` el 2027-03-01 y el apoderado tiene un tag NTAG213 en blanco, CUANDO lo acerca a su celular y elige a "Alumno Prueba", ENTONCES la app graba en un registro NDEF solo un token UUID v4 (por ejemplo, `luquita:tag:9f1c2e7a-4b3d-4e8f-a1c6-2d5e7f9b0a13`), bloquea el tag contra escritura y lo vincula al alumno.
- DADO QUE el tag ya se vinculó, CUANDO se intenta reescribirlo con otra app NFC, ENTONCES la escritura falla.
- DADO QUE el tag se vinculó, CUANDO se lee su contenido, ENTONCES no contiene DNI, nombre, montos ni identificadores secuenciales como `alumno:152`.
- DADO QUE la familia del apoderado está en estado `INACTIVO`, CUANDO intenta vincular un tag a su hijo, ENTONCES el sistema no permite la vinculación.
- [NUEVO] **Borde:** DADO QUE el apoderado tiene un tag NTAG215 en blanco, CUANDO lo acerca y elige a "Alumno Prueba", ENTONCES la app lo graba, lo bloquea y lo vincula igual que a un NTAG213. (RN-009 admite los dos tipos)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la app está grabando el token, CUANDO el apoderado aleja el tag antes de terminar, ENTONCES la app indica que la grabación no terminó, el tag no queda vinculado ni bloqueado contra escritura, y el apoderado puede reintentar con el mismo tag.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la app grabó el token en el tag, CUANDO el servidor no responde al registrar la vinculación, ENTONCES la app no bloquea el tag hasta que el servidor confirme, e indica que se debe reintentar. (RN-008 + RN-091: un tag bloqueado con un token desconocido queda inutilizable)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE "Alumno Prueba" pertenece a "Familia Prueba", CUANDO el apoderado de otra familia intenta vincularle un tag, ENTONCES el sistema responde `403` y no lo vincula.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el celular del apoderado no tiene NFC o tiene el NFC apagado, CUANDO inicia una vinculación, ENTONCES la app indica que necesita NFC activo y no continúa.

**Reglas de negocio relacionadas:** RN-006, RN-007, RN-008

**Notas:** requiere que el celular del apoderado tenga NFC con escritura (supuesto del alcance). El bloqueo usa los lock bits del NTAG (PRINCIPIO-006). El reemplazo de tag perdido y la regla de un solo tag activo por alumno (RN-010, RN-011, S3) quedan para el backlog posterior.

**Observaciones QA:**
- **INVEST:** E ⚠️. La estimación depende de definir el orden de las operaciones (registrar el token en el servidor, grabar, confirmar y recién entonces bloquear). Los lock bits son irreversibles: un error aquí desperdicia tags físicos.
- **Valores concretos:** c4 no fija el mensaje ni el código de error.
- **Riesgo de alcance:** sin RN-010 (S3), en el MVP nada impide vincular dos tags al mismo alumno. Confirmar si se acepta ese riesgo o si se rechaza un segundo tag.
- **Constitución:** c1 y c3 cumplen el PRINCIPIO-006.
- **Cobertura:** positivo (c1), negativo (c2, c4 y nuevos) y borde (c3 y nuevos).

---

### HU-010: Rechazar tags no compatibles o ya grabados

**Prioridad:** Must
**Funcionalidad del alcance:** M2
**Estimación:** 2 puntos

Como **apoderado**, quiero que la app rechace un tag que no sirve para Luquita antes de grabarlo, para no vincular a mi hijo un medio de pago que fallará en la tienda.

**Criterios de aceptación:**
- DADO QUE el apoderado inicia una vinculación, CUANDO acerca un tag MIFARE Classic, ENTONCES la app muestra "Este tag no es compatible con Luquita" y no lo vincula.
- DADO QUE el apoderado inicia una vinculación, CUANDO acerca un NTAG215 que ya tiene grabado otro token o datos de otra aplicación, ENTONCES la app rechaza la vinculación y no sobrescribe el tag.
- DADO QUE el apoderado inicia una vinculación, CUANDO acerca un NTAG215 en blanco entregado por el colegio, ENTONCES la app continúa con la grabación descrita en HU-009.
- [NUEVO] **Borde:** DADO QUE el apoderado inicia una vinculación, CUANDO acerca un NTAG216, de la misma familia de chips pero no admitido, ENTONCES la app muestra "Este tag no es compatible con Luquita" y no lo vincula. (RN-009: la lista es cerrada, solo NTAG213 y NTAG215)
- [NUEVO] **Borde:** DADO QUE el apoderado inicia una vinculación, CUANDO acerca un NTAG213 en blanco, ENTONCES la app continúa con la grabación descrita en HU-009. (RN-009)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el apoderado inicia una vinculación, CUANDO acerca un NTAG215 sin datos pero con los lock bits ya activados, ENTONCES la app lo rechaza antes de intentar grabarlo.

**Reglas de negocio relacionadas:** RN-009, RN-091

**Notas:** se puede desarrollar y probar por separado de HU-009, con lecturas NFC simuladas. El texto del mensaje para un tag ya grabado no está definido en las reglas.

**Observaciones QA:**
- **INVEST:** cumple.
- **Valores concretos:** c2 no fija el mensaje. Se propone "Este tag ya fue usado. Pide un tag nuevo en el colegio."
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c3), negativo (c1, c2 y nuevo) y borde [NUEVO].

---

## Épica 5 — Cobro

---

### HU-011: Iniciar sesión como vendedor y cambiar la clave inicial

**Prioridad:** Must
**Funcionalidad del alcance:** M3
**Estimación:** 2 puntos

Como **vendedor (dueño de la tienda)**, quiero iniciar sesión en mi app con mi correo y cambiar la clave que recibí, para empezar a cobrar con una clave que solo yo conozco.

**Criterios de aceptación:**
- DADO QUE el dueño `tienda@colegioprueba.pe` recibió su clave por correo y nunca inició sesión, CUANDO ingresa ese correo y esa clave, ENTONCES la app solo le muestra la pantalla "Cambia tu clave".
- DADO QUE el dueño todavía no cambió la clave recibida, CUANDO intenta registrar una compra de S/ 3.50, ENTONCES el sistema la rechaza y no crea ningún movimiento.
- DADO QUE el dueño cambió su clave, CUANDO inicia sesión con `tienda@colegioprueba.pe` y su nueva clave, ENTONCES accede a la pantalla de cobro.
- DADO QUE el dueño ingresó mal su clave 5 veces y el quinto intento fue a las 2026-11-04T08:00:00-05:00, CUANDO ingresa la clave correcta a las 08:05:00, ENTONCES el acceso sigue bloqueado hasta las 08:15:00.
- [NUEVO] **Borde:** DADO QUE el dueño ingresó mal su clave 4 veces, CUANDO en el quinto intento ingresa la clave correcta, ENTONCES accede a la pantalla de cobro. (RN-029)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE la cuenta se bloqueó a las 2026-11-04T08:00:00-05:00, CUANDO el dueño ingresa la clave correcta a las 08:14:59 y luego a las 08:15:00, ENTONCES el primer intento se rechaza y el segundo accede. (RN-029)
- [NUEVO] **Negativo:** DADO QUE el dueño intenta iniciar sesión, CUANDO usa como usuario `tienda`, distinto del correo registrado, ENTONCES la app no le da acceso. (RN-071)
- [NUEVO] **Negativo:** DADO QUE el dueño cambió su clave inicial, CUANDO se revisan los logs, ENTONCES no contienen la clave recibida, la nueva clave ni el JWT. (PRINCIPIO-008)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE el correo registrado es `tienda@colegioprueba.pe`, CUANDO el dueño escribe `Tienda@ColegioPrueba.pe` con su clave correcta, ENTONCES accede, porque el correo se compara sin distinguir mayúsculas.

**Reglas de negocio relacionadas:** RN-029, RN-071, RN-073

**Notas:** comparte la lógica de credenciales con HU-003, pero es otro actor y otra app (Flutter). La lógica de autenticación la implementa y revisa un humano (PRINCIPIO-009).

**Observaciones QA:**
- **INVEST:** cumple.
- **Valores concretos:** c2 dice "la rechaza" sin código de error (ver Pendiente 3).
- **Brecha:** la política de la nueva clave es la misma que falta en HU-003.
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c1, c3), negativo (c2 y nuevos) y borde (c4 y nuevos).

---

### HU-012: Cobrar una compra con tag y PIN

**Prioridad:** Must
**Funcionalidad del alcance:** M3
**Estimación:** 5 puntos

Como **vendedor**, quiero ingresar el monto, que el alumno acerque su tag y digite su PIN, y que el sistema registre la compra, para cobrar sin manejar efectivo.

**Criterios de aceptación:**
- DADO QUE "Alumno Prueba" tiene un tag activo y el PIN `730`, CUANDO el vendedor ingresa S/ 3.50 con la descripción "Jugo de naranja", el alumno acerca el tag y digita `730` el 2026-11-04T10:15:00-05:00, ENTONCES la pantalla muestra solo "Alumno Prueba" y el PIN como `•••`, y el sistema inserta un movimiento `COMPRA` de +S/ 3.50 con esa descripción.
- DADO QUE el PIN correcto es `730`, CUANDO el alumno digita `731`, ENTONCES el sistema responde `401` y no crea ningún movimiento.
- DADO QUE el vendedor ingresa el monto, CUANDO ingresa S/ 0.00 o S/ 3.505, ENTONCES el sistema rechaza la compra.
- DADO QUE el vendedor no escribe descripción, CUANDO confirma una compra de S/ 2.00, ENTONCES la compra se registra igual; y si escribe una descripción de 61 caracteres o más, ENTONCES el sistema la rechaza.
- DADO QUE dos solicitudes con `Idempotency-Key = a1b2c3` por S/ 3.50 llegan con 1 segundo de diferencia, CUANDO el servidor las procesa, ENTONCES el consumo del día sube solo S/ 3.50 y la segunda devuelve el resultado original.
- DADO QUE la app del vendedor no tiene conexión a internet, CUANDO el alumno acerca el tag, ENTONCES la app muestra "Sin conexión: no se puede cobrar con Luquita", no registra la compra y no la guarda para enviarla después.
- DADO QUE "Familia Prueba" tiene impaga la boleta de octubre de 2026, CUANDO su hijo compra S/ 2.00 en noviembre con tag y PIN válidos, ENTONCES la compra se registra.
- DADO QUE la secretaria pasó la familia a `INACTIVO` el 2027-05-03 y la app del dueño está activa, CUANDO el 2027-05-10 su hijo intenta comprar S/ 2.00, ENTONCES el sistema responde `422 FAMILIA_INACTIVA`.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la app del dueño del "Colegio Prueba" no está activa, CUANDO llega una compra de S/ 2.00 con tag y PIN, ENTONCES el sistema responde `409 VENTAS_NO_HABILITADAS`, no verifica el PIN (el contador de fallos no cambia) y no crea ningún movimiento. (RN-097)
- [NUEVO] **Negativo:** DADO QUE "Alumno Prueba" tiene un tag activo, CUANDO llega una solicitud de compra de S/ 2.00 con el token del tag pero sin PIN, ENTONCES el sistema responde `401` y no crea ningún movimiento. (RN-012, PRINCIPIO-006)
- [NUEVO] **Borde:** DADO QUE el alumno no tiene consumo en el día, CUANDO el vendedor registra una compra de S/ 0.01 con tag y PIN válidos, ENTONCES la compra se registra. (RN-014, RN-082: el mínimo válido)
- [NUEVO] **Negativo:** DADO QUE el vendedor ingresa el monto, CUANDO llega una solicitud por −S/ 3.50, ENTONCES el sistema rechaza la compra y no crea ningún movimiento. (RN-014)
- [NUEVO] **Borde:** DADO QUE el vendedor escribe una descripción de exactamente 60 caracteres que incluye tildes y ñ (por ejemplo, "Ñ" cuenta como un carácter aunque ocupe 2 bytes), CUANDO confirma la compra, ENTONCES la compra se registra con la descripción completa. (RN-050: el límite es en caracteres, no en bytes)
- [NUEVO] **Negativo:** DADO QUE el token `luquita:tag:00000000-0000-4000-8000-000000000000` no pertenece a ningún alumno, CUANDO llega una compra con ese token, ENTONCES el sistema la rechaza, no crea ningún movimiento y la pantalla no muestra ningún nombre. (PRINCIPIO-006: el servidor resuelve al alumno a partir del token)
- [NUEVO] **Negativo:** DADO QUE llega una solicitud de compra de S/ 2.00 con tag y PIN válidos, CUANDO no incluye `Idempotency-Key`, ENTONCES el sistema la rechaza sin crear ningún movimiento. (PRINCIPIO-004: cada operación lleva una clave de idempotencia)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE ya se procesó una compra de S/ 3.50 con `Idempotency-Key = a1b2c3`, CUANDO llega otra solicitud con la misma clave por S/ 5.00, ENTONCES el sistema la rechaza y no crea un movimiento de S/ 5.00.
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE el servidor registró una compra de S/ 3.50, CUANDO la app del vendedor pierde la conexión antes de recibir la respuesta, ENTONCES la app no la muestra como cobrada ni como rechazada, sino como "No se pudo confirmar el cobro", y un reintento de esa misma operación reutiliza la misma `Idempotency-Key`, sin duplicar el consumo. (RN-015 frente a RN-090)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el alumno digitó `731` (el PIN correcto es `730`), el servidor respondió `401 PIN_INVALIDO` con `Idempotency-Key = d4e5f6` y el contador de fallos quedó en 1, CUANDO la respuesta se pierde y el vendedor toca «Reintentar» con la misma clave y el mismo contenido, ENTONCES el servidor devuelve el mismo `401` sin volver a verificar el PIN y el contador sigue en 1. (RN-015, RN-027; auditoría M-01)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE una compra de S/ 5.00 se rechazó con `422 LIMITE_DIARIO_EXCEDIDO` usando `Idempotency-Key = g7h8i9`, CUANDO se reintenta con la misma clave, ENTONCES el servidor devuelve el mismo `422` y no recalcula los límites. (RN-015)
- [NUEVO] [CONFIRMADO] **Positivo:** DADO QUE el servidor respondió `401 PIN_INVALIDO`, CUANDO el alumno vuelve a digitar el PIN, ENTONCES la app envía una `Idempotency-Key` nueva, porque es un intento nuevo; con `730` la compra se registra. (RN-015; reutilizar la clave con otro PIN responde `409 IDEMPOTENCY_KEY_REUTILIZADA`)
- [NUEVO] [CONFIRMADO] **Positivo:** DADO QUE el servidor respondió `401 PIN_INVALIDO` a la clave `d4e5f6` y la respuesta se perdió, CUANDO el vendedor toca «Verificar», ENTONCES recibe el estado `RECHAZADA` con el código `PIN_INVALIDO` y la app muestra «PIN incorrecto». (RN-015, ADR-002)

**Reglas de negocio relacionadas:** RN-012, RN-013, RN-014, RN-015, RN-021, RN-040, RN-041, RN-048, RN-049, RN-050, RN-056, RN-082, RN-089, RN-090, RN-097

**Notas:** montos en `BigDecimal` escala 2 en Java, `NUMERIC(12,2)` en PostgreSQL y céntimos en Flutter; prohibido `float` o `double` (PRINCIPIO-004). La tabla `movimiento` es de solo inserción. El log de la compra solo registra `tagId`, resultado y monto (RN-030). La anulación de compras (S1) queda fuera del MVP: riesgo aceptado en el alcance. Es la HU más grande del MVP; si al refinarla supera 5 puntos, se divide en "registro de la compra" y "robustez (idempotencia y sin conexión)".

**Observaciones QA:**
- **INVEST:** S ❌. Con 16 criterios supera los 5 puntos. Se recomienda aplicar la división que la propia nota prevé:
  - *(Decisión del 2026-11-13: no se divide; el backlog la traza completa. La propuesta original queda como referencia.)*
  - **HU-012A "Registrar la compra con tag y PIN"** (c1 a c4, c7, c8 y los nuevos de PIN, monto, descripción y token): unos 3 puntos.
  - **HU-012B "Robustez del cobro"** (c5, c6 y los nuevos de idempotencia y respuesta perdida): unos 3 puntos.
- **Valores concretos:** no tienen código de error el monto S/ 0.00 o S/ 3.505 (c3), la descripción de 61 caracteres (c4), el token desconocido ni la falta de `Idempotency-Key`. Fijarlos en el OpenAPI.
- **Riesgo:** el caso de la respuesta perdida es el escenario de doble cobro más probable en una tienda escolar con wifi inestable (ver hallazgo 2).
- **Constitución:** c1 cumple los PRINCIPIOS 007 y 008. Sin contradicciones.
- **Cobertura:** positivo (c1, c7), negativo (c2, c3, c6, c8 y nuevos) y borde (c4, c5 y nuevos).

---

### HU-013: Respetar los límites de consumo diario y mensual

**Prioridad:** Must
**Funcionalidad del alcance:** M3
**Estimación:** 3 puntos

Como **apoderado**, quiero que el sistema rechace las compras que superen S/ 30.00 en el día o el tope mensual de mi hijo, para controlar cuánto gasta.

**Criterios de aceptación:**
- DADO QUE el alumno consumió S/ 27.00 el 2026-11-04, CUANDO intenta comprar S/ 5.00 ese día, ENTONCES el sistema responde `422 LIMITE_DIARIO_EXCEDIDO` y no crea ningún movimiento.
- DADO QUE el alumno consumió S/ 27.00 el 2026-11-04, CUANDO compra S/ 3.00 ese día, ENTONCES la compra se registra y el consumo del día queda exactamente en S/ 30.00.
- DADO QUE el alumno acumula S/ 298.00 en noviembre de 2026 y tiene el tope por defecto de S/ 300.00, CUANDO intenta comprar S/ 4.00, ENTONCES el sistema responde `422 LIMITE_MENSUAL_EXCEDIDO`.
- DADO QUE el alumno consumió S/ 28.00 el 2026-11-04, CUANDO compra S/ 5.00 a las 2026-11-05T00:01:00-05:00, ENTONCES la compra se registra porque cuenta para el 2026-11-05 en hora de Lima.
- DADO QUE el alumno ya consumió S/ 25.00 en el día, CUANDO llegan dos compras simultáneas de S/ 4.00, ENTONCES la primera se registra y la segunda se rechaza con `422 LIMITE_DIARIO_EXCEDIDO`.
- DADO QUE una app modificada envía una compra sin hacer ninguna validación local, CUANDO esa compra dejaría el día en S/ 33.00, ENTONCES el servidor la rechaza con `422 LIMITE_DIARIO_EXCEDIDO`.
- [NUEVO] **Borde:** DADO QUE el alumno acumula S/ 296.00 en noviembre de 2026 y no tiene consumo el 2026-11-20, CUANDO compra S/ 4.00 ese día, ENTONCES la compra se registra y el consumo del mes queda exactamente en S/ 300.00. (RN-018: "supera" es estrictamente mayor)
- [NUEVO] **Borde:** DADO QUE el alumno no tiene consumo en el día, CUANDO intenta una compra de S/ 30.01, ENTONCES el sistema responde `422 LIMITE_DIARIO_EXCEDIDO`. (RN-017)
- [NUEVO] **Borde:** DADO QUE el alumno consumió S/ 28.00 el 2026-11-04, CUANDO intenta comprar S/ 5.00 a las 2026-11-04T23:59:59-05:00 y otra vez a las 2026-11-05T00:00:00-05:00, ENTONCES la primera se rechaza con `422 LIMITE_DIARIO_EXCEDIDO` y la segunda se registra. (RN-019)
- [NUEVO] **Borde:** DADO QUE el alumno acumula S/ 300.00 en octubre de 2026, CUANDO compra S/ 2.00 a las 2026-11-01T00:00:00-05:00, ENTONCES la compra se registra porque cuenta para noviembre. (RN-018, RN-019; meses cambiados el 2026-11-11 porque en diciembre no hay ventas)
- [NUEVO] **Negativo:** DADO QUE el alumno consumió S/ 27.00 el 2026-11-04 y una compra de S/ 5.00 fue rechazada con `LIMITE_DIARIO_EXCEDIDO`, CUANDO compra S/ 3.00 ese mismo día, ENTONCES la compra se registra y el consumo del día queda en S/ 30.00, porque la compra rechazada no suma. (RN-017; PRINCIPIO-004: el consumo es la suma de los movimientos)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el alumno acumula S/ 298.00 en el mes y S/ 28.00 en el día, CUANDO intenta comprar S/ 5.00, ENTONCES el sistema responde `422 LIMITE_DIARIO_EXCEDIDO`, porque el límite diario se valida primero, y no crea ningún movimiento.

**Reglas de negocio relacionadas:** RN-017, RN-018, RN-019, RN-020, RN-078, RN-083

**Notas:** los límites se validan en el servidor dentro de la misma transacción que inserta el movimiento (PRINCIPIO-005). "Supera" significa estrictamente mayor: llegar a S/ 30.00 está permitido. El consumo se calcula con los movimientos `COMPRA` (y `ANULACION` cuando exista S1); los `AJUSTE` no cuentan (RN-080). Cambiar los valores de los límites requiere aprobación humana (PRINCIPIO-009).

**Observaciones QA:**
- **INVEST:** cumple.
- **Brecha:** no se define qué código devuelve una compra que excede los dos límites a la vez. Se propone validar primero el día; el PO debe confirmarlo.
- **Prueba:** c5 (concurrencia) debe probarse en integración contra PostgreSQL real con transacciones simultáneas, no con mocks.
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c2), negativo (c1, c3, c6 y nuevos) y borde (c4, c5 y nuevos).

---

### HU-014: Bloquear el tag tras 3 PIN fallidos

**Prioridad:** Must
**Funcionalidad del alcance:** M3
**Estimación:** 2 puntos

Como **apoderado**, quiero que el tag de mi hijo se bloquee tras 3 PIN incorrectos seguidos, para que nadie pueda adivinar el PIN con un tag perdido.

**Criterios de aceptación:**
- DADO QUE el PIN correcto de "Alumno Prueba" es `730`, CUANDO se digitan seguidos `111`, `222` y `333`, ENTONCES el tag queda bloqueado tras el tercer intento y una cuarta solicitud con `730` se rechaza sin crear movimiento.
- DADO QUE se digitaron mal 2 PIN seguidos, CUANDO el tercer intento usa el PIN correcto `730` por S/ 2.00, ENTONCES la compra se registra y el contador de intentos fallidos vuelve a cero.
- DADO QUE ocurrieron 3 intentos fallidos, CUANDO se revisa el log, ENTONCES el registro no contiene ninguno de los PIN digitados.
- [NUEVO] **Borde:** DADO QUE se digitaron `111` y `222`, luego `730` (compra registrada) y luego `333` y `444`, CUANDO se revisa el estado del tag, ENTONCES sigue activo, porque no hubo 3 fallos consecutivos. (RN-027)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE se digitaron mal 2 PIN el 2026-11-04, CUANDO el 2026-11-05 se digita mal un tercer PIN, ENTONCES el tag queda bloqueado, porque RN-027 no fija una ventana de tiempo.
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE se digitaron mal 2 PIN y el alumno ya consumió S/ 30.00 en el día, CUANDO digita el PIN correcto `730` para una compra de S/ 2.00, ENTONCES la compra se rechaza con `422 LIMITE_DIARIO_EXCEDIDO` y el contador de fallos vuelve a cero, porque el PIN fue correcto.

**Reglas de negocio relacionadas:** RN-013, RN-027, RN-030

**Notas:** solo el apoderado puede desbloquear el tag (HU-017). El reinicio del contador tras un PIN correcto se deduce de "3 veces consecutivas" (RN-027). Mitiga el riesgo R2 ("mi hijo no compró eso").

**Observaciones QA:**
- **INVEST:** cumple.
- **Valores concretos:** c1 no fija el código de la cuarta solicitud (tag bloqueado; ver Pendiente 3).
- **Brecha:** cuando el tag se bloquea por PIN fallidos, nada avisa al apoderado. El alumno se queda sin poder comprar hasta que alguien le diga al apoderado que lo desbloquee. Ver HU-024 sugerida.
- **Constitución:** sin contradicciones.
- **Cobertura:** positivo (c2), negativo (c1, c3) y borde [NUEVO].

---

## Épica 6 — Control parental

---

### HU-015: Recibir un correo por cada compra

**Prioridad:** Must
**Funcionalidad del alcance:** M4
**Estimación:** 3 puntos

Como **apoderado**, quiero recibir un correo por cada compra de mi hijo con el monto y el consumo acumulado del mes, para saber al momento en qué gasta.

**Criterios de aceptación:**
- DADO QUE el colegio registró el correo `apoderado@ejemplo.pe`, y "Alumno Prueba" ya consumió S/ 85.00 en noviembre, CUANDO compra S/ 3.50 el 2026-11-04T10:15:00-05:00, ENTONCES ese correo recibe "Compra en Tienda del colegio — 2026-11-04 10:15 — S/ 3.50 — Consumo de noviembre: S/ 88.50".
- DADO QUE el primer envío falla a las 10:15:02, CUANDO el sistema reintenta, ENTONCES hace hasta 3 reintentos con 60 segundos entre cada uno (10:16:02, 10:17:02 y 10:18:02).
- DADO QUE el correo se envió en el segundo intento, CUANDO se consulta el registro del envío, ENTONCES muestra "intento 1: fallido; intento 2: enviado".
- DADO QUE los 3 reintentos fallan, CUANDO se consulta la compra, ENTONCES la compra de S/ 3.50 sigue registrada.
- [NUEVO] **Borde:** DADO QUE "Alumno Prueba" no tiene compras en octubre de 2026, CUANDO compra S/ 3.50 el 2026-10-01T08:00:00-05:00, ENTONCES el correo dice "Consumo de octubre: S/ 3.50". (RN-023; mes cambiado el 2026-11-11 porque en diciembre no hay ventas)
- [NUEVO] **Negativo:** DADO QUE una compra de S/ 5.00 se rechaza con `422 LIMITE_DIARIO_EXCEDIDO`, CUANDO termina la solicitud, ENTONCES no se envía ningún correo al apoderado. (RN-023: solo cuando la compra queda registrada)
- [NUEVO] **Negativo:** DADO QUE dos solicitudes con `Idempotency-Key = a1b2c3` por S/ 3.50 llegan con 1 segundo de diferencia, CUANDO el servidor las procesa, ENTONCES el apoderado recibe un solo correo. (RN-015, RN-023)
- [NUEVO] **Borde:** DADO QUE "Alumno Prueba" compró S/ 3.50 el 2026-11-30T23:59:30-05:00 y el primer envío falló, CUANDO el correo se envía en el reintento del 2026-12-01T00:00:30-05:00, ENTONCES dice "2026-11-30 23:59" y "Consumo de noviembre". (RN-023, RN-063: el consumo es el del mes de la compra)
- [NUEVO] **Borde:** DADO QUE el intento inicial y los 3 reintentos fallaron, CUANDO pasan las 10:19:02, ENTONCES no hay un quinto intento y el registro muestra 4 intentos fallidos. (RN-024, RN-084)

**Reglas de negocio relacionadas:** RN-023, RN-024, RN-025, RN-061, RN-084

**Notas:** el envío es asíncrono y no forma parte de la transacción de la compra. El registro de intentos no guarda PIN, DNI ni tokens (PRINCIPIO-008). El proveedor de correo está por definir y debe desplegarse en Azure con Spring Boot (PRINCIPIO-002).

**Observaciones QA:**
- **INVEST:** cumple.
- **Corrección sugerida a c1:** agregar la precondición "con S/ 85.00 ya consumidos en noviembre". Sin ella, el valor "S/ 88.50" no se puede verificar.
- **Brecha:** RN-024 dice "reintentarlo hasta 3 veces". QA lo interpreta como 1 intento + 3 reintentos (4 en total), en línea con c2. El PO debe confirmar esta lectura.
- **Constitución:** cumple el PRINCIPIO-005.
- **Cobertura:** positivo (c1, c3), negativo (c4 y nuevos) y borde (c2 y nuevos).

---

### HU-016: Bloquear el tag de un hijo

**Prioridad:** Must
**Funcionalidad del alcance:** M4
**Estimación:** 2 puntos

Como **apoderado**, quiero bloquear al instante desde la app el tag de mi hijo, para que nadie compre con él si se pierde.

**Criterios de aceptación:**
- DADO QUE el tag de "Alumno Prueba" está activo, CUANDO el apoderado lo bloquea a las 2026-11-04T10:00:00-05:00, ENTONCES una compra con ese tag a las 10:00:01 se rechaza sin crear movimiento.
- DADO QUE el tag está bloqueado desde las 09:00:00, CUANDO se intenta una compra de S/ 5.00 a las 10:15:00 con el PIN correcto, ENTONCES el sistema la rechaza.
- DADO QUE el servidor recibe una compra, CUANDO valida el tag, ENTONCES consulta su estado actual en la base de datos y no usa un estado guardado en caché.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE "Alumno Prueba" pertenece a "Familia Prueba", CUANDO el apoderado de otra familia intenta bloquear su tag, ENTONCES el sistema responde `403` y el tag sigue activo.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la app del apoderado no tiene conexión, CUANDO toca "Bloquear tag", ENTONCES la app indica claramente que el tag NO se bloqueó, y una compra a las 10:05:00 con PIN correcto se registra.
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE el tag ya está bloqueado, CUANDO el apoderado vuelve a bloquearlo, ENTONCES el tag sigue bloqueado y la app no muestra un error.

**Reglas de negocio relacionadas:** RN-016, RN-026, RN-085

**Notas:** las reglas no definen el código de error de una compra con tag bloqueado (por ejemplo, `TAG_BLOQUEADO`); se debe fijar en la especificación OpenAPI. El botón "Bloquear tag" lleva `Semantics(label: 'Bloquear tag de Alumno Prueba')` (PRINCIPIO-011).

**Observaciones QA:**
- **INVEST:** cumple.
- **Brecha:** ninguna regla dice quién puede bloquear un tag. RN-028 solo cubre el desbloqueo.
- **Riesgo:** si el apoderado cree que bloqueó el tag y no fue así, ese es el peor resultado para esta HU. Por eso el caso sin conexión debe mostrar un resultado inequívoco.
- **Constitución:** c3 cumple el PRINCIPIO-005.
- **Cobertura:** positivo (c1), negativo (c2 y nuevos) y borde (c1 en el segundo exacto y el nuevo).

---

### HU-017: Desbloquear el tag de un hijo

**Prioridad:** Must
**Funcionalidad del alcance:** M4
**Estimación:** 2 puntos

Como **apoderado**, quiero desbloquear desde la app el tag de mi hijo, para que vuelva a comprar después de un bloqueo por PIN fallidos o de recuperar el tag.

**Criterios de aceptación:**
- DADO QUE el tag de "Alumno Prueba" se bloqueó por 3 PIN fallidos, CUANDO el apoderado lo desbloquea desde su app, ENTONCES la siguiente compra de S/ 2.00 con el PIN `730` se registra.
- DADO QUE el tag está bloqueado por PIN fallidos, CUANDO el vendedor intenta desbloquearlo desde su app, ENTONCES el sistema responde `403` y el tag sigue bloqueado.
- DADO QUE el tag de "Alumno Prueba" está bloqueado, CUANDO el apoderado de otra familia intenta desbloquearlo, ENTONCES el sistema responde `403`.
- [NUEVO] **Positivo:** DADO QUE el apoderado bloqueó a mano el tag de "Alumno Prueba" a las 2026-11-04T09:00:00-05:00, CUANDO lo desbloquea a las 12:00:00, ENTONCES una compra de S/ 2.00 con el PIN `730` a las 12:00:05 se registra. (glosario, "Desbloqueo de tag")
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE el apoderado desbloqueó un tag bloqueado por 3 PIN fallidos, CUANDO el alumno digita una vez el PIN incorrecto `111`, ENTONCES el sistema responde `401` y el tag sigue activo, porque el contador volvió a cero.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el apoderado bloqueó a mano el tag, CUANDO el vendedor intenta desbloquearlo, ENTONCES el sistema responde `403` y el tag sigue bloqueado.

**Reglas de negocio relacionadas:** RN-027, RN-028

**Notas:** el desbloqueo aplica también a un tag que el apoderado bloqueó a mano (glosario, "Desbloqueo de tag").

**Observaciones QA:**
- **INVEST:** cumple.
- **Brechas:** RN-028 solo protege el bloqueo por PIN fallidos. Falta una regla que limite al apoderado el desbloqueo de un tag bloqueado a mano. Tampoco se define si el desbloqueo reinicia el contador de fallos.
- **Constitución:** cumple el PRINCIPIO-008 ("solo el apoderado puede desbloquearlo").
- **Cobertura:** positivo (c1 y nuevo), negativo (c2, c3 y nuevo) y borde [NUEVO].

---

## Épica 7 — Consolidado mensual y app del dueño

---

### HU-018: Obtener el consolidado mensual por familia

**Prioridad:** Must
**Funcionalidad del alcance:** M5
**Estimación:** 3 puntos *(antes 5; simplificada el 2026-11-13: sin cierre automático ni suscripción)*

Como **secretaria del colegio**, quiero obtener en el panel web el consolidado de cada familia para el mes que elija, para incluir el consumo en la boleta del colegio.

**Criterios de aceptación:**
- DADO QUE "Familia Prueba" tiene dos hijos con compras en noviembre de 2026, CUANDO la secretaria consulta el consolidado de noviembre, ENTONCES ve "Alumno Prueba 1: S/ 142.00 — Alumno Prueba 2: S/ 98.50 — Total familia: S/ 240.50", sin ningún cargo de suscripción. (RN-031)
- DADO QUE el consolidado se muestra, CUANDO la secretaria lo revisa, ENTONCES muestra el nombre, el DNI y el total de cada alumno, y no lista compras individuales como "2026-11-04 10:15 — S/ 3.50". (RN-033)
- DADO QUE una compra se registró el 2027-03-31T23:30:00-05:00 (2027-04-01T04:30:00Z), CUANDO se consultan los consolidados, ENTONCES la compra aparece en el de marzo de 2027 y no en el de abril. (RN-063)
- [CONFIRMADO] **Negativo:** DADO QUE la secretaria ya obtuvo el consolidado de octubre de 2026, CUANDO una app envía el 2026-11-04 una compra con fecha 2026-10-15, ENTONCES la compra se registra con la fecha del servidor (2026-11-04) y el consolidado de octubre sigue igual. (RN-034; reemplaza el criterio del periodo cerrado, 2026-11-13)
- [NUEVO] **Borde:** DADO QUE "Familia Prueba" está `ACTIVO` y su hijo no compró nada en noviembre de 2026, CUANDO la secretaria consulta el consolidado, ENTONCES ve "Alumno Prueba: S/ 0.00". (RN-031)
- [NUEVO] **Borde:** DADO QUE una compra se registró el 2026-11-30T23:59:58-05:00 y otra el 2026-12-01T00:00:00-05:00, CUANDO se consultan los consolidados, ENTONCES la primera aparece en noviembre y la segunda en diciembre de 2026. (RN-063)
- [NUEVO] **Negativo:** DADO QUE noviembre de 2026 terminó, CUANDO el dueño de la tienda solicita el consolidado de "Familia Prueba", ENTONCES el sistema responde `403` y no le muestra el consumo por alumno. (PRINCIPIO-007; RN-042: el dueño no ve el consumo de cada alumno)
- [CONFIRMADO] **Borde:** DADO QUE es el 2026-11-15, CUANDO la secretaria consulta el consolidado de noviembre de 2026, ENTONCES ve los montos hasta ese momento con el aviso "Mes en curso: los totales pueden cambiar". (decisión del equipo 2026-11-13)
- [CONFIRMADO] **Negativo:** DADO QUE es noviembre de 2026, CUANDO la secretaria pide el consolidado de diciembre de 2026, ENTONCES el panel indica que el mes todavía no empieza y no muestra montos.
- ~~Criterios de cierre automático, suscripción, periodo cerrado, compra en el segundo del cierre, cierre de noviembre y apertura de periodos~~ *(retirados el 2026-11-13)*.

**Reglas de negocio relacionadas:** RN-031, RN-033, RN-034, RN-063

**Notas:** no hay proceso de cierre: el consolidado se calcula al consultarlo, sumando los movimientos de cada alumno en el mes calendario (hora de Lima). Como cada movimiento lleva la fecha del servidor, un mes ya entregado al colegio no cambia. En el MVP el consolidado solo suma movimientos `COMPRA`; cuando exista S1 sumará también `ANULACION` y `AJUSTE` (RN-081). El formato de descarga del consolidado (Excel o PDF) no está definido.

**Observaciones QA:**
- **INVEST:** cumple (ya no hace falta dividirla en cierre y consulta).
- **Brechas:**
  - No se define si una familia `INACTIVO` todo el mes y sin consumo aparece en el consolidado (con S/ 0.00) o se omite.
  - El formato de descarga sigue pendiente (Pendiente 5).
- **Riesgo aceptado:** si en el futuro se agregan anulaciones o ajustes (S1), habrá que volver a "congelar" los meses ya entregados al colegio.
- **Constitución:** c2 cumple el PRINCIPIO-007 y c1 el PRINCIPIO-003 (versión 1.17).
- **Cobertura:** positivo (c1), negativo (c4 y nuevos) y borde (c3 y nuevos).

---

### HU-033: Activar y desactivar la app del dueño de la tienda

**Prioridad:** Must
**Funcionalidad del alcance:** M5
**Estimación:** 2 puntos *(antes 3; simplificada el 2026-11-13: sin temporada ni periodos)*
**Origen:** decisiones del equipo del 2026-11-11 y 2026-11-13

Como **secretaria del colegio o administrador de Luquita**, quiero activar o desactivar en el panel web la app del dueño de la tienda, para decidir cuándo los alumnos pueden comprar con Luquita.

**Criterios de aceptación:**
- [CONFIRMADO] **Positivo:** DADO QUE la app del dueño del "Colegio Prueba" está desactivada, CUANDO la secretaria la activa el 2027-03-01T07:30:00-05:00, ENTONCES una compra válida de S/ 2.00 a las 10:00:00 se registra. (RN-097)
- [CONFIRMADO] **Positivo:** DADO QUE la app del dueño está desactivada, CUANDO el administrador de Luquita la activa para el "Colegio Prueba", ENTONCES queda activa. (RN-101)
- [CONFIRMADO] **Negativo:** DADO QUE el dueño de la tienda está autenticado, CUANDO intenta activar o desactivar su app, ENTONCES el sistema responde `403` y no aplica el cambio. (RN-101)
- [CONFIRMADO] **Negativo:** DADO QUE la secretaria desactivó la app del dueño el 2027-07-12T08:00:00-05:00, CUANDO llega una compra a las 08:00:05, ENTONCES el sistema responde `409 VENTAS_NO_HABILITADAS`, no verifica el PIN y no crea ningún movimiento. (RN-097, RN-102)
- [CONFIRMADO] **Borde:** DADO QUE la app del dueño está desactivada, CUANDO el dueño la abre, ENTONCES ve "La app de cobro no está activa. Consulta con la secretaría del colegio." y no puede iniciar un cobro. (RN-103)
- [CONFIRMADO] **Borde:** DADO QUE la app del dueño ya está activa, CUANDO la secretaria vuelve a activarla, ENTONCES el sistema responde con el mismo estado, sin error.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la secretaria pertenece al "Colegio Prueba", CUANDO intenta activar la app del dueño de otro colegio, ENTONCES el sistema responde `403`.

**Reglas de negocio relacionadas:** RN-097, RN-101, RN-102, RN-103

**Notas:** es un interruptor sin fechas: no hay temporada ni periodos. Cambia la lógica de compra: requiere aprobación humana (PRINCIPIO-009). El nombre técnico sigue siendo `ventas_habilitadas` y el código de error `VENTAS_NO_HABILITADAS`.

**Observaciones QA:**
- **INVEST:** cumple. Depende de HU-001 (colegio) y HU-003 (sesión de la secretaria).
- **Constitución:** cumple el PRINCIPIO-003 (versión 1.17).
- **Cobertura:** positivo (2), negativo (3) y borde (2).

---

## ~~Épica 8 — Reclamaciones~~ (retirada el 2026-11-11)

HU-019 "Registrar un reclamo en el Libro de Reclamaciones virtual" y HU-020 "Responder un reclamo" salen del alcance del piloto por decisión del equipo (constitución 1.16). Con ellas se retiran RN-043, RN-044 y RN-092 a RN-095 y el `codigoCompra` del correo de cada compra. Sus números no se reutilizan.

---

## Verificación INVEST (revisión QA)

| HU | I | N | V | E | S | T | Observación QA |
|---|---|---|---|---|---|---|---|
| HU-001 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Confirmar si el nombre del colegio es único |
| HU-002 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Falta el formato del DNI de la secretaria y el manejo de la falla del correo |
| HU-003 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Falta la política de la nueva clave |
| HU-004 | ✅ | ✅ | ✅ | ⚠️ | ✅ | ✅ | Ceros iniciales (DNI duplicado y año escolar ya resueltos) |
| HU-005 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Confirmar el cambio manual de estado |
| HU-006 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Definir qué pasa con la cuenta si la familia pasa a `INACTIVO` |
| HU-007 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Definir el reinicio del contador de fallos |
| HU-008 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Falta el formato y la unicidad del DNI del alumno; cambiar el DNI del fixture |
| HU-009 | ✅ | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | Definir el orden de grabación y bloqueo; probar con hardware real además de las simulaciones |
| HU-010 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — |
| HU-011 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Igual que HU-003 |
| HU-012 | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ | No se divide (decisión del 2026-11-13); se estima como una HU grande |
| HU-013 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Definir el código cuando se exceden los dos límites |
| HU-014 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Falta el aviso al apoderado (HU-024) |
| HU-015 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Agregar la precondición de c1 |
| HU-016 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Falta definir quién puede bloquear |
| HU-017 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Falta proteger el desbloqueo de un tag bloqueado a mano |
| HU-018 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Simplificada el 2026-11-13: solo consulta, sin cierre |
| HU-033 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Simplificada el 2026-11-13: interruptor sin fechas |
| ~~HU-019, HU-020~~ | — | — | — | — | — | — | Retiradas el 2026-11-11 |

---

## Contradicciones y brechas frente a la constitución

**Contradicciones:** ningún criterio, original o nuevo, contradice la constitución 1.16.

**Brechas (la regla es más débil que la constitución, o la constitución no tiene regla ni HU):**

| # | Brecha | Principio | Efecto | Propuesta |
|---|---|---|---|---|
| 1 | ~~RN-030 solo prohíbe PIN y DNI en los logs~~ | PRINCIPIO-008 | — | **Resuelto (2026-11-13):** RN-030 ampliada |
| 2 | Cambio del PIN del alumno sin flujo | PRINCIPIO-008 | — | **Riesgo aceptado (2026-11-13):** no hay cambio de PIN en el piloto; RN-005 queda para «definir» |
| 3 | No hay recuperación de credenciales en la app | PRINCIPIO-008 | — | **Riesgo aceptado (2026-11-13):** procedimiento manual del equipo de Luquita (arquitectura §6.7) |
| 4 | ~~RN-015 exige idempotencia solo en compras~~ | PRINCIPIO-004 | — | **Resuelto:** todo `POST` de negocio lleva `Idempotency-Key` (arquitectura §4.0) |
| 5 | ~~RN-090 frente a RN-015~~ | PRINCIPIO-004 y PRINCIPIO-005 | — | **Resuelto:** ADR-002 (cobro sin confirmar) y RN-015 ampliada |
| 6 | ~~El ejemplo de RN-047 ("DNI duplicado") frente a RN-054 (actualizar)~~ | PRINCIPIO-003 | — | **Resuelto (2026-11-11):** RN-098 y RN-054 acotada |
| 7 | ~~Quién bloquea y desbloquea un tag~~ | PRINCIPIO-005 y PRINCIPIO-008 | — | **Resuelto (2026-11-13):** solo el apoderado de la familia (RN-028 ampliada) |

---

## Trazabilidad: funcionalidades Must → HU

| Funcionalidad | Descripción | HU que la cubren | Puntos |
|---|---|---|---|
| M1 | Registro de familia y alumnos | HU-006, HU-007, HU-008 | 8 |
| M2 | Vinculación del tag NFC | HU-009, HU-010 | 7 |
| M3 | Cobro con tag + PIN | HU-011, HU-012, HU-013, HU-014 | 12 |
| M4 | Control parental | HU-015, HU-016, HU-017 | 7 |
| M5 | Panel web del colegio y de administración | HU-001, HU-002, HU-003, HU-004, HU-005, HU-018, HU-033 | 20 |
| ~~M6~~ | ~~Libro de Reclamaciones virtual~~ (retirado el 2026-11-11) | — | 0 |
| **Total** | | **19 HU** | **54** |

### Trazabilidad: reglas MUST → HU

| Categoría | Reglas → HU |
|---|---|
| Registro | RN-001, RN-004, RN-005, RN-078 → HU-008 · RN-003, RN-058, RN-059, RN-086 → HU-006 · RN-022, RN-087 → HU-005 · RN-045, RN-046, RN-047, RN-051, RN-053, RN-098 → HU-004 · RN-104 → HU-005 · RN-052, RN-054, RN-088 → HU-004, HU-005 · RN-065 → HU-001 · RN-066, RN-067, RN-075 → HU-002 |
| Tags | RN-006, RN-007, RN-008 → HU-009 · RN-009, RN-091 → HU-010 |
| Compras | RN-012, RN-014, RN-015, RN-021, RN-048, RN-049, RN-050, RN-056, RN-082, RN-089, RN-090 → HU-012 · RN-097 → HU-012, HU-033 · RN-013 → HU-012, HU-014 · RN-016 → HU-016 |
| Límites | RN-017, RN-018, RN-019, RN-020, RN-083 → HU-013 |
| Seguridad | RN-026, RN-085 → HU-016 · RN-027 → HU-014, HU-017 · RN-028 → HU-017 · RN-029 → HU-003, HU-007, HU-011 · RN-030 → HU-007, HU-014 · RN-060 → HU-007 · RN-068 → HU-003 · RN-071 → HU-001, HU-011 · RN-072, RN-074 → HU-002 · RN-073 → HU-001, HU-003, HU-011 · RN-105 → HU-001, HU-003, HU-011 |
| Notificaciones | RN-023, RN-024, RN-025, RN-061, RN-084 → HU-015 |
| Consolidado mensual | RN-031, RN-033, RN-034, RN-063 → HU-018 |
| App del dueño | RN-101, RN-102 → HU-033 |
| UX | RN-040, RN-041 → HU-012 · RN-103 → HU-033 |

**Verificación QA:** todas las reglas MUST siguen cubiertas por al menos una HU. Los criterios nuevos agregan además trazabilidad a los PRINCIPIOS 001, 003, 004, 006, 007, 008, 011 y 012.

**Reglas sin HU en esta entrega** (prioridad SHOULD o COULD, para el backlog posterior): RN-010 y RN-011 (S3); RN-035, RN-036, RN-064, RN-080 y RN-081 (S1); RN-037 y RN-038 (S2); RN-042 (S5); RN-069, RN-070, RN-076, RN-077 y RN-079 (C3). Todas tienen una HU sugerida al final de este documento.

---

## Pendientes para el Product Owner

> **Firmados el 2026-11-13 por David Yallerco** como responsable del producto: las decisiones de esta lista y los criterios confirmados quedan aprobados (T-003).

1. ~~**Fin del año escolar (RN-055).**~~ **Retirado (2026-11-13):** la baja de una familia se hace cambiando su estado (RN-104).
2. ~~**Cambio del PIN del alumno.**~~ **Riesgo aceptado (2026-11-13):** no hay cambio de PIN en el piloto (RN-005).
3. ~~**Códigos de error no definidos.**~~ **Decidido (2026-11-13):** se confirma el catálogo de la arquitectura §4.1, incluidos los códigos marcados ✱.
4. ~~**Plazo del Libro de Reclamaciones.**~~ **Retirado (2026-11-11):** el Libro de Reclamaciones salió del alcance.
5. ~~**Formato del consolidado.**~~ **Decidido (2026-11-13):** JSON en el panel y descarga en Excel (T-077, Should).
6. **Corrección de compras en el MVP:** riesgo aceptado (sin cambios).
7. ~~**[QA] Orden de grabación y bloqueo del tag.**~~ **Decidido:** reservar → grabar → verificar → bloquear → activar (ADR-003).
8. ~~**[QA] Cobro sin confirmar.**~~ **Decidido:** ADR-002 y RN-015 ampliada.
9. ~~**[QA] Identificación de la compra disputada (HU-019).**~~ **Retirado (2026-11-11):** sin reclamos, el correo de cada compra ya no incluye `codigoCompra`.
10. ~~**[QA] Importación de Excel.**~~ **Decidido (2026-11-13):** una celda de DNI que no tenga 8 dígitos se rechaza y el panel entrega una plantilla con la columna en texto.
11. ~~**[QA] Credenciales.**~~ **Decidido (2026-11-13):** política de clave nueva (RN-105); 5 fallos consecutivos, contador a cero con acceso exitoso o al terminar el bloqueo, acceso desde el segundo 900 (RN-029); recuperación manual por el equipo de Luquita.
12. ~~**[QA] Validaciones sin regla.**~~ **Decidido (2026-11-13):** todo DNI tiene 8 dígitos; el DNI del alumno es único por colegio; el correo debe tener formato `usuario@dominio`; nombres y apellidos de hasta 100 caracteres; el nombre del colegio es único.
13. ~~**[QA] Permisos sin regla.**~~ **Decidido:** solo el apoderado de la familia bloquea y desbloquea sus tags (RN-028).
14. ~~**[QA] Familia que pasa a `INACTIVO`.**~~ **Decidido (2026-11-13):** su apoderado puede iniciar sesión y bloquear o desbloquear tags, pero no vincular tags; sus hijos no compran. En el consolidado solo aparece si tiene consumo en el mes.
15. ~~**[QA] Fixtures.**~~ **Aplicado:** apoderado `00000000`, secretaria `00000001`, alumno `00000010`.
16. ~~**[Equipo] Suscripción de febrero.**~~ **Resuelto (2026-11-11):** no hay periodos de diciembre a febrero; las ventas solo se habilitan de marzo a noviembre.
17. ~~**[Equipo] Suscripción en meses parciales o pausados.**~~ **Retirado (2026-11-13):** ya no hay suscripción.

---

## HUs sugeridas

Son propuestas de QA para el próximo refinamiento. No tienen estimación; la prioridad la decide el PO. Sus criterios quedaron confirmados el 2026-11-13 para cuando se implementen después del piloto.

### A. Principios de la constitución sin regla ni HU

---

### HU-021: Cambiar el PIN de mi hijo

**Prioridad sugerida:** Must (riesgo R5) · **Origen:** PRINCIPIO-008, RN-004, RN-005

Como **apoderado**, quiero cambiar desde la app el PIN de mi hijo, para que pueda volver a comprar si lo olvida o si alguien más lo conoce.

**Criterios de aceptación:**
- [NUEVO] **Positivo:** DADO QUE "Alumno Prueba" tiene el PIN `730`, CUANDO el apoderado lo cambia a `518`, ENTONCES la siguiente compra con `518` se registra y una con `730` responde `401`. (PRINCIPIO-008)
- [NUEVO] **Negativo:** DADO QUE el apoderado cambia el PIN de su hijo, CUANDO ingresa `51` (2 dígitos), ENTONCES el sistema no guarda el cambio. (RN-004)
- [NUEVO] **Negativo:** DADO QUE "Alumno Prueba" pertenece a "Familia Prueba", CUANDO el vendedor intenta cambiar su PIN, ENTONCES el sistema responde `403`. (RN-005)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE se digitaron mal 2 PIN, CUANDO el apoderado cambia el PIN, ENTONCES el contador de fallos vuelve a cero.

---

### HU-022: Restablecer mi PIN de acceso olvidado

**Prioridad sugerida:** a decidir · **Origen:** PRINCIPIO-008 (no tiene regla)

Como **apoderado**, quiero restablecer mi PIN de acceso si lo olvido, para no perder el control de los tags de mis hijos.

**Criterios de aceptación:**
- [NUEVO] [CONFIRMADO] **Positivo:** DADO QUE el apoderado `00000000` olvidó su PIN, CUANDO solicita restablecerlo, ENTONCES recibe un código de un solo uso en `apoderado@ejemplo.pe`, el correo registrado por el colegio (RN-061), y con ese código define un nuevo PIN de 6 dígitos.
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el código se emitió a las 10:00:00, CUANDO se usa a las 10:15:01, ENTONCES el sistema lo rechaza por vencido.
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE el DNI `11111111` no tiene cuenta, CUANDO alguien solicita restablecer su PIN, ENTONCES la app muestra el mismo mensaje que para un DNI con cuenta y no envía ningún correo.

---

### HU-023: Regenerar la clave de una secretaria o de un dueño de tienda

**Prioridad sugerida:** a decidir · **Origen:** PRINCIPIO-008, RN-072, RN-074 (no hay regla de recuperación)

Como **administrador de Luquita**, quiero regenerar la clave de una secretaria o de un dueño de tienda que la olvidó, para que vuelva a entrar sin que yo conozca su clave.

**Criterios de aceptación:**
- [NUEVO] [CONFIRMADO] **Positivo:** DADO QUE el dueño `tienda@colegioprueba.pe` olvidó su clave, CUANDO el administrador la regenera, ENTONCES ese correo recibe una clave nueva de 12 caracteres (RN-074), la anterior deja de funcionar y en el siguiente inicio de sesión el dueño debe cambiarla (RN-073).
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la secretaria `00000001` inició sesión, CUANDO intenta regenerar la clave del dueño, ENTONCES el sistema responde `403`.
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE la cuenta del dueño está bloqueada por 5 fallos, CUANDO el administrador regenera la clave, ENTONCES el bloqueo se levanta.

---

### HU-024: Recibir un aviso cuando el tag de mi hijo se bloquea por PIN fallidos

**Prioridad sugerida:** Should · **Origen:** PRINCIPIO-005, PRINCIPIO-008, riesgo R2 (no tiene regla)

Como **apoderado**, quiero recibir un correo cuando el tag de mi hijo se bloquee por PIN fallidos, para desbloquearlo pronto o saber que alguien intentó usarlo.

**Criterios de aceptación:**
- [NUEVO] [CONFIRMADO] **Positivo:** DADO QUE el tag de "Alumno Prueba" se bloquea por el tercer PIN fallido a las 2026-11-04T10:20:00-05:00, CUANDO termina la solicitud, ENTONCES `apoderado@ejemplo.pe` recibe "El tag de Alumno Prueba se bloqueó por 3 PIN incorrectos — 2026-11-04 10:20".
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE el correo contiene el aviso, CUANDO se revisa su contenido, ENTONCES no incluye ninguno de los PIN digitados. (PRINCIPIO-008)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE se digitaron mal 2 PIN, CUANDO no hay un tercer fallo, ENTONCES no se envía ningún aviso.

### B. Reglas SHOULD y COULD sin HU

---

### ~~HU-025: Registrar el fin del año escolar~~

**Retirada (2026-11-13):** sin suscripción ni temporada no hace falta; la secretaria da de baja a una familia cambiando su estado (HU-005, RN-104). RN-055 y RN-057 retiradas.

---

### HU-026: Anular una compra del mes en curso

**Prioridad:** Should (S1) · **Reglas:** RN-035, RN-034

Como **vendedor**, quiero anular una compra registrada por error en el mes en curso, para que no se cobre en la boleta.

**Criterios de aceptación:**
- [NUEVO] **Positivo:** DADO QUE existe la compra de +S/ 3.50 del 2026-11-04, CUANDO el vendedor la anula, ENTONCES se inserta un movimiento `ANULACION` de −S/ 3.50, la compra original no cambia y el consumo del mes baja S/ 3.50. (RN-035, PRINCIPIO-004)
- [NUEVO] **Negativo:** DADO QUE es diciembre de 2026, CUANDO el vendedor intenta anular una compra del 2026-11-20, ENTONCES el sistema lo rechaza; la corrección solo puede hacerse con un `AJUSTE` (HU-027). (RN-034, RN-036)
- [NUEVO] **Borde:** DADO QUE el alumno llegó a S/ 30.00 en el día y se anuló una compra de S/ 3.50, CUANDO compra S/ 3.50 ese mismo día, ENTONCES la compra se registra, porque la `ANULACION` sí cuenta para los límites. (RN-080 solo excluye los `AJUSTE`)
- [NUEVO] [CONFIRMADO] **Negativo:** DADO QUE la compra ya fue anulada, CUANDO el vendedor intenta anularla otra vez, ENTONCES el sistema no inserta una segunda `ANULACION`.

---

### HU-027: Registrar un ajuste sobre un mes anterior

**Prioridad:** Should (S1) · **Reglas:** RN-036, RN-064, RN-080, RN-081

Como **secretaria del colegio**, quiero registrar un ajuste en el mes en curso, para corregir un consumo de un mes anterior.

**Criterios de aceptación:**
- [NUEVO] **Positivo:** DADO QUE una compra errónea de S/ 4.00 del 2026-10-20 ya se entregó al colegio en el consolidado de octubre, CUANDO la secretaria registra un `AJUSTE` de −S/ 4.00, ENTONCES el ajuste queda en noviembre de 2026. (RN-036, RN-064)
- [NUEVO] **Negativo:** DADO QUE el vendedor está autenticado, CUANDO intenta registrar un `AJUSTE` de −S/ 4.00, ENTONCES el sistema no lo registra. (RN-064)
- [NUEVO] **Borde:** DADO QUE en noviembre de 2026 hay un `AJUSTE` de −S/ 4.00, CUANDO el alumno llega a S/ 300.00 en compras, ENTONCES la siguiente compra se rechaza con `LIMITE_MENSUAL_EXCEDIDO`, y su consolidado de noviembre es S/ 296.00. (RN-080, RN-081)

---

### HU-028: Ver el detalle de consumo de mi hijo

**Prioridad:** Should (S2) · **Reglas:** RN-037, RN-038

Como **apoderado**, quiero ver desde la app la lista de compras del mes de mi hijo, para saber en qué gasta y revisar una compra concreta.

**Criterios de aceptación:**
- [NUEVO] **Positivo:** DADO QUE "Alumno Prueba" tiene compras en noviembre de 2026, CUANDO el apoderado pide el detalle de ese mes, ENTONCES ve "2026-11-04 10:15 — S/ 3.50 — Jugo de naranja; 2026-11-05 10:20 — S/ 2.00 — (sin descripción)". (RN-037)
- [NUEVO] **Negativo:** DADO QUE un alumno pertenece a otra familia, CUANDO el apoderado de "Familia Prueba" pide su detalle, ENTONCES recibe `403`. (RN-038)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE "Alumno Prueba" no tiene compras en octubre de 2026, CUANDO el apoderado pide ese detalle, ENTONCES la app muestra "Sin compras en octubre".

---

### HU-029: Reemplazar o desvincular el tag de mi hijo

**Prioridad:** Should (S3) · **Reglas:** RN-010, RN-011

Como **apoderado**, quiero desvincular un tag perdido y vincular uno nuevo, para que mi hijo siga comprando y nadie use el tag anterior.

**Criterios de aceptación:**
- [NUEVO] **Positivo:** DADO QUE "Alumno Prueba" tiene un tag activo, CUANDO el apoderado vincula un tag nuevo, ENTONCES el anterior pasa a estado desvinculado y el alumno queda con un solo tag activo. (RN-010)
- [NUEVO] **Negativo:** DADO QUE el tag se desvinculó a las 2026-11-04T08:00:00-05:00, CUANDO se intenta una compra con ese tag a las 08:00:05, ENTONCES el sistema la rechaza. (RN-011)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE "Alumno Prueba" tiene un único tag, CUANDO el apoderado lo desvincula sin vincular otro, ENTONCES el alumno queda sin tag activo y ninguna compra a su nombre se registra.

---

### ~~HU-030: Eliminar los datos de un año escolar a los 5 años~~

**Retirada (2026-11-11):** la conservación por 5 años y la eliminación de datos (RN-039, S4) salen del alcance del piloto (constitución 1.16).

---

### HU-031: Consultar el consumo total de un mes terminado

**Prioridad:** Should (S5) · **Reglas:** RN-042

Como **dueño de la tienda**, quiero consultar el consumo total de un mes terminado, para saber cuánto debe pagarme el colegio.

**Criterios de aceptación:**
- [NUEVO] **Positivo:** DADO QUE octubre de 2026 terminó, CUANDO el dueño consulta el total, ENTONCES ve "Consumo total de octubre: S/ 18 450.00". (RN-042)
- [NUEVO] **Negativo:** DADO QUE octubre de 2026 terminó, CUANDO el dueño intenta ver el consumo por alumno, ENTONCES el sistema no se lo muestra. (RN-042, PRINCIPIO-007)
- [NUEVO] [CONFIRMADO] **Borde:** DADO QUE es el 2026-11-15, CUANDO el dueño consulta el total de noviembre, ENTONCES la app indica que el mes aún no termina y no muestra el monto.

---

### HU-032: Configurar un tope mensual personalizado para mi hijo

**Prioridad:** Could (C3) · **Reglas:** RN-069, RN-070, RN-076, RN-077, RN-079 · **Aprobación humana:** obligatoria (PRINCIPIO-009, lógica de límites)

Como **apoderado**, quiero fijar un tope mensual menor a S/ 300.00 para mi hijo, para controlar mejor su gasto.

**Criterios de aceptación:**
- [NUEVO] **Positivo:** DADO QUE el apoderado configuró un tope de S/ 150.00 y el alumno consumió S/ 148.00 en el mes, CUANDO intenta comprar S/ 3.00, ENTONCES el sistema responde `422 LIMITE_MENSUAL_EXCEDIDO`. (RN-069)
- [NUEVO] **Negativo:** DADO QUE el apoderado edita el tope, CUANDO ingresa S/ 350.00, ENTONCES la app muestra "El tope máximo es S/ 300.00" y no guarda el cambio; y si ingresa −S/ 10.00, muestra "El tope no puede ser negativo". (RN-070, RN-077)
- [NUEVO] **Negativo:** DADO QUE "Alumno Prueba" pertenece a "Familia Prueba", CUANDO la secretaria intenta cambiar su tope, ENTONCES el sistema responde `403`. (RN-079)
- [NUEVO] **Borde:** DADO QUE el alumno consumió S/ 120.00 en el mes, CUANDO el apoderado baja el tope a S/ 100.00 a las 10:00:00, ENTONCES una compra de S/ 2.00 a las 10:00:30 se rechaza con `LIMITE_MENSUAL_EXCEDIDO`. (RN-076)
- [NUEVO] **Borde:** DADO QUE el apoderado fija el tope en S/ 0.00, CUANDO el alumno intenta comprar S/ 0.01, ENTONCES el sistema responde `422 LIMITE_MENSUAL_EXCEDIDO`. (PRINCIPIO-005: el tope admite valores entre S/ 0.00 y S/ 300.00)
