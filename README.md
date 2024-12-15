# Kubernetes Commands for Working with Namespaces and Deployments

## 1. Create a Namespace

To create a new namespace in Kubernetes:

```bash
kubectl create namespace <namespace-name>
```
## 2. List All Namespaces
- To list all namespaces in the cluster:

```bash
kubectl get namespaces
```
## 3. Switch Between Namespaces with kubens
- To switch between namespaces easily using the kubens tool:

```bash
kubens <namespace-name>
```
Example:

```bash
kubens flask-app-namespace
```
- To install kubens (part of kubectx), use the following command (on Ubuntu/Debian):

```bash
sudo apt-get install kubectx
```
## 4. Check Current Active Namespace
- To check the current active namespace (after using kubens):

```bash
kubectl config view --minify | grep namespace:
```
- It will show:

```arduino
namespace: flask-app-namespace
Kubernetes Commands for Deployments
```
## 5. Create a Deployment
- To create a deployment in Kubernetes, you need to create a YAML file (e.g., deployment.yaml), then apply it using:

```bash
kubectl apply -f deployment.yaml
```
## 6. List Deployments in the Current Namespace
- To view all deployments in the current namespace:

```bash
kubectl get deployments
```
- To specify a namespace (if you're not using the active namespace):

```bash
kubectl get deployments -n <namespace-name>
```
Example:

```bash
kubectl get deployments -n flask-app-namespace
```
## 7. View Pods in a Deployment
- To see the pods created by your deployment:

```bash
kubectl get pods
```
- To view pods in a specific namespace:

```bash
kubectl get pods -n <namespace-name>
```
Example:

```bash
kubectl get pods -n flask-app-namespace
```
## 8. Scale a Deployment
- To scale your deployment (change the number of replicas):

```bash
kubectl scale deployment <deployment-name> --replicas=<number-of-replicas>
```
Example:

```bash
kubectl scale deployment flask-env-vars --replicas=5
```
## 9. Describe a Deployment
- To get detailed information about a deployment:

```bash
kubectl describe deployment <deployment-name>
```
Example:

```bash
kubectl describe deployment flask-env-vars
```
## 10. Delete a Deployment
- To delete a deployment:

```bash
kubectl delete deployment <deployment-name>
```
Example:

```bash
kubectl delete deployment flask-env-vars
```
# Kubernetes Commands for Services
## 11. Create a Service
- To create a service that exposes your deployment (e.g., NodePort type):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-env-vars-service
  namespace: flask-app-namespace
spec:
  selector:
    app: flask-env-vars
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000
  type: NodePort
  ```
- Apply the service configuration:
```bash
kubectl apply -f service.yaml
```
## 12. Get Services in a Namespace
- To list all services in a namespace:

```bash
kubectl get svc -n <namespace-name>
```
Example:

```bash
kubectl get svc -n flask-app-namespace
```
## 13. Get External IP for Service
- If your service is of type LoadBalancer, or NodePort, you can check the external IP or port:

```bash
kubectl get svc <service-name> -n <namespace-name>
```
Example:

```bash
kubectl get svc flask-env-vars-service -n flask-app-namespace
```
# Kubernetes Commands for Logs and Debugging
## 14. View Logs of a Pod
- To view the logs of a specific pod:

```bash
kubectl logs <pod-name>
```
- If there are multiple containers in the pod, specify the container name:

```bash
kubectl logs <pod-name> -c <container-name>
```
## 15. Tail Logs in Real Time
- To stream the logs in real-time:

```bash
kubectl logs -f <pod-name>
```
- Kubernetes Commands for Namespace-Specific Deployments
## 16. Deploy in a Specific Namespace
- You can specify the namespace during deployment by either:

- Adding the namespace field directly in the YAML:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-env-vars
  namespace: flask-app-namespace  # Add this line
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-env-vars
  template:
    metadata:
      labels:
        app: flask-env-vars
    spec:
      containers:
        - name: flask-env-vars
          image: r0han01/flask_env_vars:latest
          ports:
            - containerPort: 8000
```
Or, specifying the namespace in the command:
```bash
kubectl apply -f deployment.yaml -n flask-app-namespace
```
# Kubernetes Commands for Other Useful Operations
## 17. View Resource Usage (CPU/Memory) for Pods
- To view the CPU and memory usage for the pods:

```bash
kubectl top pod
```
18. Delete a Namespace
- To delete a namespace and all its resources:

```bash
kubectl delete namespace <namespace-name>
```
Example:

```bash
kubectl delete namespace flask-app-namespace
```
19. Check the Status of All Resources
- To see the status of all resources (pods, deployments, services, etc.) in the current namespace:

```bash
kubectl get all
```
### Complete Command List
```bash
# Create a namespace
kubectl create namespace flask-app-namespace

# List namespaces
kubectl get namespaces

# Switch namespaces with kubens
kubens flask-app-namespace

# Check current active namespace
kubectl config view --minify | grep namespace:

# List deployments in the current namespace
kubectl get deployments

# List pods in the current namespace
kubectl get pods

# Create a deployment (apply YAML file)
kubectl apply -f deployment.yaml

# Scale deployment (change the number of replicas)
kubectl scale deployment flask-env-vars --replicas=5

# Describe a deployment
kubectl describe deployment flask-env-vars

# Delete a deployment
kubectl delete deployment flask-env-vars

# Create a service (apply YAML file)
kubectl apply -f service.yaml

# List services in the current namespace
kubectl get svc

# View logs for a pod
kubectl logs <pod-name>

# Stream logs for a pod
kubectl logs -f <pod-name>

# Deploy in a specific namespace (using the -n flag)
kubectl apply -f deployment.yaml -n flask-app-namespace

# Check resource usage (CPU/Memory) for pods
kubectl top pod

# Delete a namespace
kubectl delete namespace flask-app-namespace

# View all resources (pods, deployments, services)
kubectl get all
```