> minikube start

# create k8s deployment and service from yaml
> kubectl create -f grpc-deployment.yaml
> kubectl create -f grpc-service.yaml

# get address of service 
> minikube service my-service --url

# send data 
> curl --data "{\"name\": \"pc1\", \"a\":2,\"b\":3}" http://192.168.99.100:30620/echo


# log of pods
> kubectl get pods
> kubectl logs <pods id>



# Bash Outputs:

[maruf@AppsCode ~/go/src/GoglandProjects/kubernetes-sample]$ kubectl create -f grpc-deployment.yaml 
deployment "grpc-sample-yaml" created
[maruf@AppsCode ~/go/src/GoglandProjects/kubernetes-sample]$ kubectl create -f grpc-service.yaml 
service "my-service" created
[maruf@AppsCode ~/go/src/GoglandProjects/kubernetes-sample]$ minikube service my-service --url
http://192.168.99.100:30620

[maruf@AppsCode ~]$ curl --data "{\"name\": \"pc1\", \"a\":2,\"b\":3}" http://192.168.99.100:30620/echo
{"message":"Successful request","result":5}
[maruf@AppsCode ~]$ curl --data "{\"name\": \"pc2\", \"a\":2,\"b\":3}" http://192.168.99.100:30620/echo
{"message":"Successful request","result":5}⏎                                                                       
[maruf@AppsCode ~]$ curl --data "{\"name\": \"pc3\", \"a\":2,\"b\":3}" http://192.168.99.100:30620/echo
{"message":"Successful request","result":5}⏎                                                                       
[maruf@AppsCode ~]$ curl --data "{\"name\": \"pc5\", \"a\":2,\"b\":3}" http://192.168.99.100:30620/echo
{"message":"Successful request","result":5}⏎   


# Logs >>

[maruf@AppsCode ~]$ kubectl get deployments
NAME               DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
grpc-sample-yaml   3         3         3            3           16m


[maruf@AppsCode ~]$ kubectl get services
NAME         CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
kubernetes   10.0.0.1     <none>        443/TCP        18m
my-service   10.0.0.249   <pending>     80:30620/TCP   17m



[maruf@AppsCode ~]$ kubectl get pods
NAME                             READY     STATUS    RESTARTS   AGE
grpc-sample-yaml-1513588-0c9pf   1/1       Running   0          2m
grpc-sample-yaml-1513588-7n83h   1/1       Running   0          2m
grpc-sample-yaml-1513588-bskn4   1/1       Running   0          2m


[maruf@AppsCode ~]$ kubectl logs grpc-sample-yaml-1513588-0c9pf
2017/05/17 13:33:12 Server running at port 50053
2017/05/17 13:33:12 Proxy Server running at port 8088
2017/05/17 13:34:04 Request from :pc1, where numbers are 2 3
2017/05/17 13:34:05 Request from :pc1, where numbers are 2 3
2017/05/17 13:34:06 Request from :pc1, where numbers are 2 3
2017/05/17 13:34:06 Request from :pc1, where numbers are 2 3
2017/05/17 13:34:07 Request from :pc1, where numbers are 2 3
2017/05/17 13:34:07 Request from :pc1, where numbers are 2 3
2017/05/17 13:34:14 Request from :pc2, where numbers are 2 3


[maruf@AppsCode ~]$ kubectl logs grpc-sample-yaml-1513588-7n83h
2017/05/17 13:33:18 Server running at port 50053
2017/05/17 13:33:18 Proxy Server running at port 8088
[maruf@AppsCode ~]$ kubectl logs grpc-sample-yaml-1513588-bskn4
2017/05/17 13:33:08 Server running at port 50053
2017/05/17 13:33:08 Proxy Server running at port 8088
2017/05/17 13:34:19 Request from :pc3, where numbers are 2 3
2017/05/17 13:34:23 Request from :pc5, where numbers are 2 3
[maruf@AppsCode ~]$ 
