# Kubernetes Sidecar Containers, Resource Quotas and Resource Limits
### Overview
This repository provides a practical demonstration of Kubernetes Sidecar containers and Resource Quotas, as covered in our YouTube video. It includes example configurations and scripts to help you understand and implement these concepts effectively.

### Sidecar Containers
**Sidecar Containers Overview**: Explanation of Sidecar containers, their use cases, and how they complement the main containers.

- **Init Containers**: Containers that run before the main container to check dependencies.
- **Adapter Containers**: Containers that run alongside the main container for purposes such as logging, metrics collection, and proxy services.
- **Ambassador Containers**: Containers that act as proxies for external resources.

**Example:**
- **Deployment Configuration**: YAML files for deploying a service with Init Containers and Adapter Containers.
- **Service Mesh Example**: Demonstration using Istio Envoy Proxy.
### Resource Quotas
#### Resource Quotas Overview: Explanation of how Resource Quotas manage resource usage within namespaces.
**ResourceQuota**: Defines the maximum and minimum resources that can be consumed by all objects in a namespace.
   **Resource Units**: Mi (Mebibyte), m (millicores), Gi (Gibibyte)
   **Example Setup**: Configurations for applying Resource Quotas to namespaces like Development and Staging.
**Example:**

**Namespace Creation**: YAML files to create namespaces with Resource Quotas.
**Pod Creation**: Steps to deploy pods and observe resource restrictions.

**Resource Limits**: Resource limits are applied on Pods
