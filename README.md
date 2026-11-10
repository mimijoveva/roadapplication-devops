# RoadApplication – DevOps

A full-stack web application for tracking road conditions in North Macedonia, using data scraped automatically from AMSM. In this repo the application is containerized, tested and deployed through an automated CI/CD pipeline to Kubernetes.

## Tech Stack

| Layer          | Technology                          |
|----------------|-------------------------------------|
| Frontend       | Vue 3 + Vite                        |
| Backend        | FastAPI (Python), APScheduler       |
| Database       | PostgreSQL                          |
| Containers     | Docker, Docker Compose              |
| CI/CD          | GitHub Actions, Docker Hub          |
| Orchestration  | Kubernetes (Minikube)               |
| Cloud deploy   | Render.com                          |

## Features

- Automated scraping of AMSM road condition data on a schedule (APScheduler)
- REST API built with FastAPI
- Responsive Vue 3 frontend
- Multi-stage Docker builds for smaller, production-ready images
- Automated build and push to Docker Hub on every push to `main`
- Kubernetes manifests for deployments, services, config and persistent storage
- Continuous deployment to Render.com

## Architecture

```
┌────────────┐      ┌────────────┐      ┌──────────────┐
│  Frontend  │ ───> │  Backend   │ ───> │  PostgreSQL  │
│  (Vue 3)   │      │ (FastAPI)  │      │              │
└────────────┘      └────────────┘      └──────────────┘
                          │
                    APScheduler
                          │
                    AMSM scraping
```

## CI/CD Pipeline

Every push to `main` triggers the GitHub Actions workflow:

1. **Checkout** – pulls the latest code
2. **Build** – builds Docker images for frontend and backend
3. **Push** – pushes the images to Docker Hub (`mimijoveva/...`)
4. **Deploy** – triggers deployment on Render.com

Workflow file: `.github/workflows/<workflow-name>.yml`

## Run Locally with Docker Compose

```bash
git clone https://github.com/<your-username>/roadapplication-devops.git
cd roadapplication-devops
docker compose up --build
```

- Frontend: http://localhost:<frontend-port>
- Backend API: http://localhost:8000
- API docs: http://localhost:8000/docs

## Deploy on Kubernetes (Minikube)

```bash
minikube start
kubectl apply -f k8s/
kubectl get pods
minikube service <frontend-service-name>
```

The `k8s/` folder contains:

- `Deployment` for the frontend, the backend and PostgreSQL
- `Service` resources to expose the components
- `ConfigMap` / `Secret` for environment variables
- `PersistentVolumeClaim` for database storage

## Docker Images

- `mimijoveva/<frontend-image>`
- `mimijoveva/<backend-image>`

## Optimizations

- Multi-stage builds reduce the final image size
- `.dockerignore` keeps unnecessary files out of the build context
- Layer caching in the CI pipeline speeds up builds
- Environment-based configuration instead of hardcoded values

## Author

**Mimi Joveva** – Faculty of Computer Science and Engineering (FINKI), Skopje
