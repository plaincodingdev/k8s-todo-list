# Build and Push Docker Images

Build ToDo List frontend image:
```bash
# Execute from the root of the project
docker build -t 592141/react-18-todo-list:latest .
docker push 592141/react-18-todo-list:latest
```

Build ToDo List backend image:
```bash
# Execute from the root of the project
# Production image
docker build -t 592141/aspnet-core-8-api-todo-list:latest .
docker push 592141/aspnet-core-8-api-todo-list:latest
# Development image
docker build -t 592141/aspnet-core-8-api-todo-list:latest-dev -f Dockerfile.dev .
docker push 592141/aspnet-core-8-api-todo-list:latest-dev
```

# Install Ingress for Docker Desktop

Enable Kubernetes in Docker Desktop

```bash
# Verfy context (docker-desktop)
kubectl config current-context

# Verfy node (docker-desktop)
kubectl get node
```

Install helm:


Install ingress nginx controller:
https://kubernetes.github.io/ingress-nginx/deploy/#quick-start

```bash
# Deploy ingress controller
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace

# Check the status of the service
kubectl get service --namespace ingress-nginx ingress-nginx-controller --output wide --watch

# Check the status of the pod
kubectl get pod -n ingress-nginx

# Remove ingress controller
helm uninstall ingress-nginx --namespace ingress-nginx
```

# Apply Deployments and Services

```bash
kubectl apply -f postgres-secret.yaml
kubectl apply -f postgres-service.yaml
kubectl apply -f postgres-statefulset.yaml
kubectl apply -f postgres-pvc.yaml

kubectl apply -f todo-deployment.yaml
kubectl apply -f todo-service.yaml

kubectl apply -f todo-api-deployment.yaml
kubectl apply -f todo-api-service.yaml

kubectl apply -f ingress.yaml
```

Access the application at `http://localhost/`

# Delete Deployments and Services

```bash
kubectl delete -f postgres-secret.yaml
kubectl delete -f postgres-service.yaml
kubectl delete -f postgres-statefulset.yaml
kubectl delete -f postgres-pvc.yaml

kubectl delete -f todo-service.yaml
kubectl delete -f todo-deployment.yaml

kubectl delete -f todo-api-service.yaml
kubectl delete -f todo-api-deployment.yaml

kubectl delete -f ingress.yaml
```

# Create database

From the development container, run the following command to create the database:

```bash
dotnet ef database update
```