<p align="center">
  <img src="https://static.wikia.nocookie.net/logopedia/images/a/aa/UPC-Logo-con-fondo-circular.png/revision/latest/scale-to-width-down/250?cb=20230305162634&path-prefix=es" width="110" alt="UPC Logo">
</p>

<h2 align="center">Universidad Peruana de Ciencias Aplicadas</h2>

<p align="center">
<b>Ingeniería de Software</b><br>
Séptimo Ciclo (07)
</p>

<p align="center">
<img src="https://img.shields.io/badge/Curso-1ASI0732--2610-blue?style=flat-square">
<img src="https://img.shields.io/badge/Sección-12316-green?style=flat-square">
<img src="https://img.shields.io/badge/Año-2026-red?style=flat-square">
</p>



<h1 align="center">📘 Informe del Trabajo Parcial</h1>

<p align="center"><i>Diseño de Experimentos de Ingeniería de Software</i></p>



<center>
<table>
<tr><td><b>Profesor</b></td><td>Julio Manuel Noriega Melendez</td></tr>
<tr><td><b>NRC</b></td><td>12316</td></tr>
<tr><td><b>Startup</b></td><td>PlantCare</td></tr>
<tr><td><b>Producto</b></td><td>PlantCare</td></tr>
</table>
</center>



<h3 align="center">👥 Integrantes</h3>

<center>
<table>
<tr>
<th>#</th>
<th>Código</th>
<th>Apellidos y Nombres</th>
</tr>

<tr><td align="center">1</td><td>U20221B657</td><td>Casaverde De La Cruz, Ernesto david</td></tr>
<tr><td align="center">2</td><td>u202223405</td><td>Juan Sebastian Estupiñán Olortegui</td></tr>
<tr><td align="center">3</td><td>u20231b842</td><td>Enrique Manuel Mantilla Maldonado</td></tr>
<tr><td align="center">4</td><td>u20231a257</td><td>Brayan Alexis Corvacho Damian</td></tr>

</table>
</center>


<p align="center">
<b>Lima, Perú • Abril 2026</b>
</p>

<p align="center">
<img src="https://img.shields.io/badge/UPC-Final%20Report-black?style=flat-square">
<img src="https://img.shields.io/badge/PlantCare-Startup-success?style=flat-square">
</p>




<div style="page-break-after: always;"></div>

### Registro de Versiones del Informe

> En esta sección se registran las modificaciones relevantes realizadas al informe durante el ciclo de vida del proyecto.

| Versión | Fecha | Autor | Descripción de modificación |
|:------:|:------:|:------|:----------------------------|
| v1.0 | 26/04/2026 | todos | Creación inicial del informe y estructura base |
| v1.2 | 27/04/2026 | todos | creacion de la lading page y web app |
| v2.2 |10/05/2026| todos | agregar las pruebas unitarias, integrales, gherkin y selenium|



<div style="page-break-after: always;"></div>

###  Project Report Collaboration Insights

#### 🔗 Repositorio del Informe

**GitHub URL:**  
https://github.com/Grupo5-PlantCare-Diseno-de-Experimentos


##### Entrega 1

###### Actividades realizadas

- Creación del repositorio del informe.
- Definición de estructura inicial en Markdown.
- Asignación de responsabilidades entre integrantes.
- Desarrollo de portada y primeras secciones.
- creacion de la lading pague y web app


##### Entrega Trabajo Parcial

###### Actividades realizadas

- Pruebas unitarias 
- Pruebas integrales
- Pruebas en selenium
- Base de datos
- Pruebas gerkin

###  Contenido

####  Tabla de Contenidos

- [Part I: As-Is Software Project](#part-i-as-is-software-project)

  - [Capítulo I: Introducción](#capítulo-i-introducción)
    - [1.1 Startup Profile](#11-startup-profile)
      - [1.1.1 Descripción de la Startup](#111-descripción-de-la-startup)
      - [1.1.2 Perfiles de integrantes del equipo](#112-perfiles-de-integrantes-del-equipo)
    - [1.2 Solution Profile](#12-solution-profile)
      - [1.2.1 Antecedentes y problemática](#121-antecedentes-y-problemática)
      - [1.2.2 Lean UX Process](#122-lean-ux-process)
        - [1.2.2.1 Lean UX Problem Statements](#1221-lean-ux-problem-statements)
        - [1.2.2.2 Lean UX Assumptions](#1222-lean-ux-assumptions)
        - [1.2.2.3 Lean UX Hypothesis Statements](#1223-lean-ux-hypothesis-statements)
        - [1.2.2.4 Lean UX Canvas](#1224-lean-ux-canvas)
    - [1.3 Segmentos objetivo](#13-segmentos-objetivo)

  - [Capítulo II: Requirements Elicitation & Analysis](#capítulo-ii-requirements-elicitation--analysis)
    - [2.1 Competidores](#21-competidores)
      - [2.1.1 Análisis competitivo](#211-análisis-competitivo)
      - [2.1.2 Estrategias y tácticas frente a competidores](#212-estrategias-y-tácticas-frente-a-competidores)
    - [2.2 Entrevistas](#22-entrevistas)
      - [2.2.1 Diseño de entrevistas](#221-diseño-de-entrevistas)
      - [2.2.2 Registro de entrevistas](#222-registro-de-entrevistas)
      - [2.2.3 Análisis de entrevistas](#223-análisis-de-entrevistas)
    - [2.3 Needfinding](#23-needfinding)
      - [2.3.1 User Personas](#231-user-personas)
      - [2.3.2 User Task Matrix](#232-user-task-matrix)
      - [2.3.3 User Journey Mapping](#233-user-journey-mapping)
      - [2.3.4 Empathy Mapping](#234-empathy-mapping)
      - [2.3.5 As-is Scenario Mapping](#235-as-is-scenario-mapping)
    - [2.4 Ubiquitous Language](#24-ubiquitous-language)

  - [Capítulo III: Requirements Specification](#capítulo-iii-requirements-specification)
    - [3.1 To-Be Scenario Mapping](#31-to-be-scenario-mapping)
    - [3.2 User Stories](#32-user-stories)
    - [3.3 Product Backlog](#33-product-backlog)
    - [3.4 Impact Mapping](#34-impact-mapping)

  - [Capítulo IV: Product Design](#capítulo-iv-product-design)
    - [4.1 Style Guidelines](#41-style-guidelines)
      - [4.1.1 General Style Guidelines](#411-general-style-guidelines)
      - [4.1.2 Web Style Guidelines](#412-web-style-guidelines)
      - [4.1.3 Mobile Style Guidelines](#413-mobile-style-guidelines)
        - [4.1.3.1 iOS Mobile Style Guidelines](#4131-ios-mobile-style-guidelines)
        - [4.1.3.2 Android Mobile Style Guidelines](#4132-android-mobile-style-guidelines)
    - [4.2 Information Architecture](#42-information-architecture)
      - [4.2.1 Organization Systems](#421-organization-systems)
      - [4.2.2 Labeling Systems](#422-labeling-systems)
      - [4.2.3 SEO Tags and Meta Tags](#423-seo-tags-and-meta-tags)
      - [4.2.4 Searching Systems](#424-searching-systems)
      - [4.2.5 Navigation Systems](#425-navigation-systems)
    - [4.3 Landing Page UI Design](#43-landing-page-ui-design)
      - [4.3.1 Landing Page Wireframe](#431-landing-page-wireframe)
      - [4.3.2 Landing Page Mock-up](#432-landing-page-mock-up)
    - [4.4 Mobile Applications UX/UI Design](#44-mobile-applications-uxui-design)
      - [4.4.1 Mobile Applications Wireframes](#441-mobile-applications-wireframes)
      - [4.4.2 Mobile Applications Wireflow Diagrams](#442-mobile-applications-wireflow-diagrams)
      - [4.4.3 Mobile Applications Mock-ups](#443-mobile-applications-mock-ups)
      - [4.4.4 Mobile Applications User Flow Diagrams](#444-mobile-applications-user-flow-diagrams)
    - [4.5 Mobile Applications Prototyping](#45-mobile-applications-prototyping)
      - [4.5.1 Android Mobile Applications Prototyping](#451-android-mobile-applications-prototyping)
      - [4.5.2 iOS Mobile Applications Prototyping](#452-ios-mobile-applications-prototyping)
    - [4.6 Web Applications UX/UI Design](#46-web-applications-uxui-design)
      - [4.6.1 Web Applications Wireframes](#461-web-applications-wireframes)
      - [4.6.2 Web Applications Wireflow Diagrams](#462-web-applications-wireflow-diagrams)
      - [4.6.3 Web Applications Mock-ups](#463-web-applications-mock-ups)
      - [4.6.4 Web Applications User Flow Diagrams](#464-web-applications-user-flow-diagrams)
    - [4.7 Web Applications Prototyping](#47-web-applications-prototyping)
    - [4.8 Domain-Driven Software Architecture](#48-domain-driven-software-architecture)
      - [4.8.1 Software Architecture Context Diagram](#481-software-architecture-context-diagram)
      - [4.8.2 Software Architecture Container Diagrams](#482-software-architecture-container-diagrams)
      - [4.8.3 Software Architecture Components Diagrams](#483-software-architecture-components-diagrams)
    - [4.9 Software Object-Oriented Design](#49-software-object-oriented-design)
      - [4.9.1 Class Diagrams](#491-class-diagrams)
      - [4.9.2 Class Dictionary](#492-class-dictionary)
    - [4.10 Database Design](#410-database-design)
      - [4.10.1 Relational/Non-Relational Database Diagram](#4101-relationalnon-relational-database-diagram)

  - [Capítulo V: Product Implementation](#capítulo-v-product-implementation)
    - [5.1 Software Configuration Management](#51-software-configuration-management)
    - [5.2 Product Implementation & Deployment](#52-product-implementation--deployment)
    - [5.3 Video About-the-Product](#53-video-about-the-product)

- [Part II: Verification, Validation & Pipeline](#part-ii-verification-validation--pipeline)

  - [Capítulo VI: Product Verification & Validation](#capítulo-vi-product-verification--validation)
    - [6.1 Testing Suites & Validation](#61-testing-suites--validation)
    - [6.2 Static testing & Verification](#62-static-testing--verification)
    - [6.3 Validation Interviews](#63-validation-interviews)
    - [6.4 Auditoría de Experiencias de Usuario](#64-auditoría-de-experiencias-de-usuario)




### Student Outcome

<div style="page-break-before: always;"></div>

## ABET – EAC - Student Outcome 4

Cada participante del equipo debe sustentar evidencia de cómo las actividades realizadas en el trabajo final han ayudado a desarrollar las dimensiones del student outcome. Por ello en esta sección debe haber una subsección por cada alumno donde éste describa por escrito la relación entre el outcome, sus dimensiones y el trabajo que ha realizado. Esto se complementa con lo reflejado en los testimonios expuestos que forman parte del video About The Team.


<table style="width:100%; border-collapse:collapse; border:1px solid #999;">
  <tr>
    <th style="width:30%; border:1px solid #999; padding:10px; vertical-align:top; background:#f4f4f4; text-align:left;">Criterio específico</th>
    <th style="width:45%; border:1px solid #999; padding:10px; vertical-align:top; background:#f4f4f4; text-align:left;">Acciones realizadas</th>
    <th style="width:25%; border:1px solid #999; padding:10px; vertical-align:top; background:#f4f4f4; text-align:left;">Conclusiones</th>
  </tr>
  <tr>
    <td style="border:1px solid #999; padding:10px; vertical-align:top;"><strong>4.c.1</strong><br>Reconoce responsabilidad ética y profesional en situaciones de ingeniería de software</td>
    <td style="border:1px solid #999; padding:10px; vertical-align:top;">
      <p><strong style="color:#333;">Casaverde De La Cruz, Ernesto David</strong><br><span style="color:#666; font-size:0.95em;">TB1</span>: Diseño de interfaz accesible y documentación técnica; enfoque en usabilidad para adultos mayores y usuarios ocupados.<br><span style="color:#666; font-size:0.95em;">TP</span>: Implementación frontend, pruebas de usabilidad y consideraciones de privacidad.</p>
      <p><strong style="color:#333;">Brayan Alexis Corvacho Damian</strong><br><span style="color:#666; font-size:0.95em;">TB1</span>: Integración de lógica de negocio y comunicación con sensores.<br><span style="color:#666; font-size:0.95em;">TP</span>: Desarrollo de endpoints seguros y manejo responsable de datos de sensores.</p>
      <p><strong style="color:#333;">Juan Sebastian Estupiñán Olortegui</strong><br><span style="color:#666; font-size:0.95em;">TB1</span>: Scripts de procesamiento y análisis de datos de sensores.<br><span style="color:#666; font-size:0.95em;">TP</span>: Calibración y validación para minimizar falsas alertas.</p>
      <p><strong style="color:#333;">Enrique Manuel Mantilla Maldonado</strong><br><span style="color:#666; font-size:0.95em;">TB1</span>: Pruebas de hardware y control de calidad.<br><span style="color:#666; font-size:0.95em;">TP</span>: Coordinación de pruebas integradas y documentación de procedimientos.</p>
    </td>
    <td style="border:1px solid #999; padding:10px; vertical-align:top;">El grupo reconoce la responsabilidad ética y profesional en el diseño, desarrollo y prueba del sistema. Se priorizó accesibilidad, seguridad y calidad de mediciones para reducir riesgos y proteger a usuarios vulnerables. Las decisiones y documentación demuestran compromiso profesional acumulado.</td>
  </tr>
  <tr>
    <td style="border:1px solid #999; padding:10px; vertical-align:top;"><strong>4.c.2</strong><br>Emite juicios informados considerando el impacto de las soluciones de ingeniería de software en contextos globales, económicos, ambientales y sociales</td>
    <td style="border:1px solid #999; padding:10px; vertical-align:top;">
      <p><strong style="color:#333;">Casaverde De La Cruz, Ernesto David</strong><br><span style="color:#666; font-size:0.95em;">TB1</span>: UI orientada a reducir fricción y pérdidas de plantas.<br><span style="color:#666; font-size:0.95em;">TP</span>: Mensajes sobre consumo y mantenimiento.</p>
      <p><strong style="color:#333;">Brayan Alexis Corvacho Damian</strong><br><span style="color:#666; font-size:0.95em;">TB1</span>: Arquitectura optimizada para reducir recursos y costos.<br><span style="color:#666; font-size:0.95em;">TP</span>: Endpoints eficientes para minimizar consumo y latencia.</p>
      <p><strong style="color:#333;">Juan Sebastian Estupiñán Olortegui</strong><br><span style="color:#666; font-size:0.95em;">TB1</span>: Análisis para optimizar notificaciones y evitar riegos innecesarios.<br><span style="color:#666; font-size:0.95em;">TP</span>: Calibración para reducir desperdicio de agua.</p>
      <p><strong style="color:#333;">Enrique Manuel Mantilla Maldonado</strong><br><span style="color:#666; font-size:0.95em;">TB1</span>: Documentación de análisis de impacto.<br><span style="color:#666; font-size:0.95em;">TP</span>: Coordinación de pruebas piloto para evaluar efectos sociales y ambientales.</p>
    </td>
    <td style="border:1px solid #999; padding:10px; vertical-align:top;">El equipo tomó decisiones informadas que consideran ahorro de recursos, accesibilidad y beneficio social. Las pruebas y análisis permitieron priorizar sostenibilidad y equidad, reduciendo impactos negativos y mejorando el valor social y económico del producto.</td>
  </tr>
</table>

---

## Subsections por integrante (evidencia personal)

### Casaverde De La Cruz, Ernesto David
Mis actividades (diseño UI, documentación e implementación frontend) contribuyeron a Outcome 4 al priorizar la accesibilidad y la claridad en la interfaz, reduciendo la posibilidad de errores del usuario y facilitando el uso por adultos mayores. Documenté decisiones de diseño y consideraciones de privacidad, lo que demuestra responsabilidad profesional y juicio informado respecto al impacto social y económico de la solución.

### Brayan Alexis Corvacho Damian
Mi trabajo en la arquitectura, integración y endpoints se enfocó en la fiabilidad, seguridad y eficiencia. Estas acciones muestran responsabilidad profesional (calidad del software) y consideraciones de impacto económico y ambiental (optimizaciones que reducen consumo y costos operativos), apoyando juicios informados del equipo.

### Juan Sebastian Estupiñán Olortegui
Desarrollé scripts y realicé la calibración de sensores y análisis de datos. Al validar lecturas y minimizar falsas alertas, contribuí a la confianza del sistema y a decisiones que reducen desperdicio de agua y esfuerzo del usuario, vinculando directamente la ingeniería con impactos ambientales y sociales.

### Enrique Manuel Mantilla Maldonado
Encargado de pruebas de hardware, control de calidad y coordinación de pruebas integradas. Mis entregas incluyeron reportes de fallas y recomendaciones para despliegue responsable, evidenciando juicios profesionales y consideración de impactos sociales y ambientales antes del lanzamiento.

---


