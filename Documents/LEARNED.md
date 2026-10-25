# What I learned



**Self Healing:**

Kubernetes can detect failures and try to recover automatically. For example, it can restart failed containers or create replacement Pods when a Deployment needs a certain number of replicas. This is called self-healing.



**Scaling:**

Applications can run multiple copies, or **replicas**, to handle more traffic and improve availability. Increasing or decreasing the number of replicas is called **scaling**. Traffic can be distributed between replicas using a Kubernetes Service. Kubernetes does not automatically scale based on traffic unless autoscaling is configured.



**Cluster:**

A Kubernetes **cluster** consists of a control plane and one or more Nodes.



**Scheduling:**

Kubernetes decides which Node a Pod should run on. This is called **scheduling**.



**Nodes:**

Pods run on **Nodes**, which are the machines that provide CPU, memory and other resources for workloads.



**Control Plane:**

The **control plane** manages the overall state of the cluster and makes decisions such as scheduling workloads.



**Desired State:**

You define a **desired state**, for example that two replicas of an application should exist.



**Reconciliation:**

Kubernetes continuously compares the desired state with the actual state and takes action when they differ. This is called reconciliation.



**Pods:**

Containers run inside Pods. A Pod is the smallest deployable Kubernetes unit.



**ReplicaSets:**

A ReplicaSet makes sure a specified number of identical Pods are running. If one Pod disappears, the ReplicaSet creates another so the desired number of replicas is maintained.



**Deployments:**

A Deployment describes the desired state of an application, such as the container image and number of replicas. The Deployment manages ReplicaSets, which then manage the Pods. When the application version changes, the Deployment can create a new ReplicaSet and gradually replace the old Pods.



**Service:** A Service exposes a set of Pods through a stable network endpoint. Pods can be created, deleted and replaced, which means their IP addresses can change. Instead of connecting directly to a Pod IP, other applications can connect to the Service. The Service keeps track of the matching Pods and sends traffic to them.



**Kubelet:** Each node has a kubelet process running on it which makes it possible for the nodes to communicatie to each other and the preform some tasks like running application processes.

