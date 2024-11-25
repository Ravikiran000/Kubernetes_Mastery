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
Kubernetes uses a master-worker model, where the Control Plane (master components) manages the cluster, and the Worker Nodes run the application workloads. 
![image](https://github.com/user-attachments/assets/dca62900-b30a-43bd-bb58-85bc882383a2)

#### 1. Control Plane (Master Node Components)
The Control Plane is the brain of Kubernetes. It manages the state of the cluster, handles user requests, and coordinates activities between the worker nodes.
##### a. API Server (kube-apiserver)
The API server is the front-end for Kubernetes. All interactions with the cluster go through the API Server. It processes API requests and updates the state of the cluster in etcd.
##### b. etcd
A highly available key-value store that holds the cluster state. Stores all configuration data, metadata, and the desired and current states of the cluster.
##### c. Scheduler (kube-scheduler)
Assigns workloads (pods) to worker nodes based on available resources and constraints. Supports advanced scheduling policies for complex workloads.
##### d. Controller Manager (kube-controller-manager)
Runs controllers that ensure the cluster's desired state is maintained.
  ###### Responsibilities:
  Node Controller: Monitors node health and performs recovery operations.
  Replication Controller: Ensures the desired number of pod replicas are running.
  Endpoint Controller: Updates service endpoints when pods are added or removed.
  Service Account Controller: Manages service accounts and API tokens.
##### e. Cloud Controller Manager (Optional)
Integrates Kubernetes with cloud-specific resources. Manages load balancers, storage provisioning, and networking in cloud environments. Enables Kubernetes to function in hybrid or multi-cloud setups.
#### 2. Data Plane (Worker Node Components)
Worker nodes are the machines (virtual or physical) that run the containerized application workloads. Each worker node contains essential components to execute and manage containers.
##### a. kubelet
The primary agent running on each worker node, responsible for managing the state of containers. Communicates with the control plane to receive pod definitions. Ensures containers are running as per the pod specifications. Monitors container health and reports node status back to the API server.
##### b. kube-proxy
Handles networking on worker nodes to ensure seamless communication between pods and services.
##### c. Container Runtime
Executes and manages containers on the node.

## Kubernetes Cluster SetUp with KOPS
KOPS (Kubernetes Operations) is a tool used to create, manage, and maintain Kubernetes clusters in various cloud environments. It is especially well-known for its seamless integration with AWS, making it one of the most popular tools for managing Kubernetes clusters on AWS
###### Prerequisites:
1. DNS name
2. AWS account
3. One t2.medium EC2 (linux) instance ( management server, maintains the cluster). Generate ssh ( will use this to access nodes)
4. Download KOPS & Kubectl (command-line tool to interact with kuberentes cluster)

### steps 
1. Install KOPS by copying the below command to /usr/local/bin. Change Permissions
    https://github.com/kubernetes/kops/releases/download/v1.30.1/kops-linux-amd64
    mv kops-linux-amd64 kops
    chmod 777 kops
