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
```

Open http://localhost:8082 and keep the terminal running.
Press Ctrl+C to stop forwarding.

## Inspect the app

Run in PowerShell:

```powershell
kubectl --context=docker-desktop -n default get nodes
kubectl --context=docker-desktop -n default get deployment,pods,service
kubectl --context=docker-desktop -n default describe deployment yay-mini-cloud
kubectl --context=docker-desktop -n default logs deployment/yay-mini-cloud --tail=30
```

## Current limitations

- Fresh-cluster installation instructions are still in progress.
- imagePullPolicy: Never requires the image on each node that runs the app.
- The Deployment requires ConfigMap yay-settings and Secret yay-credentials.
- The Secret requires DB_USERNAME and DB_PASSWORD keys.
- Credentials are not stored in this repository.
- No database integration is demonstrated.
- Environment variables do not automatically modify the static HTML.
- The webpage's status badge is static text.
- Local replicas share one laptop and do not provide machine-level redundancy.

## Next steps

- Document and test installation in a fresh local cluster.
- Add repeatable troubleshooting exercises.
- Add CI/CD, observability, and infrastructure automation.

## Chapter 01 — Separate-namespace deployment verification

**Date:** 2026-10-05 (Philippine time)  
**Status:** Completed on the existing cluster

**Goal:** Verify that the saved manifests can deploy another working
instance of the application.

**Topics practiced:** WSL integration troubleshooting, namespaces,
ConfigMaps, Secret references, node image availability, rollout checks,
and port forwarding.


Successfully deployed the application into `yay-verify` using temporary
copies of the repository manifests:

- Changed the namespace from `default` to `yay-verify`.
- Changed the test Service from LoadBalancer to ClusterIP.
- Created `yay-credentials` with dummy lab values.
- Confirmed `yay-mini-cloud:v1` was present on both Kubernetes nodes.
- Confirmed the deployment rolled out with 3/3 ready replicas.
- Verified the website in a browser through this command:

```bash
kubectl --context=docker-desktop -n yay-verify port-forward service/yay-mini-cloud 8083:8081
```

The website was accessible at http://localhost:8083.

This test used the existing Docker Desktop cluster and preloaded image.
Building and loading the image into a fresh cluster remains to be tested.
