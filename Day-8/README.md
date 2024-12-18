# K8S RBAC (Role Based Access Control)
This guide provides instructions for setting up Role-Based Access Control (RBAC) for users and namespaces in a Kubernetes cluster.

## Why Use RBAC?
RBAC (Role-Based Access Control) is crucial for managing permissions and ensuring security within a Kubernetes cluster. It allows you to:

- **Control Access**: Define who can access specific resources and what actions they can perform. This is essential for enforcing security policies and preventing unauthorized access.
- **Segregate Duties**: Assign different roles to users based on their responsibilities. For instance, some users might only need read access, while others require full administrative privileges.
- **Enhance Security**: Minimize the risk of accidental or malicious actions by limiting user permissions to only what is necessary for their role.
- **Simplify Management**: Manage permissions at a granular level, making it easier to oversee and adjust access as needed.
