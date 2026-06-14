# NACH Payment System – Kubernetes CI/CD Infrastructure

## Overview

This project simulates a production-style NACH payment processing platform deployed on OpenShift using modern DevOps practices.

The objective of this project is to demonstrate end-to-end CI/CD implementation using Tekton, Helm, Harness, Artifactory, and OpenShift while deploying a Spring Boot application.

---

## Architecture

```text
Developer
    │
    ▼
GitHub Repository
    │
    ▼
Tekton CI Pipeline
    │
    ├── Source Code Checkout
    ├── Buildah Image Build
    └── Push Image
    ▼
JFrog Artifactory
    │
    ▼
Harness CD
    │
    ▼
Helm Deployment
    │
    ▼
OpenShift Cluster
    │
    ▼
Payment System Application
```

---

## Technology Stack

- Java 17
- Spring Boot
- Docker
- Tekton
- Buildah
- Helm
- OpenShift
- Harness
- JFrog Artifactory
- GitHub

---

## Repository Structure

```text
k8s-payment-pipeline/

├── Dockerfile
├── pom.xml

├── src/
│   └── Spring Boot Application

├── pipeline.yaml
│
├── helm/
│   ├── Chart.yaml
│   ├── values.yaml
│   ├── dev-values.yaml
│   ├── uat-values.yaml
│   ├── prod-values.yaml
│   └── templates/
│       ├── deployment.yaml
│       ├── service.yaml
│       └── route.yaml
│
└── openshift/
    ├── namespace.yaml
    ├── serviceaccount.yaml
    └── rolebinding.yaml
```

---

## CI Pipeline (Tekton)

The Tekton pipeline performs:

1. Clone source code from GitHub
2. Build container image using Buildah
3. Tag image
4. Push image to JFrog Artifactory

Pipeline parameters:

- git-url
- git-revision
- image-repository
- image-tag

---

## CD Pipeline (Harness)

Harness is used for deployment orchestration.

Deployment flow:

1. Pull image from Artifactory
2. Deploy Helm release
3. Apply environment-specific values
4. Deploy to OpenShift
5. Verify application health

---

## Helm Features

The Helm chart includes:

- Deployment
- Service
- OpenShift Route
- Rolling Update Strategy
- Readiness Probe
- Liveness Probe
- Environment Variables
- Resource Limits
- Environment-specific Values Files

Environments:

- Development
- UAT
- Production

---

## OpenShift Configuration

The project includes:

- Namespace Management
- Service Accounts
- Role-Based Access Control (RBAC)

These resources provide deployment permissions required by CI/CD tooling.

---

## Application

A lightweight Spring Boot application is included to demonstrate:

- Containerization
- Automated image creation
- Kubernetes deployment
- OpenShift routing

The application is intentionally minimal to keep the focus on infrastructure and CI/CD implementation.

---

## Future Enhancements

- SonarQube Integration
- Trivy Security Scanning
- Prometheus Monitoring
- Grafana Dashboards
- Automated Rollbacks
- Multi-Cluster Deployment
- GitOps Integration

---



DevOps Engineer | Kubernetes | OpenShift | Tekton | Helm | Harness | Artifactory