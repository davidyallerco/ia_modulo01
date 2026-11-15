# Reporte de Validación de Prototipo — Luquita

> **Alcance y límites.** No se dispuso de una descripción de los prototipos. Este reporte lo generó Claude (rol Agente Prototype Reviewer) y audita **la especificación visual** (`09-ui-spec.md`, 2026-11-14) contra `06-user-stories.md` (19 HU Must) y `03-reglas-de-negocio.md` (75 reglas MUST). Los porcentajes miden la cobertura **de la especificación**, no de un prototipo. Los datos (conteos y citas de reglas) se verificaron con búsquedas sobre los archivos.

## 1. Cobertura de HUs

Las 19 HU Must tienen al menos una pantalla en la sección 1.5 de `09-ui-spec.md` (HU-001 a HU-018 y HU-033). **Cobertura: 19/19 = 100%.**

HU con una sola pantalla o con pantallas compartidas (riesgo de que un prototipo parcial las omita): HU-010 (solo P-107), HU-015 (solo P-302, que es un correo) y HU-013 (solo P-207).

## 2. Reglas de negocio

- 60 de las 75 reglas MUST se citan en las specs de pantalla: **80.0 %**.
- Las 15 sin cita explícita son: RN-017, 018, 019, 020, 021, 023, 024, 025, 030, 040, 041, 061, 083, 084 y 089.
- De esas 15, **6 sí se reflejan en la UI** aunque no se citen: RN-017 y RN-018 (tabla de errores de P-207: `LIMITE_DIARIO_EXCEDIDO` y `LIMITE_MENSUAL_EXCEDIDO`), RN-040 y RN-041 (nombre del alumno y PIN enmascarado `•••` en P-205 y P-206), RN-089 (flujo F-14, sin conexión) y RN-023 (correo P-302 con fecha, hora, tienda, monto y consumo del mes).
- **Invisibles por diseño** (reglas de servidor sin efecto en pantalla): RN-019, 020, 021, 030, 083 y 084. Aceptable, pero conviene citarlas en las specs para dejar la trazabilidad.
- **Sin cobertura visible:** RN-024 y RN-025 (reintentos del correo y compra que no se anula si el correo falla): ningún actor se entera si el correo no llegó. RN-061 (correo al apoderado registrado por el colegio) no se menciona en P-302.

## 3. Flujo de usuario

- Los flujos F-01 a F-15 cubren el camino principal y los rechazos de cobro, sin callejones sin salida visibles.
- **Fricciones del apoderado:** no puede saber qué tag es cuál al bloquear (Q-03) y solo descubre que su familia está inactiva al intentar vincular un tag y recibir un 422 (Q-02).
- **Falta de datos del administrador:** P-012 no puede mostrar la secretaria y el dueño ya registrados (Q-01), porque no existe el endpoint de listado.
- **Credenciales olvidadas:** el canal de contacto no está definido (Q-04); el texto de P-001, P-103 y P-201 es provisional.

## 4. Observaciones

| # | Observación | Tipo | Severidad | Acción sugerida | HU afectada |
|---|---|---|---|---|---|
| 1 | P-012 no puede listar la secretaria y el dueño de la tienda al volver a entrar: no existe `GET /admin/colegios/{colegioId}/usuarios` (Q-01) | Gap | Media | Agregar el endpoint a la arquitectura y una tarea al backlog | HU-002 |
| 2 | La app del apoderado no sabe si la familia está inactiva; se entera por un 422 al vincular un tag (Q-02) | Fricción | Media | Agregar `familiaActiva` a `GET /apoderado/alumnos` y mostrar un banner que deshabilite «Vincular tag» | HU-009 |
| 3 | Los tags no se distinguen entre sí al bloquear o desbloquear: solo existe `tagId` (Q-03) | Comprensión | Media | Numerarlos «Tag 1, Tag 2» por orden de activación; mostrar «Vinculado el …» si se agrega `activadoEn` | HU-016, HU-017 |
| 4 | El canal de contacto para credenciales olvidadas no está definido (Q-04) | Gap | Media | Decidir el canal; mientras tanto, mantener el texto sin teléfono ni correo en P-001, P-103 y P-201 | HU-003, HU-007, HU-011 |
| 5 | 15 reglas MUST no se citan en ninguna spec de pantalla (ver sección 2) | Gap | Baja | Agregar cada regla a la línea «Reglas» de la pantalla que la refleja, o marcarla como «sin efecto en UI» | HU-012, HU-015 |
| 6 | Si el correo de una compra falla tras 3 reintentos, el apoderado no se entera (RN-024, RN-025) | Gap | Baja | Documentar como limitación del piloto en `09-ui-spec.md` o agregar una tarea para mostrar un aviso | HU-015 |
| 7 | El mensaje de `LIMITE_MENSUAL_EXCEDIDO` dice «tope mensual del alumno» sin el monto, a diferencia del diario («S/ 30.00») | Mejora | Baja | Mostrar el monto del tope si la respuesta de la API lo incluye; si no, dejar el mensaje genérico | HU-012 |
| 8 | Sin confirmar: formato de fecha `AAAA-MM-DD HH:mm` en las apps frente a `dd/mm/aaaa` (Q-06) | Mejora | Baja | Confirmar el formato; mantener `AAAA-MM-DD HH:mm` hasta que el equipo decida | HU-015 |

## 5. Decisiones recomendadas antes de implementar

1. Agregar a la arquitectura el endpoint de usuarios del colegio y el campo `familiaActiva` (observaciones 1 y 2).
2. Definir el canal de contacto para credenciales olvidadas (observación 4).
3. Numerar los tags para el apoderado (observación 3).
4. Completar las citas de reglas en `09-ui-spec.md` (observación 5).
5. Las observaciones 6, 7 y 8 pueden pasar al backlog.

## 6. Score de completitud

- **HU Must cubiertas por la especificación:** 100 % (19/19).
- **Reglas MUST citadas en las pantallas:** 80.0 % (60/75). Con las 6 reflejadas sin cita: **88.0 %** (66/75).
- **Observaciones:** 8 (0 Alta, 4 Media, 4 Baja). Ninguna bloquea la implementación.
