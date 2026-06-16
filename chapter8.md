# Capítulo VIII: Experiment-Driven Development

## 8.1. Experiment Planning

### 8.1.1. As-Is Summary

La aplicación actual de **PlantCare** se centra en ofrecer una plataforma integral para el monitoreo y cuidado de plantas mediante dispositivos de hardware IoT. Proporciona funcionalidades básicas como la gestión de perfiles de usuario, el registro y seguimiento de la salud de las plantas en tiempo real (midiendo humedad del suelo, temperatura ambiental, nivel de luz y nivel de batería del sensor) y un sistema básico de cálculo para la programación del próximo riego. 

Sin embargo, el rendimiento general es inconsistente al momento de cargar gráficos con métricas históricas de sensores, con tiempos de respuesta que a menudo superan los 3 segundos debido a la latencia en las consultas a la base de datos de **Supabase** y a la falta de almacenamiento en caché local en el frontend web. Además, la interfaz presenta limitaciones severas en entornos de baja luminosidad exterior debido a la ausencia de un modo oscuro, y carece de traducción en múltiples idiomas, lo que restringe su expansión a mercados internacionales.

**Problemas identificados:**
- **Rendimiento:** La aplicación experimenta lentitud y demoras en el acceso y renderizado de los históricos de métricas de sensores, lo que puede llevar a la frustración del usuario durante el monitoreo diario.
- **Usabilidad y Accesibilidad:** La falta de un modo oscuro limita la comodidad de visualización para los usuarios que interactúan con la aplicación en invernaderos o exteriores con condiciones de poca luz. Asimismo, la ausencia de atributos de accesibilidad estructurados limita el uso de tecnologías de asistencia por parte de adultos mayores o usuarios con discapacidades visuales.
- **Experiencia del usuario (UX):** Los componentes visuales y los gráficos de métricas no son completamente adaptables, lo que resulta en una experiencia inconsistente y desorganizada al alternar entre dispositivos móviles y computadoras de escritorio.
- **Funcionalidad limitada e Internacionalización:** La plataforma carece de traducciones y soporte para otros idiomas, limitando su accesibilidad y potencial de expansión global. Además, no cuenta con un sistema de alertas inmediatas fuera de la aplicación web (como notificaciones automáticas o alertas en canales externos), lo que impide un monitoreo proactivo fuera del panel de control.

**Objetivos de mejora:**
Para abordar los problemas identificados y optimizar la plataforma, se establecen los siguientes objetivos:
- **Optimización del rendimiento:** Reducir el tiempo de carga y obtención de métricas históricas a menos de 1.5 segundos mediante la optimización de consultas e índices en la base de datos de **Supabase (PostgreSQL)** y la implementación de almacenamiento en caché en el cliente web desarrollado en Vue.js.
- **Mejora de la accesibilidad y la experiencia del usuario:** Implementar un modo de alto contraste (modo oscuro), adaptar dinámicamente los gráficos de métricas al tamaño de la pantalla y configurar atributos ARIA para cumplir con los estándares de diseño inclusivo y accesibilidad.
- **Internacionalización y localización:** Integrar el soporte multiidioma mediante la biblioteca i18n para ofrecer la aplicación en inglés y español, atrayendo a una base de usuarios más amplia y diversa a nivel internacional.
- **Implementación de gamificación:** Introducir mecánicas de gamificación (tales como medallas de logros por el cuidado de las plantas, rachas de salud óptima y desafíos interactivos) para fomentar la participación constante y el compromiso de los usuarios con la plataforma.

### 8.1.2. Raw Material: Assumptions, Knowledge Gaps, Ideas, Claims

A continuación, se agrupan los elementos de base (*raw material*) recopilados para sustentar los experimentos de ingeniería de software orientados a la optimización y expansión de la plataforma **PlantCare**.

**Assumptions (Supuestos):**
- **Modo oscuro:** Se asume que los usuarios valoran la personalización de la interfaz gráfica y la comodidad visual, de modo que la implementación de un modo oscuro facilitará el monitoreo y cuidado de plantas en condiciones de baja luminosidad (de noche o en exteriores con luz cambiante), reduciendo la fatiga visual.
- **Traducciones (Localización):** Se asume que existe un interés en mercados globales de aficionados a la jardinería urbana, por lo que ofrecer la aplicación en inglés y español incrementará la base de usuarios activos.
- **Feed y foro de comunidad:** Se asume que los usuarios valoran disponer de un espacio social e interactivo dentro de la aplicación para compartir fotografías, registrar progresos de cultivo y recibir consejos colaborativos, lo que aumentará su compromiso y tasa de retención.
- **Alertas preventivas y contextuales:** Se asume que los usuarios reaccionarán con mayor rapidez y efectividad ante alertas detalladas que explican causas probables (como exceso de calor o drenaje rápido) en lugar de alertas de métricas genéricas de sensores, disminuyendo la pérdida de plantas por descuidos.
- **Modelo de suscripción premium:** Se asume que un segmento de usuarios avanzados (como jardineros coleccionistas o gestores de pequeños huertos urbanos) está dispuesto a pagar una tarifa mensual a cambio de diagnósticos avanzados detallados e historiales de métricas con retención extendida hasta 24 meses (en comparación con los 3 meses de la versión gratuita).

**Knowledge Gaps (Brechas de conocimiento):**
- **Preferencia del modo oscuro:** Falta información empírica cuantitativa sobre qué porcentaje de la base de usuarios actual prefiere una interfaz en modo oscuro de forma permanente frente a la conmutación basada en sensores de luz ambiental.
- **Demografía e idiomas:** Es necesario investigar en profundidad la distribución geográfica y demográfica de nuestra base potencial de usuarios para evaluar si es preciso priorizar otros idiomas en futuras iteraciones de localización (como portugués o alemán).
- **Interacción y utilidad de la comunidad:** Se carece de datos históricos sobre qué tópicos de discusión son más atrayentes y si la visualización de casos de éxito en el feed comunitario influye positivamente en el cuidado efectivo de las plantas de otros usuarios.
- **Impacto de las alertas contextuales:** No hay suficiente evidencia sobre si las recomendaciones de riego inteligentes disminuyen el tiempo de respuesta promedio de los usuarios en comparación con una notificación de alarma tradicional de nivel crítico.
- **Elasticidad de precios de la suscripción:** Se carece de análisis financiero sobre el precio ideal que los usuarios de diferentes segmentos estarían dispuestos a pagar por el plan premium, y qué características específicas del plan impulsan de manera determinante la decisión de compra.

**Ideas:**
- **Gamificación del cuidado diario:** Implementar un sistema de logros desbloqueables (medallas por días sin alertas críticas, rachas de plantas en estado "healthy") para incrementar el engagement de la aplicación web.
- **Alertas y Webhooks en Discord:** Integrar un canal de notificaciones automáticas mediante Webhooks de Discord para avisar de forma inmediata a los usuarios sobre riegos urgentes o pérdida de conectividad de sensores sin necesidad de que tengan la aplicación web abierta.
- **Caché en el cliente web:** Desarrollar un sistema de almacenamiento temporal en caché de las últimas métricas recibidas para que el renderizado de gráficos históricos del dashboard sea instantáneo (< 1.5 segundos).

**Claims (Afirmaciones de usuarios y stakeholders):**
- **Usuario Vasco:** "No me gusta estar amarrado a una suscripción mensual fija solo por ver las estadísticas básicas; prefiero realizar una sola compra de hardware."
- **Usuario María:** "He perdido varias plantas en el pasado por exceso de agua o fertilizante. Necesito que la aplicación me guíe de forma proactiva con alertas explicativas claras y no solo con gráficos difíciles de interpretar."
- **Usuario Natalia:** "Revisar los gráficos de la aplicación web en mi celular en pleno día cuando estoy en el jardín es complicado debido al reflejo del sol en la pantalla, por lo que necesito mejor contraste."

### 8.1.3. Experiment-Ready Questions

En esta sección se definen las preguntas listas para el experimento (*Experiment-Ready Questions*), clasificadas en dos tipos principales según el enfoque de investigación de **PlantCare**:

1. **Preguntas Impulsadas por Creencias (Belief-led):** Formuladas para validar o refutar hipótesis y supuestos específicos que el equipo considera fundamentales para el valor del producto.
2. **Preguntas Exploratorias (Exploratory):** Diseñadas para recopilar nuevos conocimientos en áreas con alta incertidumbre, donde no existen suposiciones previas validadas.

Para estructurar estas preguntas y descubrir supuestos subyacentes u ocultos, se aplica la técnica de las **5 Ws y 1 H** (*Who, What, Where, When, Why, How*).

---

#### 1. Preguntas Impulsadas por Creencias (Belief-led)

##### Pregunta 1: Monitoreo en exteriores mediante Modo Oscuro
**¿La implementación de una interfaz de modo oscuro (alto contraste) en la aplicación web incrementará la duración de la sesión promedio de los usuarios en entornos exteriores o nocturnos en al menos un 15%?**

- **Who (Quién):** Aficionados a las plantas y cuidadores (con perfiles similares a Natalia y María) que interactúan con la interfaz bajo condiciones de iluminación exterior.
- **What (Qué):** Una visualización alternativa en modo oscuro que optimice la legibilidad y reduzca el reflejo de la luz solar en las pantallas de los teléfonos móviles.
- **Where (Dónde):** En el panel principal y las secciones de gráficos históricos de la aplicación web Vue.js.
- **When (Cuándo):** Durante el monitoreo y registro de plantas en jardines o terrazas durante el día con alta luminosidad, o en interiores con poca luz durante la noche.
- **Why (Por qué):** Para mitigar la fatiga visual provocada por el contraste excesivo del sol y evitar que la incomodidad visual cause que los usuarios abandonen la visualización de datos de manera temprana.
- **How (Cómo):** Proveyendo un botón de alternancia en la configuración de la interfaz para activar de forma manual o automática el modo oscuro mediante hojas de estilo CSS dinámicas.

##### Pregunta 2: Notificaciones de Riego Crítico vía Discord Webhooks
**¿El envío automático de alertas de riego crítico a un canal dedicado de Discord mediante Webhooks reducirá el tiempo de respuesta promedio de los usuarios ante estados críticos de humedad del suelo de 12 horas a menos de 2 horas?**

- **Who (Quién):** Usuarios urbanos con rutinas ocupadas (con perfiles similares a Vasco y Ana) que a menudo olvidan abrir la aplicación web.
- **What (Qué):** Envío automático de notificaciones estructuradas con el diagnóstico contextual del sensor afectado (ej. humedad del suelo menor al 20%).
- **Where (Dónde):** Canal exclusivo `#alerts-critical` de la plataforma Discord.
- **When (Cuándo):** Inmediatamente después de que el backend de **Supabase** registre una lectura crítica de humedad del suelo.
- **Why (Por qué):** Para eliminar la necesidad de que el usuario tenga que ingresar manualmente al panel web para enterarse del estado de la planta, asegurando una intervención inmediata.
- **How (Cómo):** Conectando el pipeline de base de datos de Supabase con un servicio que envíe peticiones POST a la URL del webhook de Discord cada vez que se detecte una anomalía de riego.

---

#### 2. Preguntas Exploratorias (Exploratory)

##### Pregunta 3: Demografía lingüística de los visitantes del Landing Page
**¿Cuál es la proporción de visitas internacionales hispanohablantes frente a angloparlantes en nuestro Landing Page y qué porcentaje prefiere visualizar la información del producto en inglés?**

- **Who (Quién):** Visitantes y posibles clientes en general que navegan por el Landing Page.
- **What (Qué):** Idioma preferido del navegador e IP de origen del tráfico.
- **Where (Dónde):** En el Landing Page público de PlantCare hospedado en GitHub Pages.
- **When (Cuándo):** Durante las primeras cuatro semanas de lanzamiento de la campaña de validación inicial.
- **Why (Por qué):** Para determinar de forma cuantitativa si el desarrollo de traducciones al inglés (implementado mediante la biblioteca i18n) en la aplicación web está plenamente justificado por el volumen de tráfico de esa audiencia.
- **How (Cómo):** Evaluando las estadísticas del idioma por defecto del navegador web de las visitas mediante una herramienta analítica integrada en el Landing Page.

##### Pregunta 4: Valoración de características del plan Premium
**¿Qué características premium (el diagnóstico detallado de salud basado en datos históricos o la retención de métricas por 24 meses) despiertan mayor interés de compra entre los usuarios de la versión básica?**

- **Who (Quién):** Usuarios activos registrados con más de 3 plantas y 30 días de uso en la versión gratuita de la plataforma.
- **What (Qué):** Clics y solicitudes de información en las opciones de suscripción de la aplicación.
- **Where (Dónde):** En las secciones de analíticas y perfiles de la aplicación web Vue.js.
- **When (Cuándo):** Cada vez que el usuario alcance el límite de retención de datos básicos (3 meses) o consulte sugerencias avanzadas de salud.
- **Why (Por qué):** Para recolectar datos sobre la disposición de pago y definir qué valor agregado justifica la suscripción para el modelo de negocio SaaS.
- **How (Cómo):** Registrando de forma anónima la interacción del usuario con los botones de información del plan premium y el acceso a la sección de analíticas avanzadas.

### 8.1.4. Question Backlog

En esta sección se organiza y prioriza el conjunto de preguntas de investigación en un **Question Backlog**. A diferencia de los backlogs tradicionales de desarrollo de software (que se centran en soluciones o características técnicas), este backlog se enfoca exclusivamente en capturar la incertidumbre del negocio mediante preguntas que necesitan experimentación para ser respondidas.

El Question Backlog se divide estructuralmente en dos niveles:
1. **Backlog Amplio (Broad Backlog):** La lista completa de preguntas identificadas a partir de la recopilación de supuestos, brechas de conocimiento y afirmaciones del proyecto.
2. **Backlog Profundo (Deep Backlog):** El conjunto priorizado de preguntas que han sido refinadas, puntuadas y seleccionadas para su experimentación inmediata en los próximos ciclos.

#### Sistema de Puntuación
Para priorizar las preguntas de investigación, el equipo utiliza una escala del 1 (muy bajo) al 5 (muy alto) en cuatro criterios fundamentales:
- **Confianza (C):** Qué tan seguros estamos de poder medir y responder la pregunta con las herramientas disponibles (Sentry, Supabase Logs, analíticas web).
- **Riesgo (R):** Qué tan crítico es para la supervivencia del producto o modelo de negocio si el supuesto detrás de la pregunta resulta ser falso.
- **Impacto (I):** El beneficio potencial sobre la retención, usabilidad o rentabilidad que ofrece resolver esta incertidumbre.
- **Interés (Int):** El nivel de urgencia o motivación del equipo y los usuarios en obtener la respuesta.

El puntaje total es la suma de los cuatro criterios:
$$\text{Puntaje Total} = \text{Confianza} + \text{Riesgo} + \text{Impacto} + \text{Interés}$$

**Regla de desempate:** En caso de que dos o más preguntas obtengan el mismo puntaje total, se priorizará en una posición superior aquella pregunta que posea el mayor valor en el criterio de **Riesgo (R)**, ya que mitigar las incertidumbres más peligrosas es la prioridad del marco de trabajo experimental.

---

#### Tabla de Priorización (Broad Backlog)

El siguiente cuadro detalla la evaluación de las cuatro preguntas clave identificadas en el proyecto:

| ID | Pregunta de Investigación | Motivación ("Por qué") | C | R | I | Int | Total | Estado |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Q2** | ¿Las alertas críticas de riego vía webhooks de Discord reducirán el tiempo de respuesta del usuario a menos de 2 horas? | Evitar la pérdida física de plantas por olvidos en el riego diario en usuarios ocupados. | 3 | **5** | 4 | 4 | **16** | Priorizada (Backlog Profundo) |
| **Q4** | ¿Qué características premium (diagnósticos avanzados o historiales extendidos) despiertan mayor interés de compra? | Validar la viabilidad económica del modelo de suscripción SaaS para sustentar el proyecto. | 4 | **4** | 4 | 4 | **16** | Priorizada (Backlog Profundo) |
| **Q1** | ¿La implementación del modo oscuro aumentará el tiempo de sesión promedio en exteriores o de noche en un 15%? | Mejorar la comodidad visual en entornos con exceso de sol o de noche. | 4 | 2 | 3 | 4 | **13** | Pendiente (Backlog Amplio) |
| **Q3** | ¿Cuál es la proporción de visitas internacionales angloparlantes en nuestro Landing Page? | Justificar la necesidad y costos de soporte de internacionalización (i18n). | 5 | 1 | 2 | 3 | **11** | Pendiente (Backlog Amplio) |

*Nota sobre la aplicación de la regla de desempate:* Como se observa en la tabla, **Q2** y **Q4** empataron con un puntaje total de **16**. Aplicando la regla de desempate por riesgo, **Q2** se posiciona en el primer lugar del backlog profundo debido a que su nivel de riesgo es **5** (la pérdida de plantas debido a fallas de riego destruye el valor principal del producto), mientras que el riesgo de **Q4** es de **4** (la viabilidad de monetización).

---

#### Estructura del Backlog Profundo (Deep Backlog)

Basándonos en la priorización obtenida, el backlog profundo queda constituido por las siguientes preguntas de investigación para su experimentación inmediata:

1. **Prioridad 1 (Q2):** Impacto de las alertas de riego crítico vía webhooks de Discord en el tiempo de respuesta del usuario.
2. **Prioridad 2 (Q4):** Interés de compra y valoración de características avanzadas para el plan de suscripción Premium.

### 8.1.5. Experiment Cards

Las Tarjetas de Experimento (*Experiment Cards*) sirven como la plantilla técnica estructurada que captura la información clave para cada experimento antes de su ejecución. Se han diseñado dos tarjetas correspondientes a las preguntas prioritarias del Backlog Profundo: **EC-01** (para las alertas en Discord) y **EC-02** (para las características de la suscripción Premium).

---

#### Tarjeta de Experimento 1: EC-01 (Alertas de Riego Crítico vía Discord)

##### Lado Frontal (Front Side)
- **Pregunta (Question):** ¿El envío automático de alertas de riego crítico a un canal dedicado de Discord mediante Webhooks reducirá el tiempo de respuesta promedio de los usuarios ante estados críticos de humedad del suelo de 12 horas a menos de 2 horas?
- **Por qué (Why):** Evitar la pérdida de plantas delicadas por descuidos en el riego. Reducir el tiempo de respuesta disminuye drásticamente el estrés hídrico y la tasa de marchitez en cultivos domésticos.
- **Hipótesis (Hypothesis):** Creemos que si enviamos notificaciones proactivas con diagnóstico en tiempo real al canal de Discord del usuario, entonces el tiempo promedio para registrar el riego se reducirá a menos de 2 horas, porque el usuario recibe la alerta de inmediato en su dispositivo sin necesidad de abrir la aplicación de forma manual.
- **Qué (What - Simplest Useful Thing):** Implementar un webhook en el backend de **Supabase** que envíe de forma automática un mensaje con formato enriquecido a un canal de Discord de prueba cada vez que la métrica de un sensor registre una humedad de suelo igual o menor al 20%.

##### Lado Posterior (Back Side)
- **Medidas (Measures):**
  - **Métrica Primaria:** Tiempo transcurrido (medido en minutos) entre el registro en base de datos de la lectura crítica del sensor ($soil\_moisture\_pct \le 20$) y la inserción del evento correspondiente de riego en la tabla `watering_logs` de Supabase.
  - **Métrica Secundaria:** Tasa de clics en el enlace de diagnóstico de la alerta en Discord y retroalimentación de usuarios sobre la comodidad de la alerta.
- **Condiciones (Conditions):**
  - **Condición de Control:** Un grupo piloto de 15 usuarios que únicamente recibe alertas visuales tradicionales dentro de la aplicación web (deben ingresar manualmente a la aplicación para notar la advertencia).
  - **Condición Experimental:** Un grupo piloto de 15 usuarios que tiene configurada la integración de notificaciones de Discord a través del Webhook.
- **Escala (Scale):**
  - **Nivel de significancia ($\alpha$):** 5% (para prevenir errores de detección de falso impacto positivo).
  - **Potencia estadística ($1-\beta$):** 80% (para reducir el riesgo de no captar una mejora real).
  - **Efecto Mínimo Detectable (MDE):** Una reducción de 10 horas en el tiempo de respuesta promedio de riego.
  - **Tamaño de muestra:** Registro de al menos 30 alertas críticas por grupo durante un periodo de observación de 14 días.

---

#### Tarjeta de Experimento 2: EC-02 (Valoración de Características Premium)

##### Lado Frontal (Front Side)
- **Pregunta (Question):** ¿Qué características premium (el diagnóstico detallado de salud basado en datos históricos o la retención de métricas por 24 meses) despiertan mayor interés de compra entre los usuarios de la versión básica?
- **Por qué (Why):** Validar qué características justifican la suscripción mensual del modelo SaaS para rentabilizar la startup y priorizar recursos de desarrollo técnico.
- **Hipótesis (Hypothesis):** Creemos que el diagnóstico detallado de salud y las sugerencias avanzadas personalizadas generarán el doble de clics de interés que la retención de datos históricos por 24 meses, porque los usuarios valoran más la información orientada a la acción y salud inmediata que el almacenamiento masivo.
- **Qué (What - Simplest Useful Thing):** Modificar la interfaz web para mostrar dos botones con la etiqueta de "Ver Planes Premium" asociados respectivamente a cada funcionalidad (uno en el panel de historial y otro en el panel de diagnóstico de salud), que abran una modal informativa con los precios de suscripción.

##### Lado Posterior (Back Side)
- **Medidas (Measures):**
  - **Métrica Primaria:** Tasa de clics (Click-Through Rate - CTR) calculada como el número de clics únicos en el respectivo botón "Ver Planes Premium" dividido entre el número total de visitantes únicos que accedieron a esa sección del panel de control.
  - **Métrica Secundaria:** Tasa de conversión de la modal de planes (usuarios que hacen clic en el botón de suscripción simulada).
- **Condiciones (Conditions):**
  - **Condición Experimental:** Exposición de ambos botones de forma simultánea a toda la base de usuarios activos del plan gratuito de PlantCare durante el periodo de prueba.
- **Escala (Scale):**
- **Nivel de significancia ($\alpha$):** 5%.
  - **Potencia estadística ($1-\beta$):** 80%.
  - **Efecto Mínimo Detectable (MDE):** Una diferencia absoluta de 5% en la tasa de clics entre ambos botones.
  - **Tamaño de muestra:** Exposición mínima de 150 usuarios únicos activos del plan básico por sección durante 15 días.

## 8.2. Experiment Design

La fase de diseño del experimento constituye un pilar fundamental para garantizar que la investigación de software proporcione resultados consistentes, confiables y con validez estadística. En esta sección se establece un marco estructurado para definir formalmente las expectativas de cambio, alinear las variables experimentales con los objetivos de negocio y delimitar con precisión qué datos se recopilarán del sistema. Esto permite mitigar la incertidumbre técnica y de negocio de **PlantCare**, asegurando que las decisiones de evolución del producto estén sustentadas en evidencia empírica rigurosa y no en suposiciones subjetivas.

### 8.2.1. Hypotheses

En el marco de trabajo de desarrollo guiado por experimentos, las hipótesis no se formulan para ser validadas a priori o declaradas verdaderas sin cuestionamiento; por el contrario, se estructuran con el propósito de ser probadas (*tested*) y sometidas a un proceso de falsabilidad. Para cada uno de los experimentos prioritarios del backlog profundo (**EC-01** y **EC-02**), se define una hipótesis de trabajo ($H_1$) estructurada, medible y falsificable, junto con su correspondiente hipótesis nula ($H_0$), la cual asume la ausencia de efecto o que cualquier variación observada se debe meramente al azar.

---

#### Experimento EC-01: Alertas de Riego Crítico vía Discord

* **Hipótesis de Trabajo ($H_1$):**
  Creemos que si implementamos y habilitamos el envío automático de notificaciones de alerta de riego crítico con diagnósticos contextuales al canal de Discord de los usuarios, entonces el tiempo promedio de respuesta ante el estado de sequedad crítica de la planta se reducirá a menos de 2 horas (120 minutos), en comparación con el tiempo promedio de 12 horas (720 minutos) observado en los usuarios del grupo de control que solo reciben alertas internas tradicionales en el dashboard web.
  * **Criterio de Falsabilidad:** Esta hipótesis se considerará refutada si al finalizar el período de evaluación la diferencia en el tiempo promedio de respuesta entre el grupo experimental y el grupo de control no es estadísticamente significativa para un nivel de significación del 5% ($\alpha = 0.05$), o si el tiempo promedio de respuesta en el grupo experimental es igual o superior a 120 minutos.
  * **Criterio de Testabilidad y Medibilidad:** Se evaluará de forma empírica dividiendo aleatoriamente a la base de usuarios en dos grupos uniformes (control y experimental) durante 14 días, midiendo el intervalo en minutos mediante las marcas de tiempo registradas en las tablas de la base de datos de **Supabase (PostgreSQL)**.

* **Hipótesis Nula ($H_0$):**
  La integración y envío de alertas automatizadas de riego crítico mediante webhooks de Discord no producirá ninguna diferencia estadísticamente significativa en el tiempo promedio de respuesta ante estados críticos de humedad del suelo en los usuarios de **PlantCare**, de modo que cualquier variación observada entre el grupo experimental y el grupo de control se deberá únicamente al azar.

---

#### Experimento EC-02: Valoración de Características Premium

* **Hipótesis de Trabajo ($H_1$):**
  Creemos que el botón informativo de planes premium asociado a la característica de "diagnósticos avanzados de salud basados en datos históricos" generará una tasa de clics (CTR) que será al menos el doble (2x) de la tasa de clics obtenida por el botón informativo asociado a la característica de "retención de datos históricos por 24 meses", cuando ambos se presenten simultáneamente a los usuarios de la versión gratuita de **PlantCare**.
  * **Criterio de Falsabilidad:** Esta hipótesis se considerará refutada si la tasa de clics del botón de diagnóstico avanzado de salud no supera de forma estadísticamente significativa al doble de la tasa de clics del botón de retención de datos, o si los resultados muestran una preferencia inversa con un nivel de significación del 5% ($\alpha = 0.05$).
  * **Criterio de Testabilidad y Medibilidad:** Se probará modificando la aplicación web frontend en Vue.js para incorporar ambos botones de llamada a la acción en sus secciones correspondientes, capturando los identificadores únicos de usuario y los clics efectuados para realizar la comparación estadística directa de las tasas de clics.

* **Hipótesis Nula ($H_0$):**
  No existirá una diferencia estadísticamente significativa en la tasa de clics (CTR) de interés entre la característica de diagnósticos avanzados de salud y la retención extendida de datos históricos por 24 meses en la aplicación **PlantCare**, de forma que la preferencia de los usuarios se distribuirá de manera uniforme o cualquier variación registrada será consecuencia del azar.

---

### 8.2.2. Domain Business Metrics

Para asegurar que los experimentos no se evalúen con base en métricas de vanidad o datos aislados, el equipo establece un conjunto de métricas de negocio del dominio de **PlantCare**. Estas métricas se definen formalmente a continuación y servirán como el único catálogo de referencia para las tarjetas de experimento del proyecto.

#### 1. Tiempo Promedio de Respuesta ante Alertas Críticas (TPR)
* **Definición:** Mide la rapidez con la que un usuario interviene físicamente para regar su planta tras la emisión de una alerta por falta de humedad.
* **Fórmula de cálculo:**
  $$\text{TPR} = \frac{\sum_{i=1}^{n} (T_{\text{riego}, i} - T_{\text{alerta}, i})}{n}$$
  Donde:
  * $T_{\text{riego}, i}$ es la marca de tiempo (timestamp UTC) del registro de riego de la planta $i$ en la tabla `watering_logs` de **Supabase**.
  * $T_{\text{alerta}, i}$ es la marca de tiempo (timestamp UTC) del primer registro en la tabla `sensor_readings` que documenta un valor de humedad del suelo igual o inferior al 20% ($soil\_moisture\_pct \le 20$) para la misma planta, y que desencadenó la notificación.
  * $n$ es el número total de transiciones críticas de humedad resueltas mediante riego durante el intervalo del experimento.
* **Técnica de recolección:** Consulta SQL de agregación y cálculo de intervalos de tiempo en la base de datos de **Supabase (PostgreSQL)**, ejecutando un cruzamiento (*join*) entre las tablas `sensor_readings` y `watering_logs` agrupadas por identificador de planta (`plant_id`).
* **Meta deseada:** Lograr un $\text{TPR} \le 120$ minutos (2 horas) en el grupo experimental de usuarios que disponen del canal de Discord configurado.

#### 2. Tasa de Clics de Interés Premium (CTR)
* **Definición:** Porcentaje de usuarios únicos expuestos a una propuesta de valor premium que manifiestan interés activo interactuando con el botón de información correspondiente.
* **Fórmula de cálculo:**
  $$\text{CTR}_{\text{característica}} = \frac{U_{\text{clics, característica}}}{U_{\text{vistas, sección}}} \times 100\%$$
  Donde:
  * $U_{\text{clics, característica}}$ es el conteo de usuarios únicos que presionaron el botón "Ver Planes Premium" para una característica específica (diagnóstico avanzado o historial extendido) en un día.
  * $U_{\text{vistas, sección}}$ es el conteo de usuarios únicos que accedieron a la sección de la interfaz donde se muestra dicho botón en el mismo día.
* **Técnica de recolección:** Registro de eventos de clic y de carga de componentes del lado del cliente web en Vue.js. Los eventos se registran en una tabla de auditoría en la base de datos de **Supabase** denominada `feature_interaction_logs` mediante llamadas seguras a la API del cliente de Supabase.
* **Meta deseada:** Obtener un $\text{CTR}_{\text{diagnostico}} \ge 10\%$ y demostrar estadísticamente que la relación $\text{CTR}_{\text{diagnostico}} \ge 2 \times \text{CTR}_{\text{retencion}}$ se cumple con un intervalo de confianza del 95%.

#### 3. Tasa de Mortalidad de Plantas (TMP)
* **Definición:** Mide la eficacia global del sistema de monitoreo y alertas en la prevención de la pérdida física de cultivos y plantas domésticas.
* **Fórmula de cálculo:**
  $$\text{TMP} = \frac{P_{\text{muertas}}}{P_{\text{totales}}} \times 100\%$$
  Donde:
  * $P_{\text{muertas}}$ es el número de plantas cuyo estado en la base de datos se actualizó a "Muerta" o que fueron eliminadas por el usuario seleccionando como motivo "Marchitamiento por sequedad" durante el experimento.
  * $P_{\text{totales}}$ es el número total de plantas activas registradas en la aplicación bajo monitoreo de sensores.
* **Técnica de recolección:** Consulta periódica en la tabla `plants` de la base de datos de **Supabase**, filtrando por la columna de estado de la planta y el registro histórico de bajas.
* **Meta deseada:** Reducir la tasa de mortalidad a un $\text{TMP} \le 3\%$ en el grupo expuesto a las alertas proactivas (frente al 10% estimado en la base de usuarios sin notificaciones optimizadas).

---

### 8.2.3. Measures

La selección de medidas estructuradas define de manera explícita qué datos concretos se capturarán para responder a las preguntas de investigación de cada experimento, clasificándolos en indicadores primarios (el cambio directo esperado) e indicadores secundarios (efectos colaterales o comportamiento del usuario). Adicionalmente, se aplica el principio de economía de datos para garantizar la eficiencia de la infraestructura.

#### Clasificación de Medidas por Experimento

##### Experimento EC-01 (Alertas de Riego Crítico vía Discord)
* **Medida Primaria:**
  * El intervalo temporal en minutos transcurrido desde la marca de tiempo de la lectura crítica de sensor en la base de datos hasta la inserción de la bitácora de riego en la base de datos para la misma planta.
* **Medidas Secundarias:**
  * Cantidad de visitas únicas a la aplicación web procedentes de la URL incrustada en la notificación de Discord (rastreada mediante el parámetro de consulta `?utm_source=discord_webhook`).
  * Nivel de satisfacción de la utilidad de la alerta recopilado en el canal de Discord, calculado mediante la proporción de reacciones de emoji positivo frente a negativo en el mensaje enviado por el bot.

##### Experimento EC-02 (Valoración de Características Premium)
* **Medida Primaria:**
  * Clics únicos y eventos de carga asociados a los botones de "Ver Planes Premium" para la sección de diagnóstico avanzado de salud de la planta y la sección de retención de métricas históricas de 24 meses.
* **Medidas Secundarias:**
  * Tasa de conversión secundaria: Porcentaje de usuarios que, tras abrir el modal de planes premium, hacen clic en el botón de suscripción simulada "Comenzar Prueba de 14 Días".
  * Tiempo de permanencia promedio (en segundos) dentro de la modal de planes premium antes de que el usuario cierre el cuadro de diálogo.

#### Racionalidad y Principio de Economía de Datos

El diseño de medición de **PlantCare** se rige por el principio de economía de datos, lo que implica recopilar únicamente la información estrictamente necesaria durante el tiempo justo del experimento para minimizar la latencia de red, evitar la acumulación de datos redundantes y reducir los costos de almacenamiento y procesamiento en la base de datos de **Supabase**.

Para lograr esto, se implementan las siguientes políticas de optimización y gobernanza de datos:
1. **Frecuencia de Notificaciones Controlada por Transición:** En lugar de emitir notificaciones constantes a Discord cada vez que el sensor envíe una lectura de humedad inferior al 20% (lo cual saturaría el canal del usuario y consumiría recursos innecesarios de red y base de datos), el sistema utiliza un mecanismo de transición de estado lógico en la base de datos. Una alerta solo se dispara cuando el estado de la planta cambia de "Óptimo" a "Crítico". Las notificaciones subsiguientes quedan suspendidas hasta que se registre un evento de riego que retorne la humedad a un nivel superior al 30%.
2. **Eventos Frontend Agregados y Batching:** Para el experimento **EC-02**, los eventos de interacción con los botones de planes no se envían de forma individual e inmediata por cada acción del cursor. El frontend en Vue.js consolida los eventos de visualización y clic localmente en la memoria del navegador y los transmite en un único paquete consolidado (*batch*) cada vez que el usuario abandona la página o cierra la sesión, disminuyendo la carga de peticiones HTTP en el backend.
3. **Purga Automática de Telemetría Histórica:** Las lecturas detalladas de sensores que se envían cada minuto representan un volumen masivo de datos. En conformidad con la propuesta de valor diferenciada del plan premium, el sistema ejecuta un proceso programado (*cron job*) mensual en **Supabase** que purga de forma definitiva todas las lecturas de sensores con antigüedad mayor a 3 meses para los usuarios de la versión gratuita. Esto reduce los costos de almacenamiento físico en PostgreSQL y mantiene optimizados los índices de búsqueda para las consultas de la aplicación.

---

### 8.2.4. Conditions

Las condiciones experimentales definen el entorno operativo y los grupos a los que se exponen los usuarios para identificar de forma clara la relación causa-efecto en cada pregunta de investigación. A continuación, se detallan las condiciones estructuradas para los experimentos prioritarios de **PlantCare**.

#### Experimento EC-01: Alertas de Riego Crítico vía Discord
Para evaluar de manera confiable el impacto de las alertas proactivas, se configuran dos grupos de usuarios con perfiles comparables bajo las siguientes condiciones durante un período de prueba uniforme de 14 días:
* **Condición Experimental ($C_E$):** Constituida por un grupo piloto de 15 usuarios que activan la integración de Discord en su perfil de la aplicación. Cada vez que el backend de **Supabase** registre que la humedad del suelo de una planta desciende de la cota crítica del 20%, se gatilla un webhook que envía de forma automática una notificación enriquecida con información diagnóstica contextual al canal privado de Discord del usuario, incluyendo un enlace directo a la sección de diagnóstico de salud.
* **Condición de Control ($C_C$):** Constituida por un grupo piloto de 15 usuarios que operan bajo el comportamiento heredado (*As-Is*). Estos usuarios no reciben notificaciones automáticas fuera de la aplicación. Para enterarse del estado de sequedad crítica de sus cultivos, deben abrir manualmente la aplicación web Vue.js y consultar el estado del dashboard principal.

#### Experimento EC-02: Valoración de Características Premium
Dado el carácter exploratorio de esta investigación de mercado, se configuran límites precisos para el grupo de estudio con el fin de recopilar respuestas fiables y significativas:
* **Definición del Grupo de Estudio:** El experimento se restringe a usuarios de la versión gratuita que demuestren un uso básico consolidado de la plataforma. Los criterios de inclusión en el grupo son: poseer al menos 3 plantas registradas y tener más de 30 días de uso activo en la aplicación. Esto previene sesgos causados por usuarios nuevos o inactivos.
* **Condición Experimental 1 ($C_{E1}$):** Exposición de los usuarios elegibles al botón informativo "Ver Planes Premium" ubicado dentro del panel de diagnósticos detallados de salud de las plantas.
* **Condición Experimental 2 ($C_{E2}$):** Exposición simultánea de los mismos usuarios al botón informativo "Ver Planes Premium" ubicado dentro de la sección de visualización de gráficos históricos de sensores.

---

### 8.2.5. Scale Calculations and Decisions

Las decisiones de escala determinan la magnitud del experimento en función del nivel de certidumbre requerido y la precisión del cambio que se pretende detectar. Los parámetros estadísticos se han calibrado para equilibrar el rigor científico y la economía de datos.

#### Experimento EC-01: Alertas de Riego Crítico vía Discord
* **Certeza (Certainty):**
  * **Nivel de Significación ($\alpha$):** Se define en 5% (0.05). Esto indica que hay un 5% de probabilidad de cometer un error Tipo I (rechazar falsamente la hipótesis nula, asumiendo un beneficio de Discord que no es real).
  * **Poder Estadístico ($1-\beta$):** Se establece en 80% (0.80) para reducir al 20% la probabilidad de cometer un error Tipo II (fallar en detectar una reducción real en el tiempo de riego provocada por Discord).
* **Precisión y Efecto Mínimo Detectable (MDE):**
  * Se establece un MDE de una reducción de 10 horas (600 minutos) en el tiempo de respuesta promedio de riego. Dado que la línea base histórica del grupo de control es de 12 horas, una diferencia menor no justificaría los esfuerzos de infraestructura técnica.
* **Decisión de Tamaño de Muestra:**
  * Asumiendo una desviación estándar histórica de la respuesta de riego de aproximadamente 8 horas ($\sigma = 480$ minutos) y aplicando una prueba de diferencia de medias de dos colas con $\alpha = 0.05$ y poder de 80%, el cálculo de escala matemática determina que se requiere recopilar al menos 30 eventos críticos de riego por cada condición ($C_E$ y $C_C$). Con 15 usuarios por grupo monitoreados de manera proactiva durante 14 días (estimando entre 2 y 3 alertas críticas de sequedad por planta en dicho lapso), se garantiza superar el umbral estadístico requerido.

#### Experimento EC-02: Valoración de Características Premium
* **Certeza (Certainty):**
  * **Nivel de Significación ($\alpha$):** 5% (0.05).
  * **Poder Estadístico ($1-\beta$):** 80% (0.80).
* **Precisión y Efecto Mínimo Detectable (MDE):**
  * El MDE se define como una diferencia absoluta del 5% en la tasa de clics (CTR) entre el botón de diagnóstico avanzado de salud y el botón de retención extendida de datos.
* **Decisión de Tamaño de Muestra:**
  * Estimando una tasa de clics base (CTR) del 5% para llamados a la acción y aplicando una prueba de comparación de dos proporciones independientes, se calcula que es necesaria una exposición mínima de 150 usuarios únicos activos del plan gratuito por cada sección evaluada durante un período continuo de 15 días de observación.

---

### 8.2.6. Methods Selection

Para llevar a cabo la investigación y recopilar la evidencia empírica necesaria, es fundamental seleccionar los métodos y herramientas tecnológicas más adecuadas. La elección de estas herramientas se rige por el principio de la **Simplest Useful Thing** (la cosa más simple y útil) y un análisis comparativo de las alternativas disponibles en el mercado para el monitoreo de rendimiento, experiencia de usuario y analíticas de comportamiento.

A continuación, se presenta la tabla comparativa de herramientas evaluadas para el proceso de experimentación de **PlantCare**:

| Criterio | Google Analytics | Catchpoint | RedLine13 | Lighthouse |
| :--- | :--- | :--- | :--- | :--- |
| **Precio** | Plan gratuito / créditos gratis | Basado en suscripción, con pruebas gratuitas | Gratuito con limitaciones | Plan gratuito, disponible para ejecución local |
| **Capacidad de Análisis** | Análisis exhaustivo de métricas y datos de usuario | Monitoreo exhaustivo de rendimiento y experiencia de usuario desde múltiples ubicaciones | Análisis orientado a pruebas de carga y rendimiento de aplicaciones | Análisis orientado a la experiencia de usuario, con métricas clave de rendimiento y accesibilidad |
| **Sencillez** | Aprendizaje sencillo de las métricas | Interfaz avanzada pero detallada y completa | Información detallada y resumida sobre rendimiento | Información resumida en valores clave que puntúan aspectos de la aplicación |
| **Ventajas** | Excelente capacidad de generación de reportes y amplia integración con otros servicios | Análisis en tiempo real desde diversas ubicaciones y dispositivos, ideal para empresas con usuarios globales | Simulación de tráfico y pruebas de rendimiento bajo condiciones de carga | Evaluación de accesibilidad, rendimiento y diseño con métricas claras para optimizar la experiencia del usuario |

---

#### Justificación de la Selección de Herramientas y Métodos

Basándose en la comparación anterior y aplicando el principio de la **Simplest Useful Thing**, el equipo define la selección de métodos y herramientas para cada uno de los experimentos prioritarios de la siguiente manera:

1. **Para el Experimento EC-01 (Alertas de Riego Crítico vía Discord):**
   * **Objeto de Investigación:** Evaluar si las notificaciones externas reducen el tiempo que tarda un usuario en atender una planta con sequedad crítica.
   * **Herramientas Seleccionadas:** 
     * **Supabase Webhooks & Database Triggers:** Como la solución de infraestructura más simple y útil para el envío automático de notificaciones basadas en cambios de estado de humedad, eliminando la necesidad de servidores de mensajería intermedios.
     * **Lighthouse:** Utilizado de forma local para probar la accesibilidad y el rendimiento de renderizado en el cliente web Vue.js (asegurando que los botones de alerta y navegación cumplan con métricas de diseño óptimas y no agreguen latencia).
   * **Método de Prueba:** Una **Prueba A/B con Asignación Grupal Paralela** durante 14 días.

2. **Para el Experimento EC-02 (Valoración de Características Premium):**
   * **Objeto de Investigación:** Medir el interés real de los usuarios en diagnósticos avanzados frente a retención de datos antes de proceder con su codificación compleja.
   * **Herramientas Seleccionadas:** 
     * **Google Analytics:** Seleccionado por su facilidad de integración en aplicaciones Vue.js, su sencillo aprendizaje de métricas y su excelente capacidad para generar reportes automáticos de la tasa de clics (CTR). Es la herramienta más simple y útil en comparación con alternativas de pago complejas como Catchpoint.
   * **Método de Prueba:** Un diseño de **Botón Simulado (Fake Door Testing / Smoke Testing)** para capturar de forma directa el interés y registrar clics únicos.

---

#### Normas Estadísticas y de Convivencia de Experimentos

Para salvaguardar la validez científica del experimento y mantener la calidad del servicio a los usuarios, se establecen las siguientes reglas operativas:

1. **No Solapamiento de Experimentos:** Para evitar la contaminación cruzada (donde la fatiga de un usuario al recibir alertas de Discord afecte su probabilidad de hacer clic en los botones de planes premium), se implementa una separación por identificador de usuario (`user_id`). Los usuarios asignados al grupo experimental de Discord (**EC-01**) quedan excluidos lógicamente del registro de datos y analíticas de **EC-02**, y viceversa.
2. **Consideraciones Éticas de No Daño:** El equipo garantiza que la experimentación no cause perjuicio alguno a los usuarios o a sus plantas. Los usuarios de control en **EC-01** siguen teniendo acceso en tiempo real a las lecturas de sus sensores en el dashboard. En **EC-02**, la interacción con el botón simulado no realiza ningún cobro monetario y se les notifica de inmediato a los usuarios que su selección servirá para darles acceso priorizado a la funcionalidad cuando esté terminada, manteniendo la transparencia y honestidad.

### 8.2.8. Web and Mobile Tracking Plan

El Plan de Seguimiento (*Tracking Plan*) define formalmente los eventos de interfaz, disparadores y parámetros que se registrarán en la aplicación web para capturar las interacciones de los usuarios. Esta estructuración garantiza que la integración con **Google Analytics** recopile únicamente eventos limpios y alineados con los objetivos del diseño experimental.

| Elemento de la Interfaz | Acción del Usuario | Nombre del Evento | Parámetros del Evento | Disparador (Trigger) | Herramienta de Medición |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Botón "Ver Planes Premium" (Dashboard Salud) | Clic en el botón | `select_promotion` | `promotion_id: "premium_health"`<br>`promotion_name: "diagnostico_avanzado"`<br>`location: "panel_salud"` | Clic en el elemento `a#btn-premium-health` | Google Analytics |
| Botón "Ver Planes Premium" (Dashboard Historial) | Clic en el botón | `select_promotion` | `promotion_id: "premium_history"`<br>`promotion_name: "retencion_datos_24m"`<br>`location: "panel_historial"` | Clic en el elemento `a#btn-premium-history` | Google Analytics |
| Modal Informativo de Planes Premium | Vista del componente | `view_promotion` | `promotion_id: "premium_modal"`<br>`location: "modal_overlay"` | Renderizado del modal en la interfaz web | Google Analytics |
| Botón "Comenzar Prueba de 14 Días" | Clic en confirmación | `purchase_simulation` | `plan_id: "suscripcion_premium_mensual"`<br>`simulated: true` | Clic en el botón `button#btn-start-trial` | Google Analytics |
| Notificación de Discord (Alertas) | Clic en enlace de diagnóstico | `click_alert_link` | `alert_type: "discord_webhook"`<br>`source: "discord"`<br>`campaign: "riego_critico"` | Apertura del enlace web con el parámetro `utm_source=discord` | Google Analytics / Supabase |

---

## 8.3. Experimentation

La fase de experimentación ejecuta de forma práctica los diseños definidos para obtener los datos empíricos que darán respuesta a las hipótesis de negocio de **PlantCare**. Este proceso se integra dentro del ciclo de vida de desarrollo de la plataforma, utilizando el pipeline de entrega continua para desplegar de manera ágil los cambios en la interfaz web frontend y en los triggers del backend serverless en **Supabase**, permitiendo una recopilación controlada de eventos bajo las condiciones establecidas.

### 8.3.1. To-Be User Stories

Para dar soporte a la infraestructura de recolección de datos y ejecutar los experimentos propuestos, se incorporan al backlog dos nuevas Historias de Usuario (*User Stories*). Estas historias siguen la misma estructura y formalidad empleadas en el Capítulo III, detallando descripciones, escenarios con lenguaje Gherkin y reglas específicas de negocio.

---

#### US-041: Alertas contextuales de riego a Discord (Soporte a EC-01)
* **Epic ID:** EPIC 007 (Alertas Inteligentes Contextuales)
* **Story ID:** US-041
* **Título:** Alertas contextuales de riego a Discord
* **Descripción:** Como usuario ocupado, quiero vincular mi canal de Discord mediante un webhook en la aplicación web, para recibir alertas automatizadas con diagnósticos inmediatos en mi teléfono cuando la humedad de mi planta baje de la cota crítica del 20%.
* **Criterios de Aceptación:**
  * **Escenario 01: Vinculación exitosa del webhook de Discord**
    * Dado que un usuario registrado accede a la sección de configuración de perfil
    * Cuando ingresa una URL de webhook válida y pulsa el botón "Vincular Canal"
    * Entonces el sistema almacena la URL en la base de datos de **Supabase**, envía un mensaje de confirmación al canal de Discord y muestra una notificación visual de éxito.
  * **Escenario 02: Envío de alerta ante humedad crítica detectada**
    * Dado que el sensor IoT de una planta registra una humedad del suelo igual o inferior al 20%
    * Cuando el trigger en el backend serverless de **Supabase** detecta este cambio de estado crítico
    * Entonces realiza una petición HTTP POST a la URL de webhook almacenada, enviando una tarjeta enriquecida con la causa probable del problema y un enlace parametrizado (`?utm_source=discord`) para acceder al diagnóstico de salud.
* **Reglas de Negocio:**
  * La URL ingresada por el usuario debe cumplir con el patrón regex de validación oficial de Discord: `https://discord.com/api/webhooks/...`.
  * Para evitar spam y consumo innecesario de almacenamiento, el trigger solo envía una alerta por cada transición al estado crítico, quedando bloqueadas las alertas siguientes hasta que se inserte un registro en `watering_logs` que retorne la humedad por encima del 30%.

---

#### US-042: Simulación de compra de suscripción premium (Soporte a EC-02)
* **Epic ID:** EPIC 005 (Herramientas Avanzadas para Aficionados)
* **Story ID:** US-042
* **Título:** Simulación de compra de suscripción premium
* **Descripción:** Como usuario del plan gratuito de PlantCare, quiero pulsar sobre los botones promocionales de características premium para visualizar un modal informativo y poder confirmar mi interés en el plan, de manera que el sistema pueda registrar mis preferencias de valor.
* **Criterios de Aceptación:**
  * **Escenario 01: Visualización del modal informativo de planes**
    * Dado que un usuario gratuito hace clic en el botón "Ver Planes Premium" en el panel de diagnósticos detallados o en el de historial de gráficos
    * Cuando el modal interactivo se renderiza en la pantalla
    * Entonces se registra un evento `view_promotion` en **Google Analytics** detallando la sección de origen, y se muestran las tarifas y beneficios del plan premium.
  * **Escenario 02: Confirmación de interés mediante suscripción simulada**
    * Dado que el usuario tiene abierto el modal de planes premium
    * Cuando hace clic en el botón "Comenzar Prueba de 14 Días"
    * Enviar un evento `purchase_simulation` a **Google Analytics**, registrar el identificador único del usuario en la tabla `premium_leads` de **Supabase** y mostrar un mensaje de agradecimiento informándole que la característica estará disponible en la próxima versión sin cobros reales.
* **Reglas de Negocio:**
  * El modal de planes debe ser completamente adaptable a dispositivos móviles (*responsive*).
  * Bajo ninguna circunstancia el modal de simulación solicitará datos de tarjetas de crédito o credenciales de pago reales, garantizando la transparencia de la prueba.

---

### 8.3.2. To-Be Product Backlog

El Backlog de Producto se actualiza para incorporar las nuevas Historias de Usuario diseñadas para dar soporte a los experimentos. Para el Sprint To-Be 1, se priorizan las historias de experimentación (**US-041** y **US-042**) por encima de los requisitos pendientes del backlog original, con el fin de mitigar el riesgo de producto en la etapa más temprana posible del ciclo de desarrollo.

El backlog consolidado de **PlantCare** se detalla en la siguiente tabla:

| Orden | User Story ID | Título | Descripción | Story Points | Estado / Planificación |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **1** | **US-041** | Alertas contextuales de riego a Discord | Como usuario ocupado, quiero recibir alertas automáticas en Discord ante estados críticos de humedad | 8 | Priorizado (Sprint To-Be 1) |
| **2** | **US-042** | Simulación de compra de suscripción premium | Como usuario gratuito, quiero pulsar en opciones premium para registrar mi interés de compra | 5 | Priorizado (Sprint To-Be 1) |
| 3 | US-015 | Visualización unificada de plantas | Como usuario, quiero ver todas mis plantas en una vista consolidada con estado de salud | 8 | Completado (Sprint As-Is 1) |
| 4 | US-020 | Visualización avanzada en tiempo real | Como aficionado, quiero ver datos de sensores en tiempo real con múltiples parámetros | 8 | Completado (Sprint As-Is 1) |
| 5 | US-011 | Registro simplificado de planta | Como usuario, quiero registrar una nueva planta completando información básica esencial | 3 | Completado (Sprint As-Is 1) |
| 6 | US-012 | Edición rápida de información de planta | Como usuario, quiero editar la información básica de una planta registrada | 2 | Completado (Sprint As-Is 1) |
| 7 | US-005 | Registro de usuario | Como visitante, quiero registrarme con datos básicos protegidos para crear una cuenta segura | 5 | Completado (Sprint As-Is 1) |
| 8 | US-006 | Inicio de sesión | Como usuario registrado, quiero autenticarme de forma segura con mis credenciales | 3 | Completado (Sprint As-Is 1) |
| 9 | US-023 | Reportes de largo plazo | Como aficionado, quiero acceder a reportes mensuales y anuales consolidados | 5 | Pendiente |
| 10 | US-019 | Monitoreo detallado de humedad | Como aficionado, quiero monitorear precisamente la humedad del aire | 5 | Pendiente |
| 11 | US-040 | Sistema de confirmación y seguimiento | Como usuario, quiero confirmar cuando he aplicado riego y recibir seguimiento del historial | 5 | Pendiente |
| 12 | US-013 | Eliminación confirmada de planta | Como usuario, quiero eliminar plantas que ya no cuido | 2 | Pendiente |
| 13 | US-014 | Configuración de parámetros básicos | Como usuario, quiero definir parámetros esenciales para cada planta | 3 | Pendiente |
| 14 | US-008 | Gestión de sesión | Como usuario, quiero que mi sesión se maneje de forma segura | 2 | Pendiente |
| 15 | US-010 | Cerrar sesión | Como usuario, quiero cerrar sesión de manera definitiva | 2 | Pendiente |