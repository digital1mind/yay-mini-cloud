# Yay Mini Cloud

A local Kubernetes learning lab running a static website in a custom
NGINX container image.

## Purpose

Practice container operations, Kubernetes deployments, health checks,
resource configuration, and troubleshooting while building toward
platform engineering.

## Current setup

- Windows with WSL2 Ubuntu and Docker Desktop
- Docker Desktop Kubernetes with two local nodes
- Custom image: yay-mini-cloud:v1
- Deployment with three replicas
- HTTP readiness and liveness probes
- CPU and memory requests and limits
- ConfigMap and Secret references for environment-variable practice
- LoadBalancer Service on port 8081, targeting container port 80

## Files

| File | Purpose |
| --- | --- |
| Dockerfile | Packages the webpage using nginx:alpine |
| index.html | Static demo webpage |
| kubernetes.yaml | Deployment and Service |
| configmap.yaml | Non-sensitive environment settings |

## Verification completed

- Started the existing standalone Docker container.
- Opened the Docker-hosted page on localhost:8080.
- Confirmed both Kubernetes nodes were Ready.
- Confirmed three application replicas were ready and available.
- Opened the Kubernetes-hosted page using port-forwarding.
- Observed successful HTTP requests and Kubernetes probe requests.
- Validated the updated Deployment and Service using a server dry run.

## Access the existing Kubernetes deployment

Run in PowerShell:

```powershell
kubectl --context=docker-desktop -n default port-forward service/yay-mini-cloud 8082:8081
