## Ingress Controller

#### Implementation Steps
1. create one instance and generate TLS certificate & TLS Keys (This isntance is seperate from our cluster. Using this just for TLS cert & Keys)
```
sudo snap install --classic certbot

certbot certonly --manual --preferred-challenges-dns --key-type rsa --email your-email@gmail.com \
--server https://acme-v02.api.letsencrypt.org/directory --agree-tos -d *.your-dns.com
```
2. Copy the "_acme-challenge.your-dns.in" & create a TXT record in with name as "_acme-challenge" and it's respective value.

**3. Wait until the record is in sync and then press enter on the screen"**

4. Copy the generated TLS Cert & TLS Keys somewhere. (will be used later in Cluster).
5. Deploy the KOPS Cluster (same as Day-1 set-up)
6. Deploy Ingress Controller onto the cluster
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.0-beta.0/deploy/static/provider/cloud/deploy.yaml

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.0/deploy/static/provider/cloud/deploy.yaml
```
7. Create 2 files tls.crt & tls.key to store TLS cert & TLS key. Copy the TLS cert & TLS keys into the management instance in /tmp location.
```
cd /tmp
vi tls.crt
vi tls.key
```
8. Create a Secret for the Keys
```
kubectl create secret tls nginx-tls-default --key="tls.key" --cert="tls.crt"
```
9. check the tls secret (which is named as nginx-tls-default. If successful, will show data in num of bytes)
```
kubectl describe secret nginx-tls-default
```
10. Deploying an voting application. Below is the architecture of the application. It uses vote, results as frontend. worker as backend. redis(in-memory database) & Postgres DB for Database. Application will be deployed using yml manifest. vote, results, worker images will be pulled from dockerhub. Official redis, postgres images will be used.

![image](https://github.com/user-attachments/assets/05d6f5fe-4197-4bbe-83fd-cc5201183744)

11. Create one private docker repository to store the images. (we pull the public images and re-tag them and store them in private repository)
  - **Repository name:** votingapp
12. Download the docker images & Pushing the images into a private repository in Dockerhub.
```
# pull vote image
docker pull kiran2361993/testing:latestappvote

# tag the vote image
docker tag ravikiran000/votingapp:vote

#push the image into private repository
docker push ravikiran000/votingapp:vote

# pull result image
docker pull kiran2361993/testing:latestappresults

# tag the result image
docker tag ravikiran000/votingapp:result

#push the image into private repository
docker push ravikiran000/votingapp:result

# pull worker image
docker pull kiran2361993/testing:latestappworker

# tag the worker image
docker tag ravikiran000/votingapp:worker

#push the image into private repository
docker push ravikiran000/votingapp:worker

# Go to dockerhub and check the pushed images
```
13. Delete the images in the cluster since we pushed all the images into the private repository in the Dockerhub and will directly pull the images into the voting.yml manifest from private repository using secrets, which has username and docker token
```
docker rmi $(docker images -aq) -f
```
14. Create Secrets to pull docker images
```
kubectl create secret docker-registry docker-pwd --docker-username=<your-username> --docker-password=<your-password> --docker-email=<your-email>
```
14. Give the vote, result, worker images in voting.yml and Add imagePullSecrets under the images section in your YAML manifest.
```
imagePullSecrets:
  - name: docker-pwd
```
15. Since you deployed the Ingress controller, a network loadbalancer is created. Create an A record in Route53 using the nlb dns and give subdomain as "www". So, if someone is accessing the www.example.com, it will redirect to tour voting app.
16. Deploy Ingress Resource to route traffic according to the service.
  - If users are accessing vote service, they should be redirected to vote service.
       - Defined prefix as vote & www in Ingress resource. If anybody access vote.example.com, www.example.com then they'll be redirected to vote service, to cast a vote.
  - If users are accessing result service, they should be redirected to result service.
       - Defined prefix as result in Ingress resource. If anybody access result.example.com then they'll be redirected to result service, to see results.
   
  **For redirection we are going to create two A records in Route 53**
  
17. Go to Route 53 and create the following records:
- www
- vote
- result
