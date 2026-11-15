# 09 — UI Specification: LUQUITA

- **Producto:** Luquita, billetera digital infantil con tag NFC para la tienda del colegio.
- **Fase:** diseño de interfaz (Agente UI Designer, BMAD).
- **Documentos base:** `00-constitucion.md` (versión 1.17), `02-alcance.md`, `06-user-stories.md` (actualizado el 2026-11-13) y el documento de arquitectura (v0.5, con ADR-001 a ADR-010).
- **Fecha:** 2026-11-14.
- **Alcance de esta entrega:** specs completas de todas las pantallas Must (MVP, 19 HU) y Should (HU sugeridas para después del piloto); las Could y Won't solo figuran en el inventario.

**Convenciones de este documento:**

- Todos los textos de interfaz están en español latinoamericano (PRINCIPIO-001). Los textos entre comillas «…» son **literales**: el desarrollador los copia tal cual.
- Los identificadores sugeridos para componentes, rutas y claves de texto son ASCII, sin tildes ni ñ (por ejemplo `pantalla-cobro`, `/colegio/familias`).
- Datos de ejemplo solo ficticios (PRINCIPIO-009): apoderado `00000000`, secretaria `00000001`, alumno `00000010`, «Alumno Prueba», «Familia Prueba», «Colegio Prueba».
- Montos: `S/ 3.50` (punto decimal, dos decimales, espacio entre `S/` y el número; miles con espacio: `S/ 18 450.00`). El servidor envía los montos como string; Flutter los maneja en céntimos enteros y Angular solo muestra el string recibido (PRINCIPIO-004).
- Fechas y horas: formato de las HU, `AAAA-MM-DD HH:mm`, en hora de Lima. Los meses se nombran en minúscula: «noviembre de 2026».
- **Superficies:** **W** = Panel web (Angular 20), **A** = App Apoderados (Flutter, Android 8+), **V** = App Vendedor (Flutter, Android 8+ con NFC), **C** = plantilla de correo.
- Wireframes: cajas ASCII. `[ Botón ]` es un botón; `( )` y `(•)` son radio; `[x]` es casilla; `[____]` es un campo; `▼` es un desplegable; `‹` es volver.

---

## 0. Decisiones de diseño y supuestos

Lo que sigue son decisiones de **presentación** que el Spec Pack no fija. Ninguna cambia una regla de negocio. Las que afectan la API o el alcance están en la sección 7, «Preguntas abiertas», para que el equipo las confirme.

| # | Decisión | Motivo |
|---|---|---|
| UD-01 | El panel web tiene **un solo inicio de sesión** con el campo «DNI o correo». La secretaria usa su DNI; el administrador, su correo. | El endpoint `POST /autenticacion/personal/sesion` recibe un único campo `usuario` (arquitectura §4.2.2). |
| UD-02 | La app del vendedor sigue el orden **monto → tag → nombre → PIN → resultado**. | Flujo crítico §2.1 y HU-012. |
| UD-03 | Mientras haya un cobro sin confirmar, la app del vendedor **no deja iniciar ningún cobro nuevo** (no solo para el mismo tag) hasta que el vendedor lo verifique, lo reintente o lo descarte de forma explícita. | ADR-002 punto 3; la versión estricta es más simple de explicar y de probar. |
| UD-04 | Los PIN (6 dígitos del apoderado y 3 del alumno) se piden **dos veces** al crearlos (campo «Repite el PIN»). La confirmación es solo de la app; el servidor recibe un único `pin`. | Evita PIN mal tipeados que después bloquean la cuenta o el tag. No cambia ninguna regla. |
| UD-05 | Ninguna app valida límites de consumo, estado del tag ni estado de la familia: solo validan **formato**. Todo rechazo de negocio se muestra a partir del `codigo` del servidor. | Principio de diseño D1 y PRINCIPIO-005. |
| UD-06 | Los resultados siempre se comunican con **ícono + texto + color**, nunca solo con color. | PRINCIPIO-011 (WCAG 1.4.1). |
| UD-07 | La app del vendedor usa un **teclado numérico propio** en pantalla (monto y PIN), con teclas de 72 × 64 dp. | PRINCIPIO-011 exige ≥ 48 × 48 dp también en el teclado del PIN; además evita que el teclado del sistema muestre sugerencias o guarde lo digitado. |
| UD-08 | Tipografía: **Roboto** del sistema en Flutter y una pila de fuentes del sistema en Angular. No se agregan paquetes de fuentes ni librerías de componentes. | PRINCIPIO-009: un agente no agrega dependencias. Si el equipo quiere Angular Material, lo aprueba e incorpora un humano. |
| UD-09 | El consolidado abre por defecto en el **mes anterior** al actual. | Es el mes que normalmente va a la boleta; el mes en curso sigue disponible con su aviso. |

---

## 1. Inventario de pantallas

Prioridad MoSCoW según `02-alcance.md` y la prioridad de cada HU. «Won't (piloto)» marca las HU sugeridas que el equipo dejó fuera del piloto como riesgo aceptado.

### 1.1 Panel web (W) — secretaria del colegio y administrador de Luquita

| ID | Pantalla | Actor principal | HU | Prioridad |
|---|---|---|---|---|
| P-001 | Iniciar sesión (personal) | Secretaria, administrador | HU-001, HU-003 | Must |
| P-002 | Cambia tu clave | Secretaria, administrador | HU-001, HU-003 | Must |
| P-010 | Colegios (lista) | Administrador | HU-001 | Must |
| P-011 | Registrar colegio (diálogo) | Administrador | HU-001 | Must |
| P-012 | Detalle del colegio | Administrador | HU-002, HU-004, HU-033 | Must |
| P-013 | Registrar secretaria o dueño de tienda (diálogo) | Administrador | HU-002 | Must |
| P-020 | Inicio de la secretaria | Secretaria | HU-033, HU-018, HU-004, HU-005 | Must |
| P-021 | Importar familias (Excel) | Secretaria, administrador | HU-004 | Must |
| P-022 | Familias (búsqueda y lista) | Secretaria | HU-005 | Must |
| P-023 | Registrar familia | Secretaria | HU-005 | Must |
| P-024 | Editar familia (correo y estado) | Secretaria | HU-005 | Must |
| P-025 | App de cobro (activar o desactivar) | Secretaria, administrador | HU-033 | Must |
| P-026 | Consolidado mensual | Secretaria | HU-018 | Must (descarga en Excel: Should, T-077) |
| P-027 | Registrar ajuste | Secretaria | HU-027 | Should (S1) |
| P-028 | Regenerar clave del personal | Administrador | HU-023 | Won't (piloto): procedimiento manual §6.7 |

### 1.2 App Apoderados (A)

| ID | Pantalla | Actor principal | HU | Prioridad |
|---|---|---|---|---|
| P-101 | Bienvenida | Apoderado | HU-006, HU-007 | Must |
| P-102 | Crear cuenta | Apoderado | HU-006 | Must |
| P-103 | Iniciar sesión | Apoderado | HU-007 | Must |
| P-104 | Mis hijos (inicio) | Apoderado | HU-008, HU-009, HU-016, HU-017 | Must |
| P-105 | Registrar hijo | Apoderado | HU-008 | Must |
| P-106 | Detalle del hijo y sus tags | Apoderado | HU-009, HU-016, HU-017 | Must |
| P-107 | Vincular tag (asistente) | Apoderado | HU-009, HU-010 | Must |
| P-108 | Detalle de consumo del mes | Apoderado | HU-028 | Should (S2) |
| P-109 | Reemplazar o desvincular tag | Apoderado | HU-029 | Should (S3) |
| P-110 | Tope mensual del hijo | Apoderado | HU-032 | Could (C3) |
| P-111 | Cambiar el PIN del hijo | Apoderado | HU-021 | Won't (piloto): riesgo R5 aceptado; QA sugería Must |
| P-112 | Restablecer mi PIN | Apoderado | HU-022 | Won't (piloto): procedimiento manual §6.7 |

### 1.3 App Vendedor (V)

| ID | Pantalla | Actor principal | HU | Prioridad |
|---|---|---|---|---|
| P-201 | Iniciar sesión (vendedor) | Dueño de la tienda | HU-011 | Must |
| P-202 | Cambia tu clave (vendedor) | Dueño de la tienda | HU-011 | Must |
| P-203 | App de cobro no activa | Dueño de la tienda | HU-033 | Must |
| P-204 | Cobro: monto y descripción | Dueño de la tienda | HU-012 | Must |
| P-205 | Cobro: acercar el tag | Dueño de la tienda, alumno | HU-012 | Must |
| P-206 | Cobro: PIN del alumno | Alumno | HU-012, HU-014 | Must |
| P-207 | Resultado del cobro (cobrado o rechazado) | Dueño de la tienda | HU-012, HU-013, HU-014, HU-016 | Must |
| P-208 | Cobro sin confirmar | Dueño de la tienda | HU-012 | Must |
| P-209 | Anular un cobro | Dueño de la tienda | HU-026 | Should (S1) |
| P-210 | Consumo total del mes | Dueño de la tienda | HU-031 | Should (S5) |

### 1.4 Plantillas de correo (C)

| ID | Plantilla | Destinatario | HU | Prioridad |
|---|---|---|---|---|
| P-301 | Clave inicial del personal | Secretaria, dueño, administrador | HU-002 | Must |
| P-302 | Aviso de compra | Apoderado | HU-015 | Must |
| P-303 | Aviso de tag bloqueado por PIN | Apoderado | HU-024 | Should |

**Totales:** 40 entradas: 30 Must (28 pantallas y 2 plantillas de correo), 6 Should, 1 Could y 3 Won't (piloto). P-026 se cuenta como Must aunque su descarga en Excel es Should. Las 19 HU Must tienen al menos una pantalla o plantilla.

### 1.5 Trazabilidad inversa: HU Must → pantallas

| HU | Pantallas |
|---|---|
| HU-001 | P-001, P-002, P-010, P-011 |
| HU-002 | P-012, P-013, P-301 |
| HU-003 | P-001, P-002 |
| HU-004 | P-012, P-020, P-021 |
| HU-005 | P-020, P-022, P-023, P-024 |
| HU-006 | P-101, P-102 |
| HU-007 | P-101, P-103 |
| HU-008 | P-104, P-105 |
| HU-009 | P-104, P-106, P-107 |
| HU-010 | P-107 |
| HU-011 | P-201, P-202 |
| HU-012 | P-204, P-205, P-206, P-207, P-208 |
| HU-013 | P-207 |
| HU-014 | P-206, P-207 |
| HU-015 | P-302 |
| HU-016 | P-104, P-106, P-207 |
| HU-017 | P-104, P-106 |
| HU-018 | P-020, P-026 |
| HU-033 | P-012, P-020, P-025, P-203 |

---

## 2. User flows

Notación: `P-XXX` pantalla · `→` navegación · `⟲` vuelve a la misma pantalla con un estado nuevo · `[acción]` lo que hace el usuario · `{API}` llamada al servidor · `⚠` rama de error.

### F-01 Alta del colegio (administrador) — HU-001, HU-002

1. P-001 `[ingresa correo y clave]` {`POST /autenticacion/personal/sesion`}
   - Si `cambioClavePendiente: true` → P-002 `[define clave nueva]` {`POST …/cambio-clave`} → P-010.
   - ⚠ `401 CREDENCIALES_INVALIDAS` ⟲ P-001 con «Usuario o clave incorrectos.»
   - ⚠ `423 CUENTA_BLOQUEADA` ⟲ P-001 con el aviso de bloqueo y la hora de desbloqueo.
2. P-010 `[Registrar colegio]` → P-011 `[nombre del colegio y de la tienda → Registrar]` {`POST /admin/colegios`} → P-010 con el colegio nuevo y aviso «Colegio registrado».
   - ⚠ `409 RECURSO_DUPLICADO` ⟲ P-011 con «Ya existe un colegio con ese nombre.»
3. P-010 `[abre «Colegio Prueba»]` → P-012.
4. P-012 `[Registrar usuario]` → P-013 `[elige Secretaria, DNI 00000001, correo → Registrar]` {`POST /admin/colegios/{colegioId}/usuarios`} → P-012 con la fila nueva y estado de la clave «Enviando…».
5. P-013 otra vez para el dueño de la tienda (solo correo).
6. P-012 consulta {`GET /admin/usuarios/{id}`} y muestra «Clave enviada» o «Clave no enviada».
   - ⚠ «Clave no enviada» → el panel indica «Pide al equipo de Luquita que regenere la clave» (procedimiento manual §6.7).

### F-02 Primer acceso de la secretaria — HU-003

P-001 `[DNI 00000001 + clave recibida]` → P-002 (única pantalla disponible) `[clave nueva que cumple RN-105]` → P-020.
- ⚠ Si la secretaria escribe su correo en lugar del DNI → `401` ⟲ P-001 «Usuario o clave incorrectos.»
- ⚠ Si navega a otra URL antes de cambiar la clave (`403 CAMBIO_CLAVE_PENDIENTE`) → P-002.

### F-03 Importación anual de familias — HU-004

1. Secretaria: P-020 `[Importar familias]` → P-021. Administrador: P-012 `[Importar familias]` → P-021 con el colegio fijado.
2. P-021 `[Descargar plantilla]` (opcional) · `[Elegir archivo .xlsx]` · `[Importar]` {`POST /colegios/{colegioId}/familias/importaciones`} ⟲ P-021 con el resultado.
   - Resultado: «148 familias registradas — 2 filas rechazadas: fila 12 (falta DNI), fila 87 (DNI duplicado)» + tabla de filas rechazadas.
   - ⚠ Archivo `.csv` u otro → error antes de enviar: «Solo se aceptan archivos .xlsx.»
   - ⚠ Falta una columna → «El archivo no tiene la columna `correo`. No se registró ninguna familia.»
3. `[Ver familias]` → P-022.

### F-04 Registro manual, corrección o baja de una familia — HU-005

- Registro: P-022 `[Registrar familia]` → P-023 `[DNI, nombre, apellidos, correo → Registrar]` {`POST /colegio/familias`} → P-022 con aviso «Familia registrada».
  - ⚠ `409 RECURSO_DUPLICADO` ⟲ P-023 «Ya existe una familia con ese DNI. Búscala para editarla.» con enlace a la búsqueda.
- Corrección de correo: P-022 `[busca por DNI]` {`GET /colegio/familias?dni=`} `[Editar]` → P-024 `[correo nuevo → Guardar]` {`PATCH`} → P-024 con aviso «Correo actualizado. Los próximos correos irán a nuevo@ejemplo.pe».
- Baja: P-024 `[Pasar a inactiva]` → diálogo de confirmación → {`PATCH estado: INACTIVO`} ⟲ P-024 con la etiqueta «Inactiva».

### F-05 Activar o desactivar la app de cobro — HU-033

P-020 (tarjeta «App de cobro») o P-012 (administrador) `[Activar]` / `[Desactivar]` → diálogo de confirmación → {`PUT /colegios/{colegioId}/ventas`} ⟲ misma pantalla con el estado nuevo y la hora del cambio.

### F-06 Consolidado mensual — HU-018

P-020 `[Consolidado]` → P-026 (abre en el mes anterior) `[elige año y mes]` {`GET /colegio/consolidados/{anio}/{mes}`} ⟲ P-026.
- Mes en curso → aviso «Mes en curso: los totales pueden cambiar».
- ⚠ Mes futuro (`422 MES_FUTURO`) → «Ese mes todavía no empieza.» sin montos.
- `[Descargar Excel]` (Should, T-077).

### F-07 Alta del apoderado y registro de hijos — HU-006, HU-007, HU-008

1. P-101 `[Crear cuenta]` → P-102 `[DNI 00000000, PIN 6 dígitos, repite el PIN → Crear cuenta]` {`POST /apoderados`} → P-103 con aviso «Cuenta creada. Ya puedes iniciar sesión.»
   - ⚠ `422 DNI_NO_REGISTRADO` ⟲ P-102 «Tu DNI no está registrado en Luquita. Consulta con la secretaría del colegio.»
   - ⚠ `409 RECURSO_DUPLICADO` ⟲ P-102 «Tu familia ya tiene una cuenta en Luquita. Inicia sesión.» + `[Iniciar sesión]`.
   - ⚠ Corte de red → `[Reintentar]` reenvía con la misma `Idempotency-Key` (HU-006, criterio confirmado).
2. P-103 `[DNI + PIN]` {`POST /autenticacion/apoderados/sesion`} → P-104 (vacío la primera vez).
3. P-104 `[Registrar hijo]` → P-105 `[nombre, DNI, PIN 3 dígitos, repite el PIN → Registrar]` {`POST /apoderado/alumnos`} → P-104 con el hijo nuevo y el aviso «Alumno Prueba quedó registrado. Ahora vincula su tag.» con el botón `[Vincular tag]`.

### F-08 Vincular un tag — HU-009, HU-010 (ADR-003)

P-104 o P-106 `[Vincular tag]` → P-107:
1. **Paso «NFC»:** si el NFC está apagado o no existe → estado «Necesitas NFC activo» y no continúa.
2. **Paso «Acerca el tag»:** `[acerca el tag]` → la app lee el chip.
   - ⚠ MIFARE Classic, NTAG216 u otro → «Este tag no es compatible con Luquita».
   - ⚠ Tag con datos o con lock bits → «Este tag ya fue usado. Pide un tag nuevo en el colegio.»
3. **Reservar** {`POST /apoderado/alumnos/{id}/tags`}.
   - ⚠ `422 FAMILIA_INACTIVA` → «Tu familia no está activa en Luquita. Consulta con la secretaría del colegio.»
4. **Grabar → Verificar → Bloquear** (barra de pasos, «No retires el tag»).
   - ⚠ Tag retirado antes de bloquear → «La grabación no terminó. Vuelve a acercar el mismo tag.» (el tag sigue reescribible; reintento con el mismo token).
5. **Activar** {`POST /apoderado/tags/{id}/activacion`} → «¡Listo! El tag de Alumno Prueba ya está activo.» → `[Volver a Mis hijos]` → P-104.
   - ⚠ Sin respuesta → «El tag ya está grabado, pero falta confirmarlo con Luquita. Revisa tu conexión y toca Reintentar.» (reintento idempotente; si el apoderado sale, P-106 muestra el tag como «Pendiente de confirmar» con `[Terminar vinculación]`).

### F-09 Bloquear y desbloquear un tag — HU-016, HU-017

- P-106 `[Bloquear tag]` → diálogo «¿Bloquear el tag de Alumno Prueba? Nadie podrá comprar con él hasta que lo desbloquees.» `[Bloquear]` {`POST /apoderado/tags/{id}/bloqueo`} ⟲ P-106 con el tag «Bloqueado» y aviso «Tag bloqueado. Ninguna compra con este tag será aceptada.»
  - ⚠ Sin conexión → aviso persistente de error «El tag NO se bloqueó. Revisa tu conexión e inténtalo de nuevo» + `[Reintentar]`. El tag se sigue mostrando «Activo».
- P-106 `[Desbloquear tag]` → diálogo → {`POST …/desbloqueo`} ⟲ P-106 con el tag «Activo».

### F-10 Primer acceso del vendedor — HU-011

P-201 `[correo + clave recibida]` → P-202 `[clave nueva]` → {`GET /vendedor/ventas`} → P-204 si está activa · P-203 si no.

### F-11 Cobro exitoso — HU-012, HU-015

1. P-204 `[digita 3.50, escribe «Jugo de naranja» → Cobrar]` → la app genera la `Idempotency-Key` (solo en memoria) y verifica {`GET /vendedor/ventas`}.
2. P-205 `[el alumno acerca el tag]` {`POST /tags/resolucion`} → muestra «Alumno Prueba».
3. P-206 `[el alumno digita 730]` (se ve `•••`) `[Confirmar]` {`POST /compras`, timeout 8 s}.
4. P-207 estado **Cobrado**: «Cobrado S/ 3.50 — Alumno Prueba» → `[Nuevo cobro]` → P-204 vacío.
5. En segundo plano, el apoderado recibe P-302.

### F-12 Cobro rechazado — HU-012, HU-013, HU-014, HU-016

Desde P-206 {`POST /compras`} → P-207 estado **No se cobró** con el motivo:

| Respuesta | Motivo que se muestra | Acción principal |
|---|---|---|
| `401 PIN_INVALIDO` | «PIN incorrecto.» | `[Digitar el PIN otra vez]` → P-206 con **una `Idempotency-Key` nueva** |
| `401 PIN_INVALIDO` + `tagBloqueado: true` | «PIN incorrecto. El tag se bloqueó por 3 intentos fallidos. El apoderado debe desbloquearlo desde su app.» | `[Nuevo cobro]` |
| `422 TAG_BLOQUEADO` | «Este tag está bloqueado. El apoderado debe desbloquearlo desde su app.» | `[Nuevo cobro]` |
| `422 LIMITE_DIARIO_EXCEDIDO` | «La compra supera el límite diario de S/ 30.00.» | `[Cambiar monto]` → P-204 con el monto actual, `[Nuevo cobro]` |
| `422 LIMITE_MENSUAL_EXCEDIDO` | «La compra supera el tope mensual del alumno.» | `[Cambiar monto]`, `[Nuevo cobro]` |
| `422 FAMILIA_INACTIVA` | «La familia del alumno no está activa en Luquita. Consulta con la secretaría del colegio.» | `[Nuevo cobro]` |
| `404 TAG_NO_RECONOCIDO` (en P-205) | «Tag no reconocido. Este tag no está vinculado a ningún alumno.» (sin nombre) | `[Intentar con otro tag]` |
| `409 VENTAS_NO_HABILITADAS` | — | → P-203 |
| `503 SERVICIO_NO_DISPONIBLE` | «Luquita no está disponible en este momento. No se cobró.» | `[Intentar de nuevo]` (misma clave) |

### F-13 Cobro sin confirmar — HU-012 (ADR-002)

P-206 {`POST /compras`} → timeout de 8 s o corte → P-208 «No se pudo confirmar el cobro».
- `[Verificar]` {`GET /compras/operaciones/{clave}`}:
  - `REGISTRADA` → P-207 **Cobrado**.
  - `RECHAZADA` → P-207 **No se cobró** con el motivo de su `codigo` (por ejemplo, «PIN incorrecto»).
  - `404` ⟲ P-208 con «El cobro no llegó a Luquita. Puedes reintentarlo sin riesgo de cobrar dos veces.»
- `[Reintentar]` → el mismo `POST /compras` con la **misma** clave y el mismo contenido → P-207.
- `[Descartar]` → diálogo de advertencia → P-204. (Riesgo residual aceptado en ADR-002, punto 4.)

### F-14 Sin conexión en la tienda — HU-012

Cualquier pantalla de cobro detecta que no hay red → franja persistente «Sin conexión: no se puede cobrar con Luquita». En P-205 y P-206 el cobro no avanza y no se guarda nada en el celular. Al volver la red, la franja desaparece y el vendedor reinicia el cobro desde P-204.

### F-15 App de cobro desactivada — HU-033

Al abrir la app, antes de cada cobro o ante un `409 VENTAS_NO_HABILITADAS` → P-203 «La app de cobro no está activa. Consulta con la secretaría del colegio.» `[Volver a consultar]` {`GET /vendedor/ventas`} → P-204 si ya está activa.

### F-16 Flujos Should (resumen)

- **F-17 Anular un cobro (HU-026):** P-204 `[Cobros de hoy]` → P-209 `[elige un cobro → Anular]` → confirmación → P-209 con la fila «Anulado».
- **F-18 Ajuste (HU-027):** P-020 `[Ajustes]` → P-027 `[busca familia → elige alumno → monto con signo y motivo → Registrar]` → confirmación con el mes en que queda.
- **F-19 Detalle de consumo (HU-028):** P-106 `[Ver consumo del mes]` → P-108 `[cambia de mes]`.
- **F-20 Reemplazo de tag (HU-029):** P-106 `[Reemplazar tag]` → P-109 → P-107 → P-106.
- **F-21 Total del mes (HU-031):** P-204 menú `[Total del mes]` → P-210.

---

## 3. Screen specs

### 3.0 Patrones comunes (aplican a todas las pantallas)

**Reglas sin efecto en pantalla** (las cumple el servidor; se listan para completar la trazabilidad):

- RN-019: los límites diario y mensual se calculan con el día y el mes de Lima (`America/Lima`).
- RN-020 y RN-083: el servidor valida los límites, dentro de la misma transacción que registra la compra; la app solo muestra el rechazo (P-207).
- RN-024, RN-025 y RN-084: reintentos del correo de una compra, la compra no se anula si el correo falla y se registra el resultado de cada intento (P-302).
- RN-030: los registros del sistema no contienen PIN, DNI, claves, tokens ni secretos; ninguna pantalla los muestra fuera de lo que define su spec.

**Accesibilidad (PRINCIPIO-011, criterio transversal de las HU):**

- Contraste de texto ≥ 4.5:1 y de bordes y componentes de interfaz ≥ 3:1 (ver sección 4).
- Áreas táctiles ≥ 48 × 48 dp en Flutter y ≥ 48 × 48 px en el panel, incluidas las teclas del PIN.
- Cada control tiene `Semantics(label: …)` en Flutter o `aria-label` / `<label for>` en Angular. Los íconos decorativos se excluyen del lector (`ExcludeSemantics` / `aria-hidden="true"`).
- Texto al 200 %: ningún contenedor tiene alto fijo con texto adentro; las pantallas hacen scroll vertical; las tablas del panel pasan a tarjetas apiladas por debajo de 600 px de ancho.
- Los mensajes de resultado se anuncian: `SemanticsService.announce` en Flutter y una región `aria-live="polite"` (o `"assertive"` para errores) en Angular.
- Los errores de un campo se muestran debajo del campo, con ícono, y se asocian con `aria-describedby` / `errorText`. Al enviar un formulario con errores, el foco va al primer campo con error.
- Panel web: enlace «Saltar al contenido» al inicio, foco visible de 3 px (`--color-foco`), navegación completa con teclado y orden de tabulación igual al orden visual.
- Los campos de PIN y clave nunca se leen en voz alta: el lector anuncia «PIN, 3 de 3 dígitos ingresados».

**Datos sensibles (PRINCIPIO-007 y PRINCIPIO-008):**

- Los PIN y las claves no se guardan en el estado de la app más allá de la pantalla actual; los campos deshabilitan autocompletar, sugerencias y captura de portapapeles (`autocomplete="off"`, `enableSuggestions: false`, `autocorrect: false`).
- La app del vendedor nunca muestra DNI, consumo acumulado ni estado del tag antes del cobro.
- El panel nunca muestra el DNI completo de un apoderado ni el DNI de la secretaria. El DNI del alumno solo aparece en el consolidado (lo permite el PRINCIPIO-007).
- Ninguna pantalla incluye analítica, publicidad ni SDK de terceros (PRINCIPIO-007).

**Sesión (PRINCIPIO-008, arquitectura §4.2.4):**

- El JWT dura 15 minutos y se renueva en silencio. Si la renovación falla (`401 NO_AUTENTICADO`), la app vuelve al inicio de sesión con el aviso «Tu sesión terminó. Vuelve a iniciar sesión.»
- `403 CAMBIO_CLAVE_PENDIENTE` en cualquier pantalla lleva a «Cambia tu clave».
- `403 ACCESO_DENEGADO` muestra «No tienes permiso para hacer esta acción.» y no cambia nada en pantalla.

**Bloqueo por intentos (RN-029):** `423 CUENTA_BLOQUEADA` trae `Retry-After` en segundos. La pantalla calcula la hora de desbloqueo y muestra «Por seguridad, tu cuenta se bloqueó por 5 intentos fallidos. Podrás intentarlo de nuevo a las 08:15.» El botón de ingresar queda deshabilitado hasta esa hora (a las 08:15:00 exactas ya se habilita, HU-003).

**Avisos breves (snackbar / toast):** 4 s mínimo, descartables, con `aria-live`. Nunca se usan para errores que el usuario debe resolver: esos van en línea o en un banner persistente.

**Idempotencia en la interfaz (PRINCIPIO-004):** todo botón que dispara un `POST` de negocio se deshabilita y muestra un indicador de carga mientras espera; la `Idempotency-Key` se genera al primer toque y se reutiliza en cada «Reintentar» de esa misma operación.

**Mensajes por código de error** (tabla única que usan las tres superficies; los códigos vienen del catálogo §4.1):

| `codigo` | Texto en pantalla |
|---|---|
| `DATOS_INVALIDOS` | El mensaje de cada campo que viene en `errores[]`. |
| `DNI_INVALIDO` | «DNI inválido» |
| `PIN_FORMATO_INVALIDO` | «El PIN debe tener 6 dígitos.» (apoderado) / «El PIN debe tener 3 dígitos.» (alumno) |
| `MONTO_INVALIDO` | «Ingresa un monto mayor a S/ 0.00 con dos decimales.» |
| `DESCRIPCION_MUY_LARGA` | «La descripción puede tener hasta 60 caracteres.» |
| `NO_AUTENTICADO` | «Tu sesión terminó. Vuelve a iniciar sesión.» |
| `CREDENCIALES_INVALIDAS` | Panel y vendedor: «Usuario o clave incorrectos.» · Apoderado: «DNI o PIN incorrectos.» |
| `PIN_INVALIDO` | «PIN incorrecto.» |
| `ACCESO_DENEGADO` | «No tienes permiso para hacer esta acción.» |
| `CAMBIO_CLAVE_PENDIENTE` | Redirige a «Cambia tu clave». |
| `TAG_NO_RECONOCIDO` | «Tag no reconocido. Este tag no está vinculado a ningún alumno.» |
| `IDEMPOTENCY_KEY_REUTILIZADA` | «No se pudo procesar la operación. Empieza de nuevo. No se cobró.» |
| `VENTAS_NO_HABILITADAS` | «La app de cobro no está activa. Consulta con la secretaría del colegio.» |
| `RECURSO_DUPLICADO` | Según el contexto (ver cada pantalla). |
| `TAG_YA_GRABADO` | «Este tag ya fue usado. Pide un tag nuevo en el colegio.» |
| `TAG_YA_ACTIVO` | «Este tag ya está activo.» (se trata como éxito en el asistente) |
| `TAG_NO_COMPATIBLE` | «Este tag no es compatible con Luquita» |
| `TAG_BLOQUEADO` | «Este tag está bloqueado. El apoderado debe desbloquearlo desde su app.» |
| `FAMILIA_INACTIVA` | Vendedor: «La familia del alumno no está activa en Luquita. Consulta con la secretaría del colegio.» · Apoderado: «Tu familia no está activa en Luquita. Consulta con la secretaría del colegio.» |
| `DNI_NO_REGISTRADO` | «Tu DNI no está registrado en Luquita. Consulta con la secretaría del colegio.» |
| `LIMITE_DIARIO_EXCEDIDO` | «La compra supera el límite diario de S/ 30.00.» |
| `LIMITE_MENSUAL_EXCEDIDO` | «La compra supera el tope mensual del alumno.» |
| `MES_FUTURO` | «Ese mes todavía no empieza.» |
| `CUENTA_BLOQUEADA` | «Por seguridad, tu cuenta se bloqueó por 5 intentos fallidos. Podrás intentarlo de nuevo a las HH:MM.» |
| `SERVICIO_NO_DISPONIBLE` | «Luquita no está disponible en este momento. Inténtalo en unos minutos.» (en un cobro se agrega «No se cobró.») |
| Sin red (cliente) | Vendedor: «Sin conexión: no se puede cobrar con Luquita» · Otras: «Sin conexión. Revisa tu internet e inténtalo de nuevo.» |

---

### 3.1 Panel web (W)

#### Estructura común del panel (layout `marco-panel`)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ [Saltar al contenido]  (visible solo con foco de teclado)                     │
├──────────────────────────────────────────────────────────────────────────────┤
│ BARRA SUPERIOR (alto 64 px, fondo --color-superficie, borde inferior)         │
│  Luquita · Colegio Prueba                  Secretaria del colegio [Cerrar sesión]│
├──────────────┬───────────────────────────────────────────────────────────────┤
│ NAV LATERAL  │ CONTENIDO (<main id="contenido">, máx. 1200 px, padding 32 px)  │
│ (ancho 240px)│                                                               │
│ • Inicio     │  <h1> Título de la pantalla                                    │
│ • Familias   │  Migas: Inicio › Familias                                      │
│ • Importar   │                                                               │
│   familias   │  …                                                            │
│ • Consolidado│                                                               │
│ • App de     │                                                               │
│   cobro      │                                                               │
└──────────────┴───────────────────────────────────────────────────────────────┘
```

- Nav de la **secretaria:** Inicio, Familias, Importar familias, Consolidado, App de cobro. Nav del **administrador:** Colegios. Ítem activo: fondo `--color-primario-suave`, texto `--color-primario`, barra izquierda de 4 px y `aria-current="page"`.
- Menos de 960 px de ancho (o texto al 200 %): la nav lateral se vuelve un botón «Menú» (`aria-expanded`) en la barra superior que abre un panel.
- El rol se muestra como texto: «Secretaria del colegio» o «Administrador de Luquita». Nunca se muestra el DNI.
- Antes de iniciar sesión y en P-002 no se muestra la nav.

---

#### P-001 — Iniciar sesión (personal)

- **Propósito:** que la secretaria (con DNI) o el administrador (con correo) entren al panel.
- **Actor:** secretaria del colegio, administrador de Luquita.
- **HU:** HU-001, HU-003.
- **Ruta:** `/ingresar`.
- **Datos que muestra:** ninguno del servidor.
- **Acciones:** ingresar; mostrar u ocultar la clave.
- **Reglas:** RN-029 (5 fallos → 900 s), RN-068 (secretaria con DNI), RN-071 (correo sin distinguir mayúsculas), PRINCIPIO-008 (mismo mensaje si el usuario no existe).
- **Validación local:** ambos campos obligatorios. No se valida si es DNI o correo: lo decide el servidor (una secretaria que escribe su correo recibe el mismo `401`).

**Wireframe:**

```
┌──────────────────────────────────────────────────────────────┐
│                 (fondo --color-fondo, centrado)              │
│        ┌────────────────────────────────────────────┐        │
│        │  [logotipo textual «Luquita», 28 px, bold]   │        │
│        │  <h1> Panel del colegio                      │        │
│        │  Ingresa con tu DNI (secretaría) o tu        │        │
│        │  correo (administración).                    │        │
│        │                                              │        │
│        │  DNI o correo                                │        │
│        │  [__________________________________]        │        │
│        │                                              │        │
│        │  Clave                                       │        │
│        │  [____________________________] [👁 Mostrar] │        │
│        │                                              │        │
│        │  ┌ zona de error (aria-live=assertive) ───┐  │        │
│        │  │ ⓘ Usuario o clave incorrectos.          │  │        │
│        │  └─────────────────────────────────────────┘  │        │
│        │                                              │        │
│        │  [        Ingresar        ]  (ancho completo) │        │
│        │                                              │        │
│        │  ¿Olvidaste tu clave? Pide al equipo de       │        │
│        │  Luquita que la regenere.                     │        │
│        └────────────────────────────────────────────┘        │
│                 tarjeta: ancho máx. 420 px, padding 32 px    │
└──────────────────────────────────────────────────────────────┘
```

**Estados:**

| Estado | Comportamiento |
|---|---|
| Inicial | Foco en «DNI o correo». Botón habilitado. |
| Cargando | Botón con indicador y texto «Ingresando…», deshabilitado. |
| Error de credenciales | Banner de error con «Usuario o clave incorrectos.»; se limpia la clave; foco en «Clave». |
| Cuenta bloqueada | Banner de advertencia con el texto de `CUENTA_BLOQUEADA` y la hora; botón deshabilitado hasta esa hora (se habilita solo, sin recargar). |
| Sesión terminada | Banner informativo «Tu sesión terminó. Vuelve a iniciar sesión.» |
| Éxito | `cambioClavePendiente: true` → P-002; si no: administrador → P-010, secretaria → P-020. |

- **Componentes:** `input[type=text]` con `autocomplete="username"`; `input[type=password]` con `autocomplete="current-password"`; botón de mostrar clave con `aria-label="Mostrar clave"` / «Ocultar clave» y `aria-pressed`.

---

#### P-002 — Cambia tu clave

- **Propósito:** cambio obligatorio de la clave recibida por correo antes de cualquier otra acción.
- **Actor:** secretaria, administrador (el vendedor tiene su versión P-202).
- **HU:** HU-001, HU-003.
- **Ruta:** `/cambiar-clave` (única ruta accesible con `cambioClavePendiente: true`; un guard redirige cualquier otra).
- **Datos que muestra:** ninguno.
- **Acciones:** guardar la clave nueva; cerrar sesión.
- **Reglas:** RN-073 (cambio obligatorio), RN-105 (≥ 12 caracteres, mayúsculas, minúsculas y números, distinta de la recibida).

**Wireframe:**

```
┌──────────────────────────────────────────────────────────────┐
│        ┌────────────────────────────────────────────┐        │
│        │  <h1> Cambia tu clave                        │        │
│        │  Por seguridad, cambia la clave que          │        │
│        │  recibiste por correo antes de continuar.    │        │
│        │                                              │        │
│        │  Clave recibida por correo                   │        │
│        │  [____________________________] [👁]          │        │
│        │                                              │        │
│        │  Clave nueva                                 │        │
│        │  [____________________________] [👁]          │        │
│        │  Tu clave nueva debe tener:  (lista viva)     │        │
│        │   ✓ Al menos 12 caracteres                   │        │
│        │   ✗ Una letra mayúscula                      │        │
│        │   ✓ Una letra minúscula                      │        │
│        │   ✗ Un número                                │        │
│        │   ✓ Ser distinta de la clave recibida        │        │
│        │                                              │        │
│        │  Repite la clave nueva                       │        │
│        │  [____________________________] [👁]          │        │
│        │  ⓘ Las claves no coinciden. (si aplica)      │        │
│        │                                              │        │
│        │  [      Guardar clave nueva      ]           │        │
│        │  [ Cerrar sesión ]  (botón texto)            │        │
│        └────────────────────────────────────────────┘        │
└──────────────────────────────────────────────────────────────┘
```

**Estados:**

| Estado | Comportamiento |
|---|---|
| Inicial | «Guardar» deshabilitado hasta cumplir los 5 requisitos y que las claves coincidan. |
| Requisito cumplido / no cumplido | Ícono ✓ verde + texto, o ✗ gris + texto (nunca solo color). La lista tiene `aria-live="polite"` y cada ítem dice «cumplido» o «pendiente» al lector. |
| Error del servidor | `400 DATOS_INVALIDOS` → «La clave nueva no cumple los requisitos.» · `401 CREDENCIALES_INVALIDAS` → «La clave recibida no es correcta.» (en el primer campo). |
| Éxito | Aviso «Clave actualizada» y redirección: administrador → P-010; secretaria → P-020. |

---

#### P-010 — Colegios (lista)

- **Propósito:** ver los colegios registrados y entrar a cada uno.
- **Actor:** administrador de Luquita.
- **HU:** HU-001.
- **Ruta:** `/admin/colegios`.
- **Datos:** `GET /admin/colegios` → `nombre`, `colegioId`, `tiendaId`. El nombre se muestra exactamente como se guardó, con tildes (HU-001).
- **Acciones:** registrar colegio (abre P-011); abrir el detalle (P-012).
- **Reglas:** RN-065; solo `ADMIN_LUQUITA`.

**Wireframe:**

```
┌ marco-panel (nav: Colegios) ───────────────────────────────────────────────┐
│ <h1> Colegios                                   [ + Registrar colegio ]     │
│                                                                             │
│ ┌ tabla «Colegios registrados» ────────────────────────────────────────┐   │
│ │ Colegio                          │ Tienda              │             │   │
│ │──────────────────────────────────┼─────────────────────┼─────────────│   │
│ │ Colegio San José de Chosica      │ Tienda del colegio  │ [Ver detalle]│  │
│ └──────────────────────────────────────────────────────────────────────┘   │
│ Total: 1 colegio                                                            │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Estados:** cargando (filas esqueleto con `aria-busy="true"`); vacío («Todavía no hay colegios registrados.» + botón «Registrar colegio»); error («No se pudo cargar la lista. [Reintentar]»); con datos. El botón «Ver detalle» tiene `aria-label="Ver detalle de Colegio San José de Chosica"`.

---

#### P-011 — Registrar colegio (diálogo)

- **Propósito:** registrar un colegio y su tienda.
- **Actor:** administrador. **HU:** HU-001.
- **Datos que envía:** `nombre`, `nombreTienda` (con valor inicial «Tienda del colegio»).
- **Reglas:** nombre obligatorio, hasta 100 caracteres, único (pendiente 12 decidido).

**Wireframe (diálogo modal, `role="dialog"`, `aria-modal="true"`, ancho 480 px):**

```
┌─────────────────────────────────────────────┐
│ <h2> Registrar colegio                 [✕]   │  (✕ con aria-label «Cerrar»)
│                                             │
│ Nombre del colegio *                        │
│ [_______________________________________]   │
│ 0/100                                       │
│                                             │
│ Nombre de la tienda *                       │
│ [Tienda del colegio_____________________]   │
│ Aparece en el correo de cada compra.        │
│                                             │
│ ⓘ Ya existe un colegio con ese nombre.      │
│                                             │
│            [ Cancelar ]  [ Registrar ]      │
└─────────────────────────────────────────────┘
```

**Estados:** validación local (obligatorio, máx. 100); enviando; `409 RECURSO_DUPLICADO` → error en «Nombre del colegio»; éxito → cierra, aviso «Colegio registrado» y la lista se actualiza. El foco queda atrapado en el diálogo y vuelve al botón que lo abrió al cerrar. `Esc` cierra.

---

#### P-012 — Detalle del colegio

- **Propósito:** gestionar la secretaria y el dueño de un colegio, ver si recibieron su clave, activar la app de cobro e importar familias.
- **Actor:** administrador.
- **HU:** HU-002, HU-004, HU-033.
- **Ruta:** `/admin/colegios/:colegioId`.
- **Datos:**
  - Colegio: nombre y tienda.
  - Usuarios: `GET /admin/colegios/{colegioId}/usuarios` (lista al entrar y al volver); rol, correo, estado del envío de la clave (`PENDIENTE` → «Enviando…», `ENVIADO` → «Clave enviada», `FALLIDO` → «Clave no enviada»), `cambioClavePendiente` → «Pendiente de primer ingreso» / «Activo», `bloqueadoHasta`. **Nunca** el DNI de la secretaria ni la clave.
  - App de cobro: `GET /colegios/{colegioId}/ventas` → estado y hora del último cambio.
- **Acciones:** registrar usuario (P-013); actualizar el estado del envío; activar o desactivar la app de cobro (componente compartido de P-025); importar familias (P-021 con el colegio fijado).
- **Reglas:** RN-066, RN-067, RN-072, RN-074, RN-075, RN-101; procedimiento manual §6.7 si la clave no se envió.
- **Nota:** el panel carga los usuarios con `GET /admin/colegios/{colegioId}/usuarios` (Q-01 resuelta el 2026-11-14), así que la secretaria y el dueño ya registrados aparecen al volver a entrar.

**Wireframe:**

```
┌ marco-panel (nav: Colegios) ───────────────────────────────────────────────┐
│ Migas: Colegios › Colegio San José de Chosica                               │
│ <h1> Colegio San José de Chosica                                            │
│ Tienda: Tienda del colegio                                                  │
│                                                                             │
│ ┌ tarjeta «App de cobro» (componente interruptor-app-cobro, ver P-025) ──┐  │
│ │ ● Activa desde 2027-03-01 07:30     [ Desactivar app de cobro ]        │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│ ┌ sección «Personal del colegio» ─────────────────── [ + Registrar usuario ]┐│
│ │ Rol                   │ Correo                      │ Clave           │ Estado                    ││
│ │───────────────────────┼─────────────────────────────┼─────────────────┼───────────────────────────││
│ │ Secretaria del colegio│ secretaria@colegioprueba.pe │ ✓ Clave enviada │ Pendiente de primer ingreso││
│ │ Dueño de la tienda    │ tienda@colegioprueba.pe     │ ⚠ Clave no      │ Pendiente de primer ingreso││
│ │                       │                             │   enviada       │                           ││
│ └──────────────────────────────────────────────────────────────── [↻ Actualizar estado]┘│
│ ⚠ banner (si hay «Clave no enviada»):                                        │
│   «No se pudo enviar la clave a tienda@colegioprueba.pe. Pide al equipo de    │
│    Luquita que la regenere con el procedimiento manual.»                     │
│                                                                             │
│ ┌ sección «Familias» ────────────────────────────────────────────────────┐  │
│ │ Importa el Excel de familias de este colegio.  [ Importar familias ]   │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Estados:** cargando; colegio inexistente (`404`) → «No encontramos este colegio.» + volver; sin usuarios («Todavía no hay personal registrado.»); «Enviando…» se actualiza solo cada 10 s durante 2 minutos (luego queda el botón «Actualizar estado»); usuario bloqueado → etiqueta «Bloqueado hasta 08:15».

- Las etiquetas de estado combinan ícono + texto: ✓ verde «Clave enviada», ⏳ gris «Enviando…», ⚠ rojo «Clave no enviada».

---

#### P-013 — Registrar secretaria o dueño de tienda (diálogo)

- **Propósito:** crear un usuario del personal; el sistema genera la clave y la envía por correo.
- **Actor:** administrador. **HU:** HU-002.
- **Datos que envía:** `rol`, `dni` (solo secretaria), `correo`.
- **Reglas:** RN-066, RN-067 (el administrador nunca ve la clave), RN-052 / pendiente 12 (DNI de 8 dígitos), correo con formato `usuario@dominio` y único.

**Wireframe (diálogo, ancho 520 px):**

```
┌───────────────────────────────────────────────────┐
│ <h2> Registrar usuario                       [✕]   │
│                                                   │
│ Rol *  (radiogroup)                               │
│ (•) Secretaria del colegio                        │
│ ( ) Dueño de la tienda                            │
│                                                   │
│ DNI de la secretaria *   (solo si el rol es Secretaria)│
│ [________]  8 dígitos                             │
│ ⓘ DNI inválido                                     │
│                                                   │
│ Correo *                                          │
│ [__________________________________________]      │
│                                                   │
│ ┌ info ────────────────────────────────────────┐  │
│ │ ⓘ Luquita generará una clave y la enviará a  │  │
│ │   este correo. Tú no la verás.               │  │
│ └──────────────────────────────────────────────┘  │
│                                                   │
│                  [ Cancelar ]  [ Registrar ]      │
└───────────────────────────────────────────────────┘
```

**Estados y mensajes:**

| Caso | Mensaje (en el campo) |
|---|---|
| Secretaria sin DNI | «Falta el DNI de la secretaria» |
| DNI que no tiene 8 dígitos (`0000001`) | «DNI inválido» |
| Sin correo | «Falta el correo» |
| Correo sin formato | «Ingresa un correo válido, por ejemplo nombre@dominio.pe» |
| `409 RECURSO_DUPLICADO` | «Este correo ya está en uso.» |
| Éxito | Cierra; aviso «Usuario registrado. Le enviamos su clave por correo.»; la fila aparece en P-012 con «Enviando…». |

- El campo DNI usa `inputmode="numeric"`, `maxlength="8"`, `pattern="[0-9]{8}"` y aparece o desaparece según el rol (`aria-live` anuncia el cambio).

---

#### P-020 — Inicio de la secretaria

- **Propósito:** punto de entrada con el estado de la app de cobro y accesos a las tareas frecuentes.
- **Actor:** secretaria.
- **HU:** HU-033 (estado y cambio de la app de cobro), HU-004, HU-005, HU-018 (accesos).
- **Ruta:** `/colegio`.
- **Datos:** estado de la app de cobro (`GET /colegios/{colegioId}/ventas`); total de familias (`totalElementos` de `GET /colegio/familias?pagina=0&tamano=1`).
- **Acciones:** activar o desactivar la app de cobro; ir a importar, familias o consolidado.
- **Reglas:** RN-101, RN-102.

**Wireframe:**

```
┌ marco-panel (nav: Inicio) ─────────────────────────────────────────────────┐
│ <h1> Inicio                                                                 │
│ Colegio Prueba                                                              │
│                                                                             │
│ ┌ tarjeta destacada «App de cobro» (ancho completo) ─────────────────────┐  │
│ │ <h2> App de cobro de la tienda                                         │  │
│ │ ● Activa — los alumnos pueden comprar con Luquita.                     │  │
│ │ Último cambio: 2027-03-01 07:30                                        │  │
│ │                                   [ Desactivar app de cobro ]          │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│ ┌ tarjeta ─────────────┐ ┌ tarjeta ─────────────┐ ┌ tarjeta ─────────────┐  │
│ │ <h2> Familias        │ │ <h2> Importar         │ │ <h2> Consolidado     │  │
│ │ 150 familias         │ │ Sube el Excel de las  │ │ Consumo por familia  │  │
│ │ registradas          │ │ familias después de   │ │ para la boleta.      │  │
│ │                      │ │ la matrícula.         │ │                      │  │
│ │ [ Ver familias ]     │ │ [ Importar familias ] │ │ [ Ver consolidado ]  │  │
│ └──────────────────────┘ └───────────────────────┘ └──────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Estados de la tarjeta «App de cobro»:**

| Estado | Visual |
|---|---|
| Activa | Punto verde + «Activa — los alumnos pueden comprar con Luquita.»; botón secundario «Desactivar app de cobro». |
| Desactivada | Punto gris oscuro + «Desactivada — nadie puede comprar con Luquita.»; banner de advertencia «Si la app de cobro está desactivada, los alumnos no pueden comprar.»; botón primario «Activar app de cobro». |
| Cargando / error | Esqueleto · «No se pudo consultar el estado. [Reintentar]». |

- Tarjetas en grilla de 3 columnas (≥ 1024 px), 2 (600–1023 px) y 1 (< 600 px o 200 %).

---

#### P-021 — Importar familias (Excel)

- **Propósito:** registrar de una vez las familias que se suman al servicio después de la matrícula.
- **Actor:** secretaria (su colegio) o administrador (colegio elegido en P-012).
- **HU:** HU-004.
- **Ruta:** secretaria `/colegio/importar`; administrador `/admin/colegios/:colegioId/importar`.
- **Datos:** resultado de `POST /colegios/{colegioId}/familias/importaciones`: `anioEscolar`, `registradas`, `actualizadas`, `rechazadas[]` (`fila`, `motivo`).
- **Acciones:** descargar la plantilla; elegir archivo (arrastrar o botón); importar; ver familias; importar otro archivo.
- **Reglas:** RN-045, RN-046, RN-047, RN-051, RN-052 (8 dígitos), RN-053 (todas `ACTIVO`), RN-054 (actualiza familias que ya existían), RN-088 (año escolar del año calendario de la importación), RN-098 (DNI repetido en el archivo: vale la primera fila); pendiente 10 (plantilla con la columna DNI en texto).
- **Validación local:** extensión `.xlsx` y tamaño ≤ 2 MB antes de enviar.

**Wireframe:**

```
┌ marco-panel (nav: Importar familias) ──────────────────────────────────────┐
│ <h1> Importar familias                                                      │
│ Sube el Excel con las familias que se suman a Luquita este año. Todas las   │
│ familias del archivo quedarán activas.                                      │
│                                                                             │
│ ┌ paso 1 «Prepara el archivo» ───────────────────────────────────────────┐  │
│ │ El archivo debe ser .xlsx (máx. 2 MB) con estas columnas:              │  │
│ │   dni_apoderado · nombre · apellidos · correo                          │  │
│ │ Escribe el DNI como texto para no perder los ceros iniciales.          │  │
│ │ [ ⬇ Descargar plantilla ]                                              │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│ ┌ paso 2 «Sube el archivo» ──────────────────────────────────────────────┐  │
│ │ ┌ zona de carga (borde discontinuo 2 px, alto 160 px) ───────────────┐ │  │
│ │ │        ⬆  Arrastra el archivo aquí o  [ Elegir archivo ]           │ │  │
│ │ └────────────────────────────────────────────────────────────────────┘ │  │
│ │ Archivo elegido: familias-2027.xlsx (48 KB)   [ Quitar ]              │  │
│ │ Las familias se asociarán al año escolar 2027.                         │  │
│ │                                         [ Importar familias ]          │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│ ┌ resultado (aria-live=polite, aparece después de importar) ─────────────┐  │
│ │ ✓ <h2> Importación terminada — año escolar 2027                        │  │
│ │ 148 familias registradas — 2 filas rechazadas: fila 12 (falta DNI),    │  │
│ │ fila 87 (DNI duplicado)                                                │  │
│ │ Familias actualizadas: 0                                               │  │
│ │                                                                        │  │
│ │ <h3> Filas rechazadas                                                  │  │
│ │ │ Fila │ Motivo                          │                             │  │
│ │ │──────┼─────────────────────────────────│                             │  │
│ │ │ 12   │ Falta el DNI del apoderado      │                             │  │
│ │ │ 87   │ DNI duplicado                   │                             │  │
│ │ Corrige estas filas y regístralas a mano o súbelas en otro archivo.    │  │
│ │                                                                        │  │
│ │ [ Ver familias ]   [ Importar otro archivo ]                           │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Estados:**

| Estado | Comportamiento |
|---|---|
| Sin archivo | «Importar familias» deshabilitado. |
| Archivo no `.xlsx` | Error local: «Solo se aceptan archivos .xlsx.»; no se envía. |
| Archivo > 2 MB | «El archivo pesa más de 2 MB.» |
| Importando | Indicador «Importando familias…»; botón deshabilitado; la `Idempotency-Key` se conserva para un reintento. |
| Falta una columna (`400`) | Banner de error: «El archivo no tiene la columna `correo`. No se registró ninguna familia.» |
| Solo encabezados | Resultado: «0 familias registradas — 0 filas rechazadas» (sin error técnico). |
| Todo válido | Resultado sin la tabla de rechazadas. |
| Con rechazadas | Tabla de rechazadas; cuando hay más de 20, se pagina de a 20 y se ofrece «Copiar lista de filas rechazadas». |

- La frase del resultado sigue el formato de HU-004: «{registradas} familias registradas — {n} filas rechazadas: fila {fila} ({motivo corto}), …». En la frase, el motivo corto es «falta DNI», «DNI inválido» o «DNI duplicado»; en la tabla va el texto completo del servidor.

---

#### P-022 — Familias (búsqueda y lista)

- **Propósito:** buscar una familia por DNI del apoderado o recorrer la lista, para registrar, corregir o dar de baja.
- **Actor:** secretaria. **HU:** HU-005.
- **Ruta:** `/colegio/familias`.
- **Datos:** `GET /colegio/familias?dni=…` o `?pagina=&tamano=20`: apoderado (nombre y apellidos), correo, estado, `tieneCuenta`, `numeroAlumnos`. **No** muestra el DNI ni el consumo.
- **Acciones:** buscar por DNI; limpiar búsqueda; paginar; registrar familia (P-023); editar (P-024).
- **Reglas:** RN-052 (búsqueda solo con 8 dígitos), PRINCIPIO-007.

**Wireframe:**

```
┌ marco-panel (nav: Familias) ───────────────────────────────────────────────┐
│ <h1> Familias                                   [ + Registrar familia ]     │
│                                                                             │
│ ┌ form role=search «Buscar familia» ─────────────────────────────────────┐  │
│ │ DNI del apoderado                                                      │  │
│ │ [________]  [ Buscar ]  [ Limpiar ]                                    │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│ ┌ tabla «Familias registradas» ──────────────────────────────────────────┐  │
│ │ Apoderado             │ Correo               │ Estado     │ Cuenta en la app │ Hijos │          │ │
│ │───────────────────────┼──────────────────────┼────────────┼──────────────────┼───────┼──────────│ │
│ │ Apoderado Prueba Uno  │ apoderado@ejemplo.pe │ ● Activa   │ ✓ Creada         │ 2     │ [Editar] │ │
│ │ Apoderado Prueba Dos  │ otro@ejemplo.pe      │ ○ Inactiva │ — Sin cuenta     │ 0     │ [Editar] │ │
│ └────────────────────────────────────────────────────────────────────────┘  │
│ Mostrando 1–20 de 150      [‹ Anterior]  Página 1 de 8  [Siguiente ›]       │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Estados:** cargando; lista vacía («Todavía no hay familias. [Importar familias] o [Registrar familia]»); búsqueda sin resultado («No hay ninguna familia con ese DNI. [Registrar familia]»); DNI con formato inválido → «DNI inválido» en el campo y no se consulta; error de red.

- Estado: «● Activa» (verde) / «○ Inactiva» (gris oscuro con borde). El botón «Editar» tiene `aria-label="Editar familia de Apoderado Prueba Uno"`.
- Debajo de 600 px cada fila se vuelve una tarjeta con las mismas etiquetas.

---

#### P-023 — Registrar familia

- **Propósito:** registrar una familia que se suscribió después de la importación.
- **Actor:** secretaria. **HU:** HU-005.
- **Ruta:** `/colegio/familias/nueva`.
- **Datos que envía:** `dniApoderado`, `nombre`, `apellidos`, `correo`.
- **Reglas:** RN-052 (8 dígitos), RN-086 (un apoderado por familia; DNI único), RN-088 (año escolar), pendiente 12 (nombre y apellidos ≤ 100, correo `usuario@dominio`), PRINCIPIO-007 (solo esos 4 datos).

**Wireframe:**

```
┌ marco-panel (nav: Familias) ───────────────────────────────────────────────┐
│ Migas: Familias › Registrar familia                                         │
│ <h1> Registrar familia                                                      │
│ La familia quedará activa desde hoy.                                        │
│                                                                             │
│ ┌ formulario (ancho máx. 560 px) ─────────────────────────────────────┐     │
│ │ DNI del apoderado *                                                 │     │
│ │ [________]  8 dígitos                                               │     │
│ │                                                                     │     │
│ │ Nombre del apoderado *                                              │     │
│ │ [_____________________________________]                             │     │
│ │                                                                     │     │
│ │ Apellidos del apoderado *                                           │     │
│ │ [_____________________________________]                             │     │
│ │                                                                     │     │
│ │ Correo del apoderado *                                              │     │
│ │ [_____________________________________]                             │     │
│ │ Aquí llegará el aviso de cada compra.                               │     │
│ │                                                                     │     │
│ │                       [ Cancelar ]  [ Registrar familia ]           │     │
│ └─────────────────────────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Mensajes:** «DNI inválido» (`00A00000`, 7 o 9 dígitos); «Completa este campo»; «Máximo 100 caracteres»; «Ingresa un correo válido, por ejemplo nombre@dominio.pe»; `409 RECURSO_DUPLICADO` → «Ya existe una familia con ese DNI. [Buscarla]». Éxito → P-022 con aviso «Familia registrada: Apoderado Prueba Uno (año escolar 2027).»

---

#### P-024 — Editar familia (correo y estado)

- **Propósito:** corregir el correo del apoderado o pasar la familia a activa o inactiva.
- **Actor:** secretaria. **HU:** HU-005.
- **Ruta:** `/colegio/familias/:familiaId`.
- **Datos:** apoderado (nombre y apellidos, solo lectura), correo, estado, cuenta en la app, número de hijos. Sin DNI.
- **Acciones:** guardar correo; pasar a inactiva; volver a activar.
- **Reglas:** RN-022 y RN-087 (solo la secretaria cambia el correo; aplica de inmediato), RN-056 y RN-104 (con `INACTIVO` los hijos no compran), pendiente 14.

**Wireframe:**

```
┌ marco-panel (nav: Familias) ───────────────────────────────────────────────┐
│ Migas: Familias › Apoderado Prueba Uno                                      │
│ <h1> Apoderado Prueba Uno                         Estado: ● Activa          │
│ Cuenta en la app: ✓ Creada · Hijos registrados: 2                           │
│                                                                             │
│ ┌ sección «Correo del apoderado» ─────────────────────────────────────┐     │
│ │ Correo *                                                            │     │
│ │ [apoderado@ejemplo.pe_________________]                             │     │
│ │ Los próximos avisos de compra irán a este correo.                   │     │
│ │                                        [ Guardar correo ]           │     │
│ └─────────────────────────────────────────────────────────────────────┘     │
│                                                                             │
│ ┌ sección «Estado de la familia» ─────────────────────────────────────┐     │
│ │ Mientras la familia esté inactiva, sus hijos no pueden comprar con  │     │
│ │ Luquita. El apoderado puede seguir entrando a la app y bloquear o   │     │
│ │ desbloquear tags, pero no vincular tags nuevos.                     │     │
│ │                                   [ Pasar a inactiva ] (peligro)    │     │
│ └─────────────────────────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Diálogo «Pasar a inactiva»:**

```
┌─────────────────────────────────────────────────────┐
│ <h2> ¿Pasar a inactiva a esta familia?              │
│ Los hijos de Apoderado Prueba Uno no podrán comprar │
│ con Luquita hasta que la vuelvas a activar.          │
│                [ Cancelar ]  [ Pasar a inactiva ]   │
└─────────────────────────────────────────────────────┘
```

**Estados:** con familia inactiva, el estado dice «○ Inactiva» y el botón cambia a «Volver a activar» (primario) con su propio diálogo («Sus hijos podrán volver a comprar con Luquita.»). Éxito de correo → aviso «Correo actualizado. Los próximos correos irán a nuevo@ejemplo.pe». «Guardar correo» queda deshabilitado si el correo no cambió.

---

#### P-025 — App de cobro (activar o desactivar)

- **Propósito:** decidir cuándo los alumnos pueden comprar con Luquita.
- **Actor:** secretaria (su colegio) y administrador (cualquier colegio, embebido en P-012).
- **HU:** HU-033.
- **Ruta:** `/colegio/app-de-cobro`. El componente `interruptor-app-cobro` se reutiliza en P-012 y P-020.
- **Datos:** `GET /colegios/{colegioId}/ventas` → `habilitadas`, `cambiadasEn`.
- **Acciones:** activar; desactivar (siempre con confirmación).
- **Reglas:** RN-097 (sin app activa no hay compras), RN-101 (solo secretaria o administrador), RN-102, RN-103; ADR-010. Repetir el mismo valor no da error.

**Wireframe:**

```
┌ marco-panel (nav: App de cobro) ───────────────────────────────────────────┐
│ <h1> App de cobro de la tienda                                              │
│ Los alumnos solo pueden comprar con Luquita mientras la app de cobro esté   │
│ activa. Actívala cuando empiecen las clases y desactívala en vacaciones.    │
│                                                                             │
│ ┌ tarjeta estado (borde izquierdo 6 px: verde si activa, gris si no) ────┐  │
│ │  ● Activa                                                              │  │
│ │  Los alumnos pueden comprar con Luquita.                               │  │
│ │  Último cambio: 2027-03-01 07:30                                       │  │
│ │                                                                        │  │
│ │                                [ Desactivar app de cobro ]             │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Diálogos:**

- Desactivar: «¿Desactivar la app de cobro? Desde ahora nadie podrá comprar con Luquita en la tienda hasta que la vuelvas a activar.» `[Cancelar]` `[Desactivar]`.
- Activar: «¿Activar la app de cobro? Los alumnos podrán comprar con Luquita desde ahora.» `[Cancelar]` `[Activar]`.

**Estados:** activa · desactivada · guardando · error de red («No se pudo cambiar el estado. La app de cobro sigue {activa|desactivada}. [Reintentar]») · `403` (secretaria de otro colegio). Se usa un **botón**, no un switch, para que la acción sea explícita y vaya siempre con su confirmación; el botón lleva `aria-describedby` hacia el texto del estado actual.

---

#### P-026 — Consolidado mensual

- **Propósito:** obtener el consumo de cada alumno por familia de un mes, para cargarlo en la boleta.
- **Actor:** secretaria. **HU:** HU-018.
- **Ruta:** `/colegio/consolidado?anio=2026&mes=11`.
- **Datos:** `GET /colegio/consolidados/{anio}/{mes}`: `mes`, `mesEnCurso`, `calculadoEn`, `totalColegio`, `familias[]` con `apoderado`, `alumnos[]` (`nombre`, `dni`, `total`) y `totalFamilia`.
- **Acciones:** elegir año y mes; consultar; filtrar por apoderado o alumno (filtro local); descargar Excel (Should, T-077); imprimir.
- **Reglas:** RN-031 (sin cargo de suscripción), RN-033 (sin compras individuales), RN-034 (fecha del servidor), RN-063 (mes calendario en hora de Lima), PRINCIPIO-003 y PRINCIPIO-007. Una familia inactiva solo aparece si tiene consumo en el mes (pendiente 14).

**Wireframe:**

```
┌ marco-panel (nav: Consolidado) ────────────────────────────────────────────┐
│ <h1> Consolidado mensual                                                    │
│ Consumo de cada alumno por familia, para incluirlo en la boleta.            │
│                                                                             │
│ ┌ barra de consulta ─────────────────────────────────────────────────────┐  │
│ │ Mes [ noviembre ▼ ]  Año [ 2026 ▼ ]  [ Consultar ]                     │  │
│ │                                   [ ⬇ Descargar Excel ]  [ 🖨 Imprimir ]│  │
│ └────────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│ ┌ banner advertencia (solo si mesEnCurso) ──────────────────────────────┐   │
│ │ ⚠ Mes en curso: los totales pueden cambiar                            │   │
│ └───────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│ ┌ resumen ───────────────────────────────────────────────────────────────┐  │
│ │ <h2> noviembre de 2026                                                 │  │
│ │ Total del colegio: S/ 18 450.00   ·  Calculado: 2026-12-02 09:00       │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│ Filtrar por apoderado o alumno [____________________]                       │
│                                                                             │
│ ┌ tabla «Consolidado de noviembre de 2026» ──────────────────────────────┐  │
│ │ Familia (apoderado)   │ Alumno            │ DNI del alumno │  Total     │  │
│ │───────────────────────┼───────────────────┼────────────────┼────────────│  │
│ │ Apoderado Prueba Uno  │ Alumno Prueba 1   │ 00000010       │ S/ 142.00  │  │
│ │                       │ Alumno Prueba 2   │ 00000011       │  S/ 98.50  │  │
│ │                       │ Total familia     │                │ S/ 240.50  │  │ (fila en negrita, fondo suave)
│ │───────────────────────┼───────────────────┼────────────────┼────────────│  │
│ │ Apoderado Prueba Dos  │ Alumno Prueba 3   │ 00000012       │   S/ 0.00  │  │
│ │                       │ Total familia     │                │   S/ 0.00  │  │
│ └────────────────────────────────────────────────────────────────────────┘  │
│ 150 familias · los montos solo incluyen el consumo de la tienda.            │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Estados:**

| Estado | Comportamiento |
|---|---|
| Inicial | Abre en el mes anterior al actual (UD-09) y consulta de inmediato. |
| Cargando | Esqueleto de la tabla con `aria-busy="true"`. |
| Mes en curso | Banner de advertencia «Mes en curso: los totales pueden cambiar». |
| Mes futuro | Desde el selector, los meses futuros se muestran deshabilitados. Si llega `422 MES_FUTURO` (por URL): «Ese mes todavía no empieza.» sin tabla. |
| Sin familias | «No hay familias con consumo ni familias activas en este mes.» |
| Error | «No se pudo calcular el consolidado. [Reintentar]» |

- Los montos se alinean a la derecha, con cifras tabulares (`font-variant-numeric: tabular-nums`). Nunca se suman montos en Angular: todos los totales vienen del servidor (PRINCIPIO-004).
- La tabla usa `<th scope="col">` y, por familia, `<th scope="rowgroup">`. Al imprimir se ocultan la nav y la barra de consulta.
- No hay ninguna columna ni enlace con compras individuales.

---

#### P-027 — Registrar ajuste (Should, S1)

- **Propósito:** corregir en el mes en curso el consumo de un mes anterior ya entregado al colegio.
- **Actor:** secretaria. **HU:** HU-027.
- **Datos:** familia (buscada por DNI del apoderado), alumno, monto con signo, motivo.
- **Reglas:** RN-036 y RN-064 (el ajuste siempre queda en el mes en curso), RN-080 (no cuenta para los límites), RN-081 (sí cambia el consolidado), PRINCIPIO-004.

**Wireframe:**

```
┌ marco-panel (nav: Ajustes) ────────────────────────────────────────────────┐
│ <h1> Registrar ajuste                                                       │
│ El ajuste quedará en el consolidado de noviembre de 2026 (mes en curso).    │
│                                                                             │
│ DNI del apoderado * [________] [ Buscar ]                                   │
│ Familia: Apoderado Prueba Uno                                               │
│ Alumno *  [ Alumno Prueba 1 ▼ ]                                             │
│ Tipo *   (•) Descuento (resta)   ( ) Cargo (suma)                           │
│ Monto *  S/ [______]                                                        │
│ Motivo * [_____________________________________________]  0/60              │
│                                                                             │
│ Resumen: −S/ 4.00 para Alumno Prueba 1 en noviembre de 2026.                │
│ Este ajuste no cuenta para los límites de consumo.                          │
│                                    [ Cancelar ]  [ Registrar ajuste ]       │
└─────────────────────────────────────────────────────────────────────────────┘
```

- El signo se elige con radio (no con el carácter «−»), para que sea accesible y no haya ambigüedad. Confirmación obligatoria antes de enviar: los movimientos no se editan ni se borran.
- Pendiente: el endpoint no está en la arquitectura (S1 está fuera del MVP).

---

### 3.2 App Apoderados (A)

#### Estructura común de la app

- Barra superior Material 3 (alto 64 dp) con título y, en pantallas internas, botón «‹» con `Semantics(label: 'Volver')`.
- Sin barra de navegación inferior: el MVP tiene un solo inicio (P-104). Menú de la barra superior (⋮): «Cerrar sesión».
- Márgenes laterales de 16 dp; botones principales de ancho completo, alto 56 dp, fijados al final de la pantalla con la zona segura.
- Pensada para Android gama baja: sin animaciones pesadas, ilustraciones vectoriales simples, listas sin paginación (pocos hijos por familia).
- Los PIN se ingresan con el componente `campo-pin`: casillas que muestran `•`, teclado numérico del sistema (`TextInputType.number`, sin sugerencias) y opción «Mostrar» por 3 segundos.

---

#### P-101 — Bienvenida

- **Propósito:** elegir entre crear cuenta e iniciar sesión.
- **Actor:** apoderado. **HU:** HU-006, HU-007.
- **Datos:** ninguno.
- **Acciones:** crear cuenta (P-102); iniciar sesión (P-103).
- **Reglas:** PRINCIPIO-003 (solo familias registradas por el colegio).

**Wireframe (360 × 640 dp de referencia):**

```
┌──────────────────────────────────┐
│                                  │
│      [ilustración: tag + niño]   │  (excluida del lector)
│                                  │
│      Luquita                     │  (headline 32 sp, bold)
│      La tienda del colegio, sin  │
│      efectivo y con tu control.  │  (body 16 sp)
│                                  │
│  Para crear tu cuenta, tu familia│
│  debe estar registrada por el    │
│  colegio.                        │  (body 14 sp, texto secundario)
│                                  │
│ ┌──────────────────────────────┐ │
│ │        Crear cuenta          │ │  (primario, 56 dp)
│ └──────────────────────────────┘ │
│ ┌──────────────────────────────┐ │
│ │       Ya tengo cuenta        │ │  (secundario con borde, 56 dp)
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

- Si hay una sesión vigente (token de renovación de 30 días válido), la app salta directo a P-104.

---

#### P-102 — Crear cuenta

- **Propósito:** crear la cuenta del apoderado con su DNI y un PIN de acceso de 6 dígitos.
- **Actor:** apoderado. **HU:** HU-006.
- **Datos que envía:** `dni`, `pin` (`POST /apoderados` con `Idempotency-Key`).
- **Reglas:** RN-003 (6 dígitos numéricos; `000000` es válido), RN-058, RN-059 (DNI de una familia `ACTIVO`), RN-086 (una cuenta por familia).

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹  Crear cuenta                  │
├──────────────────────────────────┤
│ Usa el DNI con el que el colegio │
│ te registró.                     │
│                                  │
│ DNI                              │
│ [________]                       │
│ 8 dígitos                        │
│                                  │
│ Crea tu PIN de acceso            │
│ [•][•][•][•][ ][ ]   [Mostrar]   │
│ 6 números. Lo usarás para entrar.│
│                                  │
│ Repite el PIN                    │
│ [ ][ ][ ][ ][ ][ ]               │
│ ⓘ Los PIN no coinciden.          │
│                                  │
│ ┌ banner error (si aplica) ────┐ │
│ │ ⓘ Tu DNI no está registrado  │ │
│ │ en Luquita. Consulta con     │ │
│ │ la secretaría del colegio.   │ │
│ └──────────────────────────────┘ │
│                                  │
│ ┌──────────────────────────────┐ │
│ │        Crear cuenta          │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

**Estados y mensajes:**

| Caso | Mensaje / comportamiento |
|---|---|
| DNI que no tiene 8 dígitos | «DNI inválido» (en el campo) |
| PIN con menos o más de 6 dígitos o con letras (`4829`, `48a915`, `4829150`) | El campo solo acepta dígitos y como máximo 6; si llega `400 PIN_FORMATO_INVALIDO`: «El PIN debe tener 6 dígitos.» |
| PIN repetido distinto | «Los PIN no coinciden.»; botón deshabilitado |
| `422 DNI_NO_REGISTRADO` (DNI inexistente o familia inactiva) | Banner: «Tu DNI no está registrado en Luquita. Consulta con la secretaría del colegio.» |
| `409 RECURSO_DUPLICADO` | Banner: «Tu familia ya tiene una cuenta en Luquita. Inicia sesión.» + botón «Iniciar sesión» |
| Creando | Botón con indicador «Creando cuenta…» |
| Corte de red | Banner «No pudimos confirmar si tu cuenta se creó. [Reintentar]» (misma `Idempotency-Key`) |
| Éxito | P-103 con el DNI ya escrito y aviso «Cuenta creada. Ya puedes iniciar sesión.» |

- `Semantics`: el campo de PIN se anuncia «PIN de acceso, 4 de 6 dígitos ingresados»; nunca se leen los dígitos.

---

#### P-103 — Iniciar sesión

- **Propósito:** entrar con DNI y PIN de acceso.
- **Actor:** apoderado. **HU:** HU-007.
- **Datos que envía:** `dni`, `pin`.
- **Reglas:** RN-029 (5 fallos → 15 min; el 5.º intento correcto entra), RN-060, PRINCIPIO-008 (mismo mensaje si el DNI no tiene cuenta).

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹  Iniciar sesión                │
├──────────────────────────────────┤
│ DNI                              │
│ [00000000]                       │
│                                  │
│ PIN de acceso                    │
│ [•][•][•][•][•][•]   [Mostrar]   │
│                                  │
│ ┌ banner ──────────────────────┐ │
│ │ ⓘ DNI o PIN incorrectos.     │ │
│ └──────────────────────────────┘ │
│                                  │
│ ┌──────────────────────────────┐ │
│ │           Ingresar           │ │
│ └──────────────────────────────┘ │
│                                  │
│ ¿Olvidaste tu PIN? Consulta con  │
│ la secretaría del colegio para   │
│ que el equipo de Luquita lo      │
│ restablezca.                     │
└──────────────────────────────────┘
```

**Estados:** inicial; ingresando; «DNI o PIN incorrectos.» (se limpia el PIN; foco en el PIN); cuenta bloqueada (banner de advertencia con la hora; botón deshabilitado hasta esa hora); sin conexión; éxito → P-104. El botón se habilita solo con DNI de 8 dígitos y PIN de 6.

---

#### P-104 — Mis hijos (inicio)

- **Propósito:** ver a los hijos registrados y el estado de sus tags, y acceder rápido a bloquear o vincular.
- **Actor:** apoderado. **HU:** HU-008, HU-009, HU-016, HU-017.
- **Datos:** `GET /apoderado/alumnos` → por hijo: `nombre`, `topeMensual`, `familiaActiva`, `tags[]` (`estado`, `motivoBloqueo`, `activadoEn`, `bloqueadoEn`). **Sin** DNI, PIN, token ni consumo. Si `familiaActiva` es `false`, un banner arriba dice «Tu familia no está activa en Luquita. Consulta con la secretaría del colegio.» y «Vincular tag» queda deshabilitado.
- **Acciones:** registrar hijo (P-105); abrir el detalle (P-106); vincular tag (P-107); bloquear tag (acceso rápido, con el mismo diálogo de P-106).
- **Reglas:** PRINCIPIO-007 (solo sus hijos), RN-028.

**Wireframe:**

```
┌──────────────────────────────────┐
│ Mis hijos                     ⋮  │  (⋮ → «Cerrar sesión»)
├──────────────────────────────────┤
│ ┌ tarjeta hijo ────────────────┐ │
│ │ 👤 Alumno Prueba          ›  │ │  (toda la tarjeta abre P-106)
│ │ Tope mensual: S/ 300.00      │ │
│ │ ● Tag activo                 │ │  (chip verde con ícono ✓)
│ │ [ 🔒 Bloquear tag ]           │ │  (botón con borde, 48 dp)
│ └──────────────────────────────┘ │
│ ┌ tarjeta hijo ────────────────┐ │
│ │ 👤 Alumno Prueba 2        ›  │ │
│ │ Tope mensual: S/ 300.00      │ │
│ │ ⛔ Tag bloqueado: 3 PIN       │ │  (chip rojo con ícono)
│ │    incorrectos               │ │
│ │ [ 🔓 Desbloquear tag ]        │ │
│ └──────────────────────────────┘ │
│ ┌ tarjeta hijo ────────────────┐ │
│ │ 👤 Alumno Prueba 3        ›  │ │
│ │ ○ Sin tag vinculado          │ │  (chip gris)
│ │ [ 📶 Vincular tag ]           │ │  (primario)
│ └──────────────────────────────┘ │
│                                  │
│ ┌──────────────────────────────┐ │
│ │     + Registrar hijo         │ │  (secundario)
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

**Resumen del tag en la tarjeta (prioridad para mostrar un solo chip):**

| Situación | Chip | Acción rápida |
|---|---|---|
| Algún tag `BLOQUEADO` por `PIN_FALLIDO` | ⛔ «Tag bloqueado: 3 PIN incorrectos» | «Desbloquear tag» |
| Algún tag `BLOQUEADO` por `MANUAL` (y ninguno activo) | ⛔ «Tag bloqueado por ti» | «Desbloquear tag» |
| Al menos un tag `ACTIVO` | ● «Tag activo» (o «2 tags activos») | «Bloquear tag» |
| Tag `RESERVADO` vigente | ⏳ «Tag pendiente de confirmar» | «Terminar vinculación» |
| Sin tags | ○ «Sin tag vinculado» | «Vincular tag» |

Si el hijo tiene más de un tag, la acción rápida abre P-106 para elegir cuál.

**Estados de la pantalla:** cargando (tarjetas esqueleto); vacío:

```
│      [ilustración simple]        │
│  Todavía no registraste a tus    │
│  hijos.                          │
│ ┌──────────────────────────────┐ │
│ │      Registrar a mi hijo     │ │
│ └──────────────────────────────┘ │
```

error de red («No pudimos cargar a tus hijos. [Reintentar]»); con datos. Se puede actualizar deslizando hacia abajo (con alternativa accesible: ítem «Actualizar» en el menú ⋮).

- `Semantics` de la tarjeta: «Alumno Prueba, tope mensual 300 soles, tag activo. Abrir detalle.»

---

#### P-105 — Registrar hijo

- **Propósito:** registrar a un hijo con su DNI y nombre y definir su PIN de 3 dígitos.
- **Actor:** apoderado. **HU:** HU-008.
- **Datos que envía:** `nombre`, `dni`, `pin` (nada más: PRINCIPIO-007).
- **Reglas:** RN-001 (solo DNI y nombre), RN-004 (3 dígitos; `000` es válido), RN-005 (solo el apoderado define el PIN), RN-078 (tope por defecto S/ 300.00), pendiente 12 (DNI de 8 dígitos y único por colegio).

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹  Registrar hijo                │
├──────────────────────────────────┤
│ Nombre de tu hijo                │
│ [Alumno Prueba_______________]   │
│ Así lo verá el vendedor al       │
│ cobrar.                          │
│                                  │
│ DNI de tu hijo                   │
│ [________]                       │
│ 8 dígitos                        │
│                                  │
│ PIN de compra de tu hijo         │
│      [•]  [•]  [ ]   [Mostrar]   │
│ 3 números. Tu hijo lo digitará   │
│ en cada compra. No lo compartas. │
│                                  │
│ Repite el PIN                    │
│      [ ]  [ ]  [ ]               │
│                                  │
│ ┌ info ────────────────────────┐ │
│ │ ⓘ En esta versión de Luquita │ │
│ │ el PIN no se puede cambiar   │ │
│ │ después. Elige uno que tu    │ │
│ │ hijo recuerde.               │ │
│ └──────────────────────────────┘ │
│                                  │
│ Tope mensual: S/ 300.00          │
│ Límite diario: S/ 30.00          │
│                                  │
│ ┌──────────────────────────────┐ │
│ │        Registrar hijo        │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

**Mensajes:** «Completa el nombre» · «Máximo 100 caracteres» · «DNI inválido» · «El PIN debe tener 3 dígitos.» (`73`, `7302`, `7a2`; el campo solo acepta 3 dígitos) · «Los PIN no coinciden.» · `409 RECURSO_DUPLICADO` → «Este DNI ya está registrado. Consulta con la secretaría del colegio.» · éxito → P-104 con el aviso «Alumno Prueba quedó registrado. Ahora vincula su tag.» y acción «Vincular tag».

- La nota sobre el PIN refleja que el cambio de PIN no existe en el piloto (riesgo R5 aceptado).

---

#### P-106 — Detalle del hijo y sus tags

- **Propósito:** gestionar los tags de un hijo: vincular, bloquear, desbloquear y terminar una vinculación pendiente.
- **Actor:** apoderado. **HU:** HU-009, HU-016, HU-017.
- **Datos:** el hijo de `GET /apoderado/alumnos` (nombre, tope mensual, tags).
- **Acciones:** vincular tag; bloquear un tag; desbloquear un tag; terminar vinculación; (Should) ver consumo del mes (P-108); (Should) reemplazar tag (P-109).
- **Reglas:** RN-016 y RN-026 (bloqueo inmediato, sin caché), RN-027 y RN-028 (solo el apoderado desbloquea; el desbloqueo reinicia el contador), RN-085; varios tags activos permitidos en el piloto.

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹  Alumno Prueba                 │
├──────────────────────────────────┤
│ Tope mensual: S/ 300.00          │
│ Límite diario: S/ 30.00          │
│                                  │
│ Tags                             │  (titleMedium)
│ ┌ tarjeta tag ─────────────────┐ │
│ │ Tag 1                        │ │
│ │ ● Activo                     │ │
│ │ Vinculado el 2026-11-04      │ │
│ │ ┌──────────────────────────┐ │ │
│ │ │ 🔒 Bloquear tag           │ │ │  (botón tonal rojo, 48 dp)
│ │ └──────────────────────────┘ │ │
│ └──────────────────────────────┘ │
│ ┌ tarjeta tag ─────────────────┐ │
│ │ Tag 2                        │ │
│ │ ⛔ Bloqueado: 3 PIN           │ │
│ │ incorrectos                  │ │
│ │ Desde 2026-11-04 10:20       │ │
│ │ Si no fue tu hijo, alguien   │ │
│ │ intentó usar el tag.         │ │
│ │ ┌──────────────────────────┐ │ │
│ │ │ 🔓 Desbloquear tag        │ │ │  (botón primario)
│ │ └──────────────────────────┘ │ │
│ └──────────────────────────────┘ │
│                                  │
│ ┌──────────────────────────────┐ │
│ │     📶 Vincular otro tag      │ │  (secundario)
│ └──────────────────────────────┘ │
│ ¿Perdiste un tag? Bloquéalo y    │
│ vincula uno nuevo.               │
│                                  │
│ ── Should (después del MVP) ──   │
│ [ Ver consumo del mes  › ]       │
└──────────────────────────────────┘
```

**Etiquetas de estado del tag:**

| `estado` / `motivoBloqueo` | Texto | Ícono y color | Botón |
|---|---|---|---|
| `ACTIVO` | «Activo» | ✓ verde | «Bloquear tag» |
| `BLOQUEADO` / `MANUAL` | «Bloqueado por ti» + «Desde {bloqueadoEn}» | ⛔ rojo | «Desbloquear tag» |
| `BLOQUEADO` / `PIN_FALLIDO` | «Bloqueado: 3 PIN incorrectos» + «Desde {bloqueadoEn}» | ⛔ rojo | «Desbloquear tag» |
| `RESERVADO` (vigente) | «Pendiente de confirmar» + «Acerca el tag para terminar la vinculación.» | ⏳ ámbar | «Terminar vinculación» (abre P-107 en el paso de verificación) |

- Los tags se numeran «Tag 1, Tag 2…» por orden de `activadoEn` y cada tarjeta muestra «Vinculado el AAAA-MM-DD HH:mm» (Q-03 resuelta el 2026-11-14).

**Diálogos:**

- Bloquear: título «¿Bloquear este tag?», texto «Nadie podrá comprar con este tag de Alumno Prueba hasta que lo desbloquees.», botones `[Cancelar]` `[Bloquear]` (rojo).
- Desbloquear: título «¿Desbloquear este tag?», texto «Alumno Prueba podrá volver a comprar con este tag.» (si fue por PIN, se agrega «Recuérdale su PIN.»), botones `[Cancelar]` `[Desbloquear]`.

**Estados de las acciones:**

| Estado | Comportamiento |
|---|---|
| Bloqueando / desbloqueando | Botón con indicador; el resto de la tarjeta se deshabilita. |
| Éxito al bloquear | La tarjeta pasa a «Bloqueado por ti»; aviso «Tag bloqueado. Ninguna compra con este tag será aceptada.» |
| Éxito al desbloquear | La tarjeta pasa a «Activo»; aviso «Tag desbloqueado.» |
| Bloqueo sin conexión | **Banner persistente de error** (no un aviso breve): «El tag NO se bloqueó. Revisa tu conexión e inténtalo de nuevo» + `[Reintentar]`. La tarjeta sigue diciendo «Activo». Se anuncia con `SemanticsService.announce` de forma asertiva. |
| Desbloqueo sin conexión | Banner «El tag NO se desbloqueó. Revisa tu conexión e inténtalo de nuevo» + `[Reintentar]`. |
| Bloquear un tag ya bloqueado | Respuesta `200`: la tarjeta se muestra bloqueada, sin error. |
| `403` | «No tienes permiso para hacer esta acción.» |

- `Semantics` de los botones: `Semantics(label: 'Bloquear tag 1 de Alumno Prueba')`, `Semantics(label: 'Desbloquear tag 2 de Alumno Prueba')` (PRINCIPIO-011, ejemplo de la constitución).
- Si `familiaActiva` es `false`, el botón «Vincular otro tag» aparece deshabilitado y muestra debajo «Tu familia no está activa en Luquita. Consulta con la secretaría del colegio.» Si aun así el servidor responde `422 FAMILIA_INACTIVA` al vincular, se muestra el mismo texto (Q-02 resuelta el 2026-11-14).

---

#### P-107 — Vincular tag (asistente)

- **Propósito:** grabar un tag en blanco, bloquearlo contra escritura y vincularlo a un hijo sin desperdiciar tags.
- **Actor:** apoderado. **HU:** HU-009, HU-010.
- **Datos:** hijo elegido; tipo de chip leído; respuesta de la reserva (`tagId`, `contenidoNdef`, `reservaVenceEn`); respuesta de la activación.
- **Reglas:** RN-006, RN-007, RN-008 (bloqueo con lock bits), RN-009 (solo NTAG213 y NTAG215), RN-091 (tag ya usado), PRINCIPIO-006; ADR-003 (reservar → grabar → verificar → bloquear → activar). El contenido NDEF nunca se muestra en pantalla.

**Estructura:** pantalla completa con indicador de pasos arriba (`Paso 2 de 4`), ilustración central y un botón «Cancelar» que solo está disponible antes de bloquear. Pasos visibles: **1 Preparar · 2 Acercar el tag · 3 Grabar · 4 Listo**.

**Wireframe — paso 1 (Preparar):**

```
┌──────────────────────────────────┐
│ ✕  Vincular tag      Paso 1 de 4 │
├──────────────────────────────────┤
│ ¿Para quién es el tag?           │
│ (•) Alumno Prueba                │  (radio, 56 dp por opción)
│ ( ) Alumno Prueba 3              │
│                                  │
│ Antes de empezar:                │
│ ✓ NFC activado                   │  (✗ «NFC desactivado» si aplica)
│ • Usa un tag en blanco NTAG213 o │
│   NTAG215 (el que entrega el     │
│   colegio).                      │
│ • Quita la funda del celular si  │
│   es gruesa.                     │
│                                  │
│ ┌──────────────────────────────┐ │
│ │          Continuar           │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

- Si se entra desde P-106, el hijo ya viene elegido y la lista no se muestra.
- NFC no disponible o apagado: el ✓ pasa a ✗ «NFC desactivado», aparece «Tu celular necesita NFC activo para grabar el tag. Actívalo en los ajustes y vuelve.» y «Continuar» queda deshabilitado. La app vuelve a revisar el NFC al regresar a primer plano.

**Wireframe — paso 2 (Acercar el tag):**

```
┌──────────────────────────────────┐
│ ✕  Vincular tag      Paso 2 de 4 │
├──────────────────────────────────┤
│                                  │
│   [ilustración: celular con el   │
│    tag apoyado en la parte       │
│    trasera, ondas NFC]           │
│                                  │
│  Acerca el tag a la parte        │
│  trasera de tu celular y no lo   │  (headline 20 sp, centrado)
│  muevas.                         │
│                                  │
│  ◌ Esperando el tag…             │  (indicador circular)
│                                  │
│ ┌ banner error (si aplica) ────┐ │
│ │ ⛔ Este tag no es compatible  │ │
│ │ con Luquita                  │ │
│ │ Usa un tag NTAG213 o NTAG215.│ │
│ └──────────────────────────────┘ │
│ [ Probar con otro tag ]          │
└──────────────────────────────────┘
```

| Lectura | Resultado |
|---|---|
| NTAG213 o NTAG215 en blanco sin lock bits | Pasa a reservar (indicador «Preparando el tag…») y luego al paso 3. |
| MIFARE Classic, NTAG216 u otro chip | «Este tag no es compatible con Luquita» + «Usa un tag NTAG213 o NTAG215.» |
| Tag con datos (otro token u otra app) o con lock bits activos | «Este tag ya fue usado. Pide un tag nuevo en el colegio.» No se escribe nada. |
| Reserva `422 FAMILIA_INACTIVA` | «Tu familia no está activa en Luquita. Consulta con la secretaría del colegio.» Termina el asistente. |
| Reserva `403` | «No tienes permiso para vincular un tag a este alumno.» |
| Reserva sin red | «Sin conexión. Revisa tu internet e inténtalo de nuevo.» + `[Reintentar]` (misma clave). El tag no se tocó. |

**Wireframe — paso 3 (Grabar):**

```
┌──────────────────────────────────┐
│    Vincular tag      Paso 3 de 4 │  (sin ✕ durante la escritura)
├──────────────────────────────────┤
│  ⚠ No retires el tag             │  (banner ámbar, texto grande)
│                                  │
│  ✓ Tag reservado en Luquita      │
│  ◌ Grabando…                     │
│  ○ Verificando                   │
│  ○ Protegiendo contra cambios    │
│  ○ Confirmando con Luquita       │
│                                  │
│  (lista de subpasos con ícono +  │
│   texto; aria-live polite)       │
└──────────────────────────────────┘
```

| Situación | Resultado |
|---|---|
| Tag retirado antes de proteger (antes del bloqueo) | «La grabación no terminó. Vuelve a acercar el mismo tag.» + `[Reintentar]`. El tag sigue reescribible y se usa el mismo token reservado. |
| La verificación no coincide | «No pudimos verificar el tag. Vuelve a acercarlo.» (no se aplica el bloqueo). |
| Activación sin respuesta del servidor (después del bloqueo) | «El tag ya está grabado, pero falta confirmarlo con Luquita. Revisa tu conexión y toca Reintentar.» + `[Reintentar]` (idempotente) + `[Terminar después]` (P-106 lo mostrará «Pendiente de confirmar»). |
| `409 TAG_YA_ACTIVO` al activar | Se trata como éxito. |
| `422 DATOS_INVALIDOS` al activar (lo leído no coincide) | «El tag no coincide con la reserva. Contacta a la secretaría del colegio.» |

**Wireframe — paso 4 (Listo):**

```
┌──────────────────────────────────┐
│    Vincular tag      Paso 4 de 4 │
├──────────────────────────────────┤
│          ✓ (ícono 64 dp verde)   │
│  ¡Listo! El tag de Alumno Prueba │
│  ya está activo.                 │
│                                  │
│  Recuérdale a tu hijo su PIN de  │
│  3 dígitos. Lo necesitará en     │
│  cada compra.                    │
│                                  │
│ ┌──────────────────────────────┐ │
│ │      Volver a Mis hijos      │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

- Cada cambio de paso se anuncia con `SemanticsService.announce` («Paso 3 de 4: grabando. No retires el tag.»). Se usa vibración corta al leer el tag (respetando la configuración del sistema).

---

#### P-108 — Detalle de consumo del mes (Should, S2)

- **Propósito:** que el apoderado vea las compras de su hijo en un mes.
- **Actor:** apoderado. **HU:** HU-028.
- **Datos:** compras del mes: fecha y hora, monto, descripción referencial (RN-037). Solo para el apoderado de ese alumno (RN-038).

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹  Consumo de Alumno Prueba      │
├──────────────────────────────────┤
│ [‹]  noviembre de 2026  [›]      │  (botones de 48 dp con etiquetas
│                                  │   «Mes anterior» / «Mes siguiente»)
│ Total del mes: S/ 5.50           │
│ Tope mensual: S/ 300.00          │
│                                  │
│ ┌ lista ───────────────────────┐ │
│ │ 2026-11-05 10:20     S/ 2.00 │ │
│ │ (sin descripción)            │ │
│ │──────────────────────────────│ │
│ │ 2026-11-04 10:15     S/ 3.50 │ │
│ │ Jugo de naranja              │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

- Vacío: «Sin compras en octubre». «Mes siguiente» se deshabilita en el mes en curso. Cuando exista S1, las anulaciones se muestran como «Anulación −S/ 3.50» con ícono.

---

#### P-109 — Reemplazar o desvincular tag (Should, S3)

- **Propósito:** desvincular un tag perdido y, si se quiere, vincular uno nuevo.
- **Actor:** apoderado. **HU:** HU-029.
- **Reglas:** RN-010 (un solo tag activo cuando exista S3), RN-011 (un tag desvinculado no compra).

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹  Reemplazar tag                │
├──────────────────────────────────┤
│ ¿Qué tag quieres desvincular?    │
│ (•) Tag 1 — Activo               │
│ ( ) Tag 2 — Bloqueado            │
│                                  │
│ El tag desvinculado no podrá     │
│ volver a usarse.                 │
│                                  │
│ [ Desvincular y vincular uno     │
│   nuevo ]                        │  (primario → P-107)
│ [ Solo desvincular ]             │  (secundario)
└──────────────────────────────────┘
```

- Confirmación obligatoria. Con «Solo desvincular» y sin otro tag, la pantalla avisa «Alumno Prueba se quedará sin tag y no podrá comprar.»

---

### 3.3 App Vendedor (V)

#### Estructura común de la app

- Pensada para usarse **de pie, rápido y con una mano** en un recreo: textos grandes, un solo botón principal por pantalla, nada de menús escondidos durante el cobro.
- Pantalla siempre encendida durante el cobro (`wakelock` solo si un humano aprueba la dependencia; si no, se recomienda configurarlo en el celular).
- **Franja de conexión** (componente `franja-conexion`): fija debajo de la barra superior; aparece en rojo con ícono ⛔ y el texto «Sin conexión: no se puede cobrar con Luquita» cuando no hay red. Se anuncia de forma asertiva.
- Barra superior: «Tienda del colegio» y menú ⋮ (fuera del flujo de cobro): «Cerrar sesión» (y, en Should, «Cobros de hoy» y «Total del mes»).
- Ningún dato del cobro se escribe en disco; la `Idempotency-Key` vive solo en memoria de la pantalla (ADR-002).
- Teclado numérico propio (`teclado-numerico`), 3 × 4 teclas de 72 × 64 dp con 8 dp de separación; texto de tecla 28 sp; tecla «⌫» con `Semantics(label: 'Borrar')`.

---

#### P-201 — Iniciar sesión (vendedor)

- **Propósito:** entrar a la app de cobro con correo y clave.
- **Actor:** dueño de la tienda. **HU:** HU-011.
- **Datos que envía:** `usuario` (correo), `clave`.
- **Reglas:** RN-029, RN-071 (correo sin distinguir mayúsculas), RN-073.

**Wireframe:**

```
┌──────────────────────────────────┐
│                                  │
│  Luquita · Cobro                 │  (headline 28 sp)
│  App de la tienda del colegio    │
│                                  │
│  Correo                          │
│  [tienda@colegioprueba.pe____]   │  (teclado de correo)
│                                  │
│  Clave                           │
│  [••••••••••••]      [Mostrar]   │
│                                  │
│ ┌ banner ──────────────────────┐ │
│ │ ⓘ Usuario o clave incorrectos.│ │
│ └──────────────────────────────┘ │
│                                  │
│ ┌──────────────────────────────┐ │
│ │           Ingresar           │ │
│ └──────────────────────────────┘ │
│  ¿Olvidaste tu clave? Pide al    │
│  equipo de Luquita que la        │
│  regenere.                       │
└──────────────────────────────────┘
```

**Estados:** igual que P-001 (incorrecto, bloqueado con hora, sin conexión, ingresando). El correo se envía en minúsculas y sin espacios. Éxito: `cambioClavePendiente` → P-202; si no, `GET /vendedor/ventas` → P-204 o P-203.

---

#### P-202 — Cambia tu clave (vendedor)

- **Propósito / reglas:** iguales a P-002 (RN-073, RN-105), adaptadas a móvil.
- **Actor:** dueño de la tienda. **HU:** HU-011.

**Wireframe:**

```
┌──────────────────────────────────┐
│  Cambia tu clave                 │
├──────────────────────────────────┤
│ Antes de cobrar, cambia la clave │
│ que recibiste por correo.        │
│                                  │
│ Clave recibida   [__________][👁]│
│ Clave nueva      [__________][👁]│
│  ✓ Al menos 12 caracteres        │
│  ✗ Una letra mayúscula           │
│  ✓ Una letra minúscula           │
│  ✗ Un número                     │
│  ✓ Distinta de la recibida       │
│ Repite la clave  [__________][👁]│
│                                  │
│ ┌──────────────────────────────┐ │
│ │     Guardar clave nueva      │ │
│ └──────────────────────────────┘ │
│ [ Cerrar sesión ]                │
└──────────────────────────────────┘
```

- Mientras la clave no se cambie, la app no muestra ninguna pantalla de cobro (y el servidor rechaza compras con `403 CAMBIO_CLAVE_PENDIENTE`).

---

#### P-203 — App de cobro no activa

- **Propósito:** informar que no se puede cobrar porque el colegio desactivó la app.
- **Actor:** dueño de la tienda. **HU:** HU-033.
- **Datos:** `GET /vendedor/ventas` → `habilitadas: false`.
- **Acciones:** volver a consultar; cerrar sesión.
- **Reglas:** RN-097, RN-102, RN-103.

**Wireframe:**

```
┌──────────────────────────────────┐
│ Tienda del colegio            ⋮  │
├──────────────────────────────────┤
│                                  │
│        ⏸ (ícono 64 dp, gris)     │
│                                  │
│  La app de cobro no está activa. │  (headline 22 sp, centrado)
│  Consulta con la secretaría del  │
│  colegio.                        │
│                                  │
│ ┌──────────────────────────────┐ │
│ │      Volver a consultar      │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

**Estados:** consultando (indicador en el botón); sigue desactivada (aviso «La app de cobro sigue desactivada.»); activada → P-204. No hay ningún acceso al cobro desde esta pantalla.

---

#### P-204 — Cobro: monto y descripción

- **Propósito:** iniciar un cobro con el monto y una descripción opcional.
- **Actor:** dueño de la tienda. **HU:** HU-012.
- **Datos:** ninguno del servidor (solo `GET /vendedor/ventas` antes de avanzar).
- **Acciones:** digitar el monto; escribir la descripción; cobrar.
- **Reglas:** RN-014 y RN-082 (monto > 0 con 2 decimales; mínimo S/ 0.01), RN-048, RN-049, RN-050 (descripción opcional ≤ 60 caracteres, contados como caracteres), PRINCIPIO-004 (el monto se maneja en céntimos enteros). La app **no** valida los límites (UD-05). RN-089 (sin conexión no se registra la compra: franja roja y «Cobrar» deshabilitado).

**Comportamiento del monto:** entrada tipo cajero en céntimos: cada dígito empuja desde la derecha (`3` → S/ 0.03, `35` → S/ 0.35, `350` → S/ 3.50). Máximo 7 dígitos (S/ 99 999.99) para evitar desbordes; el servidor decide lo demás.

**Wireframe:**

```
┌──────────────────────────────────┐
│ Tienda del colegio            ⋮  │
│ [franja-conexion si no hay red]  │
├──────────────────────────────────┤
│ Monto a cobrar                   │  (label 16 sp)
│ ┌──────────────────────────────┐ │
│ │                    S/ 3.50   │ │  (display 44 sp, tabular, derecha)
│ └──────────────────────────────┘ │
│                                  │
│ Descripción (opcional)           │
│ [Jugo de naranja_____________]   │  (abre teclado del sistema)
│                          15/60   │
│                                  │
│ ┌─────────┬─────────┬─────────┐  │
│ │    1    │    2    │    3    │  │  teclado-numerico
│ ├─────────┼─────────┼─────────┤  │  teclas 72×64 dp
│ │    4    │    5    │    6    │  │
│ ├─────────┼─────────┼─────────┤  │
│ │    7    │    8    │    9    │  │
│ ├─────────┼─────────┼─────────┤  │
│ │   00    │    0    │    ⌫    │  │
│ └─────────┴─────────┴─────────┘  │
│ ┌──────────────────────────────┐ │
│ │     Cobrar S/ 3.50  →        │ │  (primario, 64 dp)
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

**Estados:**

| Estado | Comportamiento |
|---|---|
| Monto S/ 0.00 | «Cobrar» deshabilitado; texto de ayuda «Ingresa el monto.» |
| Descripción de 61+ caracteres | El campo no deja pasar de 60 (contador «60/60»); si el servidor responde `DESCRIPCION_MUY_LARGA`, se muestra en el campo. |
| Sin conexión | Franja roja; «Cobrar» deshabilitado. |
| Verificando la app de cobro | Indicador en el botón; si `habilitadas: false` → P-203. |
| Hay un cobro sin confirmar | No se llega aquí: P-208 bloquea hasta resolverlo (UD-03). |
| Listo | «Cobrar» genera la `Idempotency-Key` (UUID v4 en memoria) y pasa a P-205. |

- `Semantics` del monto: «Monto a cobrar, 3 soles con 50 céntimos».

---

#### P-205 — Cobro: acercar el tag

- **Propósito:** leer el tag del alumno y mostrar solo su nombre.
- **Actor:** dueño de la tienda y alumno. **HU:** HU-012.
- **Datos:** `POST /tags/resolucion` → `nombreAlumno` (nada más).
- **Acciones:** leer el tag; cancelar el cobro.
- **Reglas:** RN-040 (solo el nombre del alumno), PRINCIPIO-006 (el servidor resuelve el token), PRINCIPIO-007 (solo el nombre), ADR-007 (token en el cuerpo).

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹ Cancelar cobro                 │
│ [franja-conexion si no hay red]  │
├──────────────────────────────────┤
│ Cobro: S/ 3.50                   │  (title 22 sp)
│ Jugo de naranja                  │
│                                  │
│   [ilustración: tag acercándose  │
│    a la parte trasera]           │
│                                  │
│  Pide al alumno que acerque su   │  (headline 24 sp, centrado)
│  tag al celular.                 │
│                                  │
│  ◌ Esperando el tag…             │
│                                  │
│ ┌ banner (si aplica) ──────────┐ │
│ │ ⛔ Tag no reconocido. Este tag │ │
│ │ no está vinculado a ningún   │ │
│ │ alumno.                      │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

| Resultado | Comportamiento |
|---|---|
| `200 { nombreAlumno }` | Vibración corta y paso a P-206 con el nombre. |
| `404 TAG_NO_RECONOCIDO` | Banner de error sin ningún nombre; sigue esperando otro tag. |
| `409 VENTAS_NO_HABILITADAS` | → P-203 (se descarta el cobro). |
| Tag que no es de Luquita (otro formato NDEF) | «Este tag no es de Luquita.» (sin llamar al servidor). |
| Sin conexión | Franja roja; no se lee ni se guarda nada. |
| Cancelar | Vuelve a P-204 con el monto y la descripción conservados. |

---

#### P-206 — Cobro: PIN del alumno

- **Propósito:** que el alumno digite su PIN en el celular del vendedor y se registre la compra.
- **Actor:** alumno (digita), dueño de la tienda (supervisa). **HU:** HU-012, HU-014.
- **Datos:** `nombreAlumno`, monto; envía `POST /compras` (`token`, `pin`, `monto`, `descripcion`) con timeout de 8 s.
- **Acciones:** digitar el PIN; borrar; confirmar; cancelar.
- **Reglas:** RN-012 (tag + PIN), RN-013 y RN-027 (3 fallos seguidos bloquean el tag), RN-041 (cada dígito del PIN se muestra enmascarado), PRINCIPIO-008 (PIN enmascarado, nunca visible ni en logs), PRINCIPIO-011 (teclas ≥ 48 × 48 dp).

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹ Cancelar cobro                 │
│ [franja-conexion si no hay red]  │
├──────────────────────────────────┤
│ Alumno Prueba                    │  (headline 28 sp, bold)
│ Cobro: S/ 3.50                   │  (title 22 sp)
│                                  │
│ Digita tu PIN                    │  (body 18 sp)
│        ●    ●    ○               │  (3 círculos de 20 dp: lleno/vacío)
│                                  │
│ ┌─────────┬─────────┬─────────┐  │
│ │    1    │    2    │    3    │  │
│ ├─────────┼─────────┼─────────┤  │
│ │    4    │    5    │    6    │  │
│ ├─────────┼─────────┼─────────┤  │
│ │    7    │    8    │    9    │  │
│ ├─────────┼─────────┼─────────┤  │
│ │         │    0    │    ⌫    │  │
│ └─────────┴─────────┴─────────┘  │
│ ┌──────────────────────────────┐ │
│ │      Confirmar compra        │ │  (primario 64 dp; se habilita con 3 dígitos)
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

**Estados:**

| Estado | Comportamiento |
|---|---|
| Ingresando | Cada dígito llena un círculo; nunca se muestra el número, ni siquiera un instante (PRINCIPIO-008). Las teclas no hacen eco audible. |
| 3 dígitos | Se habilita «Confirmar compra». (No se envía solo, para que el alumno pueda corregir.) |
| Enviando | Pantalla bloqueada con «Registrando la compra…»; teclado deshabilitado; espera hasta 8 s. |
| Respuesta | `201` / `200` → P-207 «Cobrado» · `4xx` / `503` → P-207 «No se cobró» · timeout o corte → P-208. |
| Vuelta tras «PIN incorrecto» | Círculos vacíos, texto «PIN incorrecto. Digítalo otra vez.» en rojo con ícono; **nueva** `Idempotency-Key`. |

- `Semantics`: cada tecla «1», «2»…; los círculos «PIN, 2 de 3 dígitos ingresados». El lector nunca lee los dígitos digitados.
- Los dígitos del teclado **no** se reordenan al azar (afecta la accesibilidad y la velocidad); la pantalla está pensada para que solo la vea el alumno al digitar.

---

#### P-207 — Resultado del cobro

- **Propósito:** decir sin ambigüedad si se cobró o no, y por qué.
- **Actor:** dueño de la tienda. **HU:** HU-012, HU-013, HU-014, HU-016.
- **Datos:** respuesta de `POST /compras` o de `GET /compras/operaciones/{clave}`: `nombreAlumno`, `monto`, `registradaEn` o `codigo` (+ `tagBloqueado`). **Nunca** el consumo acumulado.
- **Acciones:** nuevo cobro; digitar el PIN otra vez (solo `PIN_INVALIDO` sin bloqueo); cambiar monto (solo límites).
- **Reglas:** D7 (cobrado / no cobrado / sin confirmar), RN-017 y RN-018 (rechazo por límite diario o mensual), RN-021 (la deuda de la familia con el colegio nunca es motivo de rechazo: no existe ese mensaje), PRINCIPIO-007.

**Wireframe — Cobrado:**

```
┌──────────────────────────────────┐
│ (fondo --color-exito-suave)      │
│                                  │
│        ✓ (ícono 96 dp verde)     │
│                                  │
│          Cobrado                 │  (display 36 sp, bold, --color-exito)
│          S/ 3.50                 │  (display 44 sp)
│          Alumno Prueba           │  (title 22 sp)
│          2026-11-04 10:15        │  (body 16 sp)
│                                  │
│ ┌──────────────────────────────┐ │
│ │         Nuevo cobro          │ │  (primario 64 dp, con foco inicial)
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

**Wireframe — No se cobró:**

```
┌──────────────────────────────────┐
│ (fondo --color-error-suave)      │
│                                  │
│        ✕ (ícono 96 dp rojo)      │
│                                  │
│        No se cobró               │  (display 36 sp, bold, --color-error)
│        S/ 5.00 · Alumno Prueba   │
│                                  │
│ ┌ motivo ──────────────────────┐ │
│ │ La compra supera el límite   │ │  (title 20 sp)
│ │ diario de S/ 30.00.          │ │
│ └──────────────────────────────┘ │
│                                  │
│ ┌──────────────────────────────┐ │
│ │        Cambiar monto         │ │  (solo para límites)
│ └──────────────────────────────┘ │
│ ┌──────────────────────────────┐ │
│ │         Nuevo cobro          │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

- Motivos y acciones: tabla de F-12. Con `tagBloqueado: true` el motivo es «PIN incorrecto. El tag se bloqueó por 3 intentos fallidos. El apoderado debe desbloquearlo desde su app.» y no se ofrece «Digitar el PIN otra vez».
- El resultado se anuncia al abrir la pantalla («Cobrado, 3 soles con 50, Alumno Prueba» / «No se cobró: …»). Vibración: una corta para cobrado, dos para rechazo (respetando la configuración del sistema).
- La pantalla no se cierra sola: el vendedor confirma con «Nuevo cobro», para no perder el resultado en medio de la fila.

---

#### P-208 — Cobro sin confirmar

- **Propósito:** resolver un cobro cuya respuesta se perdió, sin cobrar dos veces.
- **Actor:** dueño de la tienda. **HU:** HU-012.
- **Datos:** la `Idempotency-Key` en memoria, el monto y el nombre del alumno.
- **Acciones:** verificar; reintentar (misma clave y mismo contenido); descartar.
- **Reglas:** RN-015, RN-090, ADR-002; criterios confirmados de HU-012.

**Wireframe:**

```
┌──────────────────────────────────┐
│ (fondo --color-advertencia-suave)│
│ [franja-conexion si no hay red]  │
│                                  │
│        ⚠ (ícono 96 dp ámbar)     │
│                                  │
│  No se pudo confirmar el cobro   │  (headline 26 sp, bold)
│  S/ 3.50 · Alumno Prueba         │
│                                  │
│  No sabemos si se cobró. No      │
│  cobres de nuevo: primero        │
│  verifica.                       │
│                                  │
│ ┌──────────────────────────────┐ │
│ │          Verificar           │ │  (primario 64 dp)
│ └──────────────────────────────┘ │
│ ┌──────────────────────────────┐ │
│ │          Reintentar          │ │  (secundario 56 dp)
│ └──────────────────────────────┘ │
│ [ Descartar ]                    │  (botón de texto, 48 dp)
└──────────────────────────────────┘
```

| Acción / respuesta | Resultado |
|---|---|
| «Verificar» → `REGISTRADA` | P-207 Cobrado. |
| «Verificar» → `RECHAZADA` + `codigo` | P-207 No se cobró con su motivo (por ejemplo, «PIN incorrecto»). |
| «Verificar» → `404` | Se queda en P-208 con el texto «El cobro no llegó a Luquita. Puedes reintentarlo sin riesgo de cobrar dos veces.» y «Reintentar» pasa a primario. |
| «Verificar» o «Reintentar» sin red | Se queda en P-208; franja roja. |
| «Reintentar» | Mismo `POST /compras` con la misma clave → P-207 o vuelve a P-208. No cuenta otro PIN fallido. |
| «Descartar» | Diálogo: «¿Descartar sin verificar? Si la compra ya se registró y cobras de nuevo, el alumno pagará dos veces.» `[Volver]` `[Descartar]` → P-204. |
| Botón atrás del sistema | Abre el mismo diálogo de «Descartar». |

---

#### P-209 — Anular un cobro (Should, S1)

- **Propósito:** anular una compra registrada por error en el mes en curso.
- **Actor:** dueño de la tienda. **HU:** HU-026.
- **Datos:** los cobros que **este vendedor** registró hoy: hora, nombre del alumno, monto, descripción (sin DNI ni consumo acumulado; ver Q-05).
- **Reglas:** RN-035 (`ANULACION` por el mismo monto en signo contrario), RN-034 y RN-036 (solo mes en curso), PRINCIPIO-004.

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹  Cobros de hoy                 │
├──────────────────────────────────┤
│ ┌──────────────────────────────┐ │
│ │ 10:15  Alumno Prueba  S/ 3.50│ │
│ │ Jugo de naranja   [ Anular ] │ │
│ │──────────────────────────────│ │
│ │ 10:05  Alumno Prueba 2       │ │
│ │ S/ 2.00     ✕ Anulado        │ │
│ └──────────────────────────────┘ │
└──────────────────────────────────┘
```

- Confirmación: «¿Anular el cobro de S/ 3.50 a Alumno Prueba? Se restará del consumo del mes.» · Una compra ya anulada muestra «Anulado» sin botón.

---

#### P-210 — Consumo total del mes (Should, S5)

- **Propósito:** que el dueño sepa cuánto le debe pagar el colegio por un mes terminado.
- **Actor:** dueño de la tienda. **HU:** HU-031.
- **Reglas:** RN-042 (solo el total; nunca por alumno), PRINCIPIO-007.

**Wireframe:**

```
┌──────────────────────────────────┐
│ ‹  Total del mes                 │
├──────────────────────────────────┤
│ Mes [ octubre de 2026 ▼ ]        │
│                                  │
│ Consumo total de octubre:        │
│ S/ 18 450.00                     │  (display 40 sp)
│                                  │
│ Es el monto que el colegio debe  │
│ pagarte por este mes.            │
└──────────────────────────────────┘
```

- Mes en curso: «Noviembre todavía no termina. El total estará disponible desde el 1 de diciembre.» sin monto.

---

### 3.4 Plantillas de correo (C)

Formato: HTML simple de una columna (ancho máx. 600 px) **y** versión de texto plano; idioma `lang="es"`; contraste ≥ 4.5:1; sin imágenes obligatorias para entender el mensaje; sin píxeles de seguimiento ni enlaces de analítica (PRINCIPIO-007). Remitente: «Luquita».

#### P-301 — Clave inicial del personal (HU-002)

```
Asunto: Tu clave de acceso a Luquita

Hola:

El equipo de Luquita te registró como {Secretaria del colegio | Dueño de la tienda | Administrador de Luquita}
de {Colegio Prueba}.

Tu clave temporal es:   k7Rm2pXq9Tb4      (fuente monoespaciada, 20 px)

Ingresa con tu {DNI | correo} y esta clave. Te pediremos cambiarla la primera vez.

Si no esperabas este correo, ignóralo.
— Equipo Luquita
```

- La clave solo existe en el cuerpo del correo (que se borra al enviarse, arquitectura §3.2) y nunca en los logs.

#### P-302 — Aviso de compra (HU-015)

```
Asunto: Compra de {Alumno Prueba} — S/ 3.50

Compra en Tienda del colegio — 2026-11-04 10:15 — S/ 3.50 — Consumo de noviembre: S/ 88.50

┌──────────────────────────────────────────┐
│ Alumno       Alumno Prueba                │
│ Tienda       Tienda del colegio           │
│ Fecha y hora 2026-11-04 10:15             │
│ Monto        S/ 3.50                      │
│ Consumo de noviembre   S/ 88.50           │
└──────────────────────────────────────────┘

Si tu hijo perdió su tag, bloquéalo desde la app Luquita.
Este monto se cobrará en la boleta del colegio.
— Luquita
```

- La primera línea es **exactamente** la de HU-015 c1. El mes y el consumo son los de la compra, aunque el envío se reintente al mes siguiente (HU-015, RN-063). Sin DNI, PIN, token ni `codigoCompra` (retirado).

- **Reglas:** RN-023 (fecha, hora, tienda, monto y consumo del mes), RN-061 (se envía al correo del apoderado registrado por el colegio), RN-063. Los reintentos (RN-024), que la compra no se anule si el correo falla (RN-025) y el registro de cada intento (RN-084) no tienen efecto en pantalla (ver la sección 3.0).

#### P-303 — Aviso de tag bloqueado por PIN (Should, HU-024)

```
Asunto: El tag de {Alumno Prueba} se bloqueó

El tag de Alumno Prueba se bloqueó por 3 PIN incorrectos — 2026-11-04 10:20

Si fue tu hijo, desbloquea el tag desde la app Luquita y recuérdale su PIN.
Si no fue tu hijo, alguien intentó usar su tag: déjalo bloqueado y vincula uno nuevo.
— Luquita
```

- Nunca incluye los PIN digitados (PRINCIPIO-008).

---

## 4. Design tokens

Nombres de tokens en ASCII (PRINCIPIO-001). En Angular se definen como variables CSS (`--color-primario`); en Flutter, como constantes de un `ThemeExtension` (`TokensLuquita.colorPrimario`) que alimentan el `ThemeData` de Material 3. **Todos los contrastes indicados se calcularon con la fórmula de WCAG 2.1.**

### 4.1 Paleta de colores

| Token | Hex | Uso | Contraste verificado |
|---|---|---|---|
| `color-primario` | `#0B5FA5` | Botones principales, enlaces, ítem activo de la nav, foco | Texto blanco sobre él: **6.57:1**; él sobre blanco: 6.57:1; sobre `color-fondo`: 6.13:1 |
| `color-primario-oscuro` | `#003B6F` | Hover y presionado del primario, encabezados destacados | Sobre blanco: 11.32:1 |
| `color-primario-suave` | `#E7F0FA` | Fondo del ítem activo, banners informativos | `color-primario` sobre él: 5.71:1; `color-texto`: 13.57:1 |
| `color-secundario` | `#00695C` | Acciones secundarias con relleno, chips informativos | Blanco sobre él: 6.61:1 |
| `color-acento` | `#F2B705` | Detalles de marca, ilustraciones, indicador de pasos. **Nunca** como color de texto sobre fondo claro | `color-texto` sobre él: 8.59:1 |
| `color-fondo` | `#F5F7FA` | Fondo de las pantallas | — |
| `color-superficie` | `#FFFFFF` | Tarjetas, diálogos, barra superior | — |
| `color-texto` | `#1A2433` | Texto principal | Sobre blanco: 15.62:1; sobre fondo: 14.56:1 |
| `color-texto-secundario` | `#4A5568` | Ayudas, metadatos, etiquetas | Sobre blanco: 7.53:1; sobre fondo: 7.01:1 |
| `color-borde` | `#7A8496` | Bordes de campos, divisores, casillas | Sobre blanco: 3.77:1 (≥ 3:1, WCAG 1.4.11) |
| `color-exito` | `#1E7B34` | «Cobrado», «Activa», ✓ | Sobre blanco: 5.33:1; blanco sobre él: 5.33:1 |
| `color-exito-suave` | `#E8F5EC` | Fondo de la pantalla «Cobrado» y de banners de éxito | `color-exito` sobre él: 4.75:1 |
| `color-error` | `#B3261E` | «No se cobró», errores, tag bloqueado, acciones de peligro | Sobre blanco: 6.54:1; blanco sobre él: 6.54:1 |
| `color-error-suave` | `#FCEBEA` | Fondo de «No se cobró», banners de error | `color-error` sobre él: 5.67:1 |
| `color-advertencia` | `#8A5A00` | Texto e íconos de advertencia («Mes en curso», «sin confirmar») | Sobre blanco: 5.93:1 |
| `color-advertencia-suave` | `#FFF6DD` | Fondo de banners de advertencia y de P-208 | `color-advertencia` sobre él: 5.50:1 |
| `color-foco` | `#0B5FA5` | Anillo de foco de 3 px con 2 px de separación | 6.13:1 sobre el fondo |
| `color-deshabilitado-fondo` | `#E2E6EC` | Botones deshabilitados | — |
| `color-deshabilitado-texto` | `#4A5568` | Texto de botones deshabilitados (se mantiene legible) | 6.01:1 sobre `#E2E6EC` |

Reglas de uso:

- El color nunca es la única señal: estado = ícono + texto + color (UD-06).
- El rojo se reserva para errores, bloqueo y acciones destructivas; el verde, para éxito y estados activos.
- Modo oscuro: fuera del piloto. Si se agrega, se definen tokens paralelos y se vuelven a verificar los contrastes.

### 4.2 Tipografía

- **Flutter:** Roboto (fuente del sistema Android; no requiere paquetes). Unidades en `sp`, que respetan la escala de texto del sistema hasta 200 %.
- **Angular:** `font-family: Roboto, "Segoe UI", system-ui, -apple-system, "Helvetica Neue", Arial, sans-serif;` en `rem` (1 rem = 16 px) para respetar el zoom del navegador.
- **Cifras:** montos, DNI y horas con cifras tabulares (`FontFeature.tabularFigures()` en Flutter; `font-variant-numeric: tabular-nums` en CSS).

| Token | Tamaño | Peso | Alto de línea | Uso |
|---|---|---|---|---|
| `texto-monto` | 44 sp / 2.75 rem | 700 | 1.15 | Monto en P-204 y P-207 |
| `texto-display` | 36 sp / 2.25 rem | 700 | 1.2 | «Cobrado», «No se cobró» |
| `texto-titular` | 28 sp / 1.75 rem | 700 | 1.25 | Nombre del alumno en P-206, `<h1>` del panel |
| `texto-titulo` | 22 sp / 1.375 rem | 600 | 1.3 | Títulos de tarjeta, `<h2>` |
| `texto-subtitulo` | 18 sp / 1.125 rem | 600 | 1.4 | `<h3>`, instrucciones destacadas |
| `texto-cuerpo` | 16 sp / 1 rem | 400 | 1.5 | Texto general, campos |
| `texto-etiqueta` | 14 sp / 0.875 rem | 500 | 1.4 | Etiquetas de campo, botones de texto, chips |
| `texto-ayuda` | 14 sp / 0.875 rem | 400 | 1.4 | Ayudas y metadatos (mínimo 14: no se usa 12 para información) |
| `texto-tecla` | 28 sp | 500 | 1 | Teclado numérico |

### 4.3 Espaciado, forma y elevación

| Token | Valor | Uso |
|---|---|---|
| `espacio-1` … `espacio-8` | 4, 8, 12, 16, 24, 32, 48, 64 dp/px | Escala de 4; margen lateral móvil `espacio-4` (16); padding del panel `espacio-6` (32) |
| `radio-pequeno` | 8 | Campos, chips, botones |
| `radio-medio` | 12 | Tarjetas |
| `radio-grande` | 16 | Diálogos, hojas inferiores |
| `elevacion-tarjeta` | 1 (sombra `0 1px 3px rgba(26,36,51,.15)`) | Tarjetas |
| `elevacion-dialogo` | 3 (sombra `0 8px 24px rgba(26,36,51,.20)`) | Diálogos |
| `ancho-max-contenido` | 1200 px | Panel |
| `ancho-max-formulario` | 560 px | Formularios del panel |
| `ancho-max-tarjeta-login` | 420 px | P-001 |
| `duracion-transicion` | 150 ms (se anula con `prefers-reduced-motion` / `disableAnimations`) | Hover, cambios de estado |

### 4.4 Tamaños de componentes

| Componente | Móvil (Flutter) | Panel (Angular) | Notas |
|---|---|---|---|
| Área táctil mínima | 48 × 48 dp | 48 × 48 px | PRINCIPIO-011, en todo control |
| Botón primario | Alto 56 dp (64 dp en la app del vendedor), ancho completo | Alto 48 px, padding horizontal 24 px | Texto `texto-etiqueta` 16 sp/px, peso 600 |
| Botón secundario | Alto 56 dp, borde 1.5 dp `color-primario` | Alto 48 px, borde 1.5 px | — |
| Botón de texto | Alto mínimo 48 dp | Alto mínimo 48 px | Subrayado al pasar o enfocar |
| Campo de texto | Alto 56 dp, borde 1 dp `color-borde` (2 dp `color-primario` con foco, 2 dp `color-error` con error) | Alto 48 px | Etiqueta siempre visible arriba (no solo placeholder) |
| Casillas de PIN | 48 × 56 dp, separación 8 dp | — | Muestran `•` |
| Indicador de PIN (vendedor) | Círculos de 20 dp en área de 48 dp | — | Llenos / vacíos |
| Tecla del teclado numérico | 72 × 64 dp, separación 8 dp | — | `texto-tecla` |
| Chip de estado | Alto 32 dp, ícono 18 dp + texto 14 sp | Alto 28 px | Ícono + texto siempre |
| Tarjeta | Padding 16 dp, `radio-medio` | Padding 24 px | — |
| Barra superior | 64 dp | 64 px | — |
| Nav lateral (panel) | — | Ancho 240 px; ítems de 48 px | Colapsa bajo 960 px |
| Filas de tabla (panel) | — | Alto mínimo 52 px | Encabezados `<th scope>` |
| Diálogo | Ancho = pantalla − 48 dp | Ancho 480–520 px | Foco atrapado; `Esc` cierra en web |
| Íconos | 24 dp (dentro de un área de 48) · 64–96 dp en pantallas de resultado | 20–24 px | Material Icons del SDK de Flutter; en Angular, SVG en línea con `aria-hidden` |
| Franja de conexión | Alto mínimo 48 dp, `color-error`, texto blanco 16 sp | — | Fija bajo la barra superior |

---

## 5. Navigation map

### 5.1 Panel web (Angular 20)

```
/ingresar ............................................ P-001 Iniciar sesión (personal)
 │
 ├─ [cambioClavePendiente] → /cambiar-clave .......... P-002 Cambia tu clave
 │                              (guard: única ruta permitida hasta cambiarla)
 │
 ├─ Rol ADMIN_LUQUITA
 │   └─ /admin/colegios .............................. P-010 Colegios
 │       ├─ (diálogo) Registrar colegio .............. P-011
 │       └─ /admin/colegios/:colegioId ............... P-012 Detalle del colegio
 │           ├─ (diálogo) Registrar usuario .......... P-013
 │           ├─ (componente) App de cobro ............ P-025 embebido
 │           └─ /admin/colegios/:colegioId/importar .. P-021 Importar familias
 │
 └─ Rol SECRETARIA
     └─ /colegio ..................................... P-020 Inicio
         ├─ /colegio/familias ........................ P-022 Familias
         │   ├─ /colegio/familias/nueva .............. P-023 Registrar familia
         │   └─ /colegio/familias/:familiaId ......... P-024 Editar familia
         ├─ /colegio/importar ........................ P-021 Importar familias
         ├─ /colegio/consolidado?anio=&mes= .......... P-026 Consolidado mensual
         ├─ /colegio/app-de-cobro .................... P-025 App de cobro
         └─ /colegio/ajustes ......................... P-027 Registrar ajuste (Should)

Global: [Cerrar sesión] en la barra superior → /ingresar
        401 sin renovación posible → /ingresar («Tu sesión terminó…»)
        403 CAMBIO_CLAVE_PENDIENTE → /cambiar-clave
        Ruta inexistente → página «No encontramos esta página» con enlace al inicio de su rol
```

### 5.2 App Apoderados (Flutter)

```
Arranque
 ├─ [sesión vigente] ───────────────────────────────┐
 └─ P-101 Bienvenida                                │
     ├─ P-102 Crear cuenta ──(éxito)──► P-103       │
     └─ P-103 Iniciar sesión ──(éxito)──────────────┤
                                                    ▼
                                     P-104 Mis hijos (inicio)
                                      ├─ P-105 Registrar hijo ──(éxito)──► P-104
                                      ├─ (acción rápida) Bloquear / Desbloquear → diálogo → P-104
                                      ├─ (acción rápida) Vincular tag → P-107
                                      └─ P-106 Detalle del hijo
                                          ├─ diálogo Bloquear tag
                                          ├─ diálogo Desbloquear tag
                                          ├─ P-107 Vincular tag (asistente)
                                          │    Paso 1 Preparar → Paso 2 Acercar → Paso 3 Grabar → Paso 4 Listo
                                          │    └─(fin)──► P-104
                                          ├─ P-108 Consumo del mes (Should)
                                          ├─ P-109 Reemplazar tag (Should) → P-107
                                          └─ P-110 Tope mensual (Could)

Menú ⋮ (P-104): Actualizar · Cerrar sesión → P-101
Renovación vencida (30 días) → P-103 («Tu sesión terminó…»)
```

### 5.3 App Vendedor (Flutter)

```
Arranque
 └─ P-201 Iniciar sesión
     ├─ [cambioClavePendiente] → P-202 Cambia tu clave ──┐
     └─ (éxito) ─────────────────────────────────────────┤
                                                         ▼
                                          GET /vendedor/ventas
                                           ├─ habilitadas=false → P-203 App no activa
                                           │                       └─ [Volver a consultar] ─┐
                                           └─ habilitadas=true ◄────────────────────────────┘
                                                         ▼
           ┌──────────────► P-204 Monto y descripción
           │                 └─[Cobrar]─► P-205 Acercar el tag
           │                               └─[nombre]─► P-206 PIN del alumno
           │                                              ├─ 201/200 ─► P-207 Cobrado ─────────┐
           │                                              ├─ 4xx/503 ─► P-207 No se cobró ─────┤
           │                                              │              └─ PIN incorrecto ─► P-206 (clave nueva)
           │                                              │              └─ Límite ─► P-204 (Cambiar monto)
           │                                              ├─ 409 VENTAS_NO_HABILITADAS ─► P-203
           │                                              └─ timeout/corte ─► P-208 Sin confirmar
           │                                                                   ├─ Verificar/Reintentar ─► P-207
           │                                                                   └─ Descartar ─► P-204
           └──────────────────────────[Nuevo cobro]────────────────────────────────────────────┘

Menú ⋮ (solo en P-204 y P-203): Cobros de hoy → P-209 (Should) · Total del mes → P-210 (Should) · Cerrar sesión → P-201
Franja «Sin conexión: no se puede cobrar con Luquita»: superpuesta en P-204 a P-208
```

---

## 6. Verificación frente a la constitución

| Principio | Cómo lo cumple esta especificación |
|---|---|
| PRINCIPIO-001 Idioma | Todos los textos de interfaz en español latinoamericano; tokens, rutas y componentes en ASCII. |
| PRINCIPIO-002 Stack | Flutter (Material 3 del SDK) y Angular 20; ninguna librería de UI nueva (UD-08). |
| PRINCIPIO-003 Pospago | Ninguna pantalla de saldo, recarga, pago ni suscripción; el consolidado solo suma consumo. |
| PRINCIPIO-004 Montos | Entrada en céntimos enteros en Flutter; Angular solo muestra strings del servidor; ningún total se calcula en el cliente; botones idempotentes. |
| PRINCIPIO-005 Límites y control | Sin cobro sin conexión (franja y bloqueo de la acción); límites solo del servidor; bloqueo de tag con resultado inequívoco; correo por compra (P-302). |
| PRINCIPIO-006 Tag + PIN | Flujo de cobro con los dos factores; asistente de vinculación según ADR-003; el contenido del tag nunca se muestra. |
| PRINCIPIO-007 Datos mínimos | El vendedor solo ve el nombre; el colegio solo el consolidado; el apoderado solo a sus hijos; sin analítica ni SDK de terceros. |
| PRINCIPIO-008 Seguridad | PIN y claves enmascarados, sin autocompletar ni sugerencias; clave inicial solo en el correo; mismo mensaje si el usuario no existe; aviso de bloqueo con hora. |
| PRINCIPIO-009 Agentes | No se proponen dependencias nuevas; donde una ayudaría (`wakelock`, Angular Material), se deja la decisión a un humano. |
| PRINCIPIO-011 Accesibilidad | Contrastes verificados (sección 4.1), áreas de 48 dp incluido el teclado del PIN, `Semantics`/`aria-label` en cada control, texto al 200 % con reflujo, ícono + texto + color. |
| PRINCIPIO-012 Documentación | Este archivo vive en el repositorio en Markdown; los textos de error se alinean con el catálogo §4.1 del OpenAPI. |

---

## 7. Preguntas abiertas

Las respuestas no bloquean el diseño, pero cambian detalles de pantalla o de API. Hasta que se respondan, aplican las propuestas. **Q-01 a Q-04 y Q-06 quedaron resueltas el 2026-11-14** (ver `10-validation-log.md`, observaciones 1 a 4 y 8); sus filas conservan la propuesta aceptada.

| # | Pregunta | Pantallas | Propuesta mientras tanto |
|---|---|---|---|
| Q-01 | La arquitectura no tiene un endpoint para **listar los usuarios de un colegio**; P-012 no puede mostrar la secretaria y el dueño al volver a entrar. ¿Se agrega `GET /admin/colegios/{colegioId}/usuarios`? | P-012 | **Resuelta (2026-11-14):** se agrega `GET /admin/colegios/{colegioId}/usuarios` (arquitectura 4.3.2). |
| Q-02 | `GET /apoderado/alumnos` no dice si la **familia está inactiva**, así que la app solo se entera al intentar vincular un tag (`422`). ¿Se agrega un campo `familiaActiva`? | P-104, P-106, P-107 | **Resuelta (2026-11-14):** se agrega `familiaActiva` a `GET /apoderado/alumnos`. |
| Q-03 | Los tags no tienen un dato que los distinga para el apoderado (solo `tagId`). ¿Se agrega `activadoEn` (o un alias) a la respuesta? | P-106 | **Resuelta (2026-11-14):** se agrega `activadoEn`; los tags se numeran por orden de activación y muestran «Vinculado el …». |
| Q-04 | ¿Qué **canal de contacto** se muestra para credenciales olvidadas (procedimiento manual §6.7)? | P-001, P-103, P-201 | **Resuelta (2026-11-14):** se mantienen los textos actuales («Consulta con la secretaría del colegio…» para apoderados y vendedor, «Pide al equipo de Luquita…» para el personal), sin teléfono ni correo. |
| Q-05 | En HU-026 (Should), ¿el vendedor puede ver la **lista de sus cobros del día** con el nombre del alumno y el monto para elegir cuál anular? No contradice el PRINCIPIO-007 (no hay DNI ni acumulado), pero es información nueva para ese actor. | P-209 | Sí, solo los cobros que registró ese vendedor el mismo día. |
| Q-06 | ¿Se confirma el **formato de fecha** `AAAA-MM-DD HH:mm` en las apps, o se prefiere el formato local `dd/mm/aaaa`? | Todas | **Resuelta (2026-11-14):** se confirma `AAAA-MM-DD HH:mm`, igual que las HU y el correo de HU-015. |
| Q-07 | ¿Hay **logotipo** o colores institucionales de Luquita? | Sección 4 | Logotipo textual «Luquita» y la paleta propuesta. |
| Q-08 | El cambio de PIN del alumno no existe en el piloto. ¿Se mantiene el aviso «el PIN no se puede cambiar después» en P-105? | P-105 | Sí, para que el apoderado elija un PIN que su hijo recuerde. |
