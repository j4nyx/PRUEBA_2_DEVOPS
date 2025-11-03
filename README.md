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

- Integrante A(javiera) : 
- Integrante B(elias) : [Escribir reflexión propia sin IA]

## Licencia

MIT (opcional de acuerdo a tu preferencia).
