# 08 — Backlog técnico: LUQUITA

- **Producto:** Luquita, billetera digital infantil pospago con tag NFC para la tienda del colegio.
- **País:** Perú. **Piloto:** colegio particular en Lurigancho-Chosica, Lima (~200 alumnos de 8 a 14 años).
- **Fase:** planificación. Agente Backlog Builder (BMAD), generado desde `08-backlog.xml`.
- **Documentos base:** `07-architecture.md` v0.5 (2026-11-13), `06-user-stories-refinadas.md` (actualizado el 2026-11-11), `03-reglas-de-negocio.md` (actualizado el 2026-11-11), `02-alcance.md` (actualizado el 2026-11-11) y `00-constitucion.md` v1.17 (por referencia).
- **Alcance:** MVP (Must M1 a M5): HU-001 a HU-018 y HU-033.
- **Fecha:** 2026-11-13. **Versión:** 0.4.3 (borrador; las v0.4.2 y anteriores fueron aprobadas el 2026-11-13 por David Yallerco).
- **Cambios de la v0.2:** se incorporan las decisiones del equipo sobre las observaciones de la v0.1 (ver «Decisiones del equipo incorporadas») y se retiran los trámites legales, porque es un piloto académico.
- **Cambios de la v0.3 (decisiones del equipo tras la auditoría `09`):** DNI duplicado en el Excel (T-040); temporada de ventas de marzo a noviembre con habilitación manual (nuevas T-091 a T-093; cambian T-017, T-040, T-041, T-059, T-067, T-080 y T-084); se retiran las tareas de reclamos (T-073 a T-076 y T-085) y el consentimiento (T-046, T-049). Ver «Decisiones del equipo incorporadas».
- **Ajuste v0.3.1 (auditoría M-01):** la idempotencia de la compra guarda el resultado de cada intento, también los rechazos; cambian T-056, T-057, T-060, T-064 y T-082. Los conteos no cambian.
- **Cambios de la v0.4 (simplificación del piloto, 2026-11-13, constitución 1.17):** sin suscripción, sin cierre mensual ni periodos (se retiran T-067 y T-068; T-069 calcula el consolidado al consultarlo), sin fin del año escolar (se retiran T-071 y T-072), la app del dueño activa es un interruptor sin fechas (T-091 a T-093 simplificadas), el Excel lo importan la secretaria o el administrador sin columna `estado` (T-040, T-042), la secretaria cambia el estado de una familia (T-041, T-043) y el PIN del alumno es de 3 dígitos. HU-018 ya no se divide en HU-018 y HU-018.
- **Cambios de la v0.4.1 (cierre de hallazgos de la auditoría `09`, 2026-11-13):** nueva T-094 (pruebas de autorización); T-003 pasa a M porque los pendientes ya están decididos; T-065 pasa a ejecutor Humano; se precisan T-016, T-030, T-035, T-038, T-052, T-058, T-069, T-077, T-079 y T-080. HU-012 ya no se divide en HU-012 y HU-012.
- **Cambios de la v0.4.2 (2026-11-13):** David Yallerco aprueba los ADR, las dependencias y las decisiones del PO: T-001, T-002 y T-003 quedan completadas y las tareas que esperaban esas aprobaciones quedan desbloqueadas.
- **Cambios de la v0.4.3 (2026-11-14, validación de la UI):** cada tarea de interfaz indica las pantallas P-XXX de `09-ui-spec.md` que implementa y se agrega la sección «Trazabilidad pantalla → tareas»; nuevas T-095 a T-098 (prioridad Could) por las observaciones 6, 7, 9 y 10 de `10-validation-log.md`.

**Convenciones:**
- Tamaños: S (menos de 30 min), M (30 min a 1 h), L (1 a 2 h). Ninguna tarea supera las 2 horas.
- **Ejecutor:** «Humano» cuando la constitución lo exige (PRINCIPIO-009): autenticación, PIN, cifrado, migraciones, dependencias nuevas, aprobaciones y pruebas con hardware real. «Agente IA con revisión humana» cuando la tarea toca dinero, límites, cierre, idempotencia o seguridad sin ser código reservado.
- **Aprobación previa:** ADR, lista de dependencias o pendiente del PO (numeración de `06-user-stories-refinadas.md`) que debe estar resuelto antes de empezar. Las tareas de la Fase 0 resuelven la mayoría.
- **División de HU:** ninguna. HU-012 y HU-018 se trazan completas (2026-11-13, auditoría M-03).
- Toda tarea que crea o cambia un endpoint actualiza `docs/api/openapi.yaml` en el mismo PR (PRINCIPIO-012). Solo datos ficticios: apoderado `00000000`, secretaria `00000001`, alumno `00000010`.

> **Fuera de este backlog:** W1 a W16 (out-of-scope), S1 a S5 y C1 a C4 del alcance y las HU sugeridas HU-021 a HU-032 (HU-025 retirada el 2026-11-13).

---

## Fase 0 — Gobierno

#### T-001: Aprobar ADR-001 a ADR-008 y ADR-010

- **Estado:** ✅ Completada el 2026-11-13 (ADR-001 a ADR-008 y ADR-010 y la lógica de activación de la app del dueño aprobados por David Yallerco).
- **HU relacionada:** Infraestructura
- **Componente:** Transversal
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** Ninguna
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** `docs/arquitectura/adr/ADR-001.md` a `ADR-008.md` y `ADR-010.md`
- **Descripción:** El equipo revisa los nueve ADR propuestos en la arquitectura 0.5 (cobro en línea, cobro sin confirmar, orden de grabación del tag, bloqueo pesimista, consolidado calculado al consultarlo, monolito modular, token como HMAC, correo con outbox y app del dueño activa). ADR-009 (correlativo de reclamos) está retirado. Cada ADR pasa a «Aprobado» o vuelve con cambios. En la misma revisión se aprueba la lógica de activación de la app del dueño (sección 4.3.6). Ninguna tarea que dependa de estas aprobaciones empieza antes.
- **Criterio de completitud:** ¿Los 9 ADR vigentes y la lógica de activación de la app del dueño están en `docs/arquitectura/adr/` con estado «Aprobado», fecha y nombre de quien aprueba?

#### T-002: Aprobar la lista de dependencias nuevas

- **Estado:** ✅ Completada el 2026-11-13 (dependencias de la sección 6.1 aprobadas por David Yallerco; las versiones exactas se fijan al agregarlas (T-006, T-009, T-010)).
- **HU relacionada:** Infraestructura
- **Componente:** Transversal
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** Ninguna
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** `docs/arquitectura/dependencias-aprobadas.md`
- **Descripción:** Un humano aprueba en bloque las dependencias de la sección 6.1: Apache POI, SDK de ACS Email, BouncyCastle, Testcontainers, ArchUnit o Spring Modulith (se elige una), `flutter_secure_storage` y un plugin NFC de Flutter con NDEF y lock bits de NTAG21x (se elige uno).
- **Criterio de completitud:** ¿El archivo lista cada dependencia con su versión exacta y el estado «Aprobada»?

#### T-003: Confirmar con el PO los códigos ✱ y los pendientes que bloquean el MVP

- **Estado:** ✅ Completada el 2026-11-13 (decisiones del 2026-11-13 firmadas por David Yallerco como responsable del producto).
- **HU relacionada:** Infraestructura
- **Componente:** Transversal
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** Ninguna
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** `docs/arquitectura/decisiones-po.md`
- **Descripción:** Las decisiones sobre los códigos ✱ y los pendientes 3, 5, 7, 8 y 10 a 14 ya están escritas en las HU refinadas (2026-11-13). El PO las revisa, las firma y las copia a `docs/arquitectura/decisiones-po.md`; lo que cambie se anota como riesgo.
- **Criterio de completitud:** ¿`decisiones-po.md` tiene cada decisión con «confirmado» o «cambiado a …», la fecha y el nombre del PO?

## Fase 1 — Setup

#### T-004: Crear la estructura del repositorio

- **HU relacionada:** Infraestructura
- **Componente:** Transversal
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** Ninguna
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `backend/`, `app-apoderados/`, `app-vendedor/`, `panel-web/`, `docs/api/`, `docs/arquitectura/`, `README.md`
- **Descripción:** Crear las carpetas de los cuatro proyectos y de la documentación. El README fija las convenciones: identificadores en español ASCII, Conventional Commits en español y solo datos ficticios. Los diagramas C4 de nivel 1 y 2 se copian a `docs/arquitectura/`.
- **Criterio de completitud:** ¿Existen las 4 carpetas de proyecto y `docs/arquitectura/` contiene los diagramas C4 de nivel 1 y 2?

#### T-005: Crear el esqueleto del backend Spring Boot

- **HU relacionada:** Infraestructura
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-004
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `backend/pom.xml`; paquetes `autenticacion`, `administracion`, `familias`, `alumnos`, `tags`, `compras`, `consolidado`, `notificaciones`, `criptografia`
- **Descripción:** Proyecto Java 21 + Spring Boot 3.x con los starters Web, Security, Data JPA y Validation, más Flyway. Crea los 10 paquetes de módulo vacíos y un endpoint de salud. No agrega ninguna dependencia fuera de las que ya fija la arquitectura.
- **Criterio de completitud:** ¿`mvn verify` termina en verde y existen los 10 paquetes de módulo?

#### T-006: Agregar al pom.xml las dependencias aprobadas del backend

- **HU relacionada:** Infraestructura
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-002, T-005
- **Ejecutor:** Humano
- **Aprobación previa:** Lista de dependencias 6.1
- **Artefactos:** `backend/pom.xml`, `FronterasModulosTest.java`
- **Descripción:** Un humano agrega BouncyCastle, Apache POI, el SDK de ACS Email, Testcontainers y ArchUnit o Spring Modulith con las versiones aprobadas. Agrega también el test que verifica las fronteras entre módulos.
- **Criterio de completitud:** ¿`mvn verify` pasa y el test de fronteras falla cuando un módulo usa una clase interna de otro?

#### T-007: Crear el esqueleto de la App Apoderados

- **HU relacionada:** Infraestructura
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-004
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `app-apoderados/`
- **Descripción:** Proyecto Flutter 3.x para Android 8+ e iOS 15+, con navegación, cliente HTTP con timeouts cortos y textos en español. El tema garantiza contraste de al menos 4.5:1 y áreas táctiles de al menos 48×48 dp.
- **Criterio de completitud:** ¿`flutter test` pasa y el APK de release pesa 25 MB o menos?

#### T-008: Crear el esqueleto de la App Vendedor

- **HU relacionada:** Infraestructura
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-004
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `app-vendedor/`
- **Descripción:** Proyecto Flutter 3.x para Android 8+ con NFC, sin base de datos local. Mismo tema accesible y cliente HTTP que la App Apoderados, con timeout de 8 s preparado para el cobro.
- **Criterio de completitud:** ¿`flutter test` pasa, el APK de release pesa 25 MB o menos y el proyecto no incluye ninguna base de datos local?

#### T-009: Agregar las dependencias aprobadas a la App Apoderados

- **HU relacionada:** Infraestructura
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-002, T-007
- **Ejecutor:** Humano
- **Aprobación previa:** Lista de dependencias 6.1
- **Artefactos:** `app-apoderados/pubspec.yaml`
- **Descripción:** Un humano agrega el plugin NFC elegido (NDEF y lock bits de NTAG21x) y `flutter_secure_storage` para el token de renovación.
- **Criterio de completitud:** ¿`pubspec.yaml` agrega solo esas 2 dependencias, con las versiones aprobadas, y `flutter test` pasa?

#### T-010: Agregar las dependencias aprobadas a la App Vendedor

- **HU relacionada:** Infraestructura
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-002, T-008
- **Ejecutor:** Humano
- **Aprobación previa:** Lista de dependencias 6.1
- **Artefactos:** `app-vendedor/pubspec.yaml`
- **Descripción:** Un humano agrega el plugin NFC elegido (solo lectura en esta app) y `flutter_secure_storage` para el token de renovación del vendedor.
- **Criterio de completitud:** ¿`pubspec.yaml` agrega solo esas 2 dependencias, con las versiones aprobadas, y `flutter test` pasa?

#### T-011: Crear el esqueleto del Panel Web

- **HU relacionada:** Infraestructura
- **Componente:** C3 Panel Web
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-004
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `panel-web/`
- **Descripción:** Proyecto Angular 20 con las rutas `/admin` (administrador de Luquita) y `/colegio` (secretaria), un guard por rol y un interceptor HTTP. Todos los controles de las plantillas llevan `aria-label`.
- **Criterio de completitud:** ¿`ng test` y `ng build` pasan y cada control de las plantillas tiene `aria-label`?

#### T-012: Configurar CI con pruebas, cobertura del 80 % y gitleaks

- **HU relacionada:** Infraestructura
- **Componente:** Transversal
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-005, T-007, T-008, T-011
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Ninguna
- **Artefactos:** Pipeline de CI (por ejemplo, `.github/workflows/ci.yml`), reglas de protección de la rama principal
- **Descripción:** Un job por proyecto: JaCoCo en el backend, `flutter test --coverage` en las apps y `ng test --code-coverage` en el panel. El pipeline falla si un proyecto baja del 80 % o si gitleaks detecta un secreto. El merge exige CI en verde y al menos 1 aprobación humana.
- **Criterio de completitud:** ¿Un PR con un proyecto al 79 % de cobertura, o con un secreto de prueba, queda bloqueado?

#### T-013: Crear el esqueleto de docs/api/openapi.yaml

- **HU relacionada:** Infraestructura
- **Componente:** Transversal
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-003, T-004
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `docs/api/openapi.yaml`
- **Descripción:** Define la URL base, la seguridad Bearer JWT, el header `Idempotency-Key`, el esquema `Problem` (RFC 9457 con `codigo` e `idTraza`), el esquema `Monto` como string con dos decimales y el catálogo de errores 4.1 con las decisiones de T-003.
- **Criterio de completitud:** ¿El archivo valida como OpenAPI 3.x y contiene los 27 códigos del catálogo 4.1?

#### T-014: Crear la base de datos PostgreSQL en Azure

- **HU relacionada:** Infraestructura
- **Componente:** C7 Base de datos
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-001
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** Azure Database for PostgreSQL Flexible Server 16 (Brazil South)
- **Descripción:** Un humano crea el servidor con cifrado AES-256 con llave administrada por el cliente, conexiones solo por TLS, un rol de aplicación sin privilegios de DDL y un rol separado para las migraciones.
- **Criterio de completitud:** ¿Una conexión de prueba entra por TLS con el rol de aplicación y ese rol no puede crear tablas?

#### T-015: Configurar Key Vault, las llaves y la Managed Identity

- **HU relacionada:** Infraestructura
- **Componente:** C8 Gestión de secretos y llaves
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-001
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** Azure Key Vault: llave AES-256 del DNI, llave HMAC-SHA256, llave ES256 del JWT, CMK de PostgreSQL
- **Descripción:** Un humano crea las cuatro llaves y da a la Managed Identity del backend los permisos mínimos para usarlas. Ningún secreto queda en `application.yml` ni en variables de entorno en texto plano.
- **Criterio de completitud:** ¿La identidad del backend accede a las 4 llaves y `application.yml` no contiene ningún secreto?

#### T-016: Configurar el despliegue continuo en Azure

- **HU relacionada:** Infraestructura
- **Componente:** Transversal
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-012, T-014, T-015
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-006
- **Artefactos:** Pipeline de CD; Azure Container Apps; Azure Static Web Apps
- **Descripción:** Cada merge a la rama principal publica la imagen del backend en Container Apps con una sola réplica (mínimo y máximo 1, sin escalado a cero) y Managed Identity y el panel en Static Web Apps con HSTS. Application Insights recibe solo métricas técnicas y logs saneados. Incluye las alertas de Application Insights (correo `CLAVE_INICIAL` en `FALLIDO` y más de 5 correos de compra fallidos en una hora) y una prueba de restauración del respaldo de PostgreSQL (arquitectura §6.7).
- **Criterio de completitud:** ¿Un merge a la rama principal despliega ambos, `GET /salud` responde 200 por HTTPS y el backend tiene configurado mínimo y máximo de 1 réplica?

## Fase 2 — Base transversal

#### T-017: Escribir y ejecutar la migración V1: colegio, personal y familias

- **HU relacionada:** Infraestructura
- **Componente:** C7 Base de datos
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-006, T-014
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** `V1__colegio_personal_familias.sql`: `colegio`, `tienda`, `usuario_personal`, `anio_escolar`, `familia`, `familia_anio`, `cuenta_apoderado`
- **Descripción:** Un humano escribe y ejecuta la migración (los agentes no hacen migraciones, PRINCIPIO-009). Incluye las restricciones únicas de correo, de `dni_hmac` por colegio y de `familia_id` en `cuenta_apoderado`, con el DNI en columnas `bytea`. Incluye en `colegio` las columnas `ventas_habilitadas` (`boolean`, por defecto `false`: app del dueño activa), `ventas_cambiadas_en` y `ventas_cambiadas_por` (ADR-010). `anio_escolar` no lleva `fin_registrado_en`.
- **Criterio de completitud:** ¿Flyway aplica V1 sobre una base vacía y un segundo `cuenta_apoderado` con el mismo `familia_id` falla?

#### T-018: Escribir y ejecutar la migración V2: alumnos, tags y ledger

- **HU relacionada:** Infraestructura
- **Componente:** C7 Base de datos
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-017
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** `V2__alumnos_tags_ledger.sql`: `alumno`, `tag`, `movimiento` (sin tablas `periodo` ni `consolidado*`, retiradas el 2026-11-13)
- **Descripción:** Incluye el check de `tope_mensual` entre 0.00 y 300.00 (por defecto 300.00), `token_hmac` único, `monto NUMERIC(12,2)`, `registrado_en` y `fecha_lima` con valor por defecto del servidor, la restricción única (`registrado_por`, `idempotency_key`) y el índice (`alumno_id`, `fecha_lima`).
- **Criterio de completitud:** ¿Insertar un alumno con `tope_mensual` 300.01 falla y el índice (`alumno_id`, `fecha_lima`) existe?

#### T-019: Escribir y ejecutar la migración V3: ledger de solo inserción

- **HU relacionada:** Infraestructura
- **Componente:** C7 Base de datos
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-018
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** `V3__ledger_inmutable.sql`
- **Descripción:** Otorga al rol de aplicación solo `INSERT` y `SELECT` sobre `movimiento` y `auditoria`, y crea un trigger que rechaza cualquier `UPDATE` o `DELETE` como segunda barrera (PRINCIPIO-004).
- **Criterio de completitud:** ¿Con el rol de aplicación, `UPDATE movimiento SET monto = 0` y `DELETE FROM movimiento` fallan, y con el rol de migraciones el trigger también los rechaza?

#### T-020: Escribir y ejecutar la migración V4: correo, idempotencia, auditoría y sesiones

- **HU relacionada:** Infraestructura
- **Componente:** C7 Base de datos
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-017
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** `V4__soporte.sql`: `correo_saliente`, `correo_intento`, `operacion_idempotente`, `auditoria`, `token_renovacion`
- **Descripción:** Crea las tablas de soporte. `token_renovacion` sigue la sección 3.2: hash del token, cadena de rotación, reemplazo, vencimiento y revocación.
- **Criterio de completitud:** ¿V4 se aplica sobre V1 a V3 y `token_renovacion.token_hash` rechaza un duplicado?

#### T-021: Crear las entidades JPA de administración, familias, alumnos y tags

- **HU relacionada:** HU-001 a HU-010
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-018, T-020
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Entidades y repositorios de `colegio`, `tienda`, `usuario_personal`, `anio_escolar`, `familia`, `familia_anio`, `cuenta_apoderado`, `alumno`, `tag`
- **Descripción:** Mapea las tablas de V1 y V2. El DNI se mapea como `byte[]` sin getters de texto, y `Alumno` no tiene ningún campo fuera de DNI, nombre, PIN y tope (PRINCIPIO-007).
- **Criterio de completitud:** ¿Un test de repositorio guarda y lee cada entidad y `Alumno` no tiene campos de grado, foto ni fecha de nacimiento?

#### T-022: Crear las entidades JPA del ledger y correo

- **HU relacionada:** HU-012, HU-015, HU-018
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-019, T-020
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Entidades y repositorios de `movimiento`, `correo_saliente`, `correo_intento`, `operacion_idempotente`, `auditoria`
- **Descripción:** `Movimiento` es inmutable (sin setters) y su repositorio no expone borrados. Los montos usan `BigDecimal`.
- **Criterio de completitud:** ¿`MovimientoRepository` no expone métodos `delete` y un test confirma que modificar un movimiento ya guardado lanza una excepción?

#### T-023: Crear ReglasConsumoProperties con los valores fijos

- **HU relacionada:** HU-013, HU-014, HU-015, HU-018, HU-033
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-001, T-005
- **Ejecutor:** Agente IA
- **Aprobación previa:** ADR-004, ADR-005
- **Artefactos:** `ReglasConsumoProperties.java`
- **Descripción:** Una clase `@ConfigurationProperties` validada con los valores de la sección 6.3. Ninguna otra clase escribe estos números a mano.
- **Criterio de completitud:** ¿Un test verifica límite diario 30.00, tope 300.00, PIN del alumno de 3 dígitos, 3 fallos de PIN, 5 fallos de acceso, 900 s de bloqueo y 3 reintentos cada 60 s?

#### T-024: Implementar el manejo de errores RFC 9457

- **HU relacionada:** Infraestructura
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-005, T-013
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `ManejadorErrores.java`, excepciones de dominio
- **Descripción:** Un `@RestControllerAdvice` responde `application/problem+json` con `codigo` estable e `idTraza`, y `errores[]` por campo en `DATOS_INVALIDOS`. Si la base de datos o Key Vault no responden, devuelve `503 SERVICIO_NO_DISPONIBLE`.
- **Criterio de completitud:** ¿Un test de controlador recibe `status`, `codigo` e `idTraza` para `DATOS_INVALIDOS` y para `SERVICIO_NO_DISPONIBLE`?

#### T-025: Crear el tipo Monto

- **HU relacionada:** HU-012, HU-013, HU-018
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-005
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `Monto.java`, serializador JSON
- **Descripción:** `BigDecimal` con escala 2 y `HALF_EVEN`, parseado desde el patrón `^[0-9]{1,10}\.[0-9]{2}$` y serializado como string (`"3.50"`). Prohíbe `float` y `double` (PRINCIPIO-004).
- **Criterio de completitud:** ¿`"0.10"` + `"0.20"` da exactamente `"0.30"`, `"3.505"` y `"-3.50"` se rechazan y ninguna clase del módulo usa `float` ni `double`?

#### T-026: Implementar el filtro de enmascaramiento de logs

- **HU relacionada:** Infraestructura
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-005
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Ninguna
- **Artefactos:** Configuración de Logback, `FiltroDatosSensibles.java`
- **Descripción:** Enmascara `pin`, `clave`, `dni`, `token`, `tokenRenovacion`, el header `Authorization` y el parámetro de URL `dni`. El token del tag aparece solo con sus 4 primeros caracteres (`tagId=9f1c…`).
- **Criterio de completitud:** ¿Un test que registra `00000000`, `00000001`, `00000010`, un JWT y el PIN `730` produce un log sin ninguno de esos valores?

#### T-027: Implementar el cifrado del DNI y el índice ciego HMAC

- **HU relacionada:** HU-002, HU-004 a HU-009
- **Componente:** C8 Gestión de secretos y llaves
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-006, T-015
- **Ejecutor:** Humano
- **Aprobación previa:** ADR-007
- **Artefactos:** Módulo `criptografia`: `CifradoDni`, `IndiceCiego`
- **Descripción:** AES-256-GCM con nonce de 96 bits por registro y HMAC-SHA256, con las llaves de Key Vault cargadas por Managed Identity. El mismo HMAC sirve para el token del tag. Código de cifrado: solo humanos (PRINCIPIO-009).
- **Criterio de completitud:** ¿Cifrar `00000010` dos veces da nonces y textos cifrados distintos, ambos descifran a `00000010` y el HMAC es el mismo en los dos casos?

#### T-028: Implementar el hash Argon2id de PIN y claves

- **HU relacionada:** HU-002, HU-003, HU-006 a HU-008, HU-011, HU-012
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-006
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** Módulo `criptografia`: `HashCredenciales`
- **Descripción:** Argon2id con `m=19456 KiB, t=2, p=1` para el PIN del apoderado, el PIN del alumno y las claves del personal. Código de PIN y claves: solo humanos.
- **Criterio de completitud:** ¿El hash empieza con `$argon2id$v=19$m=19456,t=2,p=1` y verifica `730` pero no `731`?

#### T-029: Implementar JWT ES256 y las reglas de Spring Security

- **HU relacionada:** HU-003, HU-007, HU-011
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-015, T-024
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** Módulo `autenticacion`: emisión y validación de JWT, `SecurityFilterChain`
- **Descripción:** JWT ES256 de 15 minutos con los claims `sub`, `rol`, `colegioId` y `cambioClavePendiente`, sin DNI. Las reglas por rol cubren los cuatro roles, y un token con `cambioClavePendiente` solo sirve para el cambio de clave (`403 CAMBIO_CLAVE_PENDIENTE`).
- **Criterio de completitud:** ¿Un JWT con 15 min y 1 s de antigüedad responde `401 NO_AUTENTICADO` y su payload no contiene ningún DNI?

#### T-030: Implementar la renovación de sesión con tokens rotativos

- **HU relacionada:** HU-003, HU-007, HU-011
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-020, T-029
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** `POST /autenticacion/renovacion`
- **Descripción:** Cada uso invalida el token anterior y emite uno nuevo; reusar un token ya rotado revoca toda la cadena. La vigencia es de 12 h para el personal y 30 días para el apoderado, y el servidor guarda solo el hash. Un proceso diario borra los tokens vencidos hace más de 30 días.
- **Criterio de completitud:** ¿Reusar un token ya rotado responde `401` y deja inválido también el token que lo reemplazó?

#### T-031: Implementar la idempotencia de los POST de negocio distintos de la compra

- **HU relacionada:** HU-006, HU-008, HU-009, HU-016, HU-017
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-021, T-022, T-024
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Pendiente PO 3
- **Artefactos:** `InterceptorIdempotencia.java`, tabla `operacion_idempotente`
- **Descripción:** Sin header responde `400 IDEMPOTENCY_KEY_REQUERIDA`; el contenido se compara con `hash_solicitud` calculado con HMAC-SHA256 (el mismo mecanismo que usa la compra en T-056); la misma clave con el mismo contenido devuelve la respuesta original sin nuevo efecto; con otro contenido responde `409 IDEMPOTENCY_KEY_REUTILIZADA`. Un proceso diario borra los registros de más de 7 días.
- **Criterio de completitud:** ¿Dos POST de prueba con la misma clave y el mismo cuerpo producen un solo efecto y la misma respuesta, y con otro cuerpo responden `409`?

#### T-032: Tomar el colegio del JWT y registrar la auditoría

- **HU relacionada:** Infraestructura
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-021, T-029
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Ninguna
- **Artefactos:** `ContextoColegio.java`, `ServicioAuditoria.java`
- **Descripción:** El `colegio_id` de cada operación sale solo del JWT, nunca del request. La auditoría es de solo inserción y no guarda DNI, PIN, claves ni tokens.
- **Criterio de completitud:** ¿Un request que envía otro `colegioId` en el cuerpo opera sobre el colegio del JWT, y la fila de auditoría no contiene ningún DNI?

#### T-033: Implementar el encolado de correos (outbox)

- **HU relacionada:** HU-002, HU-015
- **Componente:** C6 Servicio de correo (outbox)
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-022
- **Ejecutor:** Agente IA
- **Aprobación previa:** ADR-008
- **Artefactos:** `ServicioCorreoSaliente.java`
- **Descripción:** Inserta una fila `PENDIENTE` en `correo_saliente` dentro de la transacción del llamador (propagación `MANDATORY`). Nunca envía nada de forma síncrona.
- **Criterio de completitud:** ¿Si la transacción del llamador hace rollback, no queda ninguna fila en `correo_saliente`?

#### T-034: Implementar el envío de correos con reintentos

- **HU relacionada:** HU-002, HU-015
- **Componente:** C6 Servicio de correo (outbox)
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-006, T-015, T-033
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-008
- **Artefactos:** `EnvioCorreoProgramado.java`, tabla `correo_intento`
- **Pantallas (`09-ui-spec.md`):** P-301
- **Descripción:** Un `@Scheduled` cada 10 s toma los pendientes con `FOR UPDATE SKIP LOCKED` y los envía con Azure Communication Services Email por Managed Identity: 1 intento y 3 reintentos cada 60 s, con registro de cada intento. Borra el cuerpo de `CLAVE_INICIAL` al enviarlo y el de los demás tipos 30 días después.
- **Criterio de completitud:** ¿Con un cliente de correo simulado que falla siempre, quedan 4 intentos `FALLIDO` a +0, +60, +120 y +180 s y no hay un quinto?

## Fase 3 — Features

### Épica 1 — Administración del colegio

#### T-035: Implementar el inicio de sesión del personal, el cambio de clave y el primer administrador

- **HU relacionada:** HU-001, HU-002, HU-003, HU-011
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-021, T-027, T-028, T-030
- **Ejecutor:** Humano
- **Aprobación previa:** Pendiente PO 11
- **Artefactos:** `POST /autenticacion/personal/sesion`, `POST /autenticacion/personal/cambio-clave`
- **Descripción:** La secretaria entra con DNI (búsqueda por HMAC); el dueño y el administrador de Luquita, con correo, comparado sin distinguir mayúsculas. Incluye el script de arranque que crea el primer administrador y le envía su clave aleatoria. Al 5.º fallo la cuenta se bloquea 15 minutos (`423 CUENTA_BLOQUEADA` con `Retry-After`). El cambio rechaza una clave igual a la recibida.
- **Criterio de completitud:** ¿Tras 5 fallos a las 08:00:00, la clave correcta a las 08:01:00 responde `423`, la secretaria que usa su correo como usuario recibe `401` y el administrador creado por el script entra con su correo y debe cambiar la clave? Además, ¿la clave nueva `123456` o igual a la recibida responde `400` (RN-105) y el administrador se bloquea 900 s tras 5 fallos (RN-029)?

#### T-036: Implementar el registro y la lista de colegios

- **HU relacionada:** HU-001
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-031, T-032
- **Ejecutor:** Agente IA
- **Aprobación previa:** Pendiente PO 12
- **Artefactos:** `POST /admin/colegios`, `GET /admin/colegios`, `openapi.yaml`
- **Descripción:** Solo `ADMIN_LUQUITA`. Registra el colegio con su tienda y guarda el nombre sin alterar sus tildes.
- **Criterio de completitud:** ¿El administrador registra «Colegio San José de Chosica», el GET lo devuelve con la tilde y la secretaria `00000001` recibe `403`?

#### T-037: Implementar el registro de la secretaria y del dueño con clave inicial

- **HU relacionada:** HU-002
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-027, T-028, T-033, T-036
- **Ejecutor:** Humano
- **Aprobación previa:** Pendiente PO 11
- **Artefactos:** `POST /admin/colegios/{colegioId}/usuarios`, `GET /admin/usuarios/{usuarioId}`, `GET /admin/colegios/{colegioId}/usuarios`, `openapi.yaml`
- **Descripción:** Genera con `SecureRandom` una clave de 12 caracteres con mayúscula, minúscula y número, guarda solo su hash y encola el correo `CLAVE_INICIAL`. La respuesta y los logs nunca muestran la clave; `GET` expone el estado del envío.
- **Criterio de completitud:** ¿1 000 claves generadas tienen 12 caracteres con mayúscula, minúscula y número, y ninguna aparece en la respuesta ni en los logs?

#### T-038: Crear el inicio de sesión del panel con cambio de clave obligatorio

- **HU relacionada:** HU-001, HU-003
- **Componente:** C3 Panel Web
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-011, T-035
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantallas «Iniciar sesión» y «Cambia tu clave», servicio de sesión
- **Pantallas (`09-ui-spec.md`):** P-001, P-002
- **Descripción:** La secretaria entra con DNI y el administrador de Luquita con correo. Si `cambioClavePendiente` es verdadero, el panel solo muestra «Cambia tu clave». El token se renueva en silencio antes de vencer. El token de renovación viaja en una cookie `HttpOnly`, `Secure` y `SameSite=Strict`; nunca se guarda en `localStorage` ni `sessionStorage` (arquitectura §4.2.4).
- **Criterio de completitud:** ¿Con `cambioClavePendiente = true`, navegar a `/colegio/familias` redirige a «Cambia tu clave»? Además, ¿`localStorage` y `sessionStorage` quedan vacíos después de iniciar sesión?

#### T-039: Crear las pantallas del administrador

- **HU relacionada:** HU-001, HU-002
- **Componente:** C3 Panel Web
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-036, T-037, T-038
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantallas de colegios y usuarios
- **Pantallas (`09-ui-spec.md`):** P-010, P-011, P-012, P-013, P-301
- **Descripción:** Lista y registro de colegios; registro de secretaria (DNI y correo) y de dueño (correo) con el estado del envío de la clave (`PENDIENTE`, `ENVIADO`, `FALLIDO`). Al entrar al detalle del colegio (P-012) carga la secretaria y el dueño ya registrados con `GET /admin/colegios/{colegioId}/usuarios`.
- **Criterio de completitud:** ¿Registrar a un dueño sin correo muestra «Falta el correo» sin llamar a la API?

### Épica 2 — Registro de familias

#### T-040: Implementar la importación del Excel de familias

- **HU relacionada:** HU-004
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-027, T-031, T-032
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Pendiente PO 10 (solo ceros iniciales)
- **Artefactos:** `POST /colegios/{colegioId}/familias/importaciones`, `openapi.yaml`
- **Descripción:** Solo la secretaria (su colegio) o el administrador. Lee un `.xlsx` de hasta 2 MB con Apache POI y exige las 4 columnas (`dni_apoderado`, `nombre`, `apellidos`, `correo`); el DNI debe ser texto de 8 dígitos. Toda familia importada queda `ACTIVO` (RN-053). Un DNI de una familia que ya existía antes de la importación la actualiza y la deja `ACTIVO` (RN-054); si el mismo DNI se repite dentro del archivo, solo se procesa la primera fila y las siguientes se rechazan con «DNI duplicado» (RN-098). Las familias van al año escolar del año calendario de la importación; si ese año no existe, `AnioEscolarService` lo crea.
- **Criterio de completitud:** ¿Un archivo con 148 filas válidas, la fila 12 sin DNI, la fila 45 con el DNI numérico `5` y la fila 87 con el mismo DNI que la fila 5, importado por el administrador, responde `registradas: 147`, rechaza las filas 12, 45 y 87 (esta con «DNI duplicado») y deja las 147 familias en `ACTIVO`, y el dueño que intenta importar recibe `403`?

#### T-041: Implementar el registro manual, la búsqueda y la corrección de familias

- **HU relacionada:** HU-005
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-027, T-031, T-032, T-040
- **Ejecutor:** Agente IA
- **Aprobación previa:** Pendiente PO 12
- **Artefactos:** `POST /colegio/familias`, `PATCH /colegio/familias/{familiaId}`, `GET /colegio/familias`, `openapi.yaml`
- **Descripción:** Alta manual con DNI de 8 dígitos (con la misma regla de año escolar, usando `AnioEscolarService` de T-040), corrección del correo, cambio de estado `ACTIVO`/`INACTIVO` solo por la secretaria (RN-104) y búsqueda por DNI con el índice ciego, o listado paginado de hasta 50. Las respuestas nunca devuelven el DNI completo.
- **Criterio de completitud:** ¿Tras el PATCH a `nuevo@ejemplo.pe`, el siguiente correo encolado va a esa dirección; tras el PATCH a `INACTIVO`, una compra de su hijo responde `422 FAMILIA_INACTIVA`, y el apoderado o el dueño que intentan el PATCH reciben `403`?

#### T-042: Crear la pantalla de importación del Excel

- **HU relacionada:** HU-004
- **Componente:** C3 Panel Web
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-038, T-040
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantalla de importación, plantilla `.xlsx` descargable
- **Pantallas (`09-ui-spec.md`):** P-021
- **Descripción:** Carga del archivo para la secretaria y, en la ficha del colegio, para el administrador; resumen del resultado y una plantilla de 4 columnas con `dni_apoderado` en formato texto para evitar la pérdida de ceros.
- **Criterio de completitud:** ¿El panel muestra «148 familias registradas — 2 filas rechazadas» con el motivo de cada fila y rechaza un `.csv` antes de enviarlo?

#### T-043: Crear las pantallas de búsqueda, alta y corrección de familias

- **HU relacionada:** HU-005
- **Componente:** C3 Panel Web
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-038, T-041
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantallas de familias
- **Pantallas (`09-ui-spec.md`):** P-020, P-022, P-023, P-024
- **Descripción:** Búsqueda por DNI, lista paginada, alta manual, cambio de correo y cambio de estado (`ACTIVO`/`INACTIVO`) con confirmación, con validación del DNI antes de enviar.
- **Criterio de completitud:** ¿El DNI `00A00000` muestra «DNI inválido» y la lista no muestra ningún DNI completo?

### Épica 3 — Cuenta del apoderado y alumnos

#### T-044: Implementar la creación de la cuenta del apoderado

- **HU relacionada:** HU-006
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-021, T-027, T-028, T-031
- **Ejecutor:** Humano
- **Aprobación previa:** Pendiente PO 14
- **Artefactos:** `POST /apoderados`, `openapi.yaml`
- **Descripción:** Endpoint público con límite de solicitudes por IP, en memoria (el backend tiene una sola réplica). Solo crea la cuenta si el DNI pertenece a una familia `ACTIVO` sin cuenta; el PIN de 6 dígitos se guarda con Argon2id.
- **Criterio de completitud:** ¿`00000000` (`ACTIVO`) con `482915` responde 201, `00000002` (`INACTIVO`) responde `422 DNI_NO_REGISTRADO` y el PIN `4829` responde 400?

#### T-045: Implementar el inicio de sesión del apoderado

- **HU relacionada:** HU-007
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-029, T-030, T-044
- **Ejecutor:** Humano
- **Aprobación previa:** Pendiente PO 11
- **Artefactos:** `POST /autenticacion/apoderados/sesion`, `openapi.yaml`
- **Descripción:** Inicio de sesión con DNI y PIN; un DNI sin cuenta recibe la misma respuesta que un PIN incorrecto. Al 5.º fallo, bloqueo de 15 minutos.
- **Criterio de completitud:** ¿El DNI `11111111` sin cuenta y un PIN incorrecto reciben el mismo cuerpo `401`, y 4 fallos seguidos de un acierto dan 200?

#### T-046: Implementar el registro de un hijo con su PIN

- **HU relacionada:** HU-008
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-027, T-028, T-031, T-032
- **Ejecutor:** Humano
- **Aprobación previa:** Pendiente PO 12
- **Artefactos:** `POST /apoderado/alumnos`, `openapi.yaml`
- **Descripción:** El DTO solo mapea `nombre`, `dni` y `pin`; cualquier otro campo se ignora. Guarda el tope de 300.00.
- **Criterio de completitud:** ¿Un cuerpo con `grado: "5to primaria"` guarda solo DNI y nombre, y el PIN `730` responde `400 PIN_FORMATO_INVALIDO`?

#### T-047: Implementar la lista de hijos con sus tags

- **HU relacionada:** HU-008, HU-016, HU-017
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-021, T-032
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `GET /apoderado/alumnos` (con `familiaActiva` y `activadoEn`), `openapi.yaml`
- **Descripción:** Devuelve solo los hijos de la familia del token, con sus tags y estados, sin DNI, PIN, token ni consumo. Excluye las reservas de tag vencidas. Agrega `familiaActiva` (falso si la familia está `INACTIVO`) y `activadoEn` por tag, y ordena los tags por `activadoEn`.
- **Criterio de completitud:** ¿La respuesta no contiene `00000010`, no incluye hijos de otra familia y trae `familiaActiva: false` cuando la familia está `INACTIVO`?

#### T-048: Crear las pantallas de cuenta e inicio de sesión de la App Apoderados

- **HU relacionada:** HU-006, HU-007
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-009, T-044, T-045
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantallas «Crear cuenta» e «Iniciar sesión»
- **Pantallas (`09-ui-spec.md`):** P-101, P-102, P-103
- **Descripción:** Teclado numérico accesible, mensajes del contrato 4.4.1 y token de renovación en `flutter_secure_storage`, con renovación automática.
- **Criterio de completitud:** ¿`DNI_NO_REGISTRADO` muestra «Tu DNI no está registrado en Luquita. Consulta con la secretaría del colegio.» y el token de renovación no se guarda en `SharedPreferences`?

#### T-049: Crear las pantallas de hijos de la App Apoderados

- **HU relacionada:** HU-008
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-046, T-047, T-048
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantallas «Mis hijos» y «Registrar hijo»
- **Pantallas (`09-ui-spec.md`):** P-104, P-105, P-106
- **Descripción:** Lista de hijos con el estado de sus tags y formulario con nombre, DNI y PIN de 3 dígitos enmascarado. Numera los tags por `activadoEn` («Tag 1», «Tag 2» y «Vinculado el …») y, si `familiaActiva` es falso, muestra el banner de familia inactiva y deshabilita «Vincular tag».
- **Criterio de completitud:** ¿Con un campo vacío el botón «Registrar» queda deshabilitado y el PIN `73` muestra un error?

### Épica 4 — Tags

#### T-050: Implementar la reserva del token del tag

- **HU relacionada:** HU-009, HU-010
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-021, T-027, T-031, T-032
- **Ejecutor:** Humano
- **Aprobación previa:** ADR-003, ADR-007
- **Artefactos:** `POST /apoderado/alumnos/{alumnoId}/tags`, `openapi.yaml`
- **Descripción:** Genera el token con `SecureRandom` (UUID v4), guarda solo su HMAC y su prefijo y deja el tag `RESERVADO` 24 h. Exige familia `ACTIVO` y chip NTAG213 o NTAG215.
- **Criterio de completitud:** ¿La base no contiene el token en claro, `NTAG216` responde `422 TAG_NO_COMPATIBLE` y el apoderado de otra familia recibe `403`?

#### T-051: Implementar la activación del tag

- **HU relacionada:** HU-009
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-050
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-003
- **Artefactos:** `POST /apoderado/tags/{tagId}/activacion`, `openapi.yaml`
- **Descripción:** Compara el contenido leído con el HMAC de la reserva y exige `bloqueadoContraEscritura: true` antes de pasar el tag a `ACTIVO`.
- **Criterio de completitud:** ¿Un contenido distinto responde `422 DATOS_INVALIDOS`, repetir con la misma `Idempotency-Key` responde 200 y con otra clave responde `409 TAG_YA_GRABADO`?

#### T-052: Validar el chip en la App Apoderados antes de grabar

- **HU relacionada:** HU-010
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-009 T-003
- **Ejecutor:** Agente IA
- **Aprobación previa:** Pendiente PO 7
- **Artefactos:** Servicio de lectura NFC, pruebas con lecturas simuladas
- **Pantallas (`09-ui-spec.md`):** P-107
- **Descripción:** Lee el tipo de chip, verifica que esté vacío y sin lock bits y muestra el mensaje de rechazo. Se prueba con lecturas NFC simuladas.
- **Criterio de completitud:** ¿Con lecturas simuladas, MIFARE Classic y NTAG216 muestran «Este tag no es compatible con Luquita» y un NTAG215 con datos o con lock bits se rechaza sin escribir?

#### T-053: Grabar y vincular el tag en la App Apoderados

- **HU relacionada:** HU-009
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-049, T-050, T-051, T-052
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-003
- **Artefactos:** Flujo «Vincular tag»
- **Pantallas (`09-ui-spec.md`):** P-107
- **Descripción:** Sigue el orden de ADR-003: reservar, grabar el NDEF, releer y comparar, aplicar los lock bits y activar con reintentos. Avisa si el NFC está apagado o no existe.
- **Criterio de completitud:** ¿Si la relectura simulada falla, la app no aplica los lock bits y ofrece reintentar con el mismo token?

### Épica 5 — Cobro (incluye el correo por compra de la Épica 6)

#### T-054: Implementar la resolución del nombre del alumno

- **HU relacionada:** HU-012
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-021, T-027, T-032
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-007
- **Artefactos:** `POST /tags/resolucion`, `openapi.yaml`
- **Descripción:** El token llega en el cuerpo y se busca por HMAC, sin caché. Solo responde `nombreAlumno`.
- **Criterio de completitud:** ¿La respuesta tiene un único campo, `nombreAlumno`, y el token `luquita:tag:00000000-0000-4000-8000-000000000000` responde `404 TAG_NO_RECONOCIDO`?

#### T-055: Armar el correo de cada compra

- **HU relacionada:** HU-015
- **Componente:** C6 Servicio de correo (outbox)
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-025, T-033
- **Ejecutor:** Agente IA
- **Aprobación previa:** ADR-008
- **Artefactos:** `PlantillaCorreoCompra.java`
- **Pantallas (`09-ui-spec.md`):** P-302
- **Descripción:** Arma asunto y cuerpo en el momento de la compra: fecha y hora de Lima, tienda, monto y consumo del mes de la compra.
- **Criterio de completitud:** ¿Una compra del 2026-11-30T23:59:30-05:00 genera «2026-11-30 23:59» y «Consumo de noviembre» aunque el envío ocurra el 1 de diciembre?

#### T-056: Implementar la transacción base de POST /compras

- **HU relacionada:** HU-012
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-019, T-022, T-023, T-024, T-025, T-054, T-055
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-001, ADR-004, ADR-007
- **Artefactos:** `POST /compras`, `openapi.yaml`
- **Descripción:** Valida monto y descripción y aplica la idempotencia de la compra con `operacion_idempotente` (operación `COMPRA`, `hash_solicitud` con HMAC-SHA256): si ya hay un resultado para (vendedor, clave) con el mismo contenido, lo devuelve sin ejecutar nada más, sea un cobro o un rechazo; con otro contenido responde `409 IDEMPOTENCY_KEY_REUTILIZADA`. Guarda el resultado de todo rechazo de negocio y de todo cobro en la misma transacción antes de responder; no guarda nada si la transacción no se confirma. Ante una solicitud concurrente con la misma clave, la restricción única provoca rollback completo y se devuelve el resultado ya guardado (arquitectura 2.1, M-01). Resuelve el tag por HMAC sin caché y toma `FOR UPDATE` del tag y luego del alumno. Rechaza `TAG_BLOQUEADO` y `FAMILIA_INACTIVA`, inserta la `COMPRA` y el correo en la misma transacción y no consulta deudas. Deja el punto de verificación del PIN para T-057, cerrado por defecto.
- **Criterio de completitud:** ¿S/ 3.50 válidos crean un movimiento `COMPRA` +3.50, una fila en `correo_saliente` y una en `operacion_idempotente`; S/ 0.00, S/ 3.505 o una descripción de 61 caracteres responden 400 sin movimientos, y un `422 TAG_BLOQUEADO` reintentado con la misma clave devuelve el mismo `422` sin volver a leer el tag?

#### T-057: Verificar el PIN del alumno en la compra

- **HU relacionada:** HU-012, HU-014
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-028, T-056
- **Ejecutor:** Humano
- **Aprobación previa:** ADR-004
- **Artefactos:** `POST /compras` (paso g del flujo 2.1)
- **Descripción:** Verifica Argon2id con el bloqueo tomado. Cada fallo suma 1; al 3.er fallo consecutivo el tag pasa a `BLOQUEADO` (`PIN_FALLIDO`). El contador y el resultado `401` se confirman en la misma transacción (con `tagBloqueado: true` al 3.er fallo), para que un reintento con la misma clave devuelva ese `401` sin volver a verificar (M-01). Un acierto reinicia el contador.
- **Criterio de completitud:** ¿`111`, `222` y `333` dejan el tag `BLOQUEADO` y una cuarta solicitud con `730` responde `422 TAG_BLOQUEADO` sin crear movimiento?

#### T-058: Aplicar los límites diario y mensual en la compra

- **HU relacionada:** HU-013
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-056
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-004
- **Artefactos:** `POST /compras` (pasos j y k), `openapi.yaml`
- **Descripción:** Suma `COMPRA` y `ANULACION` (sin `AJUSTE`) por `fecha_lima` (día) y por rango de `fecha_lima` del mes calendario dentro de la misma transacción. Valida primero el límite diario; «supera» es estrictamente mayor.
- **Criterio de completitud:** ¿27.00 + 3.00 se registra (día en 30.00), 27.00 + 5.00 responde `422 LIMITE_DIARIO_EXCEDIDO` y 296.00 + 4.00 en el mes se registra? Además, ¿una compra a las 23:59:59 y otra a las 00:00:00 del día siguiente cuentan en días distintos (RN-019)?

#### T-059: Verificar que la app del dueño esté activa y fijar la fecha de la compra

- **HU relacionada:** HU-012, HU-018, HU-033
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-056, T-091
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-005
- **Artefactos:** `POST /compras` y `POST /tags/resolucion` (paso c2)
- **Descripción:** Antes de leer el tag o verificar el PIN, lee `colegio.ventas_habilitadas` sin caché; si es `false` (app del dueño desactivada), responde `409 VENTAS_NO_HABILITADAS` (RN-097). Fija `registrado_en` y `fecha_lima` con el reloj del servidor en `America/Lima` e ignora cualquier fecha del cliente (RN-034).
- **Criterio de completitud:** ¿Una compra a las 2027-03-31T23:30:00-05:00 queda con `fecha_lima` 2027-03-31, una con la app del dueño desactivada responde `409 VENTAS_NO_HABILITADAS` sin cambiar el contador de PIN fallidos, y una solicitud que envía otra fecha se registra con la del servidor?

#### T-060: Implementar la verificación de un cobro sin confirmar

- **HU relacionada:** HU-012
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-056
- **Ejecutor:** Agente IA
- **Aprobación previa:** ADR-002, Pendiente PO 8
- **Artefactos:** `GET /compras/operaciones/{idempotencyKey}`, `openapi.yaml`
- **Descripción:** Devuelve el estado de una operación, solo si la registró el mismo vendedor: `REGISTRADA` con su `compraId`, `RECHAZADA` con su `codigo` (y `tagBloqueado`) o `404` si no llegó o no se confirmó (M-01).
- **Criterio de completitud:** ¿Una clave registrada devuelve `REGISTRADA` con su `compraId`, una clave con `401 PIN_INVALIDO` devuelve `RECHAZADA` con `codigo: "PIN_INVALIDO"`, una clave desconocida devuelve 404 y la clave de otro vendedor también devuelve 404?

#### T-061: Crear el inicio de sesión y el cambio de clave de la App Vendedor

- **HU relacionada:** HU-011
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-010, T-035
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantallas «Iniciar sesión» y «Cambia tu clave»
- **Pantallas (`09-ui-spec.md`):** P-201, P-202
- **Descripción:** Inicio de sesión con correo y clave, pantalla obligatoria de cambio de clave y token de renovación en almacenamiento seguro.
- **Criterio de completitud:** ¿Con `cambioClavePendiente = true` la app no muestra la pantalla de cobro?

#### T-062: Crear la pantalla de cobro de la App Vendedor

- **HU relacionada:** HU-012
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-054, T-061
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantalla «Cobrar»
- **Pantallas (`09-ui-spec.md`):** P-204, P-205, P-206
- **Descripción:** Monto en céntimos enteros enviado como string, descripción opcional de hasta 60 caracteres, lectura NFC del token, nombre del alumno y teclado de PIN enmascarado con teclas de 48×48 dp.
- **Criterio de completitud:** ¿Digitar 3,50 envía `"3.50"`, el PIN de 3 dígitos se muestra como `•••` y una descripción de 60 caracteres con «Ñ» se acepta?

#### T-063: Enviar el cobro y mostrar su resultado en la App Vendedor

- **HU relacionada:** HU-012, HU-013, HU-014
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-056, T-057, T-058, T-059, T-062
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Estados «Cobrado», «Rechazado (motivo)» y «Sin conexión»
- **Pantallas (`09-ui-spec.md`):** P-207
- **Descripción:** Traduce cada `codigo` de error a un motivo claro. Sin conexión muestra «Sin conexión: no se puede cobrar con Luquita» y no guarda nada.
- **Criterio de completitud:** ¿Sin red, la app muestra «Sin conexión: no se puede cobrar con Luquita» y no escribe ningún archivo ni preferencia con datos de la compra?

#### T-064: Manejar el cobro sin confirmar en la App Vendedor

- **HU relacionada:** HU-012
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-060, T-063
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-002, Pendiente PO 8
- **Artefactos:** Estado «No se pudo confirmar el cobro»
- **Pantallas (`09-ui-spec.md`):** P-208
- **Descripción:** La `Idempotency-Key` vive solo en memoria. Tras 8 s sin respuesta, ofrece «Verificar» y «Reintentar» (misma clave y mismo contenido) e impide iniciar otro cobro con el mismo tag. Si «Verificar» devuelve `RECHAZADA`, muestra el motivo; si el alumno vuelve a digitar el PIN, genera una clave nueva (M-01).
- **Criterio de completitud:** ¿Con una respuesta perdida simulada, «Reintentar» envía la misma `Idempotency-Key` y el servidor de prueba registra un solo movimiento?

### Épica 6 — Control parental

#### T-065: Implementar el bloqueo y el desbloqueo del tag

- **HU relacionada:** HU-016, HU-017
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** L
- **Dependencias:** T-022, T-031, T-032, T-056
- **Ejecutor:** Humano
- **Aprobación previa:** Pendiente PO 13
- **Artefactos:** `POST /apoderado/tags/{tagId}/bloqueo`, `POST /apoderado/tags/{tagId}/desbloqueo`, `openapi.yaml`
- **Descripción:** Solo el apoderado de la familia bloquea (motivo `MANUAL`, idempotente) o desbloquea (cualquier motivo, contador a cero). Toma los bloqueos en el orden tag → alumno.
- **Criterio de completitud:** ¿Con el tag bloqueado a las 10:00:00, una compra a las 10:00:01 responde `422 TAG_BLOQUEADO`, y el vendedor que intenta desbloquearlo recibe `403`?

#### T-066: Crear el bloqueo y el desbloqueo en la App Apoderados

- **HU relacionada:** HU-016, HU-017
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-049, T-065
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Botones «Bloquear tag» y «Desbloquear tag»
- **Pantallas (`09-ui-spec.md`):** P-104, P-106
- **Descripción:** Botones con `Semantics(label: 'Bloquear tag de Alumno Prueba')`. Sin conexión, el resultado es inequívoco.
- **Criterio de completitud:** ¿Sin red se muestra «El tag NO se bloqueó. Revisa tu conexión e inténtalo de nuevo» y el estado del tag en pantalla no cambia?

### Épica 7 — Consolidado mensual y app del dueño

#### T-069: Implementar la consulta del consolidado en JSON

- **HU relacionada:** HU-018
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-019, T-022, T-027
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `GET /colegio/consolidados/{anio}/{mes}`, `openapi.yaml`
- **Descripción:** Solo la secretaria. Calcula el consolidado en cada consulta sumando los movimientos de cada alumno con `fecha_lima` dentro del mes calendario (ADR-005); descifra el DNI al servir la respuesta, no lista compras ni agrega suscripción. El mes en curso se devuelve con `mesEnCurso: true`; un mes futuro responde `422 MES_FUTURO`.
- **Criterio de completitud:** ¿Octubre de 2026 devuelve `totalFamilia: "240.50"` para "Familia Prueba" (142.00 + 98.50) sin ningún campo de suscripción, el mes en curso trae `mesEnCurso: true`, un mes futuro responde `422 MES_FUTURO` y el dueño recibe `403`? Además, ¿la respuesta no contiene ninguna compra individual (RN-033)?

#### T-070: Crear la pantalla del consolidado en el panel

- **HU relacionada:** HU-018
- **Componente:** C3 Panel Web
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-038, T-069
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Pantalla «Consolidado mensual»
- **Pantallas (`09-ui-spec.md`):** P-020, P-026
- **Descripción:** Selector de año y mes, tabla por familia y aviso «Mes en curso: los totales pueden cambiar». Muestra los montos que envía el servidor, sin recalcularlos.
- **Criterio de completitud:** ¿El panel muestra el total de cada alumno y de cada familia, sin línea de suscripción, y ningún cálculo usa `number` sobre montos?

#### T-091: Implementar la activación y desactivación de la app del dueño

- **HU relacionada:** HU-033
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-021, T-022, T-032
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** ADR-010 y aprobación humana de la lógica de activación de la app del dueño (PRINCIPIO-009)
- **Artefactos:** `PUT /colegios/{colegioId}/ventas`, `GET /colegios/{colegioId}/ventas`, `GET /vendedor/ventas`, `openapi.yaml`
- **Descripción:** Solo la secretaria (su colegio) o el administrador cambian `colegio.ventas_habilitadas` (app del dueño activa), en cualquier fecha. Repetir el mismo valor responde con el mismo estado. Cada cambio queda en `auditoria`. `GET /vendedor/ventas` devuelve el estado al dueño.
- **Criterio de completitud:** ¿La secretaria activa y desactiva la app de su colegio, el administrador la de cualquier colegio, el dueño y la secretaria de otro colegio reciben `403`, y después de desactivarla una compra responde `409 VENTAS_NO_HABILITADAS`?

#### T-092: Crear el interruptor de la app del dueño en el panel

- **HU relacionada:** HU-033
- **Componente:** C3 Panel Web
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-038, T-091
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Interruptor «App del dueño activa» en la pantalla del colegio (secretaria y administrador)
- **Pantallas (`09-ui-spec.md`):** P-012, P-020, P-025
- **Descripción:** Muestra el estado actual y un interruptor «Activar app del dueño» / «Desactivar app del dueño» con confirmación.
- **Criterio de completitud:** ¿Tras activar, la pantalla muestra «App del dueño activa» y, tras desactivar, «App del dueño desactivada», sin recargar la página?

#### T-093: Mostrar en la App Vendedor si está activa

- **HU relacionada:** HU-033
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-061, T-091
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Estado «App de cobro no activa» de la pantalla de cobro
- **Pantallas (`09-ui-spec.md`):** P-203
- **Descripción:** Consulta `GET /vendedor/ventas` al abrir la pantalla de cobro y antes de cada cobro. Con `habilitadas: false` muestra «La app de cobro no está activa. Consulta con la secretaría del colegio.» y oculta el botón «Cobrar» (RN-103). Un `409 VENTAS_NO_HABILITADAS` de `POST /compras` lleva a la misma pantalla.
- **Criterio de completitud:** ¿Con `habilitadas: false` la app no permite ingresar un monto ni leer un tag, y al cambiar a `true` vuelve a mostrar «Cobrar»?

### Mejora Should de la Fase 3

#### T-077: Agregar la descarga del consolidado en Excel

- **HU relacionada:** HU-018
- **Componente:** C4 Luquita API
- **Prioridad:** Should
- **Tamaño:** M
- **Dependencias:** T-069 T-003
- **Ejecutor:** Agente IA
- **Aprobación previa:** Pendiente PO 5
- **Artefactos:** `GET /colegio/consolidados/{anio}/{mes}` con `Accept` de `.xlsx`
- **Pantallas (`09-ui-spec.md`):** P-026
- **Descripción:** Genera el mismo consolidado como `.xlsx` con Apache POI, listo para cargar en la boleta.
- **Criterio de completitud:** ¿El `.xlsx` tiene una fila por alumno, el total de cada familia y los mismos totales que el JSON?

### Mejoras surgidas de la validación de la UI (2026-11-14)

> Origen: `10-validation-log.md`, observaciones 6, 7, 9 y 10. Son mejoras posteriores al MVP (prioridad Could); ninguna bloquea el piloto.

#### T-095: Avisar al apoderado cuando el correo de una compra no se pudo enviar

- **HU relacionada:** HU-015
- **Componente:** C1 App Apoderados
- **Prioridad:** Could
- **Tamaño:** M
- **Dependencias:** T-034, T-049, T-055
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Ninguna
- **Pantallas (`09-ui-spec.md`):** P-104
- **Artefactos:** Aviso en «Mis hijos» cuando una compra quedó con el correo en `FALLIDO`
- **Descripción:** Hoy, si los 3 reintentos fallan (RN-024), la compra se mantiene (RN-025) pero el apoderado no se entera. El aviso muestra solo la fecha y el monto de la compra, sin datos del alumno.
- **Criterio de completitud:** ¿Con un correo en estado `FALLIDO`, P-104 muestra el aviso, y sin correos fallidos no muestra nada?

#### T-096: Mostrar el monto del tope en el rechazo por límite mensual

- **HU relacionada:** HU-012
- **Componente:** C2 App Vendedor
- **Prioridad:** Could
- **Tamaño:** S
- **Dependencias:** T-063
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Pantallas (`09-ui-spec.md`):** P-207
- **Artefactos:** Mensaje de `LIMITE_MENSUAL_EXCEDIDO` con el monto
- **Descripción:** Si la respuesta de la API incluye el tope del alumno, el mensaje lo muestra («La compra supera el tope mensual de S/ 300.00.»); si no, mantiene el texto genérico.
- **Criterio de completitud:** ¿Con un tope de S/ 300.00 en la respuesta, el mensaje contiene «S/ 300.00»?

#### T-097: Avisar en el monto del cobro cuando supera el límite diario

- **HU relacionada:** HU-012
- **Componente:** C2 App Vendedor
- **Prioridad:** Could
- **Tamaño:** S
- **Dependencias:** T-062
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Pantallas (`09-ui-spec.md`):** P-204
- **Artefactos:** Texto de ayuda en la pantalla del monto
- **Descripción:** Si el monto digitado supera S/ 30.00, la pantalla muestra «Este monto supera el límite diario de S/ 30.00.» antes de acercar el tag. El servidor sigue validando el límite (RN-020); el aviso es solo informativo y no bloquea el botón.
- **Criterio de completitud:** ¿Digitar S/ 35.00 muestra el aviso y «Cobrar» sigue habilitado?

#### T-098: Mostrar los intentos de PIN que le quedan al alumno

- **HU relacionada:** HU-014
- **Componente:** C2 App Vendedor
- **Prioridad:** Could
- **Tamaño:** M
- **Dependencias:** T-063
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Cambia el contrato de `POST /compras` (campo con los intentos restantes); requiere aprobación humana (PRINCIPIO-009)
- **Pantallas (`09-ui-spec.md`):** P-206, P-207
- **Artefactos:** Mensaje «PIN incorrecto. Te quedan N intentos.»
- **Descripción:** La respuesta `PIN_INVALIDO` incluye los intentos restantes antes del bloqueo del tag (RN-027) y la app los muestra.
- **Criterio de completitud:** ¿Tras el primer PIN incorrecto el mensaje dice «Te quedan 2 intentos» y tras el segundo «Te queda 1 intento»?

## Fase 4 — Integración

#### T-078: Configurar el remitente real de Azure Communication Services Email

- **HU relacionada:** HU-015
- **Componente:** C6 Servicio de correo (outbox)
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-016, T-034
- **Ejecutor:** Humano
- **Aprobación previa:** ADR-008
- **Artefactos:** Dominio remitente con SPF, DKIM y DMARC
- **Descripción:** Un humano configura el dominio remitente y prueba el envío real a un buzón del equipo, con datos ficticios.
- **Criterio de completitud:** ¿Un correo de prueba llega a la bandeja de entrada, no a spam, con SPF, DKIM y DMARC en «pass»?

#### T-079: Probar la grabación de tags con hardware real

- **HU relacionada:** HU-009, HU-010
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-016, T-053
- **Ejecutor:** Humano
- **Aprobación previa:** ADR-003
- **Artefactos:** Informe de prueba en `docs/pruebas/nfc-hardware.md`
- **Descripción:** Grabar un NTAG213 y un NTAG215 reales con un Android 8 de 2 GB de RAM contra el entorno desplegado, e intentar con un MIFARE Classic real. Antes, un humano firma el APK y lo publica por prueba interna de Google Play para instalarlo en los celulares del piloto (solo Android).
- **Criterio de completitud:** ¿Los dos NTAG quedan `ACTIVO`, otra app NFC no puede reescribirlos y el MIFARE Classic se rechaza?

#### T-080: Probar el primer cobro de punta a punta

- **HU relacionada:** HU-012, HU-013, HU-015
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-037, T-041, T-064, T-078, T-079, T-091
- **Ejecutor:** Humano
- **Aprobación previa:** Ninguna
- **Artefactos:** Informe de prueba en `docs/pruebas/primer-cobro.md`
- **Descripción:** Con datos ficticios en el entorno desplegado: colegio con la app del dueño activa, dueño, familia, alumno y tag real. El vendedor cobra S/ 3.50 con tag y PIN y se mide el tiempo hasta «Cobrado». Las apps se instalan desde la prueba interna de Google Play (T-079).
- **Criterio de completitud:** ¿La compra queda registrada en menos de 3 s con buena señal y llega al buzón de prueba un correo con el monto y el consumo del mes?

## Fase 5 — Testing

#### T-081: Probar la concurrencia de los límites contra PostgreSQL real

- **HU relacionada:** HU-013
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-006, T-058
- **Ejecutor:** Agente IA
- **Aprobación previa:** ADR-004
- **Artefactos:** `LimitesConcurrenciaIT.java` (Testcontainers)
- **Descripción:** Con S/ 25.00 ya consumidos en el día, lanza dos compras simultáneas de S/ 4.00 y repite el escenario.
- **Criterio de completitud:** ¿En 50 repeticiones hay siempre exactamente una respuesta 201 y una `422 LIMITE_DIARIO_EXCEDIDO`, y el consumo queda en 29.00?

#### T-082: Probar la idempotencia de la compra y el correo único

- **HU relacionada:** HU-012, HU-015
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-006, T-056
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `IdempotenciaCompraIT.java` (Testcontainers)
- **Descripción:** Misma clave y mismo contenido 1 s después, misma clave con otro monto, solicitud sin clave, un `401 PIN_INVALIDO` reintentado dos veces con la misma clave y dos solicitudes simultáneas con la misma clave y un PIN incorrecto (M-01).
- **Criterio de completitud:** ¿Hay 1 movimiento y 1 fila en `correo_saliente`, el reintento devuelve 200 con el mismo `compraId`, otro monto responde 409, sin clave responde 400, y en los dos escenarios del PIN incorrecto el contador de fallos queda en 1 y el tag sigue `ACTIVO`?

#### T-083: Probar el bloqueo por PIN fallidos y el desbloqueo

- **HU relacionada:** HU-014, HU-017
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-006, T-057, T-065
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `BloqueoPinIT.java` (Testcontainers)
- **Descripción:** Recorre las secuencias de HU-014: `111`, `222`, `333`; dos fallos y acierto; y `111`, `222`, `730`, `333`, `444`. Después, desbloquea y prueba un fallo.
- **Criterio de completitud:** ¿Las 3 secuencias dejan el estado esperado, el desbloqueo reinicia el contador y ningún log contiene los PIN digitados?

#### T-084: Probar el consolidado mensual y la app del dueño

- **HU relacionada:** HU-018, HU-033
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-006, T-059, T-069, T-091
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `ConsolidadoMensualIT.java` (Testcontainers, reloj controlado)
- **Descripción:** Escenarios: compras del 2026-11-30T23:59:58 y del 2026-12-01T00:00:00 en meses distintos; compra con una fecha enviada por el cliente; consolidado de octubre consultado antes y después de nuevas compras de noviembre; familia `INACTIVO`; app del dueño desactivada y vuelta a activar.
- **Criterio de completitud:** ¿Los escenarios pasan contra PostgreSQL real, el consolidado de octubre da los mismos totales antes y después de las compras de noviembre y ningún consolidado incluye suscripción?

#### T-086: Probar que los logs no contienen datos sensibles

- **HU relacionada:** Criterio transversal (todas las HU)
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-026, T-035, T-037, T-045, T-057
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `LogsSinDatosSensiblesIT.java`
- **Descripción:** Ejecuta inicio de sesión, registro de personal, registro de hijo y una compra con PIN fallido mientras captura todos los logs.
- **Criterio de completitud:** ¿Ninguna línea contiene `00000000`, `00000001`, `00000010`, `482915`, `730`, una clave, un JWT ni un token de tag completo?

#### T-087: Probar que el ledger es de solo inserción

- **HU relacionada:** HU-012, HU-018
- **Componente:** C7 Base de datos
- **Prioridad:** Must
- **Tamaño:** S
- **Dependencias:** T-006, T-019
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `LedgerInmutableIT.java` (Testcontainers)
- **Descripción:** Ejecuta `UPDATE` y `DELETE` sobre `movimiento` con el rol de aplicación y con el rol de migraciones.
- **Criterio de completitud:** ¿Todas las sentencias fallan, por permisos con el rol de aplicación y por el trigger con el rol de migraciones?

#### T-088: Probar la accesibilidad de la App Apoderados

- **HU relacionada:** Criterio transversal (HU-006 a HU-010, HU-016, HU-017)
- **Componente:** C1 App Apoderados
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-053, T-066
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Tests de widget de accesibilidad
- **Descripción:** Pruebas con texto al 200 % y las guías de accesibilidad de Flutter en todas las pantallas.
- **Criterio de completitud:** ¿Todas las pantallas pasan `androidTapTargetGuideline`, `labeledTapTargetGuideline` y `textContrastGuideline` con el texto al 200 %?

#### T-089: Probar la accesibilidad de la App Vendedor

- **HU relacionada:** Criterio transversal (HU-011, HU-012, HU-033)
- **Componente:** C2 App Vendedor
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-064, T-093
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** Tests de widget de accesibilidad
- **Descripción:** Mismas pruebas, con énfasis en el teclado de PIN y los estados del cobro.
- **Criterio de completitud:** ¿Todas las pantallas, incluido el teclado de PIN, pasan las tres guías con el texto al 200 %?

#### T-090: Probar la accesibilidad del Panel Web

- **HU relacionada:** Criterio transversal (HU-001 a HU-005, HU-018, HU-033)
- **Componente:** C3 Panel Web
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-039, T-043, T-070, T-092
- **Ejecutor:** Agente IA con revisión humana
- **Aprobación previa:** Ninguna
- **Artefactos:** Lista de verificación en `docs/pruebas/accesibilidad-panel.md`
- **Descripción:** Revisión con lector de pantalla y zoom al 200 %, más una prueba unitaria que busca controles sin `aria-label`. Automatizarla con axe-core sería una dependencia nueva y necesitaría aprobación.
- **Criterio de completitud:** ¿Las pantallas del panel no tienen controles sin `aria-label`, el contraste es de al menos 4.5:1 y al 200 % no se pierde contenido?

#### T-094: Probar la matriz de autorización

- **HU relacionada:** Criterio transversal (HU-001 a HU-018, HU-033)
- **Componente:** C4 Luquita API
- **Prioridad:** Must
- **Tamaño:** M
- **Dependencias:** T-006, T-029, T-032, T-041, T-047, T-065, T-091
- **Ejecutor:** Agente IA
- **Aprobación previa:** Ninguna
- **Artefactos:** `AutorizacionIT.java` (Testcontainers)
- **Descripción:** Recorre la matriz rol × recurso × colegio y familia: tag, alumno y consolidado de otra familia; app del dueño y Excel de otro colegio; apoderado que cambia su correo o el estado de su familia; dueño que activa su app; vendedor que desbloquea un tag (auditoría M-14).
- **Criterio de completitud:** ¿Cada combinación prohibida responde `403` sin efectos y cada combinación permitida responde 2xx?

---

## Resumen

**Total:** 89 tareas, con un máximo de 108 horas de trabajo si cada tarea usa el límite de su tamaño (v0.4.3: se agregaron T-095 a T-098; v0.4.1: se agregó T-094 y T-003 pasó de L a M; v0.4: se retiraron T-067, T-068, T-071 y T-072; v0.3: se retiraron T-073 a T-076 y T-085 y se agregaron T-091 a T-093).

### Por prioridad

| Prioridad | Tareas |
|---|---|
| Must | 84 |
| Should | 1 |
| Could | 4 |
| **Total** | **89** |

### Por componente

| Componente | Tareas |
|---|---|
| C1 App Apoderados | 10 |
| C2 App Vendedor | 12 |
| C3 Panel Web | 8 |
| C4 Luquita API | 40 |
| C6 Servicio de correo (outbox) | 4 |
| C7 Base de datos | 6 |
| C8 Gestión de secretos y llaves | 2 |
| Transversal | 7 |
| **Total** | **89** |

### Por tamaño

| Tamaño | Tareas | Horas máximas |
|---|---|---|
| S (menos de 30 min) | 14 | 7 |
| M (30 min a 1 h) | 49 | 49 |
| L (1 a 2 h) | 26 | 52 |
| **Total** | **89** | **108** |

### Por ejecutor

| Ejecutor | Tareas |
|---|---|
| Agente IA | 42 |
| Agente IA con revisión humana | 20 |
| Humano | 27 |
| **Total** | **89** |

### Por fase

| Fase | Tareas |
|---|---|
| Fase 0 — Gobierno | 3 |
| Fase 1 — Setup | 13 |
| Fase 2 — Base transversal | 18 |
| Fase 3 — Features | 42 |
| Fase 4 — Integración | 3 |
| Fase 5 — Testing | 10 |
| **Total** | **89** |

## Ruta crítica

Calculada con la duración máxima de cada tamaño (S = 0.5 h, M = 1 h, L = 2 h), suponiendo que las tareas sin dependencia entre sí avanzan en paralelo.

**Hasta el primer cobro de punta a punta** (tag grabado → `POST /compras` → correo al apoderado), 15 tareas y 20 horas:

T-004 (S) → T-005 (M) → T-006 (S) → T-017 (M) → T-018 (L) → T-019 (M) → T-022 (L) → T-031 (L) → T-044 (M) → T-045 (M) → T-048 (L) → T-049 (L) → T-053 (L) → T-079 (M) → T-080 (M)

| Tarea | Título | Tamaño | Ejecutor | Fin acumulado (h) |
|---|---|---|---|---|
| T-004 | Crear la estructura del repositorio | S | Agente IA | 0.5 |
| T-005 | Crear el esqueleto del backend Spring Boot | M | Agente IA | 1.5 |
| T-006 | Agregar al pom.xml las dependencias aprobadas del backend | S | Humano | 2 |
| T-017 | Escribir y ejecutar la migración V1: colegio, personal y familias | M | Humano | 3 |
| T-018 | Escribir y ejecutar la migración V2: alumnos, tags y ledger | L | Humano | 5 |
| T-019 | Escribir y ejecutar la migración V3: ledger de solo inserción | M | Humano | 6 |
| T-022 | Crear las entidades JPA del ledger y correo | L | Agente IA | 8 |
| T-031 | Implementar la idempotencia de los POST de negocio distintos de la compra | L | Agente IA con revisión humana | 10 |
| T-044 | Implementar la creación de la cuenta del apoderado | M | Humano | 11 |
| T-045 | Implementar el inicio de sesión del apoderado | M | Humano | 12 |
| T-048 | Crear las pantallas de cuenta e inicio de sesión de la App Apoderados | L | Agente IA con revisión humana | 14 |
| T-049 | Crear las pantallas de hijos de la App Apoderados | L | Agente IA | 16 |
| T-053 | Grabar y vincular el tag en la App Apoderados | L | Agente IA con revisión humana | 18 |
| T-079 | Probar la grabación de tags con hardware real | M | Humano | 19 |
| T-080 | Probar el primer cobro de punta a punta | M | Humano | 20 |

**Lectura:** 8 de las 15 tareas de la ruta son humanas (aprobaciones, migraciones, credenciales y prueba con hardware). El cuello de botella no es la capacidad del agente de IA, sino la disponibilidad del equipo para aprobar, migrar y escribir el código reservado. La ruta pasa por la cuenta del apoderado, el registro del hijo y la grabación del tag (T-044 a T-053), porque ese lado es más largo que el del vendedor (T-061 a T-064). La cadena T-001 → T-014 → T-017 empata con T-004 → T-005 → T-006 → T-017: si se atrasa la aprobación de los ADR, también se atrasa todo. T-091 (habilitación de ventas), nueva dependencia de T-080, termina a las 11 h y no alarga la ruta.

## Trazabilidad HU → tareas

| HU | Nombre | Tareas | Total |
|---|---|---|---|
| HU-001 | Registrar el colegio piloto | T-021, T-035, T-036, T-038, T-039, T-090 | 6 |
| HU-002 | Registrar a la secretaria y al dueño | T-021, T-027, T-028, T-033, T-034, T-035, T-037, T-039, T-090 | 9 |
| HU-003 | Sesión de la secretaria y cambio de clave | T-021, T-028, T-029, T-030, T-035, T-038, T-090 | 7 |
| HU-004 | Importar el Excel de familias | T-021, T-027, T-040, T-042, T-090 | 5 |
| HU-005 | Registrar manualmente una familia | T-021, T-027, T-041, T-043, T-090 | 5 |
| HU-006 | Crear la cuenta del apoderado | T-021, T-027, T-028, T-031, T-044, T-048, T-088 | 7 |
| HU-007 | Iniciar sesión como apoderado | T-021, T-027, T-028, T-029, T-030, T-045, T-048, T-088 | 8 |
| HU-008 | Registrar a un hijo y su PIN | T-021, T-027, T-028, T-031, T-046, T-047, T-049, T-088 | 8 |
| HU-009 | Grabar y vincular un tag | T-021, T-027, T-031, T-050, T-051, T-053, T-079, T-088 | 8 |
| HU-010 | Rechazar tags no compatibles | T-021, T-050, T-052, T-079, T-088 | 5 |
| HU-011 | Sesión del vendedor y cambio de clave | T-028, T-029, T-030, T-035, T-061, T-089 | 6 |
| HU-012 | Cobrar una compra con tag y PIN | T-022, T-025, T-028, T-054, T-056, T-057, T-059, T-060, T-062, T-063, T-064, T-080, T-082, T-087, T-089, T-096, T-097 | 17 |
| HU-013 | Límites diario y mensual | T-023, T-025, T-058, T-063, T-080, T-081 | 6 |
| HU-014 | Bloqueo tras 3 PIN fallidos | T-023, T-057, T-063, T-083, T-098 | 5 |
| HU-015 | Correo por cada compra | T-022, T-023, T-033, T-034, T-055, T-078, T-080, T-082, T-095 | 9 |
| HU-016 | Bloquear el tag | T-031, T-047, T-065, T-066, T-088 | 5 |
| HU-017 | Desbloquear el tag | T-031, T-047, T-065, T-066, T-083, T-088 | 6 |
| HU-018 | Consolidado mensual | T-022, T-023, T-025, T-059, T-069, T-070, T-077, T-084, T-087, T-090 | 10 |
| HU-033 | Activar y desactivar la app del dueño | T-017, T-023, T-059, T-084, T-089, T-090, T-091, T-092, T-093 | 9 |

Las tareas de infraestructura sin HU (T-001, T-002, T-003, T-004, T-005, T-006, T-007, T-008, T-009, T-010, T-011, T-012, T-013, T-014, T-015, T-016, T-017, T-018, T-019, T-020, T-024, T-026, T-032, T-086, T-094) sostienen a todas las HU. Ninguna HU del MVP queda sin tareas.

## Trazabilidad pantalla → tareas

Todas las pantallas del inventario de `09-ui-spec.md` (secciones 1.1 a 1.4). Las pruebas de accesibilidad T-088, T-089 y T-090 aplican a todas las pantallas de su app.

| Pantalla | Nombre | Tareas |
|---|---|---|
| P-001 | Iniciar sesión (personal) | T-038 |
| P-002 | Cambia tu clave | T-038 |
| P-010 | Colegios (lista) | T-039 |
| P-011 | Registrar colegio | T-039 |
| P-012 | Detalle del colegio | T-039, T-092 |
| P-013 | Registrar secretaria o dueño de tienda | T-039 |
| P-020 | Inicio de la secretaria | T-043, T-070, T-092 |
| P-021 | Importar familias (Excel) | T-042 |
| P-022 | Familias (búsqueda y lista) | T-043 |
| P-023 | Registrar familia | T-043 |
| P-024 | Editar familia | T-043 |
| P-025 | App de cobro (activar o desactivar) | T-092 |
| P-026 | Consolidado mensual | T-070, T-077 |
| P-101 | Bienvenida | T-048 |
| P-102 | Crear cuenta | T-048 |
| P-103 | Iniciar sesión (apoderado) | T-048 |
| P-104 | Mis hijos (inicio) | T-049, T-066, T-095 |
| P-105 | Registrar hijo | T-049 |
| P-106 | Detalle del hijo y sus tags | T-049, T-066 |
| P-107 | Vincular tag (asistente) | T-052, T-053 |
| P-201 | Iniciar sesión (vendedor) | T-061 |
| P-202 | Cambia tu clave (vendedor) | T-061 |
| P-203 | App de cobro no activa | T-093 |
| P-204 | Cobro: monto y descripción | T-062, T-097 |
| P-205 | Cobro: acercar el tag | T-062 |
| P-206 | Cobro: PIN del alumno | T-062, T-098 |
| P-207 | Resultado del cobro | T-063, T-096, T-098 |
| P-208 | Cobro sin confirmar | T-064 |
| P-301 | Clave inicial del personal (correo) | T-034, T-039 |
| P-302 | Aviso de compra (correo) | T-055 |

**Sin tarea por decisión (fuera del MVP):** P-027 (HU-027, Should S1), P-028 (HU-023, procedimiento manual), P-108 (HU-028, Should S2), P-109 (HU-029, Should S3), P-110 (HU-032, Could C3), P-111 (HU-021, riesgo R5 aceptado), P-112 (HU-022, procedimiento manual), P-209 (HU-026, Should S1), P-210 (HU-031, Should S5) y P-303 (HU-024, Should). Las tareas de estas pantallas se crearán cuando sus HU entren al alcance.

## Tareas bloqueadas por aprobaciones o pendientes

> **Actualización (2026-11-13):** T-001, T-002 y T-003 están completadas, así que **ninguna tarea de esta tabla sigue bloqueada**. La tabla queda como registro de qué aprobación necesitaba cada una.

| Tarea | Título | Espera | Se resuelve en |
|---|---|---|---|
| T-006 | Agregar al pom.xml las dependencias aprobadas del backend | Lista de dependencias 6.1 | T-002 |
| T-009 | Agregar las dependencias aprobadas a la App Apoderados | Lista de dependencias 6.1 | T-002 |
| T-010 | Agregar las dependencias aprobadas a la App Vendedor | Lista de dependencias 6.1 | T-002 |
| T-016 | Configurar el despliegue continuo en Azure | ADR-006 | T-001 |
| T-023 | Crear ReglasConsumoProperties con los valores fijos | ADR-004, ADR-005 | T-001 |
| T-027 | Implementar el cifrado del DNI y el índice ciego HMAC | ADR-007 | T-001 |
| T-031 | Implementar la idempotencia de los POST de negocio distintos de la compra | Pendiente PO 3 | T-003 |
| T-033 | Implementar el encolado de correos (outbox) | ADR-008 | T-001 |
| T-034 | Implementar el envío de correos con reintentos | ADR-008 | T-001 |
| T-035 | Implementar el inicio de sesión del personal, el cambio de clave y el primer administrador | Pendiente PO 11 | T-003 |
| T-036 | Implementar el registro y la lista de colegios | Pendiente PO 12 | T-003 |
| T-037 | Implementar el registro de la secretaria y del dueño con clave inicial | Pendiente PO 11 | T-003 |
| T-040 | Implementar la importación del Excel de familias | Pendiente PO 10 | T-003 |
| T-041 | Implementar el registro manual, la búsqueda y la corrección de familias | Pendiente PO 12 | T-003 |
| T-044 | Implementar la creación de la cuenta del apoderado | Pendiente PO 14 | T-003 |
| T-045 | Implementar el inicio de sesión del apoderado | Pendiente PO 11 | T-003 |
| T-046 | Implementar el registro de un hijo con su PIN | Pendiente PO 12 | T-003 |
| T-050 | Implementar la reserva del token del tag | ADR-003, ADR-007 | T-001 |
| T-051 | Implementar la activación del tag | ADR-003 | T-001 |
| T-052 | Validar el chip en la App Apoderados antes de grabar | Pendiente PO 7 | T-003 |
| T-053 | Grabar y vincular el tag en la App Apoderados | ADR-003 | T-001 |
| T-054 | Implementar la resolución del nombre del alumno | ADR-007 | T-001 |
| T-055 | Armar el correo de cada compra | ADR-008 | T-001 |
| T-056 | Implementar la transacción base de POST /compras | ADR-001, ADR-004, ADR-007 | T-001 |
| T-057 | Verificar el PIN del alumno en la compra | ADR-004 | T-001 |
| T-058 | Aplicar los límites diario y mensual en la compra | ADR-004 | T-001 |
| T-059 | Verificar que la app del dueño esté activa y fijar la fecha de la compra | ADR-005, ADR-010 | T-001 |
| T-060 | Implementar la verificación de un cobro sin confirmar | ADR-002, Pendiente PO 8 | T-001, T-003 |
| T-064 | Manejar el cobro sin confirmar en la App Vendedor | ADR-002, Pendiente PO 8 | T-001, T-003 |
| T-065 | Implementar el bloqueo y el desbloqueo del tag | Pendiente PO 13 | T-003 |
| T-091 | Implementar la activación y desactivación de la app del dueño | ADR-010 y aprobación humana de la lógica de activación (PRINCIPIO-009) | T-001 |
| T-077 | Agregar la descarga del consolidado en Excel | Pendiente PO 5 | T-003 |
| T-078 | Configurar el remitente real de Azure Communication Services Email | ADR-008 | T-001 |
| T-079 | Probar la grabación de tags con hardware real | ADR-003 | T-001 |
| T-081 | Probar la concurrencia de los límites contra PostgreSQL real | ADR-004 | T-001 |

## Decisiones del equipo incorporadas (2026-11-11)

| Observación de la v0.1 | Decisión | Dónde se refleja |
|---|---|---|
| `codigo_compra` no estaba en el modelo de datos | ~~Se agrega la columna única `movimiento.codigo_compra`~~. **Retirado en la v0.3:** sin reclamos no se necesita | Arquitectura 3.2; T-018, T-055 |
| Faltaba la tabla de tokens de renovación | Se agrega la tabla `token_renovacion` (hash, cadena de rotación y vencimiento) | Arquitectura 3.2 y 4.2.4; T-020, T-030 |
| No se definía cómo nace el primer periodo | ~~Marzo del primer año escolar, con periodos todos los meses~~. **Reemplazado en la v0.3** por la temporada de ventas (fila siguiente) | — |
| No se definía cómo inicia sesión el administrador | Correo y clave, igual que el dueño; el primero lo crea un humano con un script de arranque | Arquitectura 4.2.2; T-035, T-038 |
| Límite por IP con varias réplicas | El backend corre con una sola réplica, sin escalado a cero; el límite por IP queda en memoria | Arquitectura ADR-006; T-016, T-044 |
| Trámites legales | Se retiran del backlog y de la arquitectura: es un piloto académico | Arquitectura 3.4 y 6.5; HU refinadas (antes Pendiente 15 y brecha 7) |
| **v0.3 — DNI duplicado en el Excel (auditoría A-01)** | Se procesa la primera fila y se rechazan las siguientes con «DNI duplicado» (RN-098); RN-054 solo aplica a familias que ya existían antes de la importación | Arquitectura 4.3.3; T-040 |
| **v0.3 — Primer periodo después de marzo (auditoría A-02; reemplazado en la v0.4)** | Los alumnos solo compran de marzo a noviembre, con las ventas habilitadas por la secretaria o el administrador. El primer periodo es el mes de la primera habilitación; el cierre de noviembre deshabilita las ventas y no hay periodos de diciembre a febrero. `VENTAS_NO_HABILITADAS` reemplaza a `SIN_PERIODO_ABIERTO` | Arquitectura 4.3.6, 4.5.4 y ADR-010; RN-096, RN-097, RN-099 a RN-103; T-017, T-040, T-041, T-059, T-067, T-080, T-084, T-091 a T-093 |
| **v0.3 — Libro de Reclamaciones** | Se retira del piloto | Se eliminan T-073 a T-076 y T-085; cambian T-020, T-022, T-055, T-088 y T-089 |
| **v0.4 — Simplificación del piloto (2026-11-13)** | Sin suscripción, sin cierre mensual ni periodos, sin temporada ni fin del año escolar; app del dueño activa como interruptor; Excel de la secretaria o del administrador con todas las familias `ACTIVO`; PIN del alumno de 3 dígitos | Arquitectura v0.5 (ADR-005 y ADR-010 reescritos); RN-031, RN-034, RN-053, RN-063, RN-097, RN-101 a RN-104; se eliminan T-067, T-068, T-071 y T-072; cambian T-017, T-018, T-022, T-023, T-040 a T-043, T-059, T-069, T-070, T-084 y T-091 a T-093 |
| **v0.3 — Protección de datos personales** | Se retira el detalle (consentimiento y retención de 5 años) | Cambian T-021, T-046, T-049 y T-072 |

## Observaciones abiertas

1. ~~**Suscripción en la temporada de ventas.**~~ Sin efecto desde la v0.4: no hay suscripción.
2. **Accesibilidad del panel sin herramienta automática.** Automatizar la revisión con axe-core sería una dependencia nueva. T-090 la deja como revisión manual más una prueba unitaria de `aria-label`.
