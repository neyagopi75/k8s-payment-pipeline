# k8s-pipeline-2026

> Kubernetes CI/CD infrastructure using Tekton, Harness, OpenShift, and JFrog Artifactory.

---

## Overview

This repository contains the CI/CD pipeline configuration and application source for a cloud-native Java service deployed on Kubernetes. It demonstrates a production-grade pipeline that builds a Java application, containerises it with Docker/Buildah, pushes the image to JFrog Artifactory, and deploys it to an OpenShift cluster — orchestrated by Tekton and manageable through Harness.

---

## Tech Stack

| Layer | Tool |
|---|---|
| Language | Java (Maven) |
| Container Runtime | Docker |
| CI/CD Orchestration | Tekton Pipelines |
| Pipeline Management | Harness |
| Container Platform | OpenShift (Kubernetes) |
| Artifact Registry | JFrog Artifactory |
| Helm Packaging | Helm |

---

## Repository Structure

```
k8s-pipeline-2026/
├── src/                    # Java application source code
├── helm/                   # Helm chart for Kubernetes deployment
├── .mvn/wrapper/           # Maven wrapper configuration
├── pipeline.yaml           # Tekton pipeline definition
├── dockerfile              # Docker image build instructions
├── pom.xml                 # Maven project configuration
├── mvnw / mvnw.cmd         # Maven wrapper scripts (Unix/Windows)
└── README.md
```

---

## Pipeline

The Tekton pipeline (`pipeline.yaml`) runs three sequential tasks:

1. **clone-source** — Clones the application source from GitHub using the `git-clone` task.
2. **build-image** — Builds a container image using `buildah` from the project Dockerfile.
3. **push-image** — Pushes the built image to the JFrog Artifactory registry.

### Pipeline Parameters

| Parameter | Description |
|---|---|
| `git-url` | Source repository URL |
| `git-revision` | Git branch or tag to build (default: `main`) |
| `image-repository` | Target Artifactory image path |
| `image-tag` | Image tag to apply (default: `latest`) |

### Workspaces

| Workspace | Description |
|---|---|
| `source-workspace` | Shared persistent volume passed between pipeline tasks |

---

## Prerequisites

- Kubernetes cluster with Tekton Pipelines installed (`tekton.dev/v1`)
- OpenShift CLI (`oc`) or `kubectl` configured
- JFrog Artifactory instance reachable from the cluster
- Helm 3.x
- Java 17+ and Maven (or use the included `mvnw` wrapper)
- Docker or Buildah for local image builds

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/neyagopi75/k8s-pipeline-2026.git
cd k8s-pipeline-2026
```

### 2. Build the application locally

```bash
./mvnw clean package
```

### 3. Build the Docker image

```bash
docker build -f dockerfile -t my-app:local .
```

### 4. Apply the Tekton pipeline to your cluster

```bash
kubectl apply -f pipeline.yaml
```

### 5. Run the pipeline

```bash
tkn pipeline start <pipeline-name> \
  --param git-url=https://github.com/neyagopi75/k8s-pipeline-2026 \
  --param git-revision=main \
  --param image-repository=<your-artifactory-repo> \
  --param image-tag=1.0.0 \
  --workspace name=source-workspace,claimName=pipeline-pvc \
  --showlog
```

### 6. Deploy with Helm

```bash
helm upgrade --install my-app ./helm \
  --namespace my-namespace \
  --create-namespace
```

---

## Harness Integration

This project can be managed via a Harness CD pipeline. Connect this repository as a Harness Git source and reference `pipeline.yaml` as the pipeline definition. Configure the Harness Artifactory connector to match your `image-repository` parameter.

---

## License

This project is licensed under the [MIT License](LICENSE).
