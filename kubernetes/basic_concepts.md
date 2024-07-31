# Kubernetes Concepts Guide

## Pods
### What are Pods?
A Pod is the smallest and simplest Kubernetes object. It represents a single instance of a running process in your cluster. Pods contain one or more containers, such as Docker containers.

### Usage
- Pods are used to host the application containers.
- They share the same network namespace and can communicate with each other using `localhost`.
- Pods can also share storage volumes.

### References
- [Kubernetes Documentation on Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
- [YouTube Video: Kubernetes Pods Explained](https://www.youtube.com/watch?v=feA-6ABp3S0)

## Services
### What are Services?
A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them. Services enable loose coupling between dependent Pods.

### Usage
- Used to expose Pods to the outside world.
- They provide stable IP addresses and DNS names for the Pods.
- Types of services include ClusterIP, NodePort, LoadBalancer, and ExternalName.

### References
- [Kubernetes Documentation on Services](https://kubernetes.io/docs/concepts/services-networking/service/)
- [YouTube Video: Kubernetes Services Explained](https://www.youtube.com/watch?v=2O1W2Cn2QSg)

## ConfigMaps
### What are ConfigMaps?
ConfigMaps are Kubernetes objects that allow you to decouple configuration artifacts from image content to keep containerized applications portable.

### Usage
- Used to store configuration data in key-value pairs.
- Can be consumed as environment variables, command-line arguments, or configuration files in a volume.

### References
- [Kubernetes Documentation on ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)
- [YouTube Video: Kubernetes ConfigMaps Explained](https://www.youtube.com/watch?v=q7bgVI1KGp8)

## Secrets
### What are Secrets?
Secrets are objects in Kubernetes that let you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys.

### Usage
- Used to inject sensitive data into Pods.
- Can be consumed as environment variables or mounted as files in a volume.

### References
- [Kubernetes Documentation on Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)
- [YouTube Video: Kubernetes Secrets Explained](https://www.youtube.com/watch?v=ja6g2sh3DMQ)

## Ingress
### What is Ingress?
Ingress is a collection of rules that allow inbound connections to reach the cluster services. It can provide load balancing, SSL termination, and name-based virtual hosting.

### Usage
- Used to expose HTTP and HTTPS routes from outside the cluster to services within the cluster.
- Configures external access to services in a Kubernetes cluster.

### References
- [Kubernetes Documentation on Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [YouTube Video: Kubernetes Ingress Explained](https://www.youtube.com/watch?v=j6Fi2Jz0s_M)

## Deployment
### What is Deployment?
A Deployment provides declarative updates to applications and defines the desired state for your application. It manages the rollout of updates and scaling of applications.

### Usage
- Used to create and manage a replica set.
- Ensures that the specified number of pod replicas are running at any given time.

### References
- [Kubernetes Documentation on Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [YouTube Video: Kubernetes Deployments Explained](https://www.youtube.com/watch?v=UGKvbFYs9V8)

## StatefulSet
### What is StatefulSet?
StatefulSet is the workload API object used to manage stateful applications. It manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

### Usage
- Used for applications that require persistent storage and stable network identifiers.
- Ensures that each pod in a StatefulSet has a stable, unique network identity and persistent storage.

### References
- [Kubernetes Documentation on StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
- [YouTube Video: Kubernetes StatefulSets Explained](https://www.youtube.com/watch?v=1I6NzlvGzPI)

## Namespaces
### What are Namespaces?
Namespaces provide a mechanism for isolating groups of resources within a single cluster. They are intended for use in environments with many users spread across multiple teams, or projects.

### Usage
- Used to divide cluster resources between multiple users.
- Can help manage resources and limit the scope of resource names.

### References
- [Kubernetes Documentation on Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
- [YouTube Video: Kubernetes Namespaces Explained](https://www.youtube.com/watch?v=leMjD1lU2-Y)

## Volumes
### What are Volumes?
Volumes in Kubernetes provide a way for containers to access storage. They allow data to persist across container restarts and can be used to share data between containers in a Pod.

### Usage
- Used to store data that needs to persist across pod restarts.
- Can be backed by different storage solutions like NFS, AWS EBS, GCE Persistent Disk, etc.

### References
- [Kubernetes Documentation on Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)
- [YouTube Video: Kubernetes Volumes Explained](https://www.youtube.com/watch?v=_nPGv1MjjrQ)
