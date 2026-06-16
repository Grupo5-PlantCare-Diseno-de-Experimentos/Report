# Capítulo VII: DevOps Practices
## 7.1. Continuous Integration

### 7.1.1. Herramientas y prácticas recomendadas

- **Sistema CI recomendado:** GitHub Actions (integración nativa con repositorios GitHub). Alternativas: GitLab CI, Azure DevOps Pipelines, CircleCI.
- **Control de código y ramas:** rama `main` protegida; flujo Git (feature branches, pull/merge requests) y revisión de código obligatoria.
- **Verificación temprana:** ejecutar linters y formateadores (ESLint/Prettier para JS/TS, flake8/black para Python) en cada PR.
- **Pruebas automatizadas:** ejecutar suites de tests unitarios y de integración en cada push/PR.
- **Análisis de seguridad y dependencias:** SCA (Dependabot, Renovate) y escaneo estático (Snyk, GitHub Code Scanning).
- **Políticas de calidad:** thresholds mínimos para cobertura de pruebas y bloqueo de merges si fallan checks críticos.

### 7.1.2. Componentes del pipeline de build y test

- **Instalación de dependencias:** restaurar paquetes (npm/yarn/pip) con caches para acelerar builds.
- **Build artefacto:** compilar frontend/backend (por ejemplo, bundle JS, image de servicio) y generar artefactos versionados.
- **Ejecución de tests:**
	- Tests unitarios (rápidos) en paralelo.
	- Tests de integración (base de datos en memoria o contenedores Docker).
	- Tests E2E en entornos aislados (opcional en ramas principales).
- **Quality gates:** lint, style, cobertura mínima (ej. 80%), resultados de escaneos de seguridad.
- **Generación de paquetes/contenerización:** crear `docker image` etiquetada con versión/commit y publicar en registry (GitHub Packages, Docker Hub, ACR).
- **Publicación de artefactos:** almacenar binarios/imágenes y reportes de test en artefact repo o como artefactos de la pipeline.

**Ejemplo breve (GitHub Actions):**
```yaml
name: CI
on: [push, pull_request]
jobs:
	build-and-test:
		runs-on: ubuntu-latest
		steps:
			- uses: actions/checkout@v4
			- name: Setup Node
				uses: actions/setup-node@v4
				with:
					node-version: '18'
			- run: npm ci
			- run: npm run lint
			- run: npm test -- --runInBand
			- name: Build
				run: npm run build
			- name: Build and push Docker
				if: github.ref == 'refs/heads/main'
				uses: docker/build-push-action@v4
				with:
					push: true
					tags: user/plantcare:${{ github.sha }}
```

## 7.2. Continuous Delivery

### 7.2.1. Herramientas y prácticas recomendadas

- **Orquestación de despliegues:** GitHub Actions + Terraform/Ansible para IaC; para entornos Kubernetes usar ArgoCD o Flux para GitOps.
- **Repositorios de infraestructura:** mantener infraestructura como código (Terraform) en repositorio versionado y revisado por PR.
- **Gestión de secretos:** usar Azure Key Vault, AWS Secrets Manager o HashiCorp Vault; nunca almacenar secretos en repositorios.
- **Entornos separados:** `dev` → `staging` → `prod`, con automatizaciones distintas y aprobaciones manuales en promoción a `prod`.

### 7.2.2. Etapas habituales del pipeline de despliegue

1. **Build** — artefactos e imagenes construidas en CI.
2. **Deploy a Dev** — despliegue automático a entorno de desarrollo para validación rápida.
3. **Tests de integración / Smoke tests** — validaciones automáticas post-despliegue.
4. **Deploy a Staging** — promoción a pre-producción con datos de prueba y validaciones más completas.
5. **Aprobación manual / Canario** — revisión por equipo y/o despliegue canario a subset de instancias.
6. **Deploy a Producción** — despliegue controlado (rolling/blue-green) y verificación de salud.
7. **Rollback automatizado** — si fallan checks de salud, ejecutar rollback o detener promoción.

## 7.3. Operaciones en Producción y prácticas SRE

### 7.3.1. Herramientas y prácticas para producción

- **Orquestador:** Kubernetes (EKS/GKE/AKS) o servicios PaaS según presupuesto y escala.
- **Monitorización y métricas:** Prometheus + Grafana para métricas de aplicación e infraestructura.
- **Logging centralizado:** ELK (Elasticsearch, Logstash, Kibana) o alternativas gestionadas (Datadog, Splunk).
- **Alertas y gestión de incidentes:** PagerDuty / Opsgenie integrados con canales (Slack, Teams).
- **Backups y DR:** políticas de backup para bases de datos y almacenamiento; pruebas regulares de recuperación.
- **Autoscaling y límites:** establecer HPA (Horizontal Pod Autoscaler) y límites de recursos para resiliencia.
- **Observabilidad distribuida:** tracing con Jaeger o OpenTelemetry para diagnosticar latencias y errores.

### 7.3.2. Componentes del pipeline para producción

- **Pre-deploy checks:** política de aprobación, revisión de cambios críticos y validaciones de seguridad.
- **Despliegue controlado:** canary / blue-green / rolling updates con health checks y readiness probes.
- **Post-deploy verifications:** smoke tests, integraciones contractuales y monitorización de errores/latencia.
- **Automatización de rollback:** scripts o workflows que restauran versión previa ante degradación.
- **Auditoría y trazabilidad:** registrar quién promovió qué versión, timestamps y logs de despliegue.

## 7.4. Continuous Monitoring

El monitoreo continuo es un componente esencial para asegurar la estabilidad, disponibilidad y rendimiento óptimo del ecosistema de **PlantCare**. Al integrar dispositivos de hardware (sensores IoT) con una aplicación web frontend y un backend en la nube, el monitoreo continuo permite identificar comportamientos anómalos, errores de software y cuellos de botella en la infraestructura antes de que afecten a los usuarios finales o comprometan la salud de las plantas bajo supervisión.

### 7.4.1. Herramientas y prácticas recomendadas (Tools and Practices)

Para la implementación del monitoreo continuo se han seleccionado herramientas especializadas que se integran de manera fluida con el ecosistema tecnológico de la solución:

- **Sentry:** Utilizado para el monitoreo de errores y el rastreo de excepciones en tiempo real dentro de la aplicación web frontend (Vue.js). El SDK de Sentry captura automáticamente las fallas no controladas en el navegador del usuario, proporcionando la pila de llamadas (*stack trace*), la versión de la versión (*release*) afectada y el contexto de la sesión, facilitando una rápida depuración.
- **Supabase Database Metrics & Logging:** Empleado para la supervisión de la base de datos centralizada y relacional integrada en **Supabase** (que funciona sobre PostgreSQL). Registra métricas críticas de rendimiento (consumo de CPU del contenedor de base de datos, memoria RAM utilizada, conexiones activas y espacio de almacenamiento en disco) y genera reportes de consultas lentas o fallidas.
- **Supabase Logs & Analytics:** Se utiliza para supervisar la API RESTful serverless y los servicios de autenticación. Permite verificar el rendimiento de los *endpoints*, medir los tiempos de respuesta y detectar la presencia de peticiones HTTP fallidas (códigos de estado 4xx y 5xx).
- **UptimeRobot:** Herramienta externa configurada para realizar chequeos de disponibilidad HTTP a intervalos regulares (cada 5 minutos) en el dominio público de la aplicación web y en los *endpoints* vitales de la API.

**Prácticas recomendadas adoptadas:**
1. **Definición de Indicadores de Nivel de Servicio (SLIs) y Objetivos de Nivel de Servicio (SLOs):**
   - **Disponibilidad:** Se establece un SLO de 99.5% de disponibilidad mensual para la API de Supabase y el frontend web (medido vía UptimeRobot).
   - **Latencia de base de datos:** El 90% de las consultas de lectura y escritura en la base de datos de Supabase deben resolverse en menos de 500 ms (medido vía el panel de rendimiento de Supabase).
   - **Conectividad de sensores IoT:** Se establece que cada dispositivo físico debe reportar lecturas de humedad y temperatura al menos una vez cada 60 minutos.
2. **Análisis proactivo de rendimiento:** Revisión periódica de las consultas de base de datos en el panel de control de Supabase para crear los índices necesarios y evitar la degradación del rendimiento de la aplicación.

### 7.4.2. Componentes del pipeline de monitoreo (Monitoring Pipeline Components)

El pipeline de monitoreo de **PlantCare** está estructurado en tres componentes principales que aseguran el flujo continuo de datos de telemetría:

1. **Componente de Recolección (Ingesta):**
   - **Frontend:** El SDK integrado de Sentry captura los errores del lado del cliente en tiempo real y los envía mediante peticiones HTTPS a la nube de Sentry.
   - **Backend y Base de Datos:** Los servicios de diagnóstico integrados en el dashboard de Supabase recopilan logs del sistema, llamadas a la API y uso de recursos de la base de datos sin alterar la velocidad de procesamiento de las transacciones.
   - **Sensores IoT:** El microcontrolador del dispositivo de hardware incluye lógica de control que reporta códigos de error de conexión WiFi y fallas de hardware en las peticiones HTTPS dirigidas al backend.
2. **Componente de Almacenamiento y Procesamiento:**
   - Las métricas recopiladas se indexan y almacenan en las bases de datos de series temporales integradas de Supabase Metrics y los sistemas de almacenamiento de Sentry Cloud, garantizando la retención de los registros de auditoría durante un mínimo de 30 días.
3. **Componente de Visualización:**
   - Paneles interactivos (*dashboards*) personalizados en la interfaz de Supabase (Database Health) y Sentry que unifican las métricas clave del sistema para una rápida interpretación visual de la salud de la infraestructura.

### 7.4.3. Componentes del pipeline de alertas (Alerting Pipeline Components)

El componente de alertas evalúa de forma continua los datos recopilados por el pipeline de monitoreo contra reglas preestablecidas para detectar anomalías. Si una métrica excede un umbral definido, el motor de alertas activa una advertencia o un incidente crítico.

Las reglas de alerta clave configuradas para **PlantCare** son:

- **Alerta de Inactividad de Sensores (Sensor Offline):** Activada si un dispositivo IoT registrado no reporta datos al backend por más de 60 minutos consecutivos. Es una alerta crítica debido al riesgo latente de que la planta necesite riego y el sistema no pueda actuar.
- **Alerta de Tasa de Errores de API:** Se activa si el porcentaje de respuestas HTTP con código de estado 5xx supera el 2% en un intervalo de 5 minutos.
- **Alerta de Excepciones en Frontend:** Activada si la aplicación web registra más de 15 excepciones idénticas no controladas en Sentry en un período menor a 10 minutos.
- **Alerta de Capacidad en Base de Datos:** Se dispara si el uso de CPU asignado a la base de datos de **Supabase** supera el 85% durante 10 minutos continuos, o si el espacio de almacenamiento disponible en la base de datos cae por debajo del 15% de la capacidad total de la cuenta.

### 7.4.4. Componentes del pipeline de notificaciones (Notification Pipeline Components)

El pipeline de notificaciones se encarga de enrutar las alertas de forma estructurada hacia los miembros responsables del equipo a través de los canales de comunicación adecuados, permitiendo una rápida reacción e inicio del proceso de resolución.

- **Canal de comunicación principal:** Servidor oficial de Discord del equipo de desarrollo, integrado a través de Webhooks.
- **Enrutamiento y Niveles de Prioridad:**
  - **Canal `#alerts-critical` (Urgente):** Recibe las notificaciones que detienen la funcionalidad principal de la plataforma o ponen en riesgo el bienestar de los cultivos (ej., sensor inactivo, caída de la API, agotamiento de espacio en la base de datos). Estas notificaciones incluyen menciones directas a los administradores del sistema para asegurar su atención inmediata.
  - **Canal `#alerts-warnings` (Preventivo):** Recibe notificaciones sobre métricas que se aproximan a sus límites de advertencia (ej., uso de CPU en base de datos > 70%, incremento leve de latencia en peticiones HTTP). Permite al equipo planificar mantenimientos sin interrumpir el servicio.
- **Formato estándar de notificación:** Los mensajes enviados por los Webhooks a Discord contienen una estructura clara para agilizar el diagnóstico:
  - **Componente:** Nombre del componente o servicio afectado (ej., *Supabase Database*, *Vue.js Frontend*).
  - **Gravedad:** Nivel del incidente (*Crítico* / *Advertencia*).
  - **Descripción:** Detalle del evento o regla de alerta violada.
  - **Métrica actual:** Valor que disparó la alerta (ej., *CPU: 89%*, *Tiempo sin conexión: 62 min*).
  - **Enlace de diagnóstico:** Enlace directo al panel de control de Sentry o al dashboard de Supabase para agilizar la revisión del log de errores correspondiente.

---





