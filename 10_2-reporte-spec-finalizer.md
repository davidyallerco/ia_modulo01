# Reporte del Spec Finalizer — Luquita

> **Estado (2026-11-14, tras las correcciones):** los hallazgos mayores 1 y 2 y los menores 1, 2 y 3 están resueltos; quedan abiertos los menores 4, 5 y 6. El texto de abajo conserva el reporte original, con los nombres antiguos (`DNI_NO_SUSCRITO`, «suscrita») que ya fueron reemplazados.

> **Alcance y método.** Auditoría del Spec Pack v1.0 al 2026-11-14 hecha por Claude (rol Agente Spec Finalizer). Las comprobaciones de IDs, referencias cruzadas, dependencias y campos se ejecutaron con un script sobre los 11 archivos `.md`; las demás son lectura de los documentos. No intervino otro agente ni un revisor humano.

## Resumen ejecutivo

El Spec Pack está **implementable con observaciones**: **0 hallazgos críticos, 2 mayores y 6 menores.**

**Score general:** Operatividad 4/5 · Utilidad 4/5 · Orden 4/5.

Lo más relevante: la trazabilidad regla → HU → pantalla → tarea está completa y los IDs son consistentes. Los dos hallazgos mayores son restos de vocabulario del modelo anterior (suscripción) y una inconsistencia de nombre («app de cobro» frente a «app del dueño»). Ninguno impide implementar, pero un agente de IA los leería como contradicciones.

## 1. Trazabilidad completa

| Cadena | Resultado |
|---|---|
| Regla MUST → HU | 75/75 reglas MUST citadas en `06-user-stories.md` |
| HU Must → pantalla | 19/19 en `09-ui-spec.md` §1.5 |
| Pantalla Must → tarea | 30/30 en `08-backlog.md` («Trazabilidad pantalla → tareas») |
| HU Must → tarea | 19/19 |

**Observaciones:**
- 10 pantallas del inventario no tienen tarea por decisión (Should, Could y Won't); están justificadas en el backlog.
- `08-backlog.md` cita las reglas de forma indirecta (vía HU): 59 de las 75 reglas MUST no aparecen por ID en el backlog. No es un incumplimiento, pero obliga a pasar por la HU para saber qué regla implementa una tarea (hallazgo menor 5).

## 2. Consistencia de IDs

- Todos los `HU-XXX` de `07`, `08` y `09` existen en `06`.
- Todos los `RN-XXX` de `07` y `09` existen en `03`.
- Todos los `P-XXX` de `08` y `09` existen en el inventario (40 pantallas).
- Todos los `ADR-XXX` y `PRINCIPIO-XXX` citados existen.
- Las 89 tareas tienen los 10 campos requeridos, sin dependencias inexistentes, sin ciclos y con tamaños válidos (S, M, L). Los 89 criterios de completitud son preguntas verificables.
- Las citas de RN-039, 043, 044, 055, 057, 092, 095, 096 y 099 en `06` y `08` corresponden a reglas **retiradas** y aparecen solo en notas de retiro: es correcto.
- Los endpoints de `09` coinciden con `07`, salvo la notación de la variable de ruta: `09` escribe `{id}` donde `07` escribe `{colegioId}` (hallazgo menor 3).

## 3. Cobertura de la constitución

Los 12 principios están citados en la especificación y no hay citas a principios inexistentes. Se respeta el modelo de la versión 1.17 (sin suscripción, sin cierre mensual, PIN de 3 dígitos), salvo los restos de vocabulario del hallazgo mayor 1.

## 4. Glosario

- **Mayor 2:** `09-ui-spec.md` usa «app de cobro» 38 veces para lo que el glosario, las reglas y las HU llaman «app del dueño» (de la tienda). El glosario no define la equivalencia.
- **Menor 1:** el glosario no define NDEF, JWT, HMAC, *outbox* ni «token opaco». Están cubiertos Tag, Token del tag, Ledger y Clave de idempotencia.

## 5. Preguntas abiertas

- `05-preguntas-abiertas.md`: sin hallazgos abiertos (puntaje 5/5) antes de esta auditoría.
- `09-ui-spec.md` §7: Q-01 a Q-04 y Q-06 resueltas; quedan Q-05, Q-07 y Q-08, que no bloquean la implementación (Q-05 pertenece a una HU Should).

## 6. Calidad del Documento de Arquitectura

Tiene 7 secciones: overview, componentes, modelo de datos, API, ADR, restricciones técnicas y trazabilidad. Los ADR son coherentes (ADR-009 retirado y declarado). **Menor 2:** el título de la sección 6 dice «alineadas con la constitución 1.16»; debe decir 1.17. La v0.5.3 está en borrador hasta que se aprueben los cambios de API del 2026-11-14.

## 7. Calidad del backlog

89 tareas atómicas (14 S, 49 M, 26 L; 108 h máximo), con estimación, prioridad, dependencias, ejecutor, aprobación previa y criterio de completitud. Sin ciclos.

## 8. Calidad de la UI Spec

- 16 flujos documentados (F-01 a F-16) que cubren las 19 HU Must.
- Las 36 pantallas con screen spec tienen wireframe textual. **Menor 6:** P-028, P-110, P-111 y P-112 figuran solo en el inventario (Won't y Could), por decisión.
- Las 75 reglas MUST están citadas en las pantallas o en la lista de reglas sin efecto en pantalla.

## 9. Score general

| Criterio | Score | Justificación |
|---|---|---|
| Operatividad | 4/5 | Un agente de IA puede implementar el MVP: contratos, pantallas y tareas están trazados. Bajan medio punto los restos del vocabulario de suscripción y el nombre doble de la app. |
| Utilidad | 4/5 | Cubre las 19 HU Must y el flujo crítico de cobro con casos de rechazo. |
| Orden | 4/5 | IDs consistentes y trazabilidad completa; quedan términos inconsistentes. |

## Hallazgos

### Críticos
Ninguno.

### Mayores

| # | Hallazgo | Dónde | Corrección sugerida |
|---|---|---|---|
| 1 | Restos del modelo de suscripción: el código de error `DNI_NO_SUSCRITO` y su mensaje «registrado como suscrito», y los textos «familia(s) suscrita(s)» | Código en `07` (2), `08` (2), `09` (3); textos en `01` (1), `02` (6), `03` (5), `04` (2), `06` (12), `07` (4), `08` (2), `09` (5) | Renombrar el código a `DNI_NO_REGISTRADO`, cambiar el mensaje a «Tu DNI no está registrado en Luquita…» y reemplazar «suscrita» por «registrada» |
| 2 | «App de cobro» y «app del dueño» designan lo mismo sin equivalencia en el glosario | `09-ui-spec.md` (38 usos) frente a `03`, `04` y `06` | Unificar a un solo término y definirlo en `04-glosario.md` |

### Menores

| # | Hallazgo | Acción |
|---|---|---|
| 1 | Glosario sin NDEF, JWT, HMAC, outbox ni token opaco | Agregar las 5 entradas |
| 2 | `07` §6 dice «constitución 1.16» | Cambiar a 1.17 |
| 3 | `09` usa `{id}` en rutas de colegio; `07` usa `{colegioId}` | Unificar la notación |
| 4 | Q-05, Q-07 y Q-08 siguen abiertas en `09` §7 | Resolver cuando se decida el alcance de HU-026 y el logotipo |
| 5 | `08` no cita las reglas por ID (trazabilidad indirecta vía HU) | Agregar la línea «Reglas» a las tareas |
| 6 | P-028, P-110, P-111 y P-112 sin screen spec | Hacerlas cuando entren al alcance |

## Acciones recomendadas (prioridad)

1. Renombrar `DNI_NO_SUSCRITO` y reemplazar «suscrita» por «registrada» (mayor 1).
2. Unificar «app de cobro» con «app del dueño» y definirlo en el glosario (mayor 2).
3. Corregir 1.16 → 1.17 en `07` y la notación `{colegioId}` en `09` (menores 2 y 3).
4. Agregar los términos técnicos al glosario (menor 1).
5. El resto puede esperar a las siguientes iteraciones.
