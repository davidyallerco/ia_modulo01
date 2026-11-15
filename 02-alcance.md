# 02 — Alcance del producto: LUQUITA

- **Producto:** Luquita, billetera digital infantil con tag NFC para la tienda del colegio.
- **Fase:** definición de alcance (Agente PM, BMAD).
- **Piloto:** colegio particular en Lurigancho-Chosica, Lima (unos 200 alumnos de 8 a 14 años).
- **Documentos base:** `00-constitucion.md` (versión 1.17) y `01-project-brief.md`.
- **Fecha:** 2026-11-04. **Actualizaciones:** 2026-11-11 (se retiran el Libro de Reclamaciones y el detalle de protección de datos) y 2026-11-13 (simplificación: sin suscripción, sin temporada ni cierre mensual automático, compras solo con la app del dueño activa, Excel de la secretaria o del administrador con todas las familias activas, sin fin del año escolar y PIN del alumno de 3 dígitos).

---

## 1. IN-SCOPE

### Must have (MVP)

| ID | Funcionalidad | Descripción | Actor |
|---|---|---|---|
| M1 | **Registro de familia y alumnos** | El apoderado crea su cuenta con su DNI, que debe pertenecer a una familia activa registrada por el colegio, y un PIN; luego registra a sus hijos (solo DNI y nombre) y define el PIN de cada alumno. | Apoderado |
| M2 | **Vinculación del tag NFC** | Con la familia ya registrada, el apoderado usa la app móvil de Luquita para grabar un token opaco en el tag (entregado en blanco por el colegio o comprado por su cuenta), bloquearlo contra escritura y vincularlo a uno de sus hijos. | Apoderado |
| M3 | **Cobro con tag + PIN** | El vendedor ingresa el monto y, si quiere, una descripción referencial de la compra; el alumno acerca su tag y digita su PIN de 3 dígitos, y el sistema registra la compra solo si la app del dueño está activa y respeta los límites de S/ 30 diarios y S/ 300 mensuales. | Vendedor, estudiante |
| M4 | **Control parental** | El apoderado recibe un correo por cada compra con el monto y el consumo acumulado del mes, y puede bloquear o desbloquear el tag de su hijo al instante. | Apoderado |
| M5 | **Panel web del colegio y de administración** | El administrador de Luquita registra el colegio, la secretaria y el dueño de la tienda con su correo. Después de la matrícula, la secretaria o el administrador importan un Excel con el DNI, nombre, apellidos y correo de cada apoderado, y todas esas familias quedan activas (la secretaria también puede registrarlas manualmente y pasar una familia a inactiva). La secretaria o el administrador activan y desactivan la app del dueño de la tienda. La secretaria obtiene el consolidado de cualquier mes: el consumo de cada alumno por familia, calculado al consultarlo. | Administrador de Luquita, colegio |

### Should have (después del MVP)

| ID | Funcionalidad | Descripción | Actor |
|---|---|---|---|
| S1 | **Anulación y ajustes** | El vendedor anula una compra errónea del mes en curso con un movimiento de `ANULACION`; la secretaria del colegio registra como `AJUSTE`, en el mes en curso, las correcciones de meses anteriores. | Vendedor, colegio |
| S2 | **Detalle de consumo para el apoderado** | El apoderado solicita desde la app móvil de Luquita el detalle de lo consumido por su hijo en el mes: la lista de compras con fecha, hora, monto y descripción referencial. | Apoderado |
| S3 | **Reemplazo de tag perdido** | El apoderado desvincula desde la app el tag perdido y vincula uno nuevo, sin perder el historial de consumo. | Apoderado |
| S5 | **Reporte mensual para el dueño** | El dueño ve el total del consumo del mes que el colegio debe pagarle. | Vendedor |

### Could have (deseables)

| ID | Funcionalidad | Descripción | Actor |
|---|---|---|---|
| C1 | **Aviso de tope mensual cercano** | El apoderado recibe un correo cuando su hijo llega al 80 % del tope de S/ 300 del mes. | Apoderado |
| C2 | **Resumen mensual por correo** | Al terminar cada mes, el apoderado recibe un correo con el total consumido por cada hijo. | Apoderado |
| C3 | **Tope mensual personalizado** | Después de registrar a su hijo, el apoderado configura un tope mensual entre S/ 0 y S/ 300, que aplica de inmediato. | Apoderado |
| C4 | **Varios celulares de cobro** | La tienda puede cobrar desde más de un celular con NFC al mismo tiempo en recreos concurridos. | Vendedor |

---

## 2. OUT-OF-SCOPE

| # | Funcionalidad | Razón por la que está fuera |
|---|---|---|
| W1 | **Recargas y saldo prepagado** | Luquita es pospago: no recibe recargas ni guarda saldos (PRINCIPIO-003). |
| W2 | **Pagos en línea con Yape, Plin, tarjeta o pasarelas** | Luquita no procesa pagos ni se integra con medios de pago; el cobro lo hace el colegio en su boleta (PRINCIPIO-003). |
| W3 | **Emisión de boletas o comprobantes SUNAT** | Los comprobantes los emiten el colegio y el vendedor, no Luquita (PRINCIPIO-003). |
| W4 | **Cálculo o transferencia del pago a la tienda** | Todos los movimientos de dinero entre el colegio, la tienda y Luquita ocurren fuera del sistema (PRINCIPIO-003). |
| W5 | **Analítica de consumo, perfilamiento o publicidad** | Está prohibido sobre datos de menores (PRINCIPIO-007). |
| W6 | **Datos adicionales del alumno (foto, grado, alergias)** | Solo se guardan DNI y nombre (PRINCIPIO-007). |
| W7 | **Catálogo de productos y restricciones por producto** | El MVP registra solo el monto de cada compra. Qué se vende, incluida la alimentación saludable, lo deciden el colegio y el concesionario. |
| W8 | **App o pago desde el celular del alumno** | El objetivo es que el alumno no necesite celular; paga solo con tag y PIN. |
| W9 | **Compras sin conexión a internet** | El cobro siempre se valida en línea contra el servidor, para respetar los límites y el bloqueo inmediato (PRINCIPIO-005). Sin conexión no se usa Luquita. |
| W10 | **Varios colegios o varias tiendas** | El proyecto es un piloto académico en un solo colegio con una sola tienda. |
| W11 | **Uso del tag fuera de la tienda del colegio** | El consumo es exclusivo de la tienda del colegio piloto (PRINCIPIO-003). |
| W12 | **Bloqueo de compras por deuda de la boleta** | Si el alumno puede asistir al colegio, puede comprar; la deuda la gestiona el colegio fuera del sistema (PRINCIPIO-003). |
| W13 | **Libro de Reclamaciones virtual** (antes M6) | Retirado el 2026-11-11 por decisión del equipo: es un piloto académico. Las dudas sobre una compra se atienden fuera del sistema. |
| W14 | **Detalle de protección de datos personales** (consentimiento formal, registro ante la ANPD, plazo de conservación y eliminación; antes S4) | Retirado el 2026-11-11 por decisión del equipo: es un piloto académico. Se mantienen los datos mínimos del menor y la prohibición de perfilamiento (PRINCIPIO-007). |
| W15 | **Suscripción mensual** (antes S/ 5 por familia, repartida 50/50 entre el colegio y Luquita) | Retirada el 2026-11-13: es un proyecto de prueba (PRINCIPIO-003, versión 1.17). |
| W17 | **Vinculación de tags para apoderados sin celular con NFC** | Fuera del piloto. El apoderado puede iniciar sesión con su cuenta en otro celular con NFC para grabar el tag. |
| W18 | **App para iOS** | El piloto usa solo celulares Android con NFC (2026-11-13). |
| W16 | **Cierre mensual automático, temporada de ventas y fin del año escolar** | Retirados el 2026-11-13 para simplificar el piloto: el consolidado se calcula por mes al consultarlo y las compras dependen solo de que la app del dueño esté activa. |

---

## 3. MVP

Con solo el MVP, el **administrador de Luquita** registra el colegio, a su secretaria y al dueño de la tienda con su correo. La **secretaria del colegio** o el administrador importan las familias en el panel web con un Excel (todas quedan activas) y activan la app del dueño de la tienda. Un **apoderado** crea su cuenta, registra a sus hijos de 8 a 14 años con DNI y nombre, y define el PIN de cada uno. Con la familia ya registrada, el apoderado graba y vincula desde la app móvil de Luquita un tag NFC a cada hijo, usando los tags que le entrega el colegio o que compra por su cuenta. En el recreo, el **estudiante** compra acercando su tag al celular del vendedor y digitando su PIN, sin efectivo ni celular propio; el vendedor puede agregar una descripción referencial, y el sistema rechaza la compra si supera S/ 30 en el día o S/ 300 en el mes. Por cada compra, el apoderado recibe un correo con el monto y lo acumulado del mes, y si su hijo pierde el tag lo bloquea al instante. Cuando lo necesita, el **colegio** obtiene el consolidado de un mes por familia (consumo de cada hijo) para incluirlo en su boleta. Si la app del dueño se desactiva, nadie puede comprar hasta que se vuelva a activar.

---

## 4. Supuestos

### Del negocio (vienen del Project Brief)

- Unas 200 familias quieren usar Luquita. Cinco padres validaron la idea.
- El colegio acepta incluir el consumo en su boleta, pagar al dueño de la tienda cada mes y exigir el pago igual que las pensiones.
- Los alumnos de 8 a 14 años cuidan su tag y recuerdan su PIN de 3 dígitos.
- El dueño de la tienda acepta cobrar una vez al mes, a través del colegio.

### Del alcance (decisiones de este documento, por confirmar)

- El **apoderado** tiene un celular con NFC capaz de grabar el tag desde la app de Luquita.
- Los tags que el apoderado compra por su cuenta son NTAG213 o NTAG215, como exige el PRINCIPIO-006.
- El "detalle" que recibe el apoderado es la **lista de compras (fecha, hora, monto y descripción referencial)**, no un catálogo de productos (W7).
- El colegio tiene la lista de familias en un Excel con las columnas DNI del apoderado, nombre, apellidos y correo.
- Un solo celular Android con NFC basta para atender el recreo, y la tienda tiene internet estable.
- El año escolar del colegio va de marzo a diciembre, aproximadamente; la secretaria activa y desactiva la app del dueño según el calendario del colegio.

---

## 5. Restricciones

### Técnicas (de la constitución)

- **Stack cerrado:** Java 21 + Spring Boot 3, PostgreSQL, Flutter, Angular 20 para el panel web del colegio, y Azure (PRINCIPIO-002).
- **Integridad del consumo:** ledger de solo inserción, montos en `BigDecimal` o céntimos, idempotencia y movimientos con la fecha del servidor, de modo que un mes ya entregado no cambia (PRINCIPIO-004).
- **Seguridad:** tag con token opaco y PIN del alumno en cada compra (PRINCIPIO-006); cifrado AES-256 del DNI, PIN con Argon2id y secretos en Azure Key Vault (PRINCIPIO-008).
- **Calidad:** pruebas unitarias obligatorias y cobertura mínima del 80 % (PRINCIPIO-010).
- **Accesibilidad:** WCAG 2.1 AA (PRINCIPIO-011).
- **Idioma:** solo español latinoamericano (PRINCIPIO-001).

### Legales

- Retiradas del alcance del piloto académico el 2026-11-11: el Libro de Reclamaciones y el detalle de protección de datos personales (W13 y W14). Si Luquita pasa de piloto a producto, deben volver a evaluarse.

### De recursos

- Ejercicio académico de un diploma: equipo y tiempo limitados.
- Un solo colegio, una sola tienda y unos 200 alumnos.

---

## 6. Dependencias

| Dependencia | Para qué se necesita | Responsable |
|---|---|---|
| **Microsoft Azure** | Alojar el backend, la base de datos PostgreSQL y Azure Key Vault. | Equipo Luquita |
| **Servicio de envío de correo** | Clave inicial del personal y avisos de cada compra al apoderado (M4). Proveedor: Azure Communication Services Email (ADR-008). | Equipo Luquita |
| **Tags NFC NTAG213/NTAG215** | Medio de pago físico de cada alumno. | Proveedor de tags |
| **Celular con NFC del vendedor** | Cobrar las compras. | Dueño de la tienda |
| **Celular con NFC del apoderado** | Grabar y vincular los tags de sus hijos. | Apoderado |
| **Tags NFC del colegio** | Entregar tags en blanco a las familias registradas (el apoderado también puede comprarlos). | Colegio |
| **Internet en la tienda** | Validar cada compra en línea. | Colegio o dueño de la tienda |
| **Colegio** | Firmar el convenio, registrar las familias registradas, cobrar en la boleta y pagar al dueño de la tienda. | Colegio |
| **Distribución de las apps** | Instalar las apps Flutter en los celulares Android del piloto con un APK firmado por prueba interna de Google Play (iOS fuera del piloto). | Equipo Luquita |

---

## Decisiones tomadas

| Decisión | Resultado |
|---|---|
| Quién vincula los tags | El apoderado, desde la app móvil de Luquita, una vez que la familia está registrada. Los tags los entrega el colegio o los compra el apoderado. |
| Detalle para el apoderado | El apoderado lo solicita desde la app móvil; es la lista de compras (fecha, hora, monto y descripción referencial), sin catálogo de productos. PRINCIPIO-007 ajustado en las versiones 1.3 y 1.4. |
| Fechas del piloto | No son relevantes para el alcance del ejercicio académico. |
| Panel del colegio | La secretaria usa un panel web en Angular 20 para registrar las familias registradas (Excel o manual) y obtener el consolidado. |
| Deuda de la boleta | No bloquea compras: si el alumno asiste al colegio, puede comprar (W12). |
| Retención de datos | ~~5 años desde el cierre del año escolar~~. Retirado el 2026-11-11 (W14). |
| Identificación de la familia | Por el DNI del apoderado. La cuenta de la app se asocia a la familia registrada por el colegio con el mismo DNI. |
| Administración | El administrador de Luquita registra el colegio, la secretaria y el dueño de la tienda, con el correo de cada uno. |
| Tope mensual del apoderado | El apoderado puede configurar después un tope mensual propio, entre S/ 0 y S/ 300, que aplica de inmediato (C3). |
| Inicio de sesión | Apoderado: DNI y PIN. Secretaria: DNI y clave. Dueño de la tienda: correo y clave. Las claves llegan por correo al registrarlos y deben cambiarse obligatoriamente en el primer inicio de sesión. |
| Apoderados por familia | Exactamente uno. |
| Cambio de correo del apoderado | Solo lo hace la secretaria desde el panel web. |
| Registro anual | La secretaria registra las familias cada año, después de la matrícula. |
| Libro de Reclamaciones | Retirado el 2026-11-11 (W13). |
| Sin conexión a internet | No se registran compras ni se guardan para después. |
| Estado de los tags del colegio | Se entregan en blanco. |
| Tope por defecto | Cada alumno nuevo tiene un tope mensual de S/ 300; solo el apoderado puede personalizarlo (C3). |
| Consolidado mensual (2026-11-13) | Sin cierre automático: la secretaria elige un mes y el sistema suma las compras de ese mes calendario en hora de Lima. |
| App del dueño activa (2026-11-13) | Los alumnos solo compran mientras la app del dueño esté activa; la activan o desactivan la secretaria o el administrador. Reemplaza la temporada de marzo a noviembre. |
| Suscripción | Retirada el 2026-11-13 (W15). |
| PIN del alumno (2026-11-13) | 3 dígitos; el tag se sigue bloqueando tras 3 intentos fallidos. |
| Ajustes | Los registra la secretaria del colegio en el panel web. |
| Corrección de compras en el MVP | La anulación y los ajustes quedan fuera del MVP (S1). Riesgo aceptado: en el MVP no se pueden corregir compras erróneas. |
| Formato del Excel | Columnas: DNI del apoderado, nombre, apellidos y correo. Lo importan la secretaria o el administrador, y toda familia importada queda activa (2026-11-13). Si un DNI se repite dentro del archivo, se procesa la primera fila y se rechazan las siguientes (2026-11-11). La secretaria puede pasar una familia a inactiva. |
| Credenciales olvidadas (2026-11-13) | No hay recuperación en la app: un humano del equipo de Luquita regenera la clave del personal o restablece el PIN del apoderado con un procedimiento manual (arquitectura §6.7). Riesgo aceptado para el piloto. |
| Cambio del PIN del alumno (2026-11-13) | No hay operación de cambio en el piloto; el apoderado conoce el PIN que definió. Riesgo R5 aceptado. |
| Varios tags por alumno (2026-11-13) | En el piloto un alumno puede tener más de un tag `ACTIVO`: es la forma de reemplazar un tag perdido (el perdido se bloquea). Riesgo aceptado hasta S3. |
| Edad del alumno (2026-11-13) | El sistema no valida el rango de 8 a 14 años, porque no guarda la fecha de nacimiento; lo controla el colegio. |

## Decisiones pendientes

Ninguna bloquea el alcance. Las preguntas que quedan sobre las reglas están en `05-preguntas-abiertas.md`.
