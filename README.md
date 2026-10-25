# Kubernetes Application Deployment

This project deploys and manages a containerized web application using Kubernetes.

The goal was to learn how Kubernetes manages application workloads instead of manually running Docker containers.

The project focuses on Deployments, Pods, ReplicaSets, Services, configuration, health checks, scaling, self-healing, rolling updates and troubleshooting.

## Architecture

<img width="455" height="685" alt="image" src="https://github.com/user-attachments/assets/39e3541c-9c8e-4f71-868b-4d8c61a59625" />

## Technologies Used

- Kubernetes
- Minikube
- kubectl
- Docker
- Docker Hub
- YAML

## Project Structure

```text
kubernetes-application-project/
│
├── kubernetes/
│   ├── configmap.yaml
│   ├── deployment.yaml
│   └── service.yaml
│
├── docs/
│   ├── OVERVIEW.md
│   ├── DECISIONS.md
│   └── LEARNED.md
│
└── README.md
```

## Kubernetes Components

### Deployment

The application runs using a Kubernetes Deployment.

The Deployment normally requests:

```text
2 replicas
```

The Deployment manages the ReplicaSet, which makes sure the correct number of Pods are running.

### Service

A NodePort Service exposes the application outside the local Kubernetes cluster.

The Service selects Pods using:

```text
app: webapp
```

and sends traffic to:

```text
container port 3000
```

### ConfigMap

Application configuration is stored separately using a ConfigMap.

The project currently uses:

```text
APP_ENV=development
```

The Deployment loads this value into the container as an environment variable.

### Readiness Probe

The readiness probe checks:

```text
HTTP /
port 3000
```

A Pod only receives Service traffic when it is ready.

### Liveness Probe

The liveness probe also checks the application using HTTP.

If the application repeatedly fails the liveness check, Kubernetes can restart the container.

## Requirements

Before using the project you need:

- Docker
- kubectl
- Minikube

Start the local cluster:

```powershell
minikube start --driver=docker
```

Check that the cluster is working:

```powershell
minikube status
kubectl get nodes
```

The Minikube Node should show:

```text
Ready
```

## Deploying the Application

Apply the ConfigMap:

```powershell
kubectl apply -f .\kubernetes\configmap.yaml
```

Apply the Deployment:

```powershell
kubectl apply -f .\kubernetes\deployment.yaml
```

Apply the Service:

```powershell
kubectl apply -f .\kubernetes\service.yaml
```

Check the resources:

```powershell
kubectl get deployments
kubectl get replicasets
kubectl get pods
kubectl get services
kubectl get configmaps
```

## Accessing the Application

Get the local application URL:

```powershell
minikube service webapp-service --url
```

Open the returned HTTP URL in a browser.

## Scaling

The Deployment can be scaled manually.

Scale from 2 to 4 replicas:

```powershell
kubectl scale deployment webapp --replicas=4
```

Check the Pods:

```powershell
kubectl get pods
```

Scale back to 2:

```powershell
kubectl scale deployment webapp --replicas=2
```

Kubernetes creates or removes Pods until the actual number matches the requested number.

## Self-Healing

One Pod can be manually deleted:

```powershell
kubectl get pods
kubectl delete pod <pod-name>
```

Watch the Pods:

```powershell
kubectl get pods -w
```

Because the Deployment still requests 2 replicas, Kubernetes automatically creates a replacement Pod.

## ConfigMap Validation

Check the ConfigMap:

```powershell
kubectl describe configmap webapp-config
```

Check that the value reached the container:

```powershell
kubectl exec <pod-name> -- printenv APP_ENV
```

Expected:

```text
development
```

## Rolling Update

The application was updated from:

```text
v1
```

to:

```text
v2
```

by changing the Docker image in the Deployment.

Apply the change:

```powershell
kubectl apply -f .\kubernetes\deployment.yaml
```

Watch the rollout:

```powershell
kubectl rollout status deployment/webapp
kubectl get pods
kubectl get replicasets
```

Kubernetes creates a new ReplicaSet and new Pods for the updated application.

The old Pods are removed after the new Pods become ready.

## Failed Deployment Test

A broken image version was intentionally deployed:

```text
v999
```

The image did not exist.

The new Pod failed with:

```text
ErrImagePull
ImagePullBackOff
```

The problem was diagnosed using:

```powershell
kubectl describe pod <pod-name>
```

and:

```powershell
kubectl get events --sort-by=.metadata.creationTimestamp
```

The old working application Pods remained available while the new version failed.

## Rollback

Deployment history can be checked using:

```powershell
kubectl rollout history deployment/webapp
```

The failed Deployment was rolled back using:

```powershell
kubectl rollout undo deployment/webapp
```

Afterwards the manifest was changed back to the correct working image and applied again so the repository matched the desired state.

## Reproducibility Test

The complete application deployment was deleted:

```powershell
kubectl delete -f .\kubernetes\service.yaml
kubectl delete -f .\kubernetes\deployment.yaml
kubectl delete -f .\kubernetes\configmap.yaml
```

The resources were then recreated only from the files in the repository:

```powershell
kubectl apply -f .\kubernetes\configmap.yaml
kubectl apply -f .\kubernetes\deployment.yaml
kubectl apply -f .\kubernetes\service.yaml
```

After recreation:

- The Deployment was running.
- The ReplicaSet created 2 Pods.
- Both Pods became ready.
- The ConfigMap value was available inside the container.
- The Service exposed the application.
- The application was reachable again.

No manual fixes inside the cluster were needed.

## What I Learned

This project gave me practical experience with:

- Kubernetes architecture
- Pods
- Deployments
- ReplicaSets
- Services
- NodePort
- Labels and selectors
- ConfigMaps
- Readiness probes
- Liveness probes
- Scaling
- Self-healing
- Rolling updates
- Rollbacks
- Kubernetes troubleshooting
- Reproducible application deployments

The main thing I learned is that Kubernetes works around a desired state.

Instead of manually managing individual containers, I describe how the application should run and Kubernetes continuously tries to keep the actual state matching that configuration.

## Current Status

V1 is complete.

The application can be:

```text
deployed
→ exposed
→ scaled
→ updated
→ broken
→ diagnosed
→ rolled back
→ deleted
→ recreated
```

using Kubernetes manifests stored in the repository.
