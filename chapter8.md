# Capítulo VIII: Experiment-Driven Development

## 8.1. Experiment Planning

En este apartado veremos todos los artefactos para experimentar que problemas se nesesitan abordar 

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
- **Modo oscuro:** Se asume que los usuarios valoran la personalización de la interfaz gráfica y la comodidad visual, de modo que la implementación de un modo oscuro facilitará el monitoreo y cuidado de plantas en condiciones de baja luminosidad (de noche o en exteriores con luz cambiante), reduciendo la fatiga visual y prolongando la sesión del usuario.
- **Traducciones (Localización):** Se asume que existe un interés en mercados globales de aficionados a la jardinería urbana, por lo que ofrecer la aplicación en inglés y español incrementará la base de usuarios activos e impedirá abandonos debido a la barrera lingüística.
- **Alertas de riego en Discord:** Se asume que los usuarios reaccionarán con mayor rapidez y efectividad ante alertas instantáneas en un canal externo de Discord en lugar de notificaciones internas de la aplicación web, disminuyendo el tiempo de respuesta.
- **Modelo de suscripción premium:** Se asume que un segmento de usuarios avanzados (como jardineros coleccionistas o gestores de pequeños huertos urbanos) está dispuesto a pagar una tarifa mensual a cambio de diagnósticos avanzados detallados en lugar de la retención extendida de datos históricos de sensores.
- **Gamificación por logros:** Se asume que otorgar insignias virtuales por el mantenimiento de plantas sanas incentivará a los usuarios a ingresar a la plataforma de forma periódica e incremental.

**Knowledge Gaps (Brechas de conocimiento):**
- **Preferencia del modo oscuro:** Falta información empírica cuantitativa sobre si la comodidad visual del modo oscuro efectivamente se traduce en un incremento en el tiempo promedio de sesión nocturna en dispositivos móviles.
- **Uso de localización (i18n):** Se desconoce si habilitar la traducción del perfil reducirá significativamente los índices de abandono en las páginas de configuración para los usuarios angloparlantes.
- **Eficacia de Discord Webhooks:** No hay evidencia histórica sobre si las alertas proactivas a Discord reducen el tiempo promedio de respuesta ante sequedad crítica a menos de 2 o horas.
- **Preferencia de Valor Premium:** Se carece de datos históricos sobre cuál de las dos propuestas premium (diagnóstico de salud o historial de 24 meses) tiene una mayor tasa de clics y tasa de conversión simulada.
- **Frecuencia por insignias:** Falta información cuantitativa para determinar si el reconocimiento de medallas por rachas sanas incrementa la retención y visitas semanales del usuario.

**Ideas:**
- **Modo oscuro con toggle superior:** Diseñar un botón de alternancia en la barra de navegación que guarde la preferencia en `localStorage` para activar el tema oscuro en la aplicación web Vue.js.
- **Traducción i18n:** Integrar la biblioteca Vue I18n con selector manual y detección automática de idioma del navegador.
- **Alertas a Discord:** Conectar triggers de base de datos en Supabase para enviar webhooks automatizados con enlaces parametrizados.
- **Simulación de Compra (Fake Door):** Agregar botones informativos de planes premium en los dashboards de salud e historial para registrar la intención de compra del usuario mediante Google Analytics.
- **Mecánica de Logros:** Diseñar un panel lateral que compute rachas de 7 días sanas y otorgue una insignia interactiva de "Cuidador Experto".

**Claims (Afirmaciones de usuarios y stakeholders):**
- **Usuario Natalia:** "Revisar los gráficos de la aplicación web en mi celular en pleno día cuando estoy en el jardín es complicado debido al reflejo del sol en la pantalla, por lo que necesito mejor contraste."
- **Usuario Vasco:** "No me gusta estar amarrado a una suscripción mensual fija solo por ver las estadísticas básicas; prefiero realizar una sola compra de hardware."
- **Usuario María:** "He perdido varias plantas en el pasado por exceso de agua o fertilizante. Necesito que la aplicación me guíe de forma proactiva con alertas explicativas claras en mi canal de mensajería y no solo con gráficos difíciles de interpretar."
- **Usuario Derek (Internacional):** "Se me dificulta interactuar con la configuración del perfil porque no entiendo los términos específicos de riego en español; preferiría configurar la aplicación en inglés."

---

### 8.1.3. Experiment-Ready Questions

En esta sección se definen las preguntas listas para el experimento (*Experiment-Ready Questions*), clasificadas en dos tipos principales según el enfoque de investigación de **PlantCare**:

1. **Preguntas Impulsadas por Creencias (Belief-led):** Formuladas para validar o refutar hipótesis y supuestos específicos que el equipo considera fundamentales para el valor del producto.
2. **Preguntas Exploratorias (Exploratory):** Diseñadas para recopilar nuevos conocimientos en áreas con alta incertidumbre, donde no existen suposiciones previas validadas.

Para estructurar estas preguntas y descubrir supuestos subyacentes u ocultos, se aplica la técnica de las **5 Ws y 1 H** (*Who, What, Where, When, Why, How*).

---

#### 1. Preguntas Impulsadas por Creencias (Belief-led)

##### Pregunta 1: Monitoreo en exteriores mediante Modo Oscuro
**¿La implementación de una interfaz de modo oscuro (alto contraste) en la aplicación web incrementará la duración de la sesión promedio de los usuarios en dispositivos móviles durante la noche en al menos un 15%?**

- **Who (Quién):** Aficionados a las plantas y cuidadores (con perfiles similares a Natalia) que interactúan con la interfaz bajo condiciones de iluminación exterior o nocturna.
- **What (Qué):** Una visualización alternativa en modo oscuro que optimice la legibilidad y reduzca el reflejo de la luz solar en las pantallas de los teléfonos móviles.
- **Where (Dónde):** En el panel principal y las secciones de gráficos históricos de la aplicación web Vue.js.
- **When (Cuándo):** Durante el monitoreo y registro de plantas en jardines o terrazas durante el día con alta luminosidad, o en interiores con poca luz durante la noche.
- **Why (Por qué):** Para mitigar la fatiga visual provocada por el contraste excesivo del sol o la luz clara y evitar que la incomodidad visual cause que los usuarios abandonen la aplicación de manera temprana.
- **How (Cómo):** Proveyendo un botón de alternancia en la barra de navegación superior de la interfaz para activar de forma manual el modo oscuro mediante hojas de estilo CSS dinámicas.

##### Pregunta 2: Notificaciones de Riego Crítico vía Discord Webhooks
**¿El envío automático de alertas de riego crítico a un canal dedicado de Discord mediante Webhooks reducirá el tiempo de respuesta promedio de los usuarios ante estados críticos de humedad del suelo de 12 horas a menos de 2 horas?**

- **Who (Quién):** Usuarios urbanos con rutinas ocupadas (con perfiles similares a María y Vasco) que a menudo olvidan abrir la aplicación web.
- **What (Qué):** Envío automático de notificaciones estructuradas con el diagnóstico contextual del sensor afectado (ej. humedad del suelo menor al 20%).
- **Where (Dónde):** Canal exclusivo de la plataforma Discord.
- **When (Cuándo):** Inmediatamente después de que el backend de **Supabase** registre una lectura crítica de humedad del suelo.
- **Why (Por qué):** Para eliminar la necesidad de que el usuario tenga que ingresar manualmente al panel web para enterarse del estado de la planta, asegurando una intervención inmediata.
- **How (Cómo):** Conectando el pipeline de base de datos de Supabase con un servicio que envíe peticiones POST a la URL del webhook de Discord cada vez que se detecte una anomalía de riego.

##### Pregunta 3: Soporte de Multiidioma i18n
**¿La incorporación de un selector de idioma inglés/español mediante i18n reducirá la tasa de abandono de la configuración del perfil para usuarios con navegadores configurados en inglés en un 30%?**

- **Who (Quién):** Usuarios internacionales y angloparlantes (con perfiles similares a Juan) que interactúan con el formulario de registro de sensores e información personal.
- **What (Qué):** Una traducción completa y contextual de los campos del formulario de configuración mediante archivos JSON regionales en Vue.js.
- **Where (Dónde):** En la sección de perfil y la ruta `/config` de la aplicación web.
- **When (Cuándo):** En la carga inicial de configuración del perfil del usuario o del hardware IoT.
- **Why (Por qué):** Para evitar que la barrera del idioma genere confusión en los campos obligatorios del perfil y provoque que los usuarios extranjeros desistan de completar el registro.
- **How (Cómo):** Utilizando la biblioteca Vue I18n para mapear dinámicamente las cadenas del formulario y agregando un botón conmutador de traducción en la barra superior.

##### Pregunta 5: Insignias de Logros por Cuidado (Gamificación)
**¿La visualización de una insignia virtual de "Cuidador Experto" tras mantener todas las plantas sin alertas críticas por 7 días incrementará la frecuencia de visitas semanales al panel en al menos un 20%?**

- **Who (Quién):** Cuidadores domésticos que requieren consistencia y un hábito regular para el mantenimiento de su huerto urbano.
- **What (Qué):** Una medalla digital interactiva en el perfil lateral que reconoce el estado óptimo de sus plantas.
- **Where (Dónde):** En el panel principal y perfil del usuario dentro de la aplicación.
- **When (Cuándo):** Tras cumplir un lapso de 7 días consecutivos sin ningún registro de alertas de humedad críticas en la base de datos.
- **Why (Por qué):** Para aprovechar motivadores extrínsecos (recompensas virtuales) e incentivar a los usuarios a realizar ingresos recurrentes para mantener su racha y proteger su medalla.
- **How (Cómo):** Evaluando las alertas históricas en la tabla `sensor_readings` de Supabase asincrónicamente y mostrando la insignia ganada mediante animaciones CSS.

---

#### 2. Preguntas Exploratorias (Exploratory)

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

El siguiente cuadro detalla la evaluación de las cinco preguntas clave de investigación de la plataforma:

| ID | Pregunta de Investigación | Motivación ("Por qué") | C | R | I | Int | Total | Estado |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Q2** | ¿Las alertas de Discord reducirán el tiempo de respuesta del usuario ante humedad crítica a menos de 2 horas? | Evitar la pérdida física de plantas por descuidos en el riego de usuarios ocupados. | 4 | **5** | 4 | 4 | **17** | Priorizada (Backlog Profundo) |
| **Q4** | ¿Qué características premium (diagnósticos avanzados o historiales extendidos) despiertan mayor interés de compra? | Validar la viabilidad económica del modelo de suscripción SaaS para sustentar el proyecto. | 4 | **4** | 4 | 4 | **16** | Priorizada (Backlog Profundo) |
| **Q1** | ¿La interfaz de modo oscuro incrementará la duración de sesión promedio nocturna en móviles en un 15%? | Mejorar la usabilidad y reducir fatiga visual en exteriores y noches. | 4 | **2** | 3 | 4 | **13** | Priorizada (Backlog Profundo) |
| **Q5** | ¿La insignia virtual de "Cuidador Experto" incrementará las visitas semanales al panel en un 20%? | Incentivar la frecuencia de monitoreo diario mediante gamificación interactiva. | 3 | **3** | 3 | 4 | **13** | Priorizada (Backlog Profundo) |
| **Q3** | ¿El soporte i18n reducirá el abandono del formulario de perfil para usuarios con navegador en inglés en un 30%? | Eliminar la barrera de idioma en el registro inicial de perfiles y sensores. | 4 | **2** | 3 | 3 | **12** | Priorizada (Backlog Profundo) |

*Nota sobre la aplicación de la regla de desempate:* Las preguntas **Q1** y **Q5** empataron en un puntaje total de **13**. Aplicando la regla de desempate por riesgo, **Q5** se sitúa por encima de **Q1** en el backlog debido a que tiene un riesgo de **3** (las mecánicas de retención son críticas para el hábito diario) frente al riesgo de **2** de **Q1** (comodidad visual de modo oscuro).

---

#### Estructura del Backlog Profundo (Deep Backlog)

Debido a la naturaleza de las 5 hipótesis experimentales de software planteadas para las iteraciones inmediatas de desarrollo de **PlantCare**, todas las preguntas han sido seleccionadas para su experimentación y despliegue dentro del Backlog Profundo:

1. **Prioridad 1 (Q2):** Impacto de las alertas de riego crítico vía webhooks de Discord en el TPR.
2. **Prioridad 2 (Q4):** Interés de compra y valoración de características de suscripción Premium.
3. **Prioridad 3 (Q5):** Frecuencia de visitas semanales y retención mediante insignias de logros (Gamificación).
4. **Prioridad 4 (Q1):** Incremento del tiempo de sesión en móviles mediante la activación del tema Modo Oscuro.
5. **Prioridad 5 (Q3):** Reducción de la tasa de abandono en la configuración de perfiles mediante localización i18n.

---

### 8.1.5. Experiment Cards

Las Tarjetas de Experimento (*Experiment Cards*) sirven como la plantilla técnica estructurada que captura la información clave para cada experimento antes de su ejecución. Se han diseñado cinco tarjetas correspondientes a las hipótesis priorizadas del Backlog Profundo:

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
- **Hipótesis (Hypothesis):** Creemos que el diagnóstico detallado de salud y las sugerencias avanzadas personalizadas generarán el doble de clics de interés que la retención de datos históricos por 24 meses, porque los usuarios de PlantCare valoran más la información orientada a la acción y salud inmediata que el almacenamiento masivo.
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

---

#### Tarjeta de Experimento 3: EC-03 (Modo Oscuro para Monitoreo Nocturno)

##### Lado Frontal (Front Side)
- **Pregunta (Question):** ¿La implementación de una interfaz de modo oscuro en la aplicación web incrementará la duración de la sesión promedio de los usuarios en dispositivos móviles durante la noche en al menos un 15%?
- **Por qué (Why):** Evitar la fatiga visual y mejorar la legibilidad bajo condiciones de baja iluminación nocturna o excesivo brillo solar exterior, promoviendo interacciones prolongadas.
- **Hipótesis (Hypothesis):** Creemos que si implementamos un botón de selección de modo oscuro en la barra de navegación superior de la aplicación web, entonces la duración promedio de las sesiones de los usuarios en dispositivos móviles durante la noche (de 19:00 a 06:00) aumentará en un 15%, debido a la reducción de la fatiga visual bajo condiciones de baja luminosidad.
- **Qué (What - Simplest Useful Thing):** Implementar un conmutador simple en la cabecera que alterne clases CSS y guarde la preferencia del tema en `localStorage`.

##### Lado Posterior (Back Side)
- **Medidas (Measures):**
  - **Métrica Primaria:** Duración promedio en minutos de la sesión de usuario en móviles durante la noche (Duración de Sesión Nocturna - DSN).
  - **Métrica Secundaria:** Conteo de clics en el botón de alternancia del modo oscuro e interacciones con gráficos de sensores en la noche.
- **Condiciones (Conditions):**
  - **Condición de Control:** Interfaz fija en modo claro (sin opción de alternancia).
  - **Condición Experimental:** Habilitación del selector de modo oscuro en la barra de navegación superior.
- **Escala (Scale):**
  - **Nivel de significancia ($\alpha$):** 5%.
  - **Potencia estadística ($1-\beta$):** 80%.
  - **Efecto Mínimo Detectable (MDE):** Un incremento del 15% (1.2 minutos sobre una línea base de 8 minutos).
  - **Tamaño de muestra:** Recopilación de al menos 100 sesiones nocturnas registradas por cada grupo durante 14 días.

---

#### Tarjeta de Experimento 4: EC-04 (Soporte Multiidioma i18n)

##### Lado Frontal (Front Side)
- **Pregunta (Question):** ¿La integración de un selector de idioma inglés/español mediante i18n reducirá la tasa de abandono de la configuración del perfil para usuarios con navegadores configurados en inglés en un 30%?
- **Por qué (Why):** Minimizar el abandono y la fricción lingüística en el proceso de configuración del perfil y registro del hardware IoT para usuarios internacionales.
- **Hipótesis (Hypothesis):** Creemos que si incorporamos un selector de idioma (inglés/español) en la cabecera de la aplicación web mediante la biblioteca i18n, entonces la tasa de abandono en las páginas de configuración para usuarios con navegadores configurados en inglés se reducirá en un 30%, al eliminar la barra lingüística al interactuar con el sistema.
- **Qué (What - Simplest Useful Thing):** Integrar la biblioteca Vue I18n con archivos locales de traducción JSON para el formulario de perfil.

##### Lado Posterior (Back Side)
- **Medidas (Measures):**
  - **Métrica Primaria:** Tasa de Abandono de Configuración (TAC) en la ruta `/config` para usuarios con idioma del navegador en inglés.
  - **Métrica Secundaria:** Conteo de cambios de idioma manuales e incidentes de error en la validación del formulario.
- **Condiciones (Conditions):**
  - **Condición de Control:** Formulario e interfaz web fija en español.
  - **Condición Experimental:** Soporte i18n habilitado con selector inglés/español y detección automática del navegador.
- **Escala (Scale):**
  - **Nivel de significancia ($\alpha$):** 5%.
  - **Potencia estadística ($1-\beta$):** 80%.
  - **Efecto Mínimo Detectable (MDE):** Una reducción absoluta del 12% (del 40% histórico al 28% de TAC).
  - **Tamaño de muestra:** Registro de al menos 80 usuarios únicos calificados por condición durante 20 días.

---

#### Tarjeta de Experimento 5: EC-05 (Insignias de Logros por Cuidado)

##### Lado Frontal (Front Side)
- **Pregunta (Question):** ¿La visualización de una insignia virtual de "Cuidador Experto" tras mantener todas las plantas sin alertas críticas por 7 días incrementará la frecuencia de visitas semanales al panel de control en un 20%?
- **Por qué (Why):** Aumentar el compromiso del usuario y fomentar el hábito recurrente de monitoreo preventivo de plantas mediante la gamificación.
- **Hipótesis (Hypothesis):** Creemos que si mostramos una insignia virtual de "Cuidador Experto" en el perfil del usuario tras mantener todas sus plantas sin alertas críticas durante 7 días consecutivos, entonces la frecuencia de visitas semanales al panel de control aumentará en un 20%, ya que la gamificación incentiva la revisión diaria del estado de las plantas.
- **Qué (What - Simplest Useful Thing):** Agregar un panel lateral simple que dibuje el logro de la insignia tras procesarse una racha de 7 días sanos en Supabase.

##### Lado Posterior (Back Side)
- **Medidas (Measures):**
  - **Métrica Primaria:** Tasa de Retención Semanal de Visitas (TRV) calculada como días activos semanales.
  - **Métrica Secundaria:** Cantidad de insignias ganadas y número de clics en el widget de logros del perfil.
- **Condiciones (Conditions):**
  - **Condición de Control:** Panel lateral sin barra ni mecánicas de insignias.
  - **Condición Experimental:** Panel lateral con widget activo de medallas y cálculo automático de rachas sanas.
- **Escala (Scale):**
  - **Nivel de significancia ($\alpha$):** 5%.
  - **Potencia estadística ($1-\beta$):** 80%.
  - **Efecto Mínimo Detectable (MDE):** Un incremento promedio de 0.7 días en TRV (línea base de 3.5 ingresos).
  - **Tamaño de muestra:** Monitoreo de al menos 50 usuarios activos por grupo durante 14 días.

## 8.2. Experiment Design

La fase de diseño del experimento constituye un pilar fundamental para garantizar que la investigación de software proporcione resultados consistentes, confiables y con validez estadística. En esta sección se establece un marco estructurado para definir formalmente las expectativas de cambio, alinear las variables experimentales con los objetivos de negocio y delimitar con precisión qué datos se recopilarán del sistema. Esto permite mitigar la incertidumbre técnica y de negocio de **PlantCare**, asegurando que las decisiones de evolución del producto estén sustentadas en evidencia empírica rigurosa y no en suposiciones subjetivas.

### 8.2.1. Hypotheses

En el marco de trabajo de desarrollo guiado por experimentos, las hipótesis se formulan como declaraciones de creencia claras y orientadas a la acción, diseñadas con el propósito de ser probadas (*tested*) y sometidas a un proceso de falsabilidad mediante la interacción del usuario con características específicas del software. A continuación, se detallan las cinco hipótesis experimentales definidas a nivel de funcionalidades de software de **PlantCare**:

---

#### Hipótesis 1: Notificaciones de Riego Crítico vía Discord (EC-01)
Creemos que si implementamos el envío automático de notificaciones de alerta de riego crítico con diagnósticos contextuales al canal de Discord de los usuarios, entonces el tiempo promedio de respuesta ante el estado de sequedad crítica de la planta se reducirá a menos de 2 horas (120 minutos), en comparación con el tiempo promedio de 12 horas (720 minutos) observado en los usuarios que solo reciben alertas internas tradicionales en el panel de control.

#### Hipótesis 2: Valoración de Características Premium (EC-02)
Creemos que el botón informativo de planes premium asociado a la característica de "diagnósticos avanzados de salud" generará al menos el doble (2x) de la tasa de clics (CTR) en comparación con el botón informativo asociado a la característica de "retención de datos históricos por 24 meses", cuando ambos se presenten simultáneamente en la interfaz.
#### Hipótesis 3: Modo Oscuro para Monitoreo Nocturno
Creemos que si implementamos un botón de selección de modo oscuro en la barra de navegación superior de la aplicación web, entonces la duración promedio de las sesiones de los usuarios en dispositivos móviles durante la noche (de 19:00 a 06:00) aumentará en un 15%, debido a la reducción de la fatiga visual bajo condiciones de baja luminosidad.

#### Hipótesis 4: Soporte Multiidioma (i18n)
Creemos que si incorporamos un selector de idioma (inglés/español) en la cabecera de la aplicación web mediante la biblioteca i18n, entonces la tasa de abandono en las páginas de configuración para usuarios con navegadores configurados en inglés se reducirá en un 30%, al eliminar la barrera lingüística al interactuar con el sistema.

#### Hipótesis 5: Insignias de Logros por Cuidado (Gamificación)
Creemos que si mostramos una insignia virtual de "Cuidador Experto" en el perfil del usuario tras mantener todas sus plantas sin alertas críticas durante 7 días consecutivos, entonces la frecuencia de visitas semanales al panel de control aumentará en un 20%, ya que la gamificación incentiva la revisión diaria del estado de las plantas.

### 8.2.2. Domain Business Metrics

Para asegurar que los experimentos no se evalúen con base en métricas de vanidad o datos aislados, el equipo establece un conjunto de métricas de negocio del dominio de **PlantCare**. Estas métricas se definen formalmente a continuación y servirán como el único catálogo de referencia para los experimentos y el seguimiento de la plataforma.

#### 1. Tiempo Promedio de Respuesta ante Alertas Críticas (TPR)
* **Definición:** Mide la rapidez con la que un usuario interviene físicamente para regar su planta tras la emisión de una alerta por falta de humedad.
* **Fórmula de cálculo:**
  $$\text{TPR} = \frac{\sum_{i=1}^{n} (T_{\text{riego}, i} - T_{\text{alerta}, i})}{n}$$
  Donde:
  * $T_{\text{riego}, i}$ es la marca de tiempo (timestamp UTC) del registro de riego de la planta $i$ en la tabla `watering_logs` de **Supabase**.
  * $T_{\text{alerta}, i}$ es la marca de tiempo (timestamp UTC) del primer registro en la tabla `sensor_readings` que documenta un valor de humedad del suelo igual o inferior al 20% ($soil\_moisture\_pct \le 20$) para la misma planta.
  * $n$ es el número total de transiciones críticas de humedad resueltas mediante riego.
* **Técnica de recolección:** Consulta SQL de agregación en **Supabase (PostgreSQL)**, ejecutando un cruzamiento (*join*) entre las tablas `sensor_readings` y `watering_logs` agrupadas por identificador de planta (`plant_id`).
* **Meta deseada:** Lograr un $\text{TPR} \le 120$ minutos (2 horas) en el grupo expuesto a Discord.

#### 2. Tasa de Clics de Interés Premium (CTR)
* **Definición:** Porcentaje de usuarios únicos expuestos a una propuesta de valor premium que manifiestan interés activo interactuando con el botón informativo.
* **Fórmula de cálculo:**
  $$\text{CTR}_{\text{característica}} = \frac{U_{\text{clics, característica}}}{U_{\text{vistas, sección}}} \times 100\%$$
* **Técnica de recolección:** Registro de eventos de clic en Vue.js enviados a la tabla `feature_interaction_logs` de **Supabase**.
* **Meta deseada:** Obtener un $\text{CTR}_{\text{diagnostico}} \ge 10\%$ y demostrar que $\text{CTR}_{\text{diagnostico}} \ge 2 \times \text{CTR}_{\text{retencion}}$.

#### 3. Tasa de Mortalidad de Plantas (TMP)
* **Definición:** Porcentaje de plantas registradas bajo monitoreo activo que son dadas de baja o declaradas muertas.
* **Fórmula de cálculo:**
  $$\text{TMP} = \frac{P_{\text{muertas}}}{P_{\text{totales}}} \times 100\%$$
* **Técnica de recolección:** Consulta en la tabla `plants` de **Supabase**, filtrando por la columna de estado.
* **Meta deseada:** Reducir la tasa de mortalidad a un $\text{TMP} \le 3\%$.

#### 4. Duración de Sesión Nocturna en Móviles (DSN)
* **Definición:** Tiempo promedio en minutos que un usuario mantiene una sesión activa en la aplicación web en dispositivos móviles durante la noche (de 19:00 a 06:00).
* **Fórmula de cálculo:**
  $$\text{DSN} = \frac{\sum_{i=1}^{m} \text{Duración de la sesión } i}{m}$$
  Donde $m$ es el total de sesiones registradas en el rango horario establecido.
* **Técnica de recolección:** Registro de telemetría de sesiones web enviado a la tabla `user_sessions` en **Supabase**.
* **Meta deseada:** Incrementar la DSN en un 15% en el grupo expuesto al modo oscuro.

#### 5. Tasa de Abandono de Configuración (TAC)
* **Definición:** Porcentaje de usuarios con configuración de navegador en inglés que acceden a la página de configuración del perfil pero la abandonan sin guardar cambios.
* **Fórmula de cálculo:**
  $$\text{TAC} = \frac{\text{Visitas con abandono sin guardar}}{\text{Total de visitas a la sección de configuración}} \times 100\%$$
* **Técnica de recolección:** Registro de eventos de carga y abandono de la ruta `/config` a través de Google Analytics.
* **Meta deseada:** Reducir la TAC en un 30% en los usuarios con soporte de traducción i18n activo.

#### 6. Tasa de Retención Semanal de Visitas (TRV)
* **Definición:** Frecuencia promedio de días en una semana en los que un usuario ingresa a la aplicación.
* **Fórmula de cálculo:**
  $$\text{TRV} = \frac{\sum_{j=1}^{u} \text{Días de ingreso de usuario } j}{u}$$
  Donde $u$ es el total de usuarios bajo observación.
* **Técnica de recolección:** Auditoría de accesos diarios en la tabla `user_daily_access` de **Supabase**.
* **Meta deseada:** Incrementar la TRV en un 20% en el grupo expuesto al sistema de insignias de logros.

---

### 8.2.3. Measures

La selección de medidas estructuradas define de manera explícita qué datos concretos se capturarán para responder a las preguntas de investigación de cada experimento, clasificándolos en indicadores primarios (el cambio directo esperado) e indicadores secundarios (efectos colaterales o comportamiento del usuario). Adicionalmente, se aplica el principio de economía de datos para garantizar la eficiencia de la infraestructura.

#### Clasificación de Medidas por Experimento

##### Experimento 1: Notificaciones de Riego Crítico vía Discord (EC-01)
* **Medida Primaria:** Intervalo temporal en minutos transcurrido desde la lectura crítica de sensor en la base de datos hasta la inserción del log de riego en la base de datos para la misma planta.
* **Medidas Secundarias:** Clics en la URL incrustada en la notificación de Discord (`?utm_source=discord_webhook`) y nivel de satisfacción auto-reportado mediante reacciones de emoji en Discord.

##### Experimento 2: Valoración de Características Premium (EC-02)
* **Medida Primaria:** Clics únicos y eventos de carga asociados a los botones de "Ver Planes Premium" para la sección de diagnóstico de salud y la sección de retención de datos.
* **Medidas Secundarias:** Clics en el botón simulado "Comenzar Prueba de 14 Días" en la modal e intervalo de tiempo de visualización de la modal.

##### Experimento 3: Modo Oscuro para Monitoreo Nocturno
* **Medida Primaria:** Tiempo de permanencia en minutos por sesión en móviles en el rango nocturno (19:00 a 06:00).
* **Medidas Secundarias:** Frecuencia de interacción con gráficos en el panel nocturno y conteo de clics en el botón de alternancia del modo oscuro.

##### Experimento 4: Soporte Multiidioma (i18n)
* **Medida Primaria:** Conteo de cambios de idioma seleccionados y tasa de abandono en la página de configuración del perfil.
* **Medida Secundaria:** Tasa de errores en el formulario de configuración cometidos por usuarios con navegadores configurados en inglés.

##### Experimento 5: Insignias de Logros por Cuidado (Gamificación)
* **Medida Primaria:** Frecuencia de ingresos al panel de control en un período de 7 días.
* **Medidas Secundarias:** Cantidad de insignias de "Cuidador Experto" otorgadas y tasa de interacción con el widget de logros.

#### Racionalidad y Principio de Economía de Datos

El diseño de medición de **PlantCare** se rige por el principio de economía de datos para minimizar la latencia de red, evitar datos redundantes y reducir los costos de almacenamiento en la base de datos de **Supabase**:
1. **Frecuencia de Notificaciones Controlada por Transición:** Para el Experimento 1, la alerta de Discord solo se envía la primera vez que se cruza el umbral crítico del 20%, suspendiendo las notificaciones posteriores hasta que se registre un riego válido en `watering_logs`.
2. **Eventos Frontend Agregados y Batching:** Para el Experimento 2 y el Experimento 3, los eventos de clics e interacciones de sesión no se envían individualmente. Se consolidan en el almacenamiento local del navegador y se transmiten en un paquete consolidado (*batch*) al finalizar la sesión del usuario.
3. **Purga Automática de Telemetría Histórica:** Se ejecuta una tarea programada mensual en **Supabase** que elimina todas las lecturas detalladas de sensores con antigüedad mayor a 3 meses para usuarios gratuitos, manteniendo optimizados los índices de la base de datos.
4. **Cálculo Asincrónico de Logros:** Para el Experimento 5, la concesión de insignias y cálculo de metas se procesa una vez al día en segundo plano en el servidor de Supabase, evitando sobrecargar el procesamiento del lado del cliente web.

---

### 8.2.4. Conditions

Las condiciones experimentales definen el entorno operativo y los grupos a los que se exponen los usuarios para identificar la relación causa-efecto en cada hipótesis planteada.

* **Experimento 1 (Discord):**
  * *Condición Experimental ($C_E$):* Grupo piloto de 15 usuarios con la integración de Discord activa que reciben webhooks con diagnósticos contextuales al bajar la humedad del 20%.
  * *Condición de Control ($C_C$):* Grupo piloto de 15 usuarios que operan bajo el sistema tradicional, debiendo ingresar manualmente a la aplicación para notar las alertas críticas.
* **Experimento 2 (Características Premium):**
  * *Límites del Grupo de Estudio:* Exclusivo para usuarios gratuitos con más de 3 plantas y 30 días activos.
  * *Condición Experimental 1 ($C_{E1}$):* Exposición al botón "Ver Planes Premium" dentro del panel de diagnósticos detallados.
  * *Condición Experimental 2 ($C_{E2}$):* Exposición al botón "Ver Planes Premium" dentro de la sección de visualización de gráficos históricos.
* **Experimento 3 (Modo Oscuro):**
  * *Condición Experimental:* Habilitación del selector de modo oscuro (alto contraste) en la barra de navegación superior.
  * *Condición de Control:* Interfaz fija en modo claro clásica (sin opción de alternancia).
* **Experimento 4 (Soporte Multiidioma):**
  * *Condición Experimental:* Habilitación de soporte i18n con selector inglés/español y detección automática del idioma del navegador.
  * *Condición de Control:* Interfaz fija en español sin capacidad de traducción.
* **Experimento 5 (Gamificación):**
  * *Condición Experimental:* Acceso al panel lateral de logros y concesión de la insignia de "Cuidador Experto".
  * *Condición de Control:* Panel lateral básico sin insignias ni mecánicas de logros.

---

### 8.2.5. Scale Calculations and Decisions

Las decisiones de escala determinan la magnitud de cada experimento en función del nivel de certidumbre requerido y la precisión del cambio que se pretende detectar. Los parámetros estadísticos se han calibrado para equilibrar el rigor científico y la economía de datos, minimizando la recolección innecesaria de telemetría.

#### Experimento 1: Notificaciones de Riego Crítico vía Discord (EC-01)
* **Certeza (Certainty):**
  * **Nivel de Significación ($\alpha$):** 5% (0.05). Indica la probabilidad máxima aceptada de registrar un falso positivo (Error Tipo I).
  * **Poder Estadístico ($1-\beta$):** 80% (0.80). Asegura una probabilidad de detectar el impacto real si este existe (reducción de Error Tipo II).
* **Precisión y Efecto Mínimo Detectable (MDE):**
  * Se establece un MDE de una reducción de 10 horas (600 minutos) en el tiempo promedio de respuesta de riego (TPR), partiendo de la línea base histórica del grupo de control de 12 horas.
* **Decisión de Tamaño de Muestra:**
  * Asumiendo una desviación estándar histórica en la respuesta de riego de aproximadamente 8 horas ($\sigma = 480$ minutos) y aplicando una prueba de diferencia de medias con $\alpha = 0.05$ y poder de 80%, se determina que se requiere recopilar al menos 30 eventos críticos de riego por cada condición ($C_E$ y $C_C$). Con un grupo piloto de 15 usuarios por condición durante 14 días (donde se estiman 2 a 3 alertas por usuario), se cumple con la escala estadística requerida.

#### Experimento 2: Valoración de Características Premium (EC-02)
* **Certeza (Certainty):**
  * **Nivel de Significación ($\alpha$):** 5% (0.05).
  * **Poder Estadístico ($1-\beta$):** 80% (0.80).
* **Precisión y Efecto Mínimo Detectable (MDE):**
  * El MDE se define como una diferencia absoluta de 5% en la tasa de clics (CTR) entre las dos opciones expuestas.
* **Decisión de Tamaño de Muestra:**
  * Estimando una tasa de clics base (CTR) del 5% y aplicando una prueba de comparación de proporciones independientes, se determina que se requiere una exposición mínima de 150 usuarios únicos activos del plan gratuito por cada sección evaluada. Para alcanzar esto, el experimento tendrá una duración fija de 15 días continuos de observación.

#### Experimento 3: Modo Oscuro para Monitoreo Nocturno (EC-03)
* **Certeza (Certainty):**
  * **Nivel de Significación ($\alpha$):** 5% (0.05).
  * **Poder Estadístico ($1-\beta$):** 80% (0.80).
* **Precisión y Efecto Mínimo Detectable (MDE):**
  * Se establece un MDE de un incremento del 15% en la Duración de Sesión Nocturna (DSN) en móviles. Asumiendo una sesión nocturna promedio de 8 minutos, esto equivale a una mejora de 1.2 minutos.
* **Decisión de Tamaño de Muestra:**
  * Utilizando una desviación estándar histórica de la duración de sesiones móviles de 3 minutos ($\sigma = 180$ segundos) y aplicando una prueba t de Student para muestras independientes, se determina que se requiere un tamaño de muestra mínimo de 100 sesiones nocturnas registradas por cada grupo ($C_E$ y $C_C$) durante un lapso de 14 días.

#### Experimento 4: Soporte Multiidioma i18n (EC-04)
* **Certeza (Certainty):**
  * **Nivel de Significación ($\alpha$):** 5% (0.05).
  * **Poder Estadístico ($1-\beta$):** 80% (0.80).
* **Precisión y Efecto Mínimo Detectable (MDE):**
  * Se define un MDE de una reducción del 30% en la Tasa de Abandono de Configuración (TAC) para usuarios con idioma del navegador en inglés. Con una tasa de abandono histórica base del 40%, el objetivo es detectar una reducción absoluta del 12% (alcanzando una tasa del 28%).
* **Decisión de Tamaño de Muestra:**
  * Aplicando una prueba de comparación de proporciones de una cola, se determina que se requiere la participación de al menos 80 usuarios únicos que tengan el navegador configurado en inglés y accedan a la vista de configuración del perfil. Para lograr este volumen de usuarios específicos, el período de prueba se extenderá por 20 días.

#### Experimento 5: Insignias de Logros por Cuidado (EC-05)
* **Certeza (Certainty):**
  * **Nivel de Significación ($\alpha$):** 5% (0.05).
  * **Poder Estadístico ($1-\beta$):** 80% (0.80).
* **Precisión y Efecto Mínimo Detectable (MDE):**
  * Se establece un MDE de un incremento de 20% en la Tasa de Retención Semanal de Visitas (TRV). Con una frecuencia base de 3.5 ingresos por semana, esto equivale a un incremento promedio de 0.7 días.
* **Decisión de Tamaño de Muestra:**
  * Considerando una desviación estándar de la frecuencia semanal de visitas de 1.2 días ($\sigma = 1.2$) y empleando una prueba t de Student para muestras independientes, se requiere un tamaño muestral de al menos 50 usuarios activos por grupo. El experimento se ejecutará durante 14 días completos para evaluar de forma consistente la retención del hábito de cuidado.

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
   * **Método de Prueba:** Un diseño de **Botón Simulado (Prueba de Puerta Falsa / Humo (Fake Door / Smoke))** para capturar de forma directa el interés y registrar clics únicos.

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

La fase de experimentación ejecuta de forma práctica los diseños definidos para obtener los datos empíricos que darán respuesta a las hipótesis de negocio de **PlantCare**. Este proceso se integra dentro del ciclo de vida de desarrollo de la plataforma, utilizando el pipeline de entrega continua para desplegar de manera ágil los cambios en la interfaz web frontend y en los triggers del backend serverless en **Supabase**, permitiendo una recopilación controlada de eventos bajo las condiciones establecidas para las 5 hipótesis experimentales de software.

### 8.3.1. To-Be User Stories

Para dar soporte a la infraestructura de recolección de datos y ejecutar los experimentos propuestos, se incorporan al backlog cinco nuevas Historias de Usuario (*User Stories*). Estas historias siguen la misma estructura y formalidad empleadas en el Capítulo III, detallando descripciones, escenarios con lenguaje Gherkin y reglas específicas de negocio.

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

#### US-043: Activación de Modo Oscuro para Comodidad Visual (Soporte a EC-03)
* **Epic ID:** EPIC 002 (Usabilidad y Accesibilidad)
* **Story ID:** US-043
* **Título:** Activación de Modo Oscuro para Comodidad Visual
* **Descripción:** Como cuidador de plantas en exteriores, quiero cambiar la interfaz de la aplicación web a modo oscuro mediante un botón en la barra de navegación, para reducir la fatiga visual al consultar el estado de mis plantas de noche o bajo condiciones de alta luminosidad solar.
* **Criterios de Aceptación:**
  * **Escenario 01: Alternancia exitosa a modo oscuro**
    * Dado que un usuario accede al panel principal de la aplicación web
    * Cuando hace clic en el selector de modo oscuro ubicado en la barra de navegación superior
    * Entonces el tema de la aplicación cambia de forma inmediata a alto contraste oscuro y se almacena la preferencia en la memoria local del navegador (`localStorage`).
  * **Escenario 02: Registro de telemetría de la sesión nocturna**
    * Dado que un usuario inicia sesión en la aplicación móvil o web en el rango nocturno (19:00 a 06:00)
    * Cuando interactúa con el dashboard principal en modo oscuro
    * Entonces el sistema registra el tiempo de inicio y fin de la sesión nocturna y lo envía a la tabla `user_sessions` en **Supabase** de manera agregada para medir la Duración de Sesión Nocturna (DSN).
* **Reglas de Negocio:**
  * El modo oscuro debe aplicar una paleta de colores HSL consistente con alto contraste para garantizar la legibilidad en pantallas de teléfonos móviles.
  * Si la preferencia ya fue almacenada en `localStorage`, la aplicación web debe renderizar por defecto el modo oscuro en la carga inicial de la página.

---

#### US-044: Soporte de Multiidioma i18n en la Aplicación Web (Soporte a EC-04)
* **Epic ID:** EPIC 008 (Internacionalización y Localización)
* **Story ID:** US-044
* **Título:** Soporte de Multiidioma i18n en la Aplicación Web
* **Descripción:** Como usuario internacional que tiene el navegador configurado en inglés, quiero poder visualizar toda la interfaz de configuración del perfil y del dashboard en mi idioma de preferencia mediante traducción por i18n, para comprender claramente cada uno de los campos requeridos y evitar abandonar el proceso.
* **Criterios de Aceptación:**
  * **Escenario 01: Detección automática de idioma del navegador**
    * Dado que un usuario nuevo con navegador en inglés accede por primera vez a la aplicación web
    * Cuando el sistema detecta que el idioma por defecto del navegador es `en` (inglés)
    * Entonces la interfaz se renderiza automáticamente en inglés empleando las traducciones locales de la biblioteca i18n.
  * **Escenario 02: Cambio manual de idioma**
    * Dado que un usuario visualiza la interfaz de la página de configuración
    * Cuando selecciona el idioma inglés en el conmutador de la cabecera
    * Entonces el sistema actualiza la traducción de todos los textos del formulario al inglés de manera instantánea y sin recargar la página.
* **Reglas de Negocio:**
  * Toda etiqueta, botón y mensaje de validación del formulario de configuración de perfil debe contar con su correspondiente traducción en los archivos `locales/es.json` y `locales/en.json`.

---

#### US-045: Visualización de Insignia de Logros por Cuidado (Soporte a EC-05)
* **Epic ID:** EPIC 009 (Mecánicas de Gamificación)
* **Story ID:** US-045
* **Título:** Visualización de Insignia de Logros por Cuidado
* **Descripción:** Como cuidador de plantas domésticas, quiero visualizar una insignia de "Cuidador Experto" en mi panel de perfil cuando mantenga a mis plantas sanas por 7 días consecutivos, para sentirme motivado a ingresar frecuentemente a revisar su estado.
* **Criterios de Aceptación:**
  * **Escenario 01: Otorgamiento automático de insignia**
    * Dado que la base de datos registra que las plantas de un usuario no han tenido ninguna lectura de humedad del suelo menor o igual al 20% por 7 días consecutivos
    * Cuando el cron job del servidor procesa las estadísticas de cuidado diarias
    * Entonces asigna la insignia "Cuidador Experto" al usuario y guarda el logro en la tabla `user_achievements` de **Supabase**.
  * **Escenario 02: Visualización del panel de logros**
    * Dado que un usuario calificado ingresa al panel principal
    * Cuando hace clic en la pestaña "Logros" del panel lateral
    * Entonces el widget muestra la insignia virtual desbloqueada con una animación interactiva.
* **Reglas de Negocio:**
  * Si alguna planta del usuario registra un estado crítico de humedad durante el período de 7 días, el contador de días consecutivos se reinicia a cero de forma automática.
  * El widget de logros se actualizará de forma asincrónica una vez al día para no degradar el rendimiento del dashboard del cliente web.

---

### 8.3.2. To-Be Product Backlog

El Backlog de Producto se actualiza para incorporar las nuevas Historias de Usuario diseñadas para dar soporte a los experimentos. Para el Sprint To-Be 1, se priorizan las historias de experimentación (**US-041** a **US-045**) por encima de los requisitos pendientes del backlog original, con el fin de mitigar el riesgo de producto en la etapa más temprana posible del ciclo de desarrollo.

El backlog consolidado de **PlantCare** se detalla en la siguiente tabla:

| Orden | User Story ID | Título | Descripción | Story Points | Estado / Planificación |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **1** | **US-041** | Alertas contextuales de riego a Discord | Como usuario ocupado, quiero recibir alertas automáticas en Discord ante estados críticos de humedad | 8 | Priorizado (Sprint To-Be 1) |
| **2** | **US-042** | Simulación de compra de suscripción premium | Como usuario gratuito, quiero pulsar en opciones premium para registrar mi interés de compra | 5 | Priorizado (Sprint To-Be 1) |
| **3** | **US-043** | Activación de Modo Oscuro para Comodidad Visual | Como cuidador de plantas, quiero alternar a modo oscuro para mejorar la legibilidad bajo el sol o de noche | 5 | Priorizado (Sprint To-Be 1) |
| **4** | **US-044** | Soporte de Multiidioma i18n en la Aplicación Web | Como usuario internacional, quiero la interfaz en inglés para evitar errores lingüísticos | 5 | Priorizado (Sprint To-Be 1) |
| **5** | **US-045** | Visualización de Insignia de Logros por Cuidado | Como cuidador, quiero ganar medallas por la racha de plantas sanas para motivarme a ingresar | 8 | Priorizado (Sprint To-Be 1) |
| 6 | US-015 | Visualización unificada de plantas | Como usuario, quiero ver todas mis plantas en una vista consolidada con estado de salud | 8 | Completado (Sprint As-Is 1) |
| 7 | US-020 | Visualización avanzada en tiempo real | Como aficionado, quiero ver datos de sensores en tiempo real con múltiples parámetros | 8 | Completado (Sprint As-Is 1) |
| 8 | US-011 | Registro simplificado de planta | Como usuario, quiero registrar una nueva planta completando información básica esencial | 3 | Completado (Sprint As-Is 1) |
| 9 | US-012 | Edición rápida de información de planta | Como usuario, quiero editar la información básica de una planta registrada | 2 | Completado (Sprint As-Is 1) |
| 10 | US-005 | Registro de usuario | Como visitante, quiero registrarme con datos básicos protegidos para crear una cuenta segura | 5 | Completado (Sprint As-Is 1) |
| 11 | US-006 | Inicio de sesión | Como usuario registrado, quiero autenticarme de forma segura con mis credenciales | 3 | Completado (Sprint As-Is 1) |
| 12 | US-023 | Reportes de largo plazo | Como aficionado, quiero acceder a reportes mensuales y anuales consolidados | 5 | Pendiente |
| 13 | US-019 | Monitoreo detallado de humedad | Como aficionado, quiero monitorear precisamente la humedad del aire | 5 | Pendiente |
| 14 | US-040 | Sistema de confirmación y seguimiento | Como usuario, quiero confirmar cuando he aplicado riego y recibir seguimiento del historial | 5 | Pendiente |
| 15 | US-013 | Eliminación confirmada de planta | Como usuario, quiero eliminar plantas que ya no cuido | 2 | Pendiente |
| 16 | US-014 | Configuración de parámetros básicos | Como usuario, quiero definir parámetros esenciales para cada planta | 3 | Pendiente |
| 17 | US-008 | Gestión de sesión | Como usuario, quiero que mi sesión se maneje de forma segura | 2 | Pendiente |
| 18 | US-010 | Cerrar sesión | Como usuario, quiero cerrar sesión de manera definitiva | 2 | Pendiente |