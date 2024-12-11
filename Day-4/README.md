## Kubernetes Services
A Kubernetes Service is a way to expose an application running on a set of Pods. Since Pods are ephemeral and their IP addresses change, Services provide a stable endpoint for communication.
## Key Features of Kubernetes Services
1. **Discovery and Load Balancing** : Services enable automatic discovery of Pods and distribute traffic across them.
2. **Stable Networking**: Services maintain a stable IP address and DNS name, even if the underlying Pods are replaced.
3. **Flexible Exposures**: Services allow exposing applications internally within the cluster or externally to the internet.
