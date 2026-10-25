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



**Labels:**

Labels are key-value pairs attached to Kubernetes objects such as Pods. They describe identifying attributes of an object. For example, a frontend Pod could have the label tier: frontend.



**Selectors:**

Selectors are used to find Kubernetes objects based on their labels.



**Label Selectors:**

A Service can use a label selector to find the Pods it should send traffic to. For example, a Service with the selector tier: frontend will target Pods that have the label tier: frontend.

**Kubelet:** Each node has a kubelet process running on it which makes it possible for the nodes to communicatie to each other and the preform some tasks like running application processes.



**ClusterIP:**

Exposes a Service only inside the Kubernetes cluster. Applications inside the cluster can use the Service instead of connecting directly to changing Pod IP addresses. The Service routes traffic to its matching Pods.



**NodePort:**

Exposes a Service outside the cluster by opening the same port on every Node. External clients can connect to a Node's IP and the NodePort, and Kubernetes routes the traffic to the Pods behind the Service. NodePort builds on top of ClusterIP.



**LoadBalancer:**

Exposes a Service externally using an external load balancer, usually provided by the cloud or infrastructure platform. The load balancer provides an external entry point and forwards traffic to the Kubernetes Service, which then routes it to the application's Pods.



**Probe:**

A probe is a check that the kubelet performs periodically on a container. The probe can for example make an HTTP request, check a TCP port or execute a command inside the container.



**Liveness probe:**

A liveness probe checks whether the container is still healthy enough to keep running. If it keeps failing past the configured threshold, the kubelet restarts the container.



**Readiness probe:**

A readiness probe checks whether the application is ready to receive traffic. If the probe fails, the Pod stays running but Kubernetes removes it from the Service endpoints so traffic is not sent to it.



Use a liveness probe when you want Kubernetes to restart an unhealthy container. Use a readiness probe when you want Kubernetes to stop sending traffic to a Pod until it is ready again.

Liveness = should this container keep running?  \&\&  Readiness = should this Pod receive traffic?



**ConfigMaps:** A ConfigMap stores non-confidential configuration data as key-value pairs. It allows you to keep environment-specific configuration separate from the container image.

This means the same container image can be used in different environments while the configuration changes separately. For example, development and production could use the same image but different environment variables or application settings.



**RollingUpdate:** When the deployment state is changed and the cluster needs to reconciliate it won't destroy the old pods and then make new updated ones, it will first add new pods and then destroy the old ones so there is no down moment. 



**Intentional failure test:**

I changed the Deployment image to a version that did not exist. Kubernetes created a new ReplicaSet and a new Pod for that version. The Pod failed because Kubernetes could not pull the image, which caused ErrImagePull and later ImagePullBackOff.

Kubernetes kept retrying the image pull, but the new Pod never became ready. The old working Pods stayed running and ready, so the application remained available while the new version was broken.

This showed me that a Deployment does not immediately replace all working Pods with a new version. The new Pods first need to become ready before the old ReplicaSet is scaled down.



kubectl rollout undo can restore a previous Deployment revision, but because I normally manage the Deployment with kubectl apply, I should also update the manifest afterwards so the configuration stored in Git matches the actual desired state.

