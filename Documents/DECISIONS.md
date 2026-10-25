# Technical Decisions

This document contains the main technical decisions made during the project.



### Kubernetes Environment

Decision: Use Minikube.

The goal of this project is to learn Kubernetes itself.

Using Minikube gives me a local Kubernetes cluster without also needing to learn AKS, Azure networking and cloud configuration at the same time.

AKS is outside the scope of this project.



### Container Image

Decision: Reuse the Dockerized web application from my Azure/Terraform project.

The goal is to learn Kubernetes, not build another application.

The image is stored publicly on Docker Hub so Kubernetes can pull it directly.



### Deployment

Decision: Use a Kubernetes Deployment instead of manually creating Pods.

A Deployment manages ReplicaSets which manage the Pods.

This means Kubernetes can maintain the requested number of replicas and manage application updates.



### Replicas

Decision: Run 2 replicas normally.

Two replicas are enough to demonstrate:

Multiple Pods

Service routing

Scaling

Self-healing

Rolling updates

More replicas would not add anything useful to this project.



### Service Type

Decision: Use NodePort.

The application needs to be reachable from outside the local Kubernetes cluster.

NodePort provides a simple way to expose the application without requiring an external cloud load balancer.

A LoadBalancer Service would add infrastructure that is unnecessary for this local project.

### 

### Labels and Selectors

Decision: Use:

app: webapp

The Deployment adds this label to the application Pods.

The Service uses the same label in its selector to find the correct Pods.



### Application Port

Decision: Use container port 3000.

The Dockerized application listens on port 3000.

The Service sends traffic to this port using targetPort: 3000.

### 

### Configuration

Decision: Use a ConfigMap for non-sensitive configuration.

The ConfigMap contains:

APP\_ENV=development

The Deployment loads this value into the application container as an environment variable.

The application does not currently depend on this value, but it demonstrates how Kubernetes configuration can be separated from the container image.



### Readiness Probe

Decision: Use an HTTP readiness probe on / at port 3000.

The application's root endpoint already returns a successful HTTP response.

The readiness probe makes sure Kubernetes only sends traffic to a Pod after the application is ready.



### Liveness Probe

Decision: Use an HTTP liveness probe on / at port 3000.

If the application becomes unhealthy and repeatedly fails the probe, Kubernetes can restart the container.



### Scaling

Decision: Test scaling manually.

The Deployment was scaled:

2 replicas → 4 replicas → 2 replicas

Automatic scaling is outside the scope of this project.

The goal was to understand how Kubernetes changes the actual number of Pods to match the desired replica count.



### Self-Healing

Decision: Test self-healing by manually deleting a Pod.

The Deployment still required 2 replicas.

After deleting one Pod, the ReplicaSet detected that only one replica was running and created a replacement automatically.



### Rolling Update

Decision: Test a real application update from v1 to v2.

A small visible change was made to the application and pushed to Docker Hub as version v2.



The Deployment image was changed from:

v1 to: v2

Kubernetes created a new ReplicaSet, started the new Pods and removed the old Pods after the new version became ready.



### Failed Deployment

Decision: Intentionally deploy a Docker image version that does not exist.



The Deployment was changed to use:

v999

Kubernetes tried to create a Pod using this image.

The image could not be pulled, which resulted in:

ErrImagePull

ImagePullBackOff

The old working Pods stayed available while the new version failed.



### Rollback

Decision: Use Kubernetes rollout history and rollback functionality.

The failed Deployment was rolled back using:

kubectl rollout undo deployment/webapp

Afterwards the YAML file was changed back to the correct image version and reapplied so the Git configuration matched the actual desired state.



### Reproducibility

Decision: Validate that the application can be completely removed and recreated from the repository.

The Deployment, Service and ConfigMap were deleted.

They were then recreated only using the Kubernetes manifest files stored in the repository.

The application returned to a working state without manual changes inside the cluster.

