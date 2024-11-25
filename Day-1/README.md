## Kubernetes | Kubernetes Architecture | KOPS (Production Grade Kubernetes Cluster SetUp)

### Container Orchestration: 
Container orchestration is the automated process of managing (maintaining the life cycle) and scaling containerized applications. It simplifies the deployment, management, networking, scaling, and availability of containers across a cluster of nodes.
### Key Features of Container Orchestration:
1. Automated Deployment: Streamlines the process of deploying containers across multiple servers.
2. Scaling and Load Balancing: Dynamically adjusts the number of containers based on demand and distributes traffic.
3. Service Discovery: Allows containers to locate and communicate with each other automatically.
4. Resource Management: Allocates resources like CPU and memory efficiently across containers.
5. Self-healing: Restarts failed containers, replaces unhealthy nodes, and reschedules containers as needed.
6. Networking: Provides secure communication between containers and the outside world.
7. Monitoring and Logging: Tracks container performance and logs events for debugging.

## Kubernetes
Kubernetes (often abbreviated as K8s) is an open-source container orchestration platform for automating the deployment, scaling, and management of containerized applications. 
### Architecture of Kubernetes:
![image](https://github.com/user-attachments/assets/dca62900-b30a-43bd-bb58-85bc882383a2)
Kubernetes uses a master-worker model, where the Control Plane (master components) manages the cluster, and the Worker Nodes run the application workloads. 
#### 1. Control Plane Components
