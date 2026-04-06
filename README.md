# Kubernetes Weather App

This repository contains a sample weather application with an authentication service, a UI service, a weather API service, MySQL initialization scripts, and Kubernetes manifests for deploying the application.

## Repository Structure

- `auth/`
  - Go-based authentication service
  - `authdb/` contains database helper code
  - `main/` contains the auth service entrypoint
  - `Dockerfile` builds the auth service image
- `weather/`
  - Python Flask service that fetches weather data from an external API
  - `requirements.txt` lists dependencies
- `UI/`
  - Node.js + Express UI service
  - `package.json` lists frontend dependencies and start script
- `mysql-init/`
  - `init.sql` contains database initialization SQL statements
- `kubernetes-lab/`
  - Kubernetes manifests for the auth service, UI, weather service, MySQL, and related resources

## Services

### Auth Service

- Built in Go using `gin` and JWT-based authentication
- Connects to MySQL for user storage
- Exposes endpoints for health checks, user creation, and login
- Configured via environment variables in the Docker/Kubernetes deployment

### Weather Service

- Built using Flask
- Exposes a simple weather endpoint that proxies requests to a third-party weather API
- Requires `APIKEY` environment variable for the RapidAPI weather API

### UI Service

- Built with Node.js and Express
- Uses `axios`, `jsonwebtoken`, and related middleware
- Serves the frontend for the weather app

## Kubernetes Deployment

The `kubernetes-lab/` folder includes manifest files for:

- `auth-service/` deployments and service definitions
- `weather/` deployment and service definitions
- `ui/` deployment, service, and ingress definitions
- `mysql/` statefulset, headless service, and init job

## Prerequisites

- Docker
- Kubernetes cluster (e.g. Minikube, Kind, or a managed cluster)
- `kubectl`
- Go toolchain for building the auth service
- Python 3 and pip for running the weather service locally
- Node.js and npm for running the UI locally

## Running Locally

### Weather Service

```bash
cd weather
python -m pip install -r requirements.txt
set APIKEY=your_api_key
python main.py
```

### UI Service

```bash
cd UI
npm install
npm start
```

### Auth Service

Build and run the Go auth service:

```bash
cd auth
go build ./main
set DB_HOST=127.0.0.1
set DB_USER=authuser
set DB_PASSWORD=authpassword
set DB_NAME=weatherapp
set DB_PORT=3306
set SECRET_KEY=xco0sr0fh4e52x03g9mv
set AUTH_PORT=8080
./main.exe
```

## Notes

- Update the database credentials and service URLs as needed for your environment.
- The weather service depends on an external weather API and requires a valid API key.
- Kubernetes manifests can be applied with:

```bash
kubectl apply -f kubernetes-lab/
```
