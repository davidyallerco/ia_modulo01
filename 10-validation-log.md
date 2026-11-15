# Validation Log — Luquita

## Sesión de validación: 2026-11-14

### Fuentes
- **Compañero (simulado):** no hubo equipo vecino asignado; las 3 observaciones son simuladas, de bajo impacto.
- **Agente Reviewer:** reporte `09_1-reporte-prototype-reviewer.md`. Es una revisión de la especificación `09-ui-spec.md`, no de un prototipo.
- **Propia (simulada):** 3 observaciones simuladas, de bajo impacto.

Ninguna observación bloquea la implementación. No hay prototipo; las pantallas revisadas son P-001 a P-028, P-101 a P-112 y P-201 a P-210.

## Tabla de observaciones

| # | Observación | Fuente | Tipo | Severidad | Acción sugerida | HU / RN / P |
|---|---|---|---|---|---|---|
| 1 | P-012 no puede listar la secretaria y el dueño ya registrados: no existe `GET /admin/colegios/{colegioId}/usuarios` (Q-01) | Agente Reviewer | Gap | Media | Agregar el endpoint a la arquitectura y una tarea al backlog | HU-002 / P-012 |
| 2 | La app del apoderado no sabe si la familia está inactiva; se entera por un 422 al vincular un tag (Q-02) | Agente Reviewer | Fricción | Media | Agregar `familiaActiva` a `GET /apoderado/alumnos` y un banner que deshabilite «Vincular tag» | HU-009 / P-104, P-106, P-107 |
| 3 | Los tags no se distinguen entre sí al bloquear o desbloquear: solo existe `tagId` (Q-03) | Agente Reviewer | Comprensión | Media | Numerarlos «Tag 1, Tag 2» por orden de activación; mostrar «Vinculado el …» si se agrega `activadoEn` | HU-016, HU-017 / P-106 |
| 4 | El canal de contacto para credenciales olvidadas no está definido (Q-04) | Agente Reviewer | Gap | Media | Decidir el canal; mientras tanto, texto sin teléfono ni correo | HU-003, HU-007, HU-011 / P-001, P-103, P-201 |
| 5 | 15 reglas MUST no se citan en ninguna spec de pantalla (RN-017 a 021, 023 a 025, 030, 040, 041, 061, 083, 084, 089) | Agente Reviewer | Gap | Baja | Agregarlas a la línea «Reglas» de la pantalla que las refleja o marcarlas «sin efecto en UI» | HU-012, HU-015 / RN-017 … RN-089 |
| 6 | Si el correo de una compra falla tras 3 reintentos, el apoderado no se entera | Agente Reviewer | Gap | Baja | Documentar como limitación del piloto o agregar un aviso | HU-015 / RN-024, RN-025 / P-302 |
| 7 | El mensaje de `LIMITE_MENSUAL_EXCEDIDO` no incluye el monto del tope, a diferencia del diario | Agente Reviewer | Mejora | Baja | Mostrar el monto si la API lo incluye; si no, dejar el mensaje genérico | HU-012 / RN-018 / P-207 |
| 8 | Sin confirmar: formato de fecha `AAAA-MM-DD HH:mm` en las apps frente a `dd/mm/aaaa` (Q-06) | Agente Reviewer | Mejora | Baja | Confirmar el formato; mantener `AAAA-MM-DD HH:mm` hasta decidir | HU-015 |
| 9 | P-204 no avisa que S/ 35.00 supera el límite diario hasta después del tag y el PIN | Propia (simulada) | Fricción | Baja | Avisar antes de continuar si el monto supera S/ 30.00 | HU-012 / RN-017 / P-204 |
| 10 | «PIN incorrecto.» no indica cuántos intentos quedan antes del bloqueo del tag | Propia (simulada) | Mejora | Baja | Mostrar «Te quedan N intentos» con el dato del contador | HU-014 / RN-027 / P-206, P-207 |
| 11 | Sin descarga en Excel (Should), la secretaria solo puede imprimir o copiar el consolidado | Propia (simulada) | Mejora | Baja | Mantener la descarga como T-077 | HU-018 / P-026 |
| 12 | P-204: no queda claro si la descripción es obligatoria; sugiere indicar «(opcional)» | Compañero (simulado) | Comprensión | Baja | Verificar que el prototipo muestre «Descripción (opcional)», como ya define la especificación | HU-012 / P-204 |
| 13 | P-026: no se ve qué mes se consulta; sugiere el mes en el título | Compañero (simulado) | Fricción | Baja | Verificar que el título muestre el mes («noviembre de 2026»), como ya define la especificación | HU-018 / P-026 |
| 14 | P-203: falta decir a quién avisar para activar la app | Compañero (simulado) | Gap | Baja | Verificar que el texto «Consulta con la secretaría del colegio.» aparezca, como ya define la especificación | HU-033 / P-203 |

**Nota:** las observaciones 12, 13 y 14 ya están resueltas en `09-ui-spec.md`; solo hay que respetarlas al prototipar.

## Decisiones tomadas

Estados de cada observación al 2026-11-14: las observaciones PENDIENTE originales ya se resolvieron.

| # | Estado | Detalle |
|---|---|---|
| 1 | APLICADA | RESUELTA 2026-11-14: `GET /admin/colegios/{colegioId}/usuarios` en `07` §4.3.2; T-037 y T-039 actualizadas |
| 2 | APLICADA | RESUELTA 2026-11-14: `familiaActiva` en `GET /apoderado/alumnos`; T-047 y T-049 actualizadas |
| 3 | APLICADA | RESUELTA 2026-11-14: `activadoEn`, numeración «Tag 1, Tag 2» y «Vinculado el …»; T-047 y T-049 actualizadas |
| 4 | APLICADA | RESUELTA 2026-11-14: se mantienen los textos actuales, sin teléfono ni correo (`09-ui-spec.md` Q-04) |
| 5 | APLICADA | RESUELTA 2026-11-14: las 15 reglas se citan en P-204, P-205, P-206, P-207 y P-302, o figuran en «Reglas sin efecto en pantalla» (`09-ui-spec.md` §3.0) |
| 6 | BACKLOG | T-095 (agregada a `08-backlog.md` el 2026-11-14) |
| 7 | BACKLOG | T-096 (agregada el 2026-11-14) |
| 8 | APLICADA | RESUELTA 2026-11-14: se confirma `AAAA-MM-DD HH:mm` (`09-ui-spec.md` Q-06) |
| 9 | BACKLOG | T-097 (agregada el 2026-11-14) |
| 10 | BACKLOG | T-098 (agregada el 2026-11-14) |
| 11 | BACKLOG | T-077 (ya existe en `08-backlog.md`) |
| 12 | APLICADA | Ya definido en la especificación de P-204 |
| 13 | APLICADA | Ya definido en la especificación de P-026 |
| 14 | APLICADA | Ya definido en la especificación de P-203 |

## Archivos actualizados

**Ya modificado el 2026-11-14:** `08-backlog.md` v0.4.3 (T-095 a T-098, columna de pantallas en las tareas de interfaz y sección «Trazabilidad pantalla → tareas»).

**Modificados el 2026-11-14 por las observaciones 1 a 4:** `07-architecture.md` v0.5.3, `09-ui-spec.md` (P-012, P-104, P-106, Q-01 a Q-04) y `08-backlog.md` (T-037, T-039, T-047, T-049).

**Modificado el 2026-11-14 por las observaciones 5 y 8:** `09-ui-spec.md` (citas de reglas en P-204, P-205, P-206, P-207 y P-302, sección «Reglas sin efecto en pantalla» y Q-06).

**Pendiente de modificar:** ninguno. Quedan solo las tareas BACKLOG (T-095 a T-098 y T-077).
