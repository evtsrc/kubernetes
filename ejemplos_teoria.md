# Ejemplos teoría

## Autocompletado
source <(kubectl completion bash)

## Namespaces

### Crear un namespace comando
kubectl create namespace first-namespace

### Crear namespace con fichero
kubectl create -f namespace.yaml

### Borrar namespace
```bash
kubectl delete namespaces namespace-from-yaml
kubectl delete namespaces first-namespace
```

## Pod

### Crear pod
```bash
kubectl apply -f new_nginx_pod.yaml
# Comprobar el estado
kubectl get pods -w
# Consultar su descripcion
kubectl describe pod new-nginx
```
> ¿Cómo puedo ver la imagen descargada? `eval $(minikube docker-env)`

### Crear pod en un namespace en concreto
```bash
# Crear el namespace
kubectl create namespace first-namespace
# Crear pod en namespace diferente a default
kubectl apply -f new_nginx_pod.yaml -n first-namespace 
# Se obtienen pod de los diferentes namespaces
kubectl get pods
kubectl get pods -n first-namespace
# Comprobar en el describe el campo namespace 
kubectl describe pod new-nginx
kubectl describe pod new-nginx -n first-namespace
# Limpieza
kubectl delete pod new-nginx
kubectl delete pod new-nginx -n first-namespace
kubectl delete namespaces first-namespace
```
### Borrar el pod
kubectl delete pod new-nginx

## Deployment

### Crear deployment
```bash
kubectl apply -f nginx_deployment.yaml
kubectl get deployments -w
kubectl get pods -o wide
# Consultar el replicaset
kubectl get rs
kubectl describe rs nginx-deployment
```

**Uso de labels en la consulta**
```bash
kubectl get pods --show-labels
kubectl get pods -A
kubectl get pods -A -l app=nginx
```

### Limpieza
kubectl delete deployment nginx-deployment

## Servicios

### ClusterIP
```bash
kubectl create namespace test-servicios
kubectl apply -f hello_deployment.yaml -n test-servicios
kubectl get deployments.apps -n test-servicios
kubectl apply -f hello_clusterip.yaml -n test-servicios 
kubectl get svc -n test-servicios
# Comprobar que funciona desde dentro del cluster
kubectl run -i --tty --rm debug --image=alpine --restart=Never -n test-servicios -- wget -qO - hello-svc:80
kubectl delete service hello-svc -n test-servicios
```

### NodePort
```bash
kubectl apply -f hello_nodeport.yaml -n test-servicios
kubectl get services -n test-servicios
# Comprobar que funciona desde fuera del cluster pero en el nodo
docker exec -it minikube curl http://localhost:30100
kubectl delete service hello-svc -n test-servicios
```

### LoadBalancer
```bash
kubectl apply -f hello_loadbalancer.yaml -n test-servicios
kubectl get services -n test-servicios
kubectl describe service hello-svc -n test-servicios
# Comprobar que funciona sin entrar la cluster
minikube service hello-svc -n test-servicios
kubectl delete service hello-svc -n test-servicios
```