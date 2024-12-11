# Kubernetes Services
A Kubernetes Service is a way to expose an application running on a set of Pods. Since Pods are ephemeral and their IP addresses change, Services provide a stable endpoint for communication.
## Key Features of Kubernetes Services
1. **Discovery and Load Balancing** : Services enable automatic discovery of Pods and distribute traffic across them.
2. **Stable Networking**: Services maintain a stable IP address and DNS name, even if the underlying Pods are replaced.
3. **Flexible Exposures**: Services allow exposing applications internally within the cluster or externally to the internet.

**NOTE**: Using Day-1's K8s cluster setup.
## Types of Kubernetes Services
### 1. ClusterIP (Default):
- Exposes the Service on an internal IP in the cluster.
- Accessible only within the cluster.
- **Use Case**: Internal communication between microservices.

### ClusterIP Implementation:
1. Deploy an application and expose it using ClusterIP
```
kubectl create deployment app1 --image kiran2391993/kubegame:v1 --replicas 2
kubectl expose deployment app1 --port 80 --target-port 80 --type ClusterIP  
```
2. Try to access the application
```
nslookup app1
curl http://app1
```
You can't access the application,since you are not inside the cluster and using ClusterIP service type.

3. Create a pod which has troubleshooting tools like nslookup, curl, etc and login to the pod and try to access the deployment.
```
kubectl run troubleshootpod --image kiran2361993/troubleshootingtools:v1
kubectl exec -it troubleshootpod -- bash
nslookup app1
curl http://app1
```
You can access the application. Logout of the pod and try to access the deployment application, you can't access it since you are  outside of the cluster.
**NOTE:** Delete the service & Deployemnt after the work is done
### 2. NodePort
- Exposes the Service on a static port on each Node’s IP.
- Accessible from outside the cluster via <NodeIP>:<NodePort>.
- **Use Case**: Simple external access for debugging or testing.

### NodePort Implementation:
1. Deploy an application and expose it using NodePort Service type.
```
kubectl create deployment app1 --image kiran2391993/kubegame:v1 --replicas 2
kubectl expose deployment app1 --port 80 --target-port 80 --type NodePort
```
This will give you a port ranges between 30000-32000

2. Get the assigned port to the NodePort
kubectl get svc

3. Copy the Public IP of the Control plane and access the application on browser using the IP & Port.
4. Check the Load Balancing.
```
while true; do curl -sL http://<node-ip>:<node-port> | grep -i 'IP A'; sleep 1; done
```
**NOTE**: NodePort Service is not suitable for production as the NodePort cannot be given to the customer.

### LoadBalancer:
Exposes the Service externally using a cloud provider's load balancer.
**Use Case**: Production applications that require external access with automatic scaling.

### LoadBalancer Implementation:
1. Deploy an application and expose it using LoadBalancer Service type
```
kubectl create deployment app1 --image kiran2391993/kubegame:v1 --replicas 2
kubectl expose deployment app1 --port 80 --target-port 80 --type LoadBalancer
```
This will create a loadbalancer. Go to the AWS console. Add the LoadBalancer DNS to Route 53 by creating an A record. Give www as subdomain before actual domain (example.in)

2. Access the application on the browser
www.example.in
