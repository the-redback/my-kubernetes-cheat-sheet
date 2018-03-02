# my-kubernetes-cheat-sheet

```console
> minikube version
> minikube start
> kubectl version
> kubectl get nodes
```

## Run app in kubernetes


### example code of kubernetes

```console
> kubectl run kubernetes-bootcamp --image=docker.io/jocatalin/kubernetes-bootcamp:v1 --port=8080
```

### also run GRPS_Sample //problematic

```console
> kubectl run kubernetes-grpc-sample --image=maruftuhin/server_grpc_sample --port=8088
```

### See deployments

```console
> kubectl get deployments
```

### run proxy and echo 

```console
> kubectl proxy
```

### setting up POD_NAME variable

```console
> export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
> echo Name of the Pod: $POD_NAME

> curl http://localhost:8001/api/v1/proxy/namespaces/default/pods/$POD_NAME
```


## Pods

```console
> kubectl get pods
> kubectl describe pods
> kubectl logs $POD_NAME
```

## Execute commands on container

### settings up env variables

```console
> kubectl exec $POD_NAME env
```

### run a conainer

```console
> kubectl exec -ti $POD_NAME bash
> exit
```

## Services on kubernetes

```console
> kubectl get services
```

### create and expose new service

```console
> kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

### setting up Node Port variable

```console
> export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
> echo NODE_PORT=$NODE_PORT
```

### accessing POD from external

```console
> curl host01:$NODE_PORT
```

### accessing pod from internal

```console
> kubectl exec -ti $POD_NAME curl localhost:8080
```

### get description of only one deployment

```console
> kubectl get pods -l <label>
i.e. 
> kubectl get pods -l run=kubernetes-bootcamp
> kubectl get services -l run=kubernetes-bootcamp
```

### 1. setting up lable. 1st get POD NAME. then

```console
> export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
> echo Name of the Pod: $POD_NAME
```

### 2. label it

```console
> kubectl label pod $POD_NAME app=v1
```

### List of pods using label "app=v1" 

```console
> kubectl get pods -l app=v1
```

### delete services with label run=kubernetes-bootcamp

```console
> kubectl delete service -l run=kubernetes-bootcamp
```

## Scaling

```console
> kubectl get deployments
```

### scale to 4 replicas. [kubernetes--bootmcamp] is name of deloyments

```console
> kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

### current pods

```console
> kubectl get pods -o wide
```

### Change log

```console
> kubectl describe deployments/kubernetes-bootcamp
```

### find exposed ip and service

```console
> kubectl describe services/kubernetes-bootcamp
```

### set environment variable NODE PORT

```console
> export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
> echo NODE_PORT=$NODE_PORT
```

### Run this command to see which pod is servicing 

```console
> curl host01:$NODE_PORT
```

### Scale down service to 2 replicas from 4 replicas

```console
> kubectl scale deployments/kubernetes-bootcamp --replicas=2
```

### Description again

```console
> kubectl get deployments
> kubectl get pods -o wide
```

## Updating Application

```console
> kubectl get deployments
> kubectl get pods
```

### current image version of app

```console
> kubectl describe pods
```

### update the image to version 2

```console
> kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```
### Changes in Pods

```console
> kubectl get pods
```

##  Verify update

### service description

```console
> kubectl describe services/kubernetes-bootcamp
```

### setting up NODE PORT

```console
> export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
> echo NODE_PORT=$NODE_PORT
```

### rollout status command to see update

```console
> kubectl rollout status deployments/kubernetes-bootcamp
```

## Rollback update

### First deploy update tag v10 for example

```console
> kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v10
```

### status

```console
> kubectl get deployments
> kubectl get pods
> kubectl describe pods
# we can see, not all are v10 applied
```

### rollback to previous working state

```console
> kubectl rollout undo deployments/kubernetes-bootcamp
```

## Run own program docker-gRPC

```console
> docker images
> kubectl run grpc-sample3 --image=maruftuhin/server_grpc_sample:tag --port=8088
> kubectl get pods
```

### access from internal

```console
> kubectl proxy
> curl --data "{\"name\": \"pc1\", \"a\":2,\"b\":3}" http://localhost:8001/api/v1/proxy/namespaces/default/pods/<pods id >/echo
```

### access from external [Expose ]

```console
> kubectl expose deployment/grpc-sample3 --type="NodePort" --port 8088
> kubectl get services/grpc-sample3
```

### setting $Node_Port variable. but the ports can also be used from "kubectl get services/grpc-sample3" results.  

```console
> export NODE_PORT=$(kubectl get services/grpc-sample3 -o go-template='{{(index .spec.ports 0).nodePort}}')
> kubectl get services/grpc-sample3
> kubectl get nodes
> kubectl get svc grpc-sample3 -o yaml
> minikube service grpc-sample3 --url
> curl --data "{\"name\": \"pc1\", \"a\":2,\"b\":3}" http://192.168.99.100:32258/echo
```
