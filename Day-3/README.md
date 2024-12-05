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
