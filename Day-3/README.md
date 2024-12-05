## Deployments | DaemonSets | StatefulSets

### Deployment
In Kubernetes, a Deployment is a higher-level abstraction that manages a set of Pods (replicas of an application) and ensures they are available, up-to-date, and running as intended. Deployments provide declarative updates to applications, making it easier to manage, roll out, and scale them.
#### Key Features of Deployments
1. Declarative Management: You define the desired state of your application (e.g., number of replicas, container images, etc.) in a YAML or JSON manifest, and Kubernetes ensures that the actual state matches the desired state.

2. Replica Management: Deployments manage the number of replicas (instances) of an application to ensure high availability.

3. Rolling Updates: Deployments enable rolling updates to incrementally replace old versions of Pods with new ones, minimizing downtime.

4. Rollback: If an update causes issues, you can roll back to a previous version using the Deployment's revision history.

5. Self-Healing: If a Pod crashes or is terminated, the Deployment ensures a new Pod is created to maintain the desired state.

6. Scaling: Deployments allow you to scale your application horizontally by increasing or decreasing the number of replicas.

#### Deployment Architecture
A Deployment works with the following Kubernetes objects:

ReplicaSet: A Deployment automatically creates and manages a ReplicaSet to maintain the desired number of Pods.

Pods: The ReplicaSet created by the Deployment manages the actual Pods that run the application containers.

### Example Deployment YAML
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:1.21
        ports:
        - containerPort: 80
```
#### Explanation of the Deployment file:
```
1. apiVersion: apps/v1
 Specifies the API version for the Deployment resource. apps/v1 is the stable API version for Deployments.

2. kind: Deployment
 Indicates that this resource is a Deployment.

3. metadata
   3.1 name: The name of the Deployment (my-app).
   3.2 labels: Metadata labels to categorize and identify the Deployment. Here, the label app: my-app is applied.

4. spec
Specifies the desired state of the Deployment.
   4.1 replicas: 3: Specifies that 3 replicas (pods) of the application should be running.
   4.2 selector:
     4.2.1 matchLabels: Ensures that the pods managed by this Deployment are identified by the label app: my-app.
   4.3 template:
     Describes the pod template for the Deployment. Pods created by the Deployment will match this template.
     4.3.1 metadata:
       4.3.1.1 labels: Labels applied to pods. These must match the Deployment's selector.
     4.3.2 spec:
       4.3.2.1 containers: Specifies the containers in each pod.
         4.3.2.1.1 name: Name of the container (my-container).
         4.3.2.1.2 image: The Docker image to use (nginx:1.21).
         4.3.2.1.3 ports: Defines which container port should be exposed. In this case, port 80.
```
