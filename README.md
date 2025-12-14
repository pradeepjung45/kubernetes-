# Kubernetes Practice Project

This repository contains my Kubernetes practice exercises where I learned to deploy a Next.js application using Kubernetes resources.

## 🎯 What I Practiced

### 1. Docker Containerization
- Created a multi-stage `Dockerfile` for a Next.js application
- Used Node.js 18 Alpine images for smaller container size
- Implemented builder and runner stages for optimized production builds
- Configured proper environment variables and port exposure

### 2. Kubernetes Deployment
Created `deployment.yaml` with the following Kubernetes concepts:

#### **Deployment Resource**
- Defined a Deployment with 1 replica
- Configured pod labels and selectors for proper routing
- Set up container specifications with custom image (`kuber:latest`)
- Configured `imagePullPolicy: IfNotPresent` for local development

#### **Resource Management**
- **Requests**: 
  - Memory: 64Mi
  - CPU: 100m (0.1 CPU cores)
- **Limits**:
  - Memory: 128Mi
  - CPU: 200m (0.2 CPU cores)

#### **Health Checks**
- **Liveness Probe**: 
  - HTTP GET request to `/` on port 3000
  - Initial delay: 5 seconds
  - Check interval: every 10 seconds
- **Readiness Probe**:
  - HTTP GET request to `/` on port 3000
  - Initial delay: 30 seconds
  - Check interval: every 5 seconds

#### **Service Resource**
- Created a Service of type `NodePort` for local access
- Configured port forwarding: 3000 → 3000
- Used label selectors to connect to deployment pods

### 3. Kubernetes Ingress
Created `ingress.yaml` with the following:

- **Ingress Controller Configuration**
  - API version: `networking.k8s.io/v1`
  - Host-based routing: `kuber.local`
  - Path-based routing with `Prefix` type
  - Backend service routing to `kuber-service` on port 3000

## 📁 Project Files

```
kubernetes/
├── Dockerfile           # Multi-stage Docker build for Next.js app
├── deployment.yaml      # Kubernetes Deployment and Service definitions
├── ingress.yaml        # Kubernetes Ingress for routing
└── README.md           # This file
```

## 🚀 How to Deploy

### Prerequisites
- Docker installed
- Kubernetes cluster (Minikube/Kind/K3s for local testing)
- kubectl configured to access your cluster

### Steps

1. **Build Docker Image**
   ```bash
   docker build -t kuber:latest .
   ```

2. **Apply Kubernetes Resources**
   ```bash
   # Apply Deployment and Service
   kubectl apply -f deployment.yaml
   
   # Apply Ingress
   kubectl apply -f ingress.yaml
   ```

3. **Verify Deployment**
   ```bash
   # Check pods
   kubectl get pods
   
   # Check service
   kubectl get svc
   
   # Check ingress
   kubectl get ingress
   ```

4. **Access Application**
   - For NodePort: `http://localhost:<nodeport>`
   - For Ingress: Add `127.0.0.1 kuber.local` to `/etc/hosts` and visit `http://kuber.local`

## 🔧 Useful Commands

```bash
# View pod logs
kubectl logs -l app=kuber

# Describe deployment
kubectl describe deployment kuber-deployment

# Scale deployment
kubectl scale deployment kuber-deployment --replicas=3

# Delete resources
kubectl delete -f deployment.yaml
kubectl delete -f ingress.yaml

# Port forward for testing
kubectl port-forward svc/kuber-service 3000:3000
```

## 📚 Kubernetes Concepts Learned

- ✅ **Deployments**: Managing application replicas and updates
- ✅ **Services**: Exposing applications within and outside the cluster
- ✅ **Ingress**: HTTP/HTTPS routing to services
- ✅ **Resource Limits**: CPU and memory constraints
- ✅ **Health Probes**: Liveness and readiness checks
- ✅ **Labels & Selectors**: Organizing and routing traffic to pods
- ✅ **Multi-stage Docker Builds**: Optimizing container images
- ✅ **NodePort Service**: Accessing applications on local clusters

## 📝 Notes

- This is a practice project for learning Kubernetes fundamentals
- The application is a Next.js website running on port 3000
- Configured for local development with Minikube/Kind
- Uses `imagePullPolicy: IfNotPresent` to work with locally built images

---

**Practice Date**: December 2025  
**Technologies**: Kubernetes, Docker, Next.js