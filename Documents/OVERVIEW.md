# Project Overview



### Scenario

A small company has a containerized web application.



Running one Docker container manually works, but it has some problems:

\- if the container crashes, someone has to restart it;

\- scaling requires manually starting more containers;

\- deploying a new version risks downtime;

\- application configuration is tied too closely to the container;

\- there is no consistent way to check whether an instance is actually ready to receive traffic.



### Goal

Deploy the application on Kubernetes and use Kubernetes to manage it's lifecycle



### Learning Goal

Learn how Kubernetes deploys, exposes, scales, monitors, and updates a containerized application.



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



### Planned Architecture



### Validation Plan

