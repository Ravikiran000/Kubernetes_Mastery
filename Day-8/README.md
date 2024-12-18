# K8S RBAC (Role Based Access Control)
This guide provides instructions for setting up Role-Based Access Control (RBAC) for users and namespaces in a Kubernetes cluster.

## Why Use RBAC?
RBAC (Role-Based Access Control) is crucial for managing permissions and ensuring security within a Kubernetes cluster. It allows you to:

- **Control Access**: Define who can access specific resources and what actions they can perform. This is essential for enforcing security policies and preventing unauthorized access.
- **Segregate Duties**: Assign different roles to users based on their responsibilities. For instance, some users might only need read access, while others require full administrative privileges.
- **Enhance Security**: Minimize the risk of accidental or malicious actions by limiting user permissions to only what is necessary for their role.
- **Simplify Management**: Manage permissions at a granular level, making it easier to oversee and adjust access as needed.

## RBAC Implementation Steps:
**NOTE** : Using Day-1 cluster setup. Create 2 namespaces "development & production" in the cluster.
#### 1. Utilizing the same 'certificate and key' from Ingress Controller setup (Day-5)
#### 2. Login to Control Plane and fetch the ca.crt & ca.key. These are used to create users in the cluster.
```
# find the kubernetes-ca.crt and kubernetes-ca.key location
find / -name kops-controller

# Go to ca.crt and ca.key location
cd <location given by find command>

# copy the crt and key some where and add them into management server /tmp location. (storing in /tmp for temporary location.)
cat kubernetes-ca.crt
cat kubernetes-ca.key

# Go to management server and paste the keys in files ca.crt & ca.key
```
#### 3. Create users "User1, User2 & Admin" on the Management server. 
```
# creating user1

openssl genrsa -out user1.key 2048
openssl req -new -key user1.key -out user1.csr -subj "/CN=user1/O=development"
openssl x509 -req -in user1.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out user1.crt -days 365

# creating user2

openssl genrsa -out user2.key 2048
openssl req -new -key user2.key -out user2.csr -subj "/CN=user2/O=development"
openssl x509 -req -in user2.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out user2.crt -days 365

# creating admin user (ravikiran)

openssl genrsa -out ravikiran.key 2048
openssl req -new -key ravikiran.key -out ravikiran.csr -subj "/CN=ravikiran/O=development"
openssl x509 -req -in ravikiran.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out ravikiran.crt -days 365
```
#### 4. You can see that .crt & .key are created for each user. (EX: user1.crt, user1.key). Copy all these .crt & .key from management to master server location.

#### 5. Copy the kubeconfig file from the management server and edit it according to your reuirement and deploy the kubeconfig files for users into master node. 
```
cd ~/.kube/
ls -al
cat config  # This will you give you the kubeconfig file. Change the crt key in kube config while copying this into master node. You can also get the config file from the s3 bucket.

# save the kubeconfig files like below
vi USER1-CONFIG
export KUBECONFIG=/root/USER1-CONFIG

vi USER2-CONFIG
export KUBECONFIG=/root/USER2-CONFIG

# Now check the pods in cli. You will get forbidden error. Because the user doesn't have access to the cluster. You can give access by creating a role for the user in the cluster and creating a rolebinding for binding the role and user. 
kubectl get pods

# In Kubernetes, the kubeconfig file alone (with the user information) establishes who can access the cluster, but it does not define what actions they are allowed to perform within the cluster. This is where Role and RoleBinding come into play.

# Create roles in the cluster from the management server. This is like creating an IAM role (which can be assigned to users to give access) that has access to resources. 
Deploy the role with 
echo '
<role definition>
kubectl apply -f -
