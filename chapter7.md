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

---





