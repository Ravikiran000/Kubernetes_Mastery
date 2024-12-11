## Kubernetes Services
A Kubernetes Service is a way to expose an application running on a set of Pods. Since Pods are ephemeral and their IP addresses change, Services provide a stable endpoint for communication.
### Key Features of Kubernetes Services
1. **Discovery and Load Balancing** : Services enable automatic discovery of Pods and distribute traffic across them.
2. **Stable Networking**: Services maintain a stable IP address and DNS name, even if the underlying Pods are replaced.
3. **Flexible Exposures**: Services allow exposing applications internally within the cluster or externally to the internet.

### Types of Kubernetes Services
#### 1.ClusterIP (Default):
- Exposes the Service on an internal IP in the cluster.
- Accessible only within the cluster.
- **Use Case**: Internal communication between microservices.

### ClusterIP Implementation:
1. Deploy an application and expose it to ClusterIP
```
kubectl create deployment app1 --image kiran2391993/kubegame:v1 --replicas 2
kubectl expose deployment app1 --port 80 --target-port 80 --type ClusterIP  
```
