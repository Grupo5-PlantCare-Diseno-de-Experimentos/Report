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



<h1 align="center">📘 Informe del Trabajo Final</h1>

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
| v3.1 |10/05/2026| todos | Capítulo VIII: Experiment-Driven Development|




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
  - [Capítulo VII: DevOps Practices](#capítulo-vii-devops-practices)
    - [7.1. Continuous Integration](#71-continuous-integration)
    - [7.2. Continuous Delivery](#72-continuous-delivery)
    - [7.3. Operaciones en Producción y prácticas SRE](#73-operaciones-en-producción-y-prácticas-sre)
    - [7.4. Continuous Monitoring](#74-continuous-monitoring)
  - [Capítulo VIII: Experiment-Driven Development](#capítulo-viii-experiment-driven-development)
    - [8.1. Experiment Planning](#81-experiment-planning)
      - [8.1.1. As-Is Summary](#811-as-is-summary)
      - [8.1.2. Raw Material: Assumptions, Knowledge Gaps, Ideas, Claims](#812-raw-material-assumptions-knowledge-gaps-ideas-claims)
      - [8.1.3. Experiment-Ready Questions](#813-experiment-ready-questions)
      - [8.1.4. Question Backlog](#814-question-backlog)
      - [8.1.5. Experiment Cards](#815-experiment-cards)
    - [8.2. Experiment Design](#82-experiment-design)
      - [8.2.1. Hypotheses](#821-hypotheses)
      - [8.2.2. Domain Business Metrics](#822-domain-business-metrics)
      - [8.2.3. Measures](#823-measures)
      - [8.2.4. Conditions](#824-conditions)
      - [8.2.5. Scale Calculations and Decisions](#825-scale-calculations-and-decisions)
      - [8.2.6. Methods Selection](#826-methods-selection)
      - [8.2.7. Data Analytics: Goals, KPIs and Metrics Selection](#827-data-analytics-goals-kpis-and-metrics-selection)
      - [8.2.8. Web and Mobile Tracking Plan](#828-web-and-mobile-tracking-plan)
    - [8.3. Experimentation](#83-experimentation)
      - [8.3.1. To-Be User Stories](#831-to-be-user-stories)
      - [8.3.2. To-Be Product Backlog](#832-to-be-product-backlog)
      - [8.3.3. Pipeline-supported, Experiment-Driven To-Be Software Platform Lifecycle](#833-pipeline-supported-experiment-driven-to-be-software-platform-lifecycle)
        - [8.3.3.1. To-Be Sprint Backlogs](#8331-to-be-sprint-backlogs)
        - [8.3.3.2. Implemented To-Be Landing Page Evidence](#8332-implemented-to-be-landing-page-evidence)
        - [8.3.3.3. Implemented To-Be Frontend-Web Application Evidence](#8333-implemented-to-be-frontend-web-application-evidence)
        - [8.3.3.4. Implemented To-Be Native-Mobile Application Evidence](#8334-implemented-to-be-native-mobile-application-evidence)
        - [8.3.3.5. Implemented To-Be RESTful API and/or Serverless Backend Evidence](#8335-implemented-to-be-restful-api-andor-serverless-backend-evidence)
        - [8.3.3.6. Team Collaboration Insights](#8336-team-collaboration-insights)
      - [8.3.4. To-Be Validation Interviews](#834-to-be-validation-interviews)
        - [8.3.4.1. Diseño de Entrevistas](#8341-diseño-de-entrevistas)
        - [8.3.4.2. Registro de Entrevistas](#8342-registro-de-entrevistas)
    - [8.4. Experiment Aftermath & Analysis](#84-experiment-aftermath--analysis)
      - [8.4.1. Analysis and Interpretation of Results](#841-analysis-and-interpretation-of-results)
      - [8.4.2. Re-scored and Re-prioritized Question Backlog](#842-re-scored-and-re-prioritized-question-backlog)
    - [8.5. Continuous Learning](#85-continuous-learning)
      - [8.5.1. Shareback Session Artifacts: Learning Workflow](#851-shareback-session-artifacts-learning-workflow)
    - [8.6. To-Be Software Platform Pre-launch](#86-to-be-software-platform-pre-launch)
      - [8.6.1. About-the-Product Intro Video](#861-about-the-product-intro-video)
    - [Matriz de Evaluación Ética y de Impacto](#matriz-de-evaluación-ética-y-de-impacto)
    - [Conclusiones](#conclusiones)
    - [Video App Validation](#video-app-validation)
    - [Video About-the-Team](#video-about-the-team)
    - [Bibliografía](#bibliografía)
    - [Anexos](#anexos)

### Student Outcome

<div style="page-break-before: always;"></div>

## ABET – EAC - Student Outcome 4

Cada participante del equipo debe sustentar evidencia de cómo las actividades realizadas en el trabajo final han ayudado a desarrollar las dimensiones del student outcome. Por ello en esta sección debe haber una subsección por cada alumno donde éste describa por escrito la relación entre el outcome, sus dimensiones y el trabajo que ha realizado. Esto se complementa con lo reflejado en los testimonios expuestos que forman parte del video About The Team.


| Criterio específico | Acciones realizadas | Conclusiones |
|---|---|---|
| **4.c.1** Reconoce responsabilidad ética y profesional en situaciones de ingeniería de software | **Casaverde De La Cruz, Ernesto David** TB1: Diseño de interfaz accesible y documentación técnica; enfoque en usabilidad para adultos mayores y usuarios ocupados. TP: Implementación frontend, pruebas de usabilidad y consideraciones de privacidad. TB2: Elaboración de As-Is Summary, To-Be User Stories y apoyo en la definición del To-Be Product Backlog. **Brayan Alexis Corvacho Damian** TB1: Integración de lógica de negocio y comunicación con sensores. TP: Desarrollo de endpoints seguros y manejo responsable de datos de sensores. TB2: Desarrollo de Raw Material: Assumptions, Knowledge Gaps, Ideas, Claims y participación en Scale Calculations and Decisions. **Juan Sebastian Estupiñán Olortegui** TB1: Scripts de procesamiento y análisis de datos de sensores. TP: Calibración y validación para minimizar falsas alertas. TB2: Formulación de Experiment-Ready Questions, gestión del Question Backlog y definición de métricas para experimentación. **Enrique Manuel Mantilla Maldonado** TB1: Pruebas de hardware y control de calidad. TP: Coordinación de pruebas integradas y documentación de procedimientos. TB2: Elaboración de Experiment Cards, definición de Hypotheses, Conditions y Web and Mobile Tracking Plan. | El grupo reconoce la responsabilidad ética y profesional en el diseño, desarrollo y prueba del sistema. Se priorizó accesibilidad, seguridad y calidad de mediciones para reducir riesgos y proteger a usuarios vulnerables. Las decisiones y documentación demuestran compromiso profesional acumulado. |
| **4.c.2** Emite juicios informados considerando el impacto de las soluciones de ingeniería de software en contextos globales, económicos, ambientales y sociales | **Casaverde De La Cruz, Ernesto David** TB1: UI orientada a reducir fricción y pérdidas de plantas. TP: Mensajes sobre consumo y mantenimiento. TB2: Participación en la definición de historias To-Be orientadas a mejorar la experiencia de usuario y el valor del producto. **Brayan Alexis Corvacho Damian** TB1: Arquitectura optimizada para reducir recursos y costos. TP: Endpoints eficientes para minimizar consumo y latencia. TB2: Análisis de supuestos y decisiones experimentales para optimizar recursos y viabilidad técnica. **Juan Sebastian Estupiñán Olortegui** TB1: Análisis para optimizar notificaciones y evitar riegos innecesarios. TP: Calibración para reducir desperdicio de agua. TB2: Definición de preguntas, métricas y KPIs para evaluar el impacto funcional y operativo de la solución. **Enrique Manuel Mantilla Maldonado** TB1: Documentación de análisis de impacto. TP: Coordinación de pruebas piloto para evaluar efectos sociales y ambientales. TB2: Diseño de hipótesis, condiciones de prueba y mecanismos de seguimiento para validar impactos sociales y ambientales. | El equipo tomó decisiones informadas que consideran ahorro de recursos, accesibilidad y beneficio social. Las pruebas y análisis permitieron priorizar sostenibilidad y equidad, reduciendo impactos negativos y mejorando el valor social y económico del producto. |

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


