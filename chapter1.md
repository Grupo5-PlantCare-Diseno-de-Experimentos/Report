# Capítulo I: Presentación

## 1.1. Startup Profile

### 1.1.2 Perfiles de Integrantes del Equipo

---

#### 👤 Integrante 1

<p align="center">
  <img src="https://camo.githubusercontent.com/240cc4f5cfad98a03626304d463d59fde9918d805e8df2f6ca4ec9970814489d/68747470733a2f2f692e706f7374696d672e63632f476d4a6a513048472f45726e6573746f2d656c6567616e74652e706e67" width="140" alt="Integrante 1">
</p>

| Campo | Información |
|:--|:--|
| **Nombres y Apellidos** | Casaverde De La Cruz, Ernesto David |
| **Código de Estudiante** | U20221b657|
| **Carrera** | Ingeniería de Software |

**Resumen Profesional**

Soy un estudiante con conocimientos en desarrollo frontend, diseño de interfaces modernas y metodologías ágiles. Posee habilidades en trabajo colaborativo, organización de tareas y resolución de problemas. Puede aportar en diseño UX/UI, documentación técnica y desarrollo web.

---

#### 👤 Integrante 2

<p align="center">
  <img src="images/member2.jpg" width="140" alt="Integrante 2">
</p>

| Campo | Información |
|:--|:--|
| **Nombres y Apellidos** | ______________________________ |
| **Código de Estudiante** | U____________ |
| **Carrera** | Ingeniería de Software |

**Resumen Profesional**

lorem ips.

---

### 1.1.1. Descripción de la Startup
PlantCare fue diseñado para ayudar a los usuarios a mantener plantas saludables y reducir la carga del cuidado manual. Hemos observado que, debido a falta de tiempo y olvido, muchos usuarios pierden plantas y experimentan frustración, lo que disminuye su bienestar. ¿Cómo podríamos mejorar PlantCare para que las personas con agendas ocupadas y adultos mayores puedan mantener sus plantas vivas y saludables, midiendo el éxito por la retención de usuarios, la reducción de plantas muertas y la satisfacción percibida?

### 1.1.2. Perfiles de integrantes del equipo




## 1.2. Solution Profile

### 1.2.1 Antecedentes y problemática
Con la finalidad de poder conocer y comprender con mayor precisión las necesidades de nuestros usuarios, en este caso universitarios, hemos hecho un estudio por medio de la técnica 5w’s & 2H’s. Según el sitio web Rockcontent (2019) 5w’s & 2H’s es una de las metodologías de gestión empresarial más utilizadas. Puede aplicarse en muchos momentos, empresas y proyectos, ayuda a responder una serie de preguntas decisivas para hacer que las acciones de un negocio sean más estratégicas y precisas. Sin más preámbulos, por siguiente mostraremos la información que hemos logrado recopilar por medio de esta técnica.

| LAS 5W y 2H | Pregunta                                                | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------- | ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What?        | ¿Cuál es el problema?           | Personas (ocupadas, aficionados con varias macetas, adultos mayores) no mantienen humedades óptimas en macetas por falta de tiempo, olvido, lo que provoca plantas estresadas/muertas, frustración y pérdida de bienestar.    |
| When?       | ¿Cuándo sucede el problema?  | Ocurre de forma recurrente: durante periodos de alta ocupación laboral (semanas laborales), viajes de fin de semana, temporadas cambiantes (verano/invierno)                                                       |
| Where?       | ¿Dónde sucede el problema?        |  En hogares urbanos, departamentos, oficinas; en espacios con varios contenedores donde es difícil llevar el control manual. |
| Why?      | ¿Por qué sucede el problema?    | Por combinación de: (1) falta de tiempo/distribución de actividades, (2) falta de recordatorios prácticos, (3) desconocimiento de necesidades particulares de cada especie. |
| Who?        | ¿Qué llevara a las personas a usar nuestro producto?  | - Valor percibido (evitar plantas muertas) + comodidad (notificaciones que indican qué y cuándo regar). <br> - Adultos mayores: apoyo para mantener independencia y ocupación terapéutica. <br> - Profesionales: ahorro de tiempo y tranquilidad; aficionados: optimizar salud de colecciones.  |
| How?        | ¿En qué condiciones los clientes usaran nuestro producto?   | Clientes usarán la app y sensores en entornos con conectividad, recibirán notificaciones push; usarán la solución completa con sensores. Uso en interiores y balcones.  |
| How Much?   | ¿Con qué frecuencia o en qué cantidad se utilizará nuestro producto? |  Frecuencia: check diario/semana de humedad (dependiendo de planta). Notificaciones por maceta típicamente 1–4 veces/mes según espécie y estación. Cantidad: usuarios básicos comienzan con 1–3 macetas; aficionados 5–20 macetas; la tarifa se ajusta por maceta adicional. |         

### 1.2.2 Lean UX Process.

#### 1.2.2.1. Lean UX Problem Statements.

##### Dominio:

El sector de aplicación es cuidado doméstico de plantas mediante IoT y software móvil, con foco en hogares urbanos (departamentos y oficinas) que buscan soluciones de bajo esfuerzo para mantener plantas saludables. El enfoque combina hardware (sensores de humedad), conectividad, analítica ligera y una aplicación web y móvil para automatizar monitoreo, notificaciones y recomendaciones por especie, optimizando tiempo y reduciendo la ansiedad asociada al cuidado de plantas.

##### Segmentos de Clientes

__Segmento 1 — Personas ocupadas (departamentos / oficinas)__

- Edad aproximada: 25–55 años.
- Poseen pocas plantas (1–5) con valor decorativo.
- Necesitan soluciones “set-and-forget”: configuración simple, - notificaciones fiables, mínima interacción diaria.
- Sus principales requisitos: tranquilidad, fiabilidad y ahorro de tiempo.

__Segmento 2 — Aficionados__

- Edad aproximada: 30–65 años.
- Tienen colecciones más grandes (5–20 plantas).
- Buscan aprendizaje, datos históricos y recomendaciones por especie.
- Sus principales requisitos: perfiles de planta, gráficos/historial, y control sobre configuraciones avanzadas.

##### Puntos de Dolor (Pain Points)

- Olvido y ansiedad por el riego: los usuarios olvidan regar y temen perder plantas.
- Muertes o deterioro de plantas: pérdidas económicas y emocionales al no detectar condiciones críticas a tiempo.
- Falsas alertas / falta de confianza: notificaciones incorrectas que erosionan la confianza en el sistema.
- Sensores inexactos o defectuosos: hardware sin calibración provoca errores en decisiones.
- Tiempo y complejidad: configurar y mantener varias macetas puede ser tedioso.
- Falta de conocimiento: los usuarios no siempre saben las necesidades específicas por especie.
- Soluciones fragmentadas o caras: alternativas en el mercado son costosas, complejas o no integradas (hardware + software).

##### Brecha Identificada (Gap)

Existe una carencia de soluciones domésticas que sean integradas, confiables y asequibles, con características específicas:

- Confiabilidad técnica: sensores calibrados + algoritmos que minimicen falsas alertas.
- Experiencia simple: onboarding y configuración pensados para usuarios ocupados.
- Personalización por especie: perfiles de planta y recomendaciones accionables.
- Modelo de precios claro: planes escalables, adaptados a distintos tamaños de colección.
- Captura de feedback in-app: mecanismo sencillo para que el usuario marque alertas útiles/incorrectas y así mejorar el sistema.

##### Visión / Estrategia
Visión: Democratizar el cuidado de plantas en el hogar transformando la tecnología en una experiencia confiable y de bajo esfuerzo que entregue tranquilidad, plantas más saludables y aprendizaje progresivo.

Estrategia (resumen de iniciativas):

- Producto: desarrollar una solución hardware + app modular y de bajo costo: sensores de humedad calibrables, notificaciones inteligentes, perfiles de planta y dashboard multi-maceta.
- Calidad y confianza: priorizar QA de hardware, algoritmos de calibración y captura de feedback para mantener falsas alertas <10%.
- MVP enfocado: lanzar funciones críticas primero (conexión de primera maceta, notificaciones configurables).
- Medición y metas: alinear desarrollo con KPIs concretos: Activación ≥40% (1 maceta en 7 días), Retención ≥25% a 30 días, Conversión 3–7% a 90 días, Falsas alertas <10%.
- Monetización: modelo básico (hasta 3 macetas / Premium hasta 10 + pago por maceta extra).
- Comunidad y aprendizaje: crear canales para compartir aprendizajes, recomendaciones.


#### 1.2.2.2. Lean UX Assumptions.

Para desarrollar la aplicación PlantCare, partimos de varias suposiciones clave que guiarán nuestro proceso de diseño y desarrollo. Estas suposiciones están basadas en una comprensión inicial de las necesidades y problemas de nuestros usuarios objetivo, así como en los resultados esperados para el negocio. A medida que avanzamos en el desarrollo, estas suposiciones se validarán mediante pruebas y retroalimentación continua para asegurar que la solución propuesta cumpla con las expectativas y resuelva eficazmente los desafíos identificados.


**Features:**
- Sensores de humedad por maceta .
- Notificaciones push (alerta cuando regar).
- Perfiles de planta.
- Historial y gráficas de humedad / riegos.
- Gestión de múltiples macetas (agregar/editar).
- Planes y facturación: Básico (hasta 3 macetas), Premium (hasta 10), pago por maceta extra.

**Business Outcomes:**

Definimos nuestro éxito a través de los siguientes resultados medibles, que nos indicarán si estamos progresando hacia nuestra visión:

**1. Adquisición**

Describir si los usuarios nuevos están llegando al producto.

**Métrica principal**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Adquisición de usuarios | Número de descargas de la app o visitas a la página de registro | Por medir | Aumentar adquisición en **X%** |

---

**2. Activación**

Evaluar si los nuevos usuarios completan la primera experiencia de valor.

**Métrica 1 — Conectar primera maceta**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Usuarios que conectan ≥1 maceta en sus primeros 7 días | % de nuevos usuarios con ≥1 maceta conectada en 7 días | Por medir | **≥ 40%** |

**Métrica 2 — Completar onboarding de primera maceta**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Finalización del onboarding (configuración + calibración) | % de usuarios que completan el onboarding en su primera sesión | Por medir | Establecer tras primeras cohortes |

---

**3. Retención**

Determinar si los usuarios encuentran valor continuo en el sistema.

**Métrica 1 — Retención a 30 días**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Usuarios que revisan el estado o realizan acciones significativas | % de usuarios activados con ≥1 acción significativa a los 30 días | Por medir | **≥ 25%** |

**Métrica 2 — Acciones registradas tras una notificación**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Registro manual (ej. riego) en respuesta a una notificación | Número de acciones registradas / notificaciones enviadas | Por medir | Mejorar progresivamente (indicador de utilidad) |

**Métrica 3 — Feedback sobre alertas**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Marcado de alertas como útiles o incorrectas | % de alertas con feedback + ratio positivo/negativo | Por medir | Reducir falsas alertas y mejorar calibración |

---

**4. Monetización**

Validar si el producto genera ingresos sostenibles.

**Métrica 1 — Conversión a pago**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Usuarios que actualizan a Premium o compran una maceta extra en 90 días | % conversiones a 90 días | Por medir | **3–7%** |

**Métrica 2 — Expansión dentro de la cuenta**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Añadir macetas adicionales al sistema | Número promedio de macetas por cuenta pagada | Por medir | Incremento sostenido por cohorte |

---

**5. Remisión**

Medir si los usuarios satisfechos invitan a otros.

**Métrica principal**
| Indicador | Cómo medir | Baseline | Objetivo |
|----------|------------|-----------|----------|
| Recomendaciones de usuarios | % de usuarios que usan el mecanismo de referral; % de nuevas inscripciones por referral | Por medir | Incrementar adquisición por recomendaciones |
"""


**Users:**<br> 
#####  Segmento 1 – Personas ocupadas (departamentos/oficinas): <br> 
**Diferencias que hacen la diferencia**
- Vive en espacios urbanos (departamento u oficina) con espacio limitado para plantas.  
- Tiene 1–5 plantas con valor decorativo (no coleccionista).  
- Usa smartphone (iOS o Android) y espera experiencias móviles sencillas.  
- Tiene poco tiempo libre; valora soluciones "set-and-forget".

**Qué está tratando de lograr (metas)**
- Mantener sus plantas saludables con mínima intervención diaria.  
- Evitar la ansiedad y la culpa por olvidar regar.  
- Tener una solución confiable que demande poca configuración.

**Comportamientos actuales**
- Riega por calendario o cuando nota seco el sustrato.  
- Recurre a alarmas del teléfono o recordatorios manuales.  
- Evita apps complejas; abandona herramientas que consumen tiempo.

**Obstáculos / frustraciones**
- Falta de tiempo y disponibilidad para supervisión frecuente.  
- Notificaciones poco fiables o demasiado ruidosas (falsas alertas).  
- Sensores o dispositivos complicados de instalar o calibrar.

**Cómo quiere sentirse**
- Tranquilo(a), confiado(a) en que las plantas están bien; alivio de la carga emocional.

**Señales observables de éxito**
- Conecta al menos 1 maceta en los primeros 7 días.  
- Responde y confirma acciones ante notificaciones relevantes.  
- Reduce consultas al soporte relacionadas con alertas.  
- Usa el sistema de feedback in-app para corregir alertas (si existe).

**Criterios de reclutamiento**
- Edad aproximada: 25–55.  
- Vive en departamento o trabaja muchas horas fuera de casa.  
- Posee 1–5 plantas en su hogar/oficina.  
- Usa smartphone con apps instaladas (iOS/Android).  
- Dispuesto(a) a probar un dispositivo hardware sencillo y una app.

##### Segmento 2 – Aficionados: <br> 
**Diferencias que hacen la diferencia**
- Vive en casa o departamento con mayor espacio para plantas; colección de 5–20 macetas.  
- Tiene interés por aprender sobre especies y técnicas de cuidado.  
- Usa smartphone y, potencialmente, una tablet para revisar datos; tolera interfaces ligeramente más ricas.  
- Dispuesto(a) a invertir algo de tiempo en configurar y optimizar.

**Qué está tratando de lograr (metas)**
- Maximizar salud y supervivencia de su colección.  
- Aprender y aplicar recomendaciones por especie.  
- Comparar y analizar patrones entre macetas para mejorar prácticas.

**Comportamientos actuales**
- Busca información en foros, redes sociales o comunidad de plantas.  
- Lleva registros manuales o mentales de riego y observaciones.  
- Experimenta con distintos sustratos, tiempos y cantidades de riego.

**Obstáculos / frustraciones**
- Falta de datos históricos y contexto para entender patrones.  
- Herramientas fragmentadas (foro + app + hardware disociado).  
- Sensores imprecisos que no aportan confianza en las decisiones.

**Cómo quiere sentirse**
- Competente, empoderado y orgulloso de su colección; siente progreso y control.

**Señales observables de éxito**
- Consulta historial / gráficas al menos una vez en 30 días.  
- Añade y gestiona varias macetas desde la app.  
- Se suscribe al plan Premium o paga por macetas extras para acceder a análisis.  
- Ajusta parámetros de riego basados en insights (p. ej., cambios de umbral).

**Criterios de reclutamiento**
- Edad aproximada: 30–65.  
- Posee 5–20 plantas y lleva un interés activo en su cuidado.  
- Dispuesto(a) a probar funcionalidades de datos/historial y a compartir prácticas.  
- Participa en comunidades de plantas o busca recursos online con regularidad.



**User Outcomes & Benefits:**

##### Segmento 1 — Personas ocupadas (departamentos / oficinas)

**¿Qué está tratando de lograr?**  
- Mantener 1–5 plantas decorativas saludables con el mínimo esfuerzo y sin supervisión diaria intensiva.  
- Evitar perder plantas por olvido o por una mala interpretación de su estado.

**¿Cómo quiere sentirse durante y después del proceso?**  
- Tranquilo(a) y despreocupado(a); con confianza en que las plantas están bien cuidada(s).  
- Aliviado(a) del sentimiento de culpa o ansiedad por no regar a tiempo.

**¿Cómo nuestro producto le acerca a una meta vital o sueño?**  
- Permite al usuario dedicar su tiempo a otras tareas sabiendo que la responsabilidad del riego está delegada en un sistema fiable. Esto mejora su calidad de vida (menos estrés doméstico) y la percepción de su hogar como un espacio cuidado.

**¿Por qué buscaría PlantCare?**  
- Para dejar de preocuparse por revisar la humedad manualmente y evitar que las plantas se deterioren por descuido.  
- Porque quiere una solución “set-and-forget” fácil de configurar que ofrezca notificaciones fiables.

**¿Qué comportamiento observable indica que ha conseguido su objetivo?**  
- Conecta al menos 1 maceta en los primeros 7 días.  
- Responde a una notificación (p. ej., confirma o registra riego) cuando procede.  
- Disminución de tickets o consultas al soporte por alertas erróneas.  
- Usa el feedback in‑app para marcar alertas como útiles/incorrectas en las primeras semanas (señal de compromiso con la mejora del sistema).

**Beneficios clave para este segmento**  
- Funcional: Menor necesidad de supervisión manual; notificaciones fiables y configurables.  
- Emocional: Tranquilidad, confianza en el sistema y reducción de ansiedad.

---

##### Segmento 2 — Aficionados

**¿Qué está tratando de lograr?**  
- Cuidar una colección más amplia (5–20 plantas) con éxito, aprender sobre cada especie y mejorar sus habilidades como cuidador.  
- Acceder a datos históricos para entender patrones de riego y salud de las plantas.

**¿Cómo quiere sentirse durante y después del proceso?**  
- Competente y empoderado; sentir que está aprendiendo y mejorando como cuidador.  
- Satisfecho al ver progresos y resultados (plantas más saludables, menos muertes).

**¿Cómo nuestro producto le acerca a una meta vital o sueño?**  
- Ofrece datos y recomendaciones (perfiles por especie, historial, alertas con contexto) que transforman la experiencia en aprendizaje activo y ayudan a construir una colección saludable a largo plazo.

**¿Por qué buscaría PlantCare?**  
- Para obtener insights accionables y poder comparar evolución entre macetas/especies.  
- Para tener control y capacidad de ajustar parámetros avanzados según cada especie.

**¿Qué comportamiento observable indica que ha conseguido su objetivo?**  
- Consulta el historial o gráficas al menos una vez en 30 días.  
- Añade y gestiona varias macetas desde la app (expansión de colección).  
- Se suscribe o considera el plan Premium para obtener perfiles extendidos y análisis.  
- Usa datos históricos para cambiar comportamientos de riego/manual adjustments.

**Beneficios clave para este segmento**  
- Funcional: Perfiles por especie, historial detallado y análisis que facilitan decisiones informadas.  
- Emocional: Sensación de progreso, orgullo por la colección y reducción de la frustración al ver resultados positivos.


**User Assumptions:**

- **¿Quién es el usuario?** Segmento 1: Personas ocupadas (25-55 años) que viven en departamentos u oficinas.
Segmento 2: Aficionados (55+ años) con más plantas y interés en optimizar su cuidado.


- **¿Dónde encaja la aplicación en su vida?** Encaja como una herramienta de apoyo en su rutina diaria/semanal en el hogar. La consultan para ver el estado de las plantas y confían en sus notificaciones para actuar. Para el Segmento 2, también es una herramienta de aprendizaje.

- **¿Qué problemas tienen nuestros usuarios y como se puede resolver?** Resuelve el problema principal del olvido y la falta de tiempo para regar (Segmento 1). También soluciona la falta de conocimiento sobre las necesidades específicas de cada tipo de planta y la gestión de múltiples plantas (Segmento 2).

- **¿Dónde y cuándo es usada nuestra aplicación?**  Se usa predominantemente en el hogar. Las notificaciones se reciben y actúan durante las mañanas, tardes y fines de semana. La aplicación se consulta de forma rápida para ver estados. El Segmento 2 puede usarla con más frecuencia para revisar historiales y gráficas.

- **¿Qué características son importantes?** Para el Segmento 1: Notificaciones push precisas y facilidad de configuración. Para el Segmento 2: Perfiles de planta, historial y gráficas de humedad/riegos, y gestión de múltiples macetas.
 Para ambos: Fiabilidad y precisión de los sensores.

- **¿Cómo debe verse nuestra aplicación y como debe comportarse?** Debe tener una interfaz de usuario (UI) limpia, minimalista y mobile-first. Su comportamiento debe ser discreto (notificaciones solo cuando es necesario) y confiable. Para el Segmento 2, la UI debe permitir un acceso rápido a datos históricos.



**Business Assumptions**
- Creo que mis clientes tienen la necesidad de mantener sus plantas vivas y saludables sin que les consuma mucho tiempo o les genere estrés.

- Estas necesidades se pueden resolver con un sistema de sensores de humedad económicos, conectados a una aplicación que envíe notificaciones inteligentes y personalizadas por tipo de planta.

- Mis clientes iniciales son (o serán) profesionales ocupados de 25-55 años que viven en departamentos (Segmento 1) y adultos mayores de 55+ años aficionados a las plantas (Segmento 2).

- El valor principal que un cliente quiere obtener de mi servicio es tranquilidad y confianza de que sus plantas están siendo cuidadas correctamente.

- El cliente también puede obtener estos beneficios adicionales aprendizaje sobre el cuidado de plantas, ahorro de tiempo y dinero al no tener que reemplazar plantas muertas.

- Adquiriré a la mayoría de mis clientes a través de marketing digital (redes sociales, blogs de jardinería) y boca a boca entre comunidades de entusiastas.

- Ganaré dinero ofreciendo planes de suscripción: Básico (hasta 3 macetas), Premium (hasta 10) y un cargo adicional por maceta extra más allá de eso.

- Mi competencia principal en el mercado será aplicaciones de recordatorio genéricas, sensores de humedad de otras marcas y métodos tradicionales de cuidado.

- Les venceremos debido a una experiencia integrada y sencilla (hardware + software), notificaciones más inteligentes basadas en perfiles de planta específicos y un modelo de precios claro.

- Mi mayor riesgo de producto es que los sensores fallen o generen falsas alertas (>10%), lo que llevaría a la pérdida de confianza del usuario y a un abandono del servicio.

- Resolveremos esto a través de un riguroso control de calidad del hardware, algoritmos de calibración y una fácil reposición de sensores defectuosos.

#### 1.2.2.3. Lean UX Hypothesis Statements.
Para asegurar que nuestra solución esté alineada con las necesidades y expectativas de nuestros usuarios, hemos formulado las siguientes hipótesis utilizando el enfoque Lean UX. Este enfoque nos permitirá validar nuestras suposiciones a través de iteraciones constantes y ajustes basados en el feedback de los usuarios.


- **Creemos que** lograremos una activación mayor al 40%
**si** personas ocupadas y aficionados nuevos
**alcanzan** confianza inmediata sobre el funcionamiento del sistema tras la primera experiencia de uso
**con una demo interactiva** en la app que simula la lectura de humedad y envía una notificación de prueba.

- **Creemos que lograremos** la retención mayor al 25% a 30 días
**si** personas ocupadas (Segmento 1)
**alcanzan** tranquilidad continua y respuestas rápidas cuando reciben alertas
**con notificaciones push** calibradas por especie y contexto.

- **Creemos que lograremos** la retención mayor al 25% a 30 días
**si** aficionados (Segmento 2)
**alcanzan** mayor engagement y utilidad por disponer de datos históricos relevantes
**con historial y gráficas** de humedad/riego accesibles desde la pantalla principal.

- **Creemos que lograremos** la retención mayor al 25% a 30 días
**si** ambos segmentos
**alcanzan mayor** confianza en las alertas gracias a la mejora continua del sistema
**con un mecanismo simple de feedback** para marcar alertas “útiles / incorrectas”.

- **Creemos que lograremos** la monetización en 90 días
**si** aficionados con >5 macetas (Segmento 2)
**alcanzan** una percepción clara de valor agregado por funcionalidades avanzadas
**con un plan Premium** que incluye perfiles por especie, historial extendido y análisis avanzados.

#### 1.2.2.4. Lean UX Canvas.
<table border="1" cellpadding="10" cellspacing="0">
    <tr>
        <td><strong>Lean UX Canvas</strong></td>
        <td><strong>Fecha:</strong> 05/09/2025</td>
        <td><strong>Primera Iteración</strong></td>
    </tr>
    <tr>
        <td>
            <strong>Business Problem</strong><br>
            Nuestro servicio de cuidado doméstico de plantas fue diseñado para entregar tranquilidad a usuarios urbanos que quieren mantener plantas saludables con el mínimo esfuerzo, ofreciendo monitoreo automático, notificaciones y recomendaciones personalizadas por especie. Hemos notado que el producto no cumple con esos objetivos en la actualidad: numerosos usuarios sufren de ansiedad y olvido en relación con el riego, mueren o se deterioran las plantas debido a una detección tardía, hay escasa confianza por alertas falsas e inexactitud de los sensores, así como también fricción en la configuración para colecciones múltiples. Esto está causando que la experiencia sea percibida como poco confiable, lo que genera un mayor peso en soporte y obstáculos para activar y retener a los usuarios.¿De qué manera podríamos optimizar nuestro servicio para que los clientes de las ciudades tengan más éxito al cuidar plantas sanas y confíen en el sistema?
        </td>
        <td>
            <strong>Solutions</strong><br>
            - Sensores de humedad calibrables por maceta.<br> 
            - App móvil/web con notificaciones push inteligentes.<br> 
            - Perfiles de planta personalizados según especie.<br> 
            - Historial y gráficas de humedad/riegos.<br> 
            - Gestión de múltiples macetas (añadir, editar, eliminar).<br> 
            - Planes de suscripción escalables (Básico, Premium, extra por maceta)
        </td>
        <td>
            <strong>Business Outcomes</strong><br>
            - <strong>Activación:</strong> ≥40% conectan ≥1 maceta en los primeros 7 días.<br> 
            - <strong>Retención:</strong> ≥25% de usuarios activos a los 30 días.<br> 
            - <strong>Monetización:</strong> 3–7% conversión a plan pago en 90 días.<br> 
            - <strong>Calidad:</strong> Falsas alertas <10% del total de notificaciones.
        </td>
    </tr>
    <tr>
        <td>
            <strong>Users</strong><br>
            - <strong>Segmento 1 – Personas ocupadas (departamentos/oficinas):</strong>  profesionales con jornada completa, viven en departamentos, suelen tener 1–5 plantas decorativas. Buscan soluciones que requieran poco mantenimiento y ofrezcan tranquilidad. <br>
            - <strong>Segmento 2 – Aficionados:</strong> personas con más plantas (5–20), desean optimizar salud de sus colecciones, les interesa las recomendaciones para diferentes especies.
        </td>
        <td>
            <strong>Hypotheses</strong><br>
            - <strong>H1:</strong> Creemos que lograremos <em>una activación mayor al 40%</em> si los nuevos usuarios alcanzan confianza inmediata con el sistema al experimentar una <em>demo interactiva</em> que simula lectura y notificación de prueba.<br><br> 
            - <strong>H2:</strong> Creemos que lograremos <em>la retención mayor al 25% a 30 días</em> si las personas ocupadas experimentan <em>tranquilidad</em> al recibir <em>notificaciones push precisas y calibradas por especie</em>.<br><br> 
            - <strong>H3:</strong> Creemos que lograremos <em>la retención mayor al 25% a 30 días</em> si los aficionados obtienen <em>valor continuo</em> a través de <em>historiales y gráficas de humedad/riego</em> accesibles desde la pantalla principal.<br><br> 
            - <strong>H4:</strong> Creemos que lograremos <em>confianza sostenida</em> y retención si ambos segmentos pueden <em>marcar alertas útiles o incorrectas</em> mediante un mecanismo de feedback simple.<br><br> 
            - <strong>H5:</strong> Creemos que lograremos <em>la monetización en 90 días</em> si los usuarios con >5 macetas perciben <em>valor agregado</em> en un <em>plan Premium</em> con perfiles por especie y análisis avanzados. </td>
        </td>
        <td>
            <strong>User Outcomes & Benefits</strong><br> 
            1. Tranquilidad y paz mental — Eliminar la ansiedad por el riego mediante notificaciones fiables y reducción de incertidumbre.<br> 
            2. Plantas más saludables y menos pérdidas — Aumentar la supervivencia y el vigor de las plantas; menos reemplazos costosos o decepcionantes.<br> 
            3. Ahorro de tiempo y esfuerzo — Automatizar la monitorización y reducir tareas manuales repetitivas.<br> 
            4. Aprendizaje y mejora continua — Proveer información y recomendaciones que permiten a los usuarios mejorar su habilidad de cuidado con el tiempo.<br> 
        </td>
    </tr>
    <tr>
        <td>
            <strong>What's the most important thing we need to learn first?</strong><br>
            - Si los usuarios confían en las alertas y las perciben como útiles.<br> 
            - Si las notificaciones generan acciones reales (riego, revisión).<br> 
            - Si existe disposición a pagar por planes con funciones avanzadas.
        </td>
        <td colspan="2">
            <strong>What's the least amount of work we need to do to learn the next most important?</strong><br>
            Ejecutar un experimento mínimo:<br> 
            1) Landing page + oferta pre-order para medir interés y willingness-to-pay;<br> 
            2) Contar con ~20 usuarios por 4 semanas para validar que las alertas generan acción; <br>
            3) Prototipo y notificaciones para tests moderados (8–12 sesiones).<br> 
            Estos tres pasos requieren bajo coste y entregan la evidencia clave para decidir invertir en hardware.
        </td>
    </tr>
</table>




## 1.3. Segmentos objetivo

Para asegurar el éxito de PlantCare, hemos identificado dos segmentos clave que serán el foco principal de nuestras estrategias de desarrollo y marketing. Estos segmentos representan a nuestros usuarios ideales y nos permitirán adaptar nuestras funcionalidades y servicios a sus necesidades específicas, maximizando así el impacto de la plataforma.

- **Segmento 1 – Personas ocupadas (departamentos/oficinas):** La mayoría de las personas en este grupo son expertos con edades comprendidas entre 25 y 45 años, que viven en zonas urbanas de Lima Metropolitana y están empleados a tiempo completo o mediante un modelo híbrido. Generalmente poseen entre una y cinco plantas ornamentales en sus hogares o lugares de trabajo, que utilizan para embellecer sus entornos, reducir el estrés y fomentar su bienestar. Sin embargo, su vida apresurada y la falta de tiempo dificultan el adecuado cuidado de las plantas, lo que les genera frustración cuando estas se deterioran o mueren.

- **Segmento 2 – Aficionados:** Esta agrupación está compuesta por personas de entre 30 y 65 años, quienes muestran un interés constante en la jardinería y el cuidado de las plantas en casa. Su meta es optimizar el bienestar de sus plantas, lo cual se logra mediante la regulación de elementos como la humedad, el tipo de suelo y la frecuencia del riego. Poseen entre 5 y 20 plantas de distintas especies. <br> Este público tiene una preferencia especial por los atributos avanzados: personalización por especie, gráficos de comparación, sugerencias basadas en información y registros de riego. Para ellos, PlantCare es una herramienta tecnológica que mezcla la precisión, el aprendizaje constante y un manejo eficiente de sus recursos y tiempo.