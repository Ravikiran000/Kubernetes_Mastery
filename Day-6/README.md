# Kubernetes Probes: Readiness and Liveness
In this repository, we'll explore how to use Kubernetes Readiness and Liveness Probes to ensure your applications run smoothly and handle failures effectively.

### What are Probes?
Kubernetes Probes are health checks that allow the platform to manage the lifecycle of your Pods. There are two main types of probes:

- **Readiness Probe**: Checks if a container is ready to start accepting traffic.
- **Liveness Probe**: Checks if a container is still alive and should be restarted if it fails.
### Why Use Probes?
Probes help in maintaining the reliability and availability of your applications.

- **Readiness Probe**: Ensures that your service doesn't receive traffic until it's fully ready.
- **Liveness Probe**: Detects when your container is in a bad state and needs a restart to recover.
## How to Implement Probes
### Readiness Probe
A Readiness Probe determines if a container is ready to handle requests. If the probe fails, the container is temporarily removed from service.

**Example:**
```
apiVersion: v1
kind: Pod
metadata:
  name: readiness-example
spec:
  containers:
  - name: myapp
    image: myapp:latest
    readinessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10
```
**In this example:**

- **httpGet**: The probe sends an HTTP GET request to /healthz on port 8080.
- **initialDelaySeconds**: The probe waits 5 seconds before starting checks.
- **periodSeconds**: The probe checks the endpoint every 10 seconds.
### Liveness Probe
A Liveness Probe ensures that your container is running properly. If the probe fails, Kubernetes will restart the container.

**Example:**
```
apiVersion: v1
kind: Pod
metadata:
  name: liveness-example
spec:
  containers:
  - name: myapp
    image: myapp:latest
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 3
      periodSeconds: 5
```
#### In this example:
- **livenessProbe**: This defines the liveness probe, which Kubernetes uses to check if the application running in the container is still alive. If the liveness check fails, Kubernetes will restart the container.
- **httpGet**: Similar to the Readiness Probe, but used to determine if the container should be restarted. This is the endpoint that Kubernetes will call to check the health of the container. It should return a 200 OK response when the container is healthy.
- **initialDelaySeconds**: This specifies the number of seconds to wait after the container starts before performing the first liveness probe. This allows the application some time to start up.
- **periodSeconds**: The probe checks the endpoint every 5 seconds. This defines the interval (in seconds) between consecutive liveness probes. Kubernetes will call the /healthz endpoint every 5 seconds.
#### Practical Example
Imagine you have a web server that takes a few seconds to start. You can use a Readiness Probe to ensure it’s ready before receiving traffic. If your web server crashes or becomes unresponsive, the Liveness Probe will restart it automatically.

**Sample Application Deployment**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: probe-example
spec:
  replicas: 1
  selector:
    matchLabels:
      app: probe-example
  template:
    metadata:
      labels:
        app: probe-example
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 10
        livenessProbe:
          httpGet:
            path: /live
            port: 8080
          initialDelaySeconds: 3
          periodSeconds: 5
```
### In this deployment:
Readiness Probe checks if the application is ready to serve traffic by accessing /ready.
Liveness Probe monitors the health of the container through the /live endpoint.

### Conclusion
Using Readiness and Liveness Probes in your Kubernetes deployment ensures that your application is reliable, responsive, and capable of self-healing. Start implementing these probes to make your services more robust and fault-tolerant.