# Capítulo IV: Solution UI/UX Design

## 4.1. Style Guidelines.

Esta sección presenta los estilos establecidos, con el fin de garantizar consistencia, claridad y una experiencia de usuario coherente en todas las plataformas y componentes desarrollados.

### 4.1.1. General Style Guidelines.

**Branding**
El branding de PlantCare está diseñado para transmitir bienestar, conexión con la naturaleza y compromiso con el cuidado responsable de las plantas. La identidad visual busca reflejar un enfoque accesible, amigable y moderno, orientado a personas que desean mejorar el mantenimiento de sus plantas. Se utilizan elementos gráficos que evocan crecimiento, frescura y equilibrio, creando una imagen visual clara, cercana y fácil de reconocer para usuarios interesados en el autocuidado verde y la sostenibilidad.

**Typography**
La tipografía seleccionada es Raleway, una fuente moderna, estilizada y de alta legibilidad, que aporta un aspecto limpio y profesional a la interfaz. Se utilizará Raleway tanto para encabezados como para el cuerpo de texto, aprovechando su versatilidad en diferentes pesos tipográficos para marcar jerarquías visuales claras. Esta elección equilibra seriedad y cercanía, manteniendo una apariencia accesible y coherente con el enfoque natural y amigable de la aplicación PlantCare.

[![raleway.png](https://i.postimg.cc/GhX13nJC/raleway.png)](https://postimg.cc/1fVdMTjC)

**Colors**
La paleta de colores de PlantCare está compuesta por tonos verdes y cremas, cuidadosamente seleccionados para reflejar la esencia de la naturaleza y promover una experiencia visual tranquila y confiable. Los tonos verdes comunican frescura, crecimiento y sostenibilidad, valores clave en el cuidado de plantas. Por su parte, los tonos crema aportan calidez y naturalidad, generando una conexión visual con la tierra y el entorno orgánico. Estos colores se aplicarán estratégicamente en la interfaz para garantizar una experiencia armoniosa, accesible y agradable en todo tipo de dispositivos, especialmente móviles.

[![Plant-Care.png](https://i.postimg.cc/2ymfyzKd/Plant-Care.png)](https://postimg.cc/WF9yfPjt)

**Spacing**
Se utilizó un espaciado apropiado en toda la interfaz para evitar la saturación de elementos, facilitando así una navegación clara y confortable. Los márgenes y las separaciones entre los distintos componentes se planificaron con detalle para lograr un diseño armónico y bien estructurado.

**Tono de Comunicación y Lenguaje Aplicado:**
El tono de comunicación de PlantCare será cercano, motivador y accesible, con el objetivo de acompañar al usuario en el cuidado de sus plantas de forma clara y positiva. El lenguaje aplicado se define como divertido, casual, respetuoso y entusiasta.
Este enfoque busca generar una experiencia amigable, que inspire confianza y fomente una conexión genuina con la naturaleza a través del uso cotidiano de la aplicación.

### 4.1.2. Web, Mobile and IoT Style Guidelines.

El diseño web, mobile e IoT de PlantCare seguirá principios de accesibilidad, usabilidad y consistencia visual, asegurando que la experiencia del usuario sea clara y fluida en distintos dispositivos.

**Colores**  
Se utilizará la misma paleta de colores definida para la identidad de PlantCare, garantizando coherencia.

| Nombre       | Código HEX | Muestra                                                                                   |
| ------------ | ---------- | ----------------------------------------------------------------------------------------- |
| Primary     | #8AC73D    | ![#8AC73D](https://img.shields.io/badge/-8AC73D-8AC73D?style=flat-square&logoColor=white) |
| Secondary   | #85B88F    | ![#85B88F](https://img.shields.io/badge/-85B88F-85B88F?style=flat-square&logoColor=white) |
| Accent       | #03383C    | ![#03383C](https://img.shields.io/badge/-03383C-03383C?style=flat-square&logoColor=white) |
| Background  | #F7F7ED    | ![#F7F7ED](https://img.shields.io/badge/-F7F7ED-F7F7ED?style=flat-square&logoColor=black) |
| Text | #002933    | ![#002933](https://img.shields.io/badge/-002933-002933?style=flat-square&logoColor=white) |

#### **Tipografía**

- Encabezados: **Raleway** (Google Fonts).
- Cuerpo de texto: **Raleway** (Google Fonts).
- Jerarquía visual clara mediante tamaños escalonados (H1-H6).

#### **Componentes UI**

- **Sidebar** en la parte izquierda con enlaces principales.
- **Cards** para mostrar información de plantas.
- **Gráficos** para análisis.

#### **Interacciones**

- **Animaciones ligeras** para transiciones de secciones.
- **Diseño responsive** adaptando las vistas a pantallas pequeñas.

## 4.2. Information Architecture.

La arquitectura de la información es un elemento clave en el diseño y la facilidad de uso de un sistema digital. Cuando se implementa adecuadamente, facilita que los usuarios localicen, comprendan y utilicen el contenido de forma clara y eficaz.

### 4.2.1. Organization Systems.

Los sistemas de organización en PlantCare estructuran la información y funcionalidades para que la navegación sea intuitiva, eficiente y centrada en el usuario. Se aplican los siguientes enfoques:

- **Jerárquica (Visual Hierarchy):** Se destacan primero los elementos clave como métricas generales (total de plantas, alertas activas, humedad promedio) y un llamado a la acción claro para la siguiente planta que necesita riego. Este orden visual prioriza las tareas más importantes y urgentes para el usuario.

- **Secuencial (Step-by-step):** Aplicado en flujos como agregar una nueva planta, donde el usuario sigue un proceso guiado para ingresar datos como nombre, tipo, frecuencia de riego y sensor vinculado, de forma progresiva y ordenada.

- **Por Tópicos:** Visible en secciones como Settings, donde las opciones están agrupadas por funciones (perfil, suscripción, dispositivos). Este tipo de organización también se refleja en History, donde las acciones se ordenan cronológicamente.

- **Según Audiencia:** Funciones como la gestión de sensores o los planes de suscripción premium están pensadas para usuarios más avanzados o con múltiples dispositivos, adaptando así la experiencia según el perfil y nivel de compromiso del usuario.

[![image.png](https://i.postimg.cc/wB0jZx9R/image.png)](https://postimg.cc/hJQn7BzK)

### 4.2.2. Labeling System.

Se han implementado etiquetas **claras, concisas y accesibles** para representar funciones específicas. Estas etiquetas permiten a los usuarios identificar rápidamente cada sección o acción, sin necesidad de interpretación adicional ni conocimientos técnicos.

Las principales etiquetas utilizadas son:

- **Dashboard**
- **Plants**
- **Add plant**
- **History**
- **Analytics**
- **Community**
- **Configuration**
- **Edit/Delete plant**

Cada etiqueta está acompañada por íconos representativos que refuerzan visualmente su propósito, facilitando la navegación especialmente en interfaces móviles. El enfoque es directo y amigable, asegurando que tanto usuarios principiantes como avanzados puedan comprender rápidamente las funcionalidades disponibles.

### 4.2.3. SEO Tags and Meta Tags

Para optimizar la visibilidad en motores de búsqueda, se proponen las siguientes etiquetas:

- **Title:** PlantCare – Cuidado inteligente de tus plantas
- **Meta Tags Description:** Plataforma digital para el monitoreo y cuidado automatizado de plantas mediante sensores, alertas y recomendaciones personalizadas.
- **Keywords:** plantas, jardinería, sensores de humedad, riego inteligente, cuidado de plantas, app de plantas, PlantCare
- **Author:** PlantCare

### 4.2.4. Searching Systems.

Los sistemas de búsqueda permiten a los usuarios localizar información específica de manera rápida y eficiente dentro de la aplicación, optimizando el acceso al contenido y mejorando la experiencia de uso.

- **Filtros personalizados** para el historial y planta.
- **Resultados presentados con etiquetas visuales**, íconos e información clave como el nombre de la planta, humedad actual, último riego y estado general.

Estos sistemas hacen que la gestión y monitoreo de las plantas sea más ágil, especialmente útil para usuarios con múltiples registros o sensores activos.

### 4.2.5. Navigation Systems.

Este punto define los sistemas de navegación implementados, los cuales permiten a los usuarios desplazarse de forma fluida y coherente por la aplicación en sus versiones web y mobile. Se prioriza la simplicidad, la accesibilidad y la visibilidad de funciones clave.

#### **Web**

- **Barra de navegación lateral (Sidebar):** Ubicada en la parte lateral, permite el acceso rápido a secciones como *Dashboard*, *Plantas*, *Historial* y *Configuración*. Este diseño mantiene visible lo esencial, incluso al hacer scroll.

- **Enlaces jerárquicos:** Se implementan rutas jerárquicas que permiten al usuario avanzar o retroceder sin perder el contexto.

- **Flujos de usuario optimizados:** Tareas como agregar una planta o configurar un sensor siguen una estructura paso a paso, reduciendo errores y facilitando el proceso, especialmente para nuevos usuarios.

- **Indicadores visuales y estados activos:** El sistema resalta con claridad el menú activo y utiliza iconos e indicadores visuales para reforzar la sección actual, mejorando la orientación dentro del sistema.

#### **Mobile**

- **Navegación inferior (Bottom Navigation):** Presente como barra fija con accesos directos a secciones clave como *Dashboard*, *Plantas*, *Historial* y *Configuración*.
- **Enlaces jerárquicos:** Permiten regresar o avanzar entre pantallas sin perder contexto, especialmente dentro del detalle de cada planta.
- **Flujos guiados:** Acciones como añadir una planta o revisar su historial están divididas en pasos claros y secuenciales.
- **Indicadores visuales:** Estados activos e íconos se usan para mantener siempre visible la ubicación actual del usuario.

## 4.3. Landing Page UI Design.

### 4.3.1. Landing Page Wireframe.

En esta sección, se presentan los wireframes de la Landing Page, hechas en la herramienta Figma.

[https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=45-2&t=GWLix1KIlk7wI6Qs-1](https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=45-2&t=GWLix1KIlk7wI6Qs-1)

[![Landing-Page-Wireframe.png](https://i.postimg.cc/zG3tr4XW/Landing-Page-Wireframe.png)](https://postimg.cc/Mn2b0PBK)

### 4.3.2. Landing Page Mock-up.

En esta sección, se presentan los mock-ups de la Landing Page, hechas en la herramienta Figma.

[https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=45-2&t=GWLix1KIlk7wI6Qs-1](https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=45-2&t=GWLix1KIlk7wI6Qs-1)

[![Landing-Page-Mock-Up.png](https://i.postimg.cc/hvLFV67L/Landing-Page-Mock-Up.png)](https://postimg.cc/zHGPrd7v)


<div style="page-break-after: always;"></div>


## 4.4. Applications UX/UI Design.

### 4.4.1. Applications Wireframes.

En esta sección, se presentan los wireframes de la aplicación web y mobile, en la herramienta Figma.

#### Web Application Wireframes

[https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=13-3315&t=3nMXXTmW2i14bcze-1](https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=13-3315&t=3nMXXTmW2i14bcze-1)

Onboarding:

[![Onboard-Wireframe.png](https://i.postimg.cc/prNmfDKW/Onboard-Wireframe.png)](https://postimg.cc/ygP6B3ft)

Sign Up:

[![Sign-Up-Wireframe.png](https://i.postimg.cc/2yFSngzk/Sign-Up-Wireframe.png)](https://postimg.cc/rRsTkn03)

Sign In:

[![Sign-In-Wireframe.png](https://i.postimg.cc/ZR6bhXv8/Sign-In-Wireframe.png)](https://postimg.cc/tZCQ3S5J)

Dashboard:

[![Dashboard-Wireframe.png](https://i.postimg.cc/QtZDSSYM/Dashboard-Wireframe.png)](https://postimg.cc/5HgD25Vc)

Plant Grid:

[![Plant-Grid-Wireframe.png](https://i.postimg.cc/NFNWhxDV/Plant-Grid-Wireframe.png)](https://postimg.cc/8f6yMMqB)

Plant Detail:

[![Plant-Detail-Wireframe.png](https://i.postimg.cc/QCMyqTDf/Plant-Detail-Wireframe.png)](https://postimg.cc/1nLBm4fF)

History:

[![History-Wireframe.png](https://i.postimg.cc/25F9Rw8y/History-Wireframe.png)](https://postimg.cc/yJxy00RC)

Analytics:

[![Analytics-Wireframe.png](https://i.postimg.cc/TP90H3tX/Analytics-Wireframe.png)](https://postimg.cc/XZr9r4M2)

Community:

[![Community-Wireframe.png](https://i.postimg.cc/d3zjTkVC/Community-Wireframe.png)](https://postimg.cc/sG4hk2Sf)

Settings:

[![Settings-Wireframe.png](https://i.postimg.cc/wB6cJdgq/Settings-Wireframe.png)](https://postimg.cc/QFyTGwFw)

#### Mobile Application Wireframes

[https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=8-2270&t=3nMXXTmW2i14bcze-1](https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=8-2270&t=3nMXXTmW2i14bcze-1)

Onboarding:

[![Onboarding-Wireframe.png](https://i.postimg.cc/G3YQZK7y/Onboarding-Wireframe.png)](https://postimg.cc/6TWnZrNW)

Sign Up:

[![Sign-Up-Wireframe-1.png](https://i.postimg.cc/T3Hgy5N6/Sign-Up-Wireframe-1.png)](https://postimg.cc/QF1HwCr4)

Sign In:

[![Sign-In-Wireframe-1.png](https://i.postimg.cc/wvNXYxVx/Sign-In-Wireframe-1.png)](https://postimg.cc/xJ0JK2hB)

Dashboard:

[![Dashboard-Wireframe-1.png](https://i.postimg.cc/d3BTJhZm/Dashboard-Wireframe-1.png)](https://postimg.cc/GT4pQhQH)

Plant Grid:

[![Plant-Grid-Wireframe-1.png](https://i.postimg.cc/T2CwYM4X/Plant-Grid-Wireframe-1.png)](https://postimg.cc/F1kmDqvP)

Plant Detail:

[![Plant-Detail-Wireframe-1.png](https://i.postimg.cc/qvCMLtMB/Plant-Detail-Wireframe-1.png)](https://postimg.cc/rDMkywr7)

Analytics:

[![Analytics-Wireframe-1.png](https://i.postimg.cc/4dwJdVwy/Analytics-Wireframe-1.png)](https://postimg.cc/8JJ8XF2g)

Community:

[![Community-Wireframe-1.png](https://i.postimg.cc/28gCVzdt/Community-Wireframe-1.png)](https://postimg.cc/HV0qN1h0)

History:

[![History-Wireframe-1.png](https://i.postimg.cc/jj4TgyV0/History-Wireframe-1.png)](https://postimg.cc/mckKhz28)

Settings:

[![Settings-Wireframe-1.png](https://i.postimg.cc/Xqp0Yhrp/Settings-Wireframe-1.png)](https://postimg.cc/WqLy956v)

### 4.4.2. Applications Wireflow Diagrams.

Los Wireflows se utilizan principalmente en el diseño UX y especialmente para aplicaciones que involucran flujos de trabajo e interacciones complejas.

Se pueden visualizar mediante el siguiente link: [https://lucid.app/lucidchart/4b0365af-e9c3-4dd8-8d37-09ced3e121e4/edit?viewport_loc=-1088%2C-2778%2C3837%2C1752%2C0_0&invitationId=inv_ebe9c381-d702-46b5-83d5-7a684d13a341](https://lucid.app/lucidchart/4b0365af-e9c3-4dd8-8d37-09ced3e121e4/edit?viewport_loc=-1088%2C-2778%2C3837%2C1752%2C0_0&invitationId=inv_ebe9c381-d702-46b5-83d5-7a684d13a341)

**User Goal 1:** Como usuario, quiero acceder a la sección de Analytics, identificar una tendencia, ingresar al Historial para confirmar el insight, y ajustar y guardar exitosamente el nuevo umbral de riego, con el fin de optimizar el cuidado de mis plantas según las condiciones detectadas.

[![Application-User-Flow-Diagram.png](https://i.postimg.cc/ncvVJmmX/Application-User-Flow-Diagram.png)](https://postimg.cc/zVGYwV0N)

**User Goal 2:** Como usuario, quiero ingresar a la vista de Comunidad, y ayudar a otro usuario, con el fin de darles confianza y ampliar sus conocimientos sobre el cuidado de plantas.

[![Application-User-Flow-Diagram-1.png](https://i.postimg.cc/501qsL8S/Application-User-Flow-Diagram-1.png)](https://postimg.cc/Ty7Dwy7K)

**User Goal 3:** Como usuario, quiero navegar a la sección de Configuración y modificar los ajustes, con el fin de adaptar la aplicación perfectamente a mis necesidades.

[![Application-User-Flow-Diagram-2.png](https://i.postimg.cc/TY8m0rGp/Application-User-Flow-Diagram-2.png)](https://postimg.cc/7bVfYJ5k)

### 4.4.3. Applications Mock-ups.

En esta sección, se presentan los wireframes de la aplicación web y mobile, en la herramienta Figma.

#### Web Application Mock-ups

[https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=13-3315&t=3nMXXTmW2i14bcze-1](https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=13-3315&t=3nMXXTmW2i14bcze-1)

Onboarding:

[![Onboard-Mock-Up.png](https://i.postimg.cc/VkYW0bqV/Onboard-Mock-Up.png)](https://postimg.cc/RNDHjhb7)

Sign Up:

[![Sign-Up-Mock-Up.png](https://i.postimg.cc/vHSZfJfs/Sign-Up-Mock-Up.png)](https://postimg.cc/7f04yQKs)

Sign In:

[![Sign-In-Mock-Up.png](https://i.postimg.cc/wTtp87yh/Sign-In-Mock-Up.png)](https://postimg.cc/TpXSn2cw)

Dashboard:

[![Dashboard-Mock-Up.png](https://i.postimg.cc/5y6ZVR0v/Dashboard-Mock-Up.png)](https://postimg.cc/N2vN8bRf)

Plant Grid:

[![Plant-Grid-Mock-Up.png](https://i.postimg.cc/rwM2dgYT/Plant-Grid-Mock-Up.png)](https://postimg.cc/JG62v3LY)

Plant Detail:

[![Plant-Detail-Mock-Up.png](https://i.postimg.cc/BQZCXKc1/Plant-Detail-Mock-Up.png)](https://postimg.cc/QVP7zVzX)

History:

[![History-Mock-Up.png](https://i.postimg.cc/B6GTsLnm/History-Mock-Up.png)](https://postimg.cc/mc8zyrM9)

Analytics:

[![Analytics-Mock-Up.png](https://i.postimg.cc/gk0vYPmy/Analytics-Mock-Up.png)](https://postimg.cc/yWwJPtJW)

Community:

[![Community-Mock-Up.png](https://i.postimg.cc/jSpNQwNv/Community-Mock-Up.png)](https://postimg.cc/SnGJmKgz)

Settings:

[![Settings-Mock-Up.png](https://i.postimg.cc/d1ByY5XP/Settings-Mock-Up.png)](https://postimg.cc/8J75dmNt)

#### Mobile Application Mock-ups

[https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=8-2270&t=3nMXXTmW2i14bcze-1](https://www.figma.com/design/soYjRW2pgWZNuAPj7SOik1/PlantCare?node-id=8-2270&t=3nMXXTmW2i14bcze-1)

Onboarding:

[![Onboard-Mock-Up.png](https://i.postimg.cc/CM9qVnrK/Onboard-Mock-Up.png)](https://postimg.cc/K4rzPj2X)

Sign Up:

[![Sign-Up-Mock-Up-1.png](https://i.postimg.cc/PJ1rHJNW/Sign-Up-Mock-Up-1.png)](https://postimg.cc/H8sC4pwn)

Sign In:

[![Sign-In-Mock-Up-1.png](https://i.postimg.cc/W3ksmzGQ/Sign-In-Mock-Up-1.png)](https://postimg.cc/PpdGTtF4)

Dashboard:

[![Dashboard-Mock-Up-1.png](https://i.postimg.cc/tTWHZfbv/Dashboard-Mock-Up-1.png)](https://postimg.cc/bDy54ms1)

Plant Grid:

[![Plant-Grid-Mock-Up-1.png](https://i.postimg.cc/FsDXL0t7/Plant-Grid-Mock-Up-1.png)](https://postimg.cc/tYZc0Z4j)

Plant Detail:

[![Plant-Detail-Mock-Up-1.png](https://i.postimg.cc/kGqLVgf6/Plant-Detail-Mock-Up-1.png)](https://postimg.cc/bDVmWpXp)

Analytics:

[![Analytics-Mock-Up-1.png](https://i.postimg.cc/fRG2tbby/Analytics-Mock-Up-1.png)](https://postimg.cc/5XmqDbLW)

Community:

[![Community-Mock-Up-1.png](https://i.postimg.cc/XYCxb0kT/Community-Mock-Up-1.png)](https://postimg.cc/3k32XV6B)

History:

[![History-Mock-Up-1.png](https://i.postimg.cc/QNYqKM6P/History-Mock-Up-1.png)](https://postimg.cc/grRhfGbq)

Settings:

[![Settings-Mock-Up-1.png](https://i.postimg.cc/50Nw6c8L/Settings-Mock-Up-1.png)](https://postimg.cc/2qMqKKcS)

### 4.4.4. Applications User Flow Diagrams.

Los User Flows que se muestran a continuación ilustran los trayectos principales que un usuario puede tomar dentro de la aplicación, desde el ingreso inicial hasta la adaptación de su experiencia. Cada flujo está diseñado para cumplir un objetivo particular del usuario (User Goal) y se fundamenta en los mockups creados.

Se pueden visualizar mediante el siguiente link: [https://lucid.app/lucidchart/4b0365af-e9c3-4dd8-8d37-09ced3e121e4/edit?viewport_loc=-8930%2C3000%2C4391%2C2005%2C0_0&invitationId=inv_ebe9c381-d702-46b5-83d5-7a684d13a341](https://lucid.app/lucidchart/4b0365af-e9c3-4dd8-8d37-09ced3e121e4/edit?viewport_loc=-8930%2C3000%2C4391%2C2005%2C0_0&invitationId=inv_ebe9c381-d702-46b5-83d5-7a684d13a341)

**User Goal 1:** Como usuario, quiero aprovechar los datos de la aplicación para optimizar la configuración de cuidado y prevenir futuros problemas de salud en mis plantas, con el fin de mantenerlas sanas y en óptimas condiciones.

[![Application-User-Flow-Diagram-6.png](https://i.postimg.cc/VN1jXHTV/Application-User-Flow-Diagram-6.png)](https://postimg.cc/68j2XzBr)

**User Goal 2:** Como usuario, quiero compartir mis experiencias y resultados de jardinería dentro de una comunidad, para recibir consejos valiosos de otros usuarios, resolver problemas de forma colaborativa y ganar confianza en el uso de la tecnología IoT para el cuidado de plantas.

[![Application-User-Flow-Diagram-7.png](https://i.postimg.cc/pTShc0Gh/Application-User-Flow-Diagram-7.png)](https://postimg.cc/cgQLrcN0)

**User Goal 3:** Como usuario, quiero que mis plantas alcancen una salud óptima con el mínimo esfuerzo posible, con el fin de mantenerlas siempre en las mejores condiciones sin dedicar demasiado tiempo.

[![Application-User-Flow-Diagram-8.png](https://i.postimg.cc/JzQQYLbR/Application-User-Flow-Diagram-8.png)](https://postimg.cc/zyVW34tQ)

<div style="page-break-after: always;"></div>

## 4.5. Applications Prototyping.

La presente sección incluye los prototipos interactivos desarrollados para representar el comportamiento funcional de la aplicación web y móvil en sus principales vistas. Estos prototipos permiten simular la navegación entre pantallas, validar la lógica de interacción y evaluar la experiencia del usuario antes del desarrollo final.

- Criterios de interacción seleccionados:

    -   Se implementó un sistema de navegación lateral persistente, que permite desplazarse fácilmente entre secciones clave como Dashboard, Plants, History, Analytics, Community y Settings.

    -   Las interacciones siguen un enfoque jerárquico y contextual, facilitando tareas esenciales como revisar el estado de las plantas, visualizar alertas o acceder al historial de riegos.

- Relación con la arquitectura de información:

    -   Sistema de navegación: jerárquico en la versión de escritorio, con un menú lateral que utiliza íconos y etiquetas textuales claras para mejorar la orientación del usuario.

    -   Organización visual: disposición del contenido en tarjetas informativas, uso de métricas destacadas (como humedad promedio o temperatura) y segmentación de secciones para facilitar la comprensión inmediata.

### Web Application Prototype

[![image.png](https://i.postimg.cc/qqSKHrVC/image.png)](https://postimg.cc/Fkbz02K9)

Link del video demostrativo en Microsoft Stream: [https://upcedupe-my.sharepoint.com/:v:/g/personal/u20221c362_upc_edu_pe/ER2OB0Z8ZYxDgETMe6z03FcBhrHLRdMAlxvaR94OMswaEw?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&e=gPzSG4](https://upcedupe-my.sharepoint.com/:v:/g/personal/u20221c362_upc_edu_pe/ER2OB0Z8ZYxDgETMe6z03FcBhrHLRdMAlxvaR94OMswaEw?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&e=gPzSG4)

### Mobile Application Prototype

[![image.png](https://i.postimg.cc/0jVvHY3T/image.png)](https://postimg.cc/MffgnQz5)

Link del video demostrativo en Microsoft Stream: [https://upcedupe-my.sharepoint.com/:v:/g/personal/u20221c362_upc_edu_pe/EUp1F0x6NpNGmcnWxEIED-kBeIuYPmkCeOyooGUNKfKkWQ?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&e=jAriNx](https://upcedupe-my.sharepoint.com/:v:/g/personal/u20221c362_upc_edu_pe/EUp1F0x6NpNGmcnWxEIED-kBeIuYPmkCeOyooGUNKfKkWQ?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&e=jAriNx)
