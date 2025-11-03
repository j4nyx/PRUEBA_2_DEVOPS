PRUEBA_2_DEVOPS

Utilizaremos Python + FastAPI y Java + Spring Boot


# EP2 DOY0101 - Pipeline CI/CD con FastAPI

Este repositorio implementa un pipeline CI/CD completo para un microservicio **Python + FastAPI**, conforme a la evaluación EP2 (contenedores, pruebas automatizadas, seguridad, despliegue automatizado y orquestación).

## Objetivos y Cobertura de Indicadores
#1.- Dockerfile optimizado y publicación de imagen.
#2.- Pruebas automatizadas con Pytest en GitHub Actions.
#3.- Dependabot + análisis SAST con Snyk y política de bloqueo por severidad.
#4.- Despliegue automático a entorno simulado con Docker Compose y *smoke tests*.
# 5.- Orquestación con Kubernetes (réplicas, probes y recursos).

# Estructura

.
├─ .github/
│  ├─ dependabot.yml
│  └─ workflows/
│     ├─ ci-fastapi.yml
│     └─ deploy-compose.yml
├─ fastapi-app/
│  ├─ app/
│  │  └─ main.py
│  ├─ tests/
│  │  └─ test_health.py
│  ├─ requirements.txt
│  └─ Dockerfile
├─ deploy/
│  ├─ docker-compose.yml
│  └─ k8s/
│     ├─ fastapi-deployment.yaml
│     └─ fastapi-service.yaml
└─ docs/
   └─ EVIDENCIAS.md
```

## Uso local
```bash
# FastAPI local (dev)
cd fastapi-app
pip install -r requirements.txt
uvicorn app.main:app --reload
# http://localhost:8000/health
```

## Docker local
```bash
cd fastapi-app
docker build -t fastapi-app:local .
docker run -p 8000:8000 fastapi-app:local
```

## Docker Compose (entorno simulado)

```bash
cd deploy
docker compose up -d
curl http://localhost:8000/health
```

## Kubernetes (minikube o kind)

```bash
kubectl apply -f deploy/k8s
kubectl get deploy,svc
```

## CI/CD

- Al hacer `push`:
  - Se ejecutan tests (`pytest`) y análisis de seguridad (Snyk). Si hay vulnerabilidades severas, el pipeline falla.
  - Se construye y publica la imagen en GHCR.
- El despliegue simulado con Compose se lanza:
  - Automáticamente al cambiar `deploy/docker-compose.yml` o por `workflow_dispatch`.
  - Incluye *smoke test* del endpoint `/health`.

## Requisitos de configuración

- GitHub Container Registry (GHCR): No requiere configuración manual si se usa `${{ secrets.GITHUB_TOKEN }}` para `docker/login-action`.
- Snyk: Agregar `SNYK_TOKEN` en *Settings > Secrets and variables > Actions > New repository secret*.
- Opcionalmente agregar *branch protection rules* para exigir que `ci-fastapi` y `deploy-compose` pasen antes de mergear a `main`.

## Declaración de uso de IA

Se utilizó IA como apoyo para estructurar archivos y plantillas técnicas.


## Reflexiones individuales (colocar en `docs/EVIDENCIAS.md`)

# Integrante A(javiera) :
Aprendizaje técnico:
Durante el desarrollo del pipeline comprendí cómo integrar pruebas automatizadas en GitHub Actions y cómo estas ayudan a mantener la estabilidad del código. 
Aprendí a usar pytest para validar endpoints y a configurar Dependabot para detectar dependencias vulnerables.
También entendí la importancia del análisis de seguridad con Snyk y de aplicar políticas de bloqueo cuando se detectan vulnerabilidades críticas.

Contribución personal:
Me encargué de crear el Dockerfile y configurar el workflow de integración continua. 
Mi principal desafío fue comprender el flujo entre checkout, build y push en GitHub Actions, pero logré resolverlo probando y revisando la documentación oficial.
Me siento satisfecho porque el pipeline ahora automatiza completamente las pruebas y la construcción de la imagen.

Reflexión personal:
Este proyecto me permitió aplicar por primera vez una automatización completa de CI/CD. 
Valoro el aprendizaje práctico porque entendí la conexión entre desarrollo, pruebas y despliegue automatizado, lo que considero esencial para cualquier entorno DevOps. 


# Integrante B (elias) :
Aprendizaje técnico:
Aprendí a usar Docker Compose y Kubernetes para simular un entorno cloud de despliegue. 
Pude entender cómo los manifiestos (Deployment, Service) ayudan a escalar y mantener la aplicación disponible. También mejoré mi manejo de curl y de las herramientas de validación de endpoints en entornos automatizados.

Contribución personal:
Me encargué de la parte de despliegue y orquestación. Creé los archivos docker-compose.yml y los manifiestos de Kubernetes con probes de salud y límites de recursos.
Mi principal reto fue entender cómo probar el despliegue localmente con docker compose up y verificar la trazabilidad de los pipelines en Actions.

Reflexión personal:
Este proyecto me ayudó a comprender que un pipeline no solo automatiza tareas, sino que también asegura calidad y gobernanza del sistema. 
Aprendí a trabajar en equipo con control de versiones y a mantener trazabilidad en GitHub, lo que considero una habilidad clave para mi formación profesional.


## Licencia
Este proyecto se entrega únicamente con fines académicos para la asignatura DOY0101 - Ingeniería DevOps. 

No posee licencia de uso comercial.

