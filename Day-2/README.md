## Overview on PODS | DEPLOYMENTS | REPLICASETS | NAMESPACES | SERVICES 

#### Pod
Pods are the smallest and simplest unit of deployment. A pod represents a single instance of a running process in a cluster and can contain one or more containers, usually Docker containers. 
In summary, a pod represents one instance of an application running in the cluster, and it can be scaled up or down as needed to meet the application's requirements.

#### Deployment
In Kubernetes, a Deployment is a resource that manages and automates the creation, scaling, and updating of Pods. It provides a declarative way to define your desired application state, and Kubernetes ensures that the cluster matches this state.

#### Replicaset
ReplicaSet is a resource that ensures a specified number of replicas (instances) of Pods are running at any given time.

##### Relationship Between Deployment and ReplicaSet
- When you create a Deployment, Kubernetes automatically creates and manages a ReplicaSet for you.
- The Deployment ensures that the ReplicaSet has the correct number of Pods.
- If you update the Deployment (e.g., change the container image), Kubernetes creates a new ReplicaSet for the updated Pods and gradually shifts traffic to the new ReplicaSet while scaling down the old one (rolling update).

#### Namespaces
Namespaces are used to divide cluster resources between multiple users. They are useful for scenarios where multiple teams (e.g., Alpha and Bravo teams) share the same cluster but require resource isolation.

#### Services
Services provide stable IP addresses and DNS names to Pods. They allow you to expose your applications within or outside the cluster.
##### Types of Services
###### 1. ClusterIP (Default):
- Purpose: Exposes the Service only within the cluster.
- Use Case: Internal communication between components within the cluster.
###### 2. NodePort:
- Purpose: Opens a specific port (NodePort) on each Kubernetes node to expose the Service externally.
- Use Case: Makes the Service accessible from outside the cluster using <NodeIP>:<NodePort>.
###### 3. LoadBalancer:
- Purpose: Exposes the Service externally using a cloud provider's load balancer.
- Use Case: Provides an externally accessible IP address, suitable for internet-facing applications.
