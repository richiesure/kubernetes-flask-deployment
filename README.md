# Kubernetes Flask Deployment

## Overview
Production-ready Kubernetes deployment of Flask REST API with autoscaling, health checks, and service mesh configuration.

## Architecture
- 3 replica pods for high availability
- ClusterIP service for internal communication
- NodePort service for external access
- Horizontal Pod Autoscaler for dynamic scaling
- Resource limits and requests configured
- Liveness and readiness probes enabled

## Components
- **Namespace**: flask-application
- **Deployment**: flask-app (3-10 replicas)
- **Services**: flask-app-service (ClusterIP), flask-app-nodeport (NodePort)
- **ConfigMap**: flask-app-config
- **HPA**: flask-app-hpa (CPU: 70%, Memory: 80%)

## Deployment Structure
```
k8s-manifests/
├── namespace.yaml
├── deployment.yaml
├── service.yaml
├── service-nodeport.yaml
├── configmap.yaml
└── hpa.yaml
```

## Resource Specifications
- CPU Request: 100m per pod
- CPU Limit: 200m per pod
- Memory Request: 128Mi per pod
- Memory Limit: 256Mi per pod

## API Endpoints
- GET / - Welcome message
- GET /health - Health check
- GET /api/users - User list

## Deployment Commands

### Deploy Application
```bash
kubectl apply -f k8s-manifests/namespace.yaml
kubectl apply -f k8s-manifests/deployment.yaml
kubectl apply -f k8s-manifests/service.yaml
kubectl apply -f k8s-manifests/service-nodeport.yaml
kubectl apply -f k8s-manifests/configmap.yaml
kubectl apply -f k8s-manifests/hpa.yaml
```

### Verify Deployment
```bash
kubectl get all -n flask-application
kubectl get hpa -n flask-application
kubectl top pods -n flask-application
```

### Access Application
```bash
minikube service flask-app-nodeport -n flask-application --url
```

### Scale Manually
```bash
kubectl scale deployment flask-app -n flask-application --replicas=5
```

### View Logs
```bash
kubectl logs -n flask-application -l app=flask-app --tail=50
```

### Update Application
```bash
kubectl set image deployment/flask-app flask-app=devsecops-app:v2.0 -n flask-application
kubectl rollout status deployment/flask-app -n flask-application
```

### Rollback Deployment
```bash
kubectl rollout undo deployment/flask-app -n flask-application
```

## Monitoring
- Health checks run every 10 seconds
- Readiness checks run every 5 seconds
- Autoscaling based on CPU and memory utilization
- Resource metrics available via kubectl top

## Technologies
- Kubernetes 1.34
- Minikube
- Docker
- Python Flask
- Horizontal Pod Autoscaler v2
<img width="1911" height="954" alt="image" src="https://github.com/user-attachments/assets/005706fb-0966-4c2b-888d-d1b5ce6d4584" />

## Status
All components deployed and operational.
