# Project Overview



### Scenario

A small company has a containerized web application.



Running the application as one Docker container works, but it has some problems:

\- If the container fails it needs to be restarted.

\- Scaling requires manually starting more containers.

\- Updating the application can cause downtime.

\- Container IP addresses can change.

\- Application configuration should not be stored directly inside the image.

Kubernetes will be used to manage the application instead.



### Goal

Deploy and manage a containerized web application using Kubernetes.

The goal is to learn how Kubernetes manages application workloads instead of manually running containers with Docker.

The environment should use Kubernetes to keep the application available, expose it to users, manage multiple replicas, handle updates and recover from failures.



### Learning Goal

Learn how Kubernetes deploys, exposes, scales, updates and recovers containerized applications.



The main concepts are:

* Pods
* Deployments
* ReplicaSets
* Services
* Labels and selectors
* ConfigMaps
* Readiness probes
* Liveness probes
* Scaling
* Self-healing
* Rolling updates
* Rollbacks
* Troubleshooting



### Definition of Done

The project is complete when:

\- A local Kubernetes cluster can run the containerized web application.

\- The application is deployed using Kubernetes manifests stored in Git.

\- A Deployment manages at least two application replicas.

\- A Service exposes the application through one stable endpoint.

\- Application configuration is supplied using a ConfigMap.

\- Readiness and liveness probes are configured and tested.

\- The Deployment can be scaled up and down.

\- Deleting a Pod causes Kubernetes to automatically create a replacement.

\- A new application version can be deployed using a rolling update.

\- A broken application version can be diagnosed using Kubernetes tooling.

\- The Deployment can be rolled back to the previous working version.

\- The full application deployment can be removed and recreated from the repository.

\- I can explain what Pods, Deployments, Services, ConfigMaps and probes do and why they are used.



### Requirements



### Scope

I will use a Minikube for this project so cloud billing is left out and the project can fully be about Kubernetes.



### Architecture

(See architecture diagram in README)

The web application listens on port 3000 inside the container.

The NodePort Service exposes the application outside the local cluster.



### Kubernetes Files

kubernetes/

├── configmap.yaml

├── deployment.yaml

└── service.yaml



The Deployment contains:

* 2 replicas
* Docker image
* container port
* readiness probe
* liveness probe
* ConfigMap environment variable
* The Service selects Pods with:
* app=webapp



### Validation

The project was validated by:

* Checking the Deployment.
* Checking the ReplicaSet.
* Checking that both Pods were running.
* Accessing the application through the NodePort Service.
* Scaling the Deployment from 2 to 4 replicas and back to 2.
* Deleting a Pod and watching Kubernetes create a replacement.
* Checking that the ConfigMap value reached the container.
* Deploying application version v2.
* Watching Kubernetes perform a rolling update.
* Deploying a broken image version.
* Diagnosing ErrImagePull and ImagePullBackOff.
* Rolling back the failed Deployment.
* Deleting all application resources.
* Recreating the complete application from the Kubernetes manifests.
* Validating that the recreated application worked without manual fixes.
* The full delete and recreate test completed successfully.

