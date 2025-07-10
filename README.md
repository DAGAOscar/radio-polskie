# Helm Charts Architecture and Deployment Guide

This repository contains three main Helm charts for deploying a typical web application stack on Kubernetes:

- **task-minio-chart**: Deploys a MinIO object storage service.
- **task-nginx-frontend-chart**: Deploys an Nginx-based frontend, serving static content and configuration.
- **task-nginx-ingress-chart**: Deploys the Nginx Ingress Controller for routing external traffic to services within the cluster.

## Architecture Overview

```
[Internet]
    |
    v
[Nginx Ingress Controller]  (task-nginx-ingress-chart)
    |
    v
[Nginx Frontend Service]   (task-nginx-frontend-chart)
    |
    v
[MinIO Object Storage]     (task-minio-chart)
```

- **Nginx Ingress Controller** exposes HTTP(S) endpoints and routes requests to the frontend service.
- **Nginx Frontend** serves static files (e.g., `index.html`) and can be configured via ConfigMaps.
- **MinIO** provides S3-compatible object storage, accessible by the frontend or other services.

## Helm Deployment Commands

### 1. Deploy MinIO
```powershell
helm install minio ./task-minio-chart
```

### 2. Deploy Nginx Frontend
```powershell
helm install nginx-frontend ./task-nginx-frontend-chart
```

### 3. Deploy Nginx Ingress Controller
```powershell
helm install nginx-ingress ./task-nginx-ingress-chart
```

> **Note:**
> - Ensure you have Helm installed and are connected to your Kubernetes cluster.
> - You may need to customize `values.yaml` in each chart for your environment.
> - The order above is recommended, but not strictly required unless dependencies exist between charts.

## Chart Details

### task-minio-chart
- Deploys MinIO using the packaged chart (`minio-17.0.9.tgz`).
- Configuration via `values.yaml`.

### task-nginx-frontend-chart
- Deploys Nginx using the packaged chart (`nginx-21.0.6.tgz`).
- Serves static files from `files/` (e.g., `index.html`).
- Custom configuration via `templates/nginx-frontend-configmap.yaml`.

### task-nginx-ingress-chart
- Deploys the Nginx Ingress Controller (`ingress-nginx-4.13.0.tgz`).
- Can be configured to route traffic to the frontend service.

### Scren
- Rancher Desktop (Container Runtime: dockerd)
  ![image](https://github.com/user-attachments/assets/43e81d6c-12df-4f2a-932c-6c7bfd33d8a0)

 ![image](https://github.com/user-attachments/assets/f40e2493-4479-4cdd-90d1-9552f1a088d6)
- K3S cluster with at least 1 server and 2 agents. HA expansion (more servers, embedded etcd, load balancer).
![image](https://github.com/user-attachments/assets/662058ff-1627-4812-914e-215e6e5f9711)


---

For more advanced usage, refer to the official Helm and Kubernetes documentation.
