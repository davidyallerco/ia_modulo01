# 01 — Project Brief: LUQUITA

- **Producto:** Luquita, billetera digital infantil con tag NFC para la tienda del colegio.
- **Fase:** descubrimiento (Agente Analyst, BMAD).
- **País de operación:** Perú.
- **Piloto:** colegio particular en Lurigancho-Chosica, Lima.
- **Naturaleza:** ejercicio académico del Diploma en AI-First Software Architecture.
- **Fecha:** 2026-11-04.
- **Documento base:** `00-constitucion.md` (versión 1.17). **Actualizaciones:** 2026-11-11 (se retiran el Libro de Reclamaciones y el detalle de protección de datos) y 2026-11-13 (se retira la suscripción de S/ 5; las compras solo se permiten con la app del dueño activa; PIN del alumno de 3 dígitos).

---

## 1. Problema y oportunidad

### El problema

En el Perú, los padres de familia pagan cada vez más con billeteras digitales como Yape y Plin, y llevan cada vez menos efectivo. Pero sus hijos de 8 a 14 años siguen necesitando efectivo para comprar en la tienda del colegio. Esto deja un vacío: el apoderado que ya no maneja efectivo tiene que conseguir sencillo cada mañana para darle al niño, y no es recomendable que un niño de esa edad lleve un celular para pagar.

### Quién lo sufre y cómo

| Actor | Situación actual | Qué pierde |
|---|---|---|
| **Padres** | Dan efectivo al hijo cada día; el gasto promedio es de unos S/ 10 por alumno al día. | Tiempo y esfuerzo para conseguir efectivo cuando ya no lo manejan. Dinero perdido: el hijo del usuario ha perdido monedas en un par de ocasiones. |
| **Estudiantes (8 a 14 años)** | Pagan en efectivo en la tienda. | Las monedas que se pierden y, con ellas, la posibilidad de comprar ese día. |
| **Dueño de la tienda** | Cobra en efectivo. Hoy no hay colas en el recreo. | Hoy no se identificó una pérdida significativa para él. |

### La oportunidad

- **Colegio piloto:** un colegio particular de unos 1 500 alumnos en Lurigancho-Chosica, que promueve la alimentación saludable y está dispuesto a comunicar Luquita en sus agendas.
- **Mercado inicial:** unos 200 alumnos usuarios esperados, que gastan en promedio unos S/ 10 diarios.
- **Tendencia de fondo:** la digitalización de los pagos en las familias peruanas.

> **Nota del analista:** el dolor principal es la falta de efectivo de los padres. La pérdida de monedas es un dolor secundario y poco frecuente. La idea se validó de forma cualitativa con unos cinco padres, que la calificaron como muy buena.

---

## 2. Usuarios y segmentos objetivo

| Segmento | Quiénes son | Qué necesitan |
|---|---|---|
| **Padres de familia (pagadores)** | Padres del colegio piloto que usan pagos digitales. Una familia puede tener uno o más hijos en el colegio. | Que su hijo pueda comprar sin que ellos tengan que conseguir efectivo, y tener control del gasto: saber de cada compra, poner límites y bloquear el medio de pago si se pierde. |
| **Estudiantes de 8 a 14 años (consumidores)** | Alumnos de primaria alta y secundaria baja, sin celular en el colegio. | Comprar en la tienda de forma rápida y sin preocuparse por traer dinero. |
| **Dueño o vendedor de la tienda** | Concesionario de la tienda del colegio. Tiene un celular con NFC y ya conversó con el usuario sobre la idea. | Cobrar sin manejar efectivo, sin registrar pagos ni recargas a mano, y recibir su pago completo cada mes. |
| **Colegio** | Institución particular que firma el convenio, cobra el consumo en su boleta y promueve la alimentación saludable. | Ofrecer un servicio moderno a las familias sin asumir carga operativa excesiva. |

**Fuera del segmento inicial:** alumnos de inicial y de los primeros años de primaria (menores de 8 años), alumnos mayores de 14 años y otros colegios.

---

## 3. Propuesta de valor

**Para los padres:** su hijo compra en la tienda del colegio sin efectivo ni celular, con límites de gasto, aviso de cada compra y control inmediato si pierde el tag. Todo se paga una sola vez al mes, junto con la boleta del colegio.

**Para los estudiantes:** pueden comprar todos los días sin depender de que sus padres tengan sencillo, y sin el riesgo de perder monedas.

**Para el dueño de la tienda:** cobra sin efectivo ni vuelto, no registra recargas y recibe el pago completo del consumo cada mes, aunque alguna familia no pague su boleta.

**Para el colegio:** un servicio diferenciador para las familias, sin costo de suscripción en el piloto.

**¿Por qué Luquita y no el efectivo?** Porque hoy los padres viven en un mundo de pagos digitales al que sus hijos no pueden entrar sin un celular. Luquita lleva ese paso digital a los niños con un medio físico (tag) y una clave que el apoderado crea para su hijo.

---

## 4. Modelo de negocio preliminar

| Elemento | Definición |
|---|---|
| **Fuente de ingresos** | **Ninguna en el piloto.** La suscripción de S/ 5 por familia se retiró el 2026-11-13: es un proyecto de prueba. Un modelo de ingresos se definiría si Luquita pasa de piloto a producto. |
| **Forma de cobro** | El consumo del mes se incluye en la boleta del colegio. Luquita no cobra directamente. |
| **Pago a la tienda** | El dueño recibe del colegio, cada mes, el total del consumo de los alumnos. Recibe siempre el monto completo. |
| **Morosidad** | Si una familia no paga la boleta, la pérdida la asume **el colegio**. Los hijos de esa familia siguen comprando mientras asistan al colegio; la deuda la gestiona el colegio. |

> **Nota del analista:** la morosidad se considera un riesgo bajo, porque el apoderado debe estar al día con el colegio para que su hijo pase al siguiente año o reciba sus documentos para la universidad. *(La proyección de ingresos por suscripción se retiró el 2026-11-13.)*

---

## 5. Métricas de éxito (KPIs medibles)

| # | KPI | Cómo se mide | Meta | Estado |
|---|---|---|---|---|
| 1 | **Renovación del convenio** | Convenio firmado por el colegio para el año escolar 2028 (sí o no). | Firmado. | Meta confirmada. Fecha de decisión por definir. |
| 2 | **Alumnos activos** | Alumnos con al menos una compra registrada en el mes. | 200 alumnos. | Meta confirmada. Plazo para alcanzarla por definir. |
| 3 | **Familias registradas** | Familias que el colegio registró en Luquita y que tienen al menos un tag vinculado. | Por definir. | Por definir. |
| 4 | **Tasa de morosidad** | Familias con boleta impaga ÷ familias registradas, por mes (dato que reporta el colegio). | Por definir. | Por definir. |
| 5 | **Compras disputadas** | Compras no reconocidas que los apoderados reportan al colegio o a la tienda ÷ total de compras del mes (dato fuera del sistema: el Libro de Reclamaciones se retiró el 2026-11-11). | Por definir. | Por definir. |
| 6 | **Uso frente al efectivo** | Ventas con Luquita ÷ ventas totales de la tienda, por mes (dato que reporta el dueño). | Por definir. | Por definir. |

Los KPIs 1 y 2 son los que el usuario definió. Del 3 al 6 son métricas propuestas para dar seguimiento a los riesgos principales; sus metas deben fijarse antes del piloto.

---

## 6. Riesgos identificados y supuestos

### Riesgos

| # | Riesgo | Impacto | Prioridad |
|---|---|---|---|
| R1 | **Morosidad de los padres en la boleta.** La pérdida del consumo impago la asume el colegio. Mitigación: el apoderado debe estar al día con el colegio para que su hijo pase de año o reciba sus documentos. | Medio: el pago suele recuperarse, pero puede demorar meses y afectar el flujo del colegio. | Baja. |
| R2 | **Disputas del tipo "mi hijo no compró eso".** | Medio: genera reclamos, desconfianza y carga para el colegio o la tienda. La mitigación definida es exigir el tag más un PIN creado por el apoderado. | Alta (indicada por el usuario). |
| R3 | **Baja adopción:** que menos de 200 familias se sumen al piloto. | Alto: sin uso suficiente, el piloto no demuestra el valor del servicio. | Media. |
| R4 | **Dolor no compartido por todas las familias:** hoy no hay colas y las pérdidas de monedas son poco frecuentes. Las familias que todavía usan efectivo con comodidad pueden no ver motivo para cambiar. | Medio. | Baja: cinco padres validaron la idea. |
| R5 | **Pérdida del tag u olvido del PIN** por alumnos de 8 a 14 años. | Medio: interrumpe la compra en el recreo. | Media. |
| R6 | **Dependencia de un solo colegio y de un solo concesionario.** Si el colegio no renueva o cambia de concesionario, el piloto termina. | Alto. | Media. |
| R7 | **Cumplimiento de la Ley 29733** por tratar DNI y nombres de menores. | Medio. El detalle (consentimiento, ANPD y plazos) se retiró del alcance del piloto el 2026-11-11; riesgo aceptado mientras sea un ejercicio académico con datos ficticios. | Media. |

### Supuestos (no validados)

| # | Supuesto | Cómo se podría validar |
|---|---|---|
| S1 | Unas 200 familias quieren usar Luquita. **(Supuesto explícito del usuario.)** Cinco padres valoraron la idea. *(El supuesto del precio de S/ 5 se retiró junto con la suscripción, 2026-11-13.)* | Encuesta o reunión de padres antes del piloto. |
| S2 | Los padres del colegio prefieren pagar de forma digital y tienen dificultad para conseguir efectivo. **(Validado cualitativamente con cinco padres.)** | Encuesta a más padres del rango de 8 a 14 años. |
| S8 | El colegio puede exigir el pago del consumo de la tienda, igual que las pensiones, como condición para la matrícula del año siguiente o la entrega de documentos. | Confirmarlo con la administración del colegio e incluirlo en el convenio, ya que el consumo de la tienda no es una pensión educativa. |
| S3 | El colegio acepta incluir el consumo en su boleta, y pagar mensualmente al dueño de la tienda. | Firma del convenio. |
| S4 | La secretaria del colegio puede registrar a inicio de año las familias registradas, importando un Excel o manualmente. | Prueba con la lista real de familias del colegio. |
| S5 | Los alumnos de 8 a 14 años cuidan el tag y recuerdan su PIN. | Prueba con un grupo pequeño de alumnos. |
| S6 | Un solo celular con NFC basta para atender el recreo sin generar colas. | Prueba en un recreo real. |
| S7 | El dueño de la tienda acepta recibir el pago una vez al mes, en lugar de cobrar en efectivo al momento. | Conversación con el dueño. |

---

## 7. Restricciones conocidas

### De la constitución (`00-constitucion.md`, versión 1.17)

- **Modelo:** Luquita solo registra el consumo pospago y lo consolida por mes para el colegio. No procesa pagos ni cobra, no maneja recargas ni saldos, no se integra con Yape, Plin ni pasarelas, y no emite comprobantes SUNAT (PRINCIPIO-003).
- **Sin suscripción:** Luquita no cobra suscripción ni comisiones en el piloto (PRINCIPIO-003).
- **App del dueño activa:** los alumnos solo compran mientras la app del dueño de la tienda esté activa; la activan o desactivan la secretaria o el administrador (PRINCIPIO-003).
- **Límites de consumo:** S/ 30 por día y S/ 300 por mes por alumno (PRINCIPIO-005). La deuda de la boleta no bloquea compras (PRINCIPIO-003).
- **Seguridad de la compra:** cada compra requiere el tag NFC más el PIN del alumno (PRINCIPIO-006).
- **Datos de menores:** solo DNI y nombre. Sin analítica ni perfilamiento. El colegio recibe solo los totales y el detalle de compras se entrega solo al apoderado, desde la app móvil (PRINCIPIO-007).
- **Stack:** Java 21 + Spring Boot 3, PostgreSQL, Flutter, Angular 20 para el panel web del colegio y Azure (PRINCIPIO-002).
- **Idioma:** solo español latinoamericano (PRINCIPIO-001).
- **Accesibilidad:** WCAG 2.1 AA (PRINCIPIO-011).

### Legales

- El Libro de Reclamaciones y el detalle de protección de datos personales (Ley 29733) se retiraron del alcance del piloto académico el 2026-11-11.
- El colegio promueve la alimentación saludable (Ley 30021); el catálogo de la tienda lo define el colegio y el concesionario, no Luquita.

### De recursos y alcance

- Ejercicio académico: un solo colegio piloto, sin plan de expansión.
- Un solo punto de venta: la tienda del colegio, atendida con un celular con NFC.
- Público limitado a alumnos de 8 a 14 años.

---

## Pendientes para el Product Manager

- Fecha en que el colegio decide la renovación del convenio para 2028.
- Plazo para alcanzar los 200 alumnos activos.
- Metas de los KPIs 3 a 6.
- Validación del PIN del alumno propuesto en la constitución (3 dígitos desde la versión 1.17, bloqueo tras 3 intentos fallidos).
