
### Docker Download from
https://www.docker.com/products/docker-desktop


### Minikube Install from
https://minikube.sigs.k8s.io/docs/start/


### minikube steps:
mvn clean install
docker build -f Dockerfile -t productservice .
docker tag productservice mail4dhananjaya/productservice
docker push mail4dhananjaya/productservice:latest

minikube start
minikube status
kubectl get nodes
kubectl create namespace productservice
kubectl create deployment productservice --image=mail4dhananjaya/productservice:latest --dry-run=client -o=yaml > deployment.yaml
kubectl create service clusterip productservice --tcp=8080:8080 --dry-run=client -o=yaml > service.yaml
kubectl create -f deployment.yaml -n productservice
kubectl create -f service.yaml -n productservice
kubectl get all -n productservice
kubectl logs pod/productservice-58fd9946c4-brd8d -n productservice
kubectl port-forward service/productservice 8080:8080 -n productservice


~~~
ty-master>kubectl get all -n productservice
NAME                                  READY   STATUS    RESTARTS   AGE
pod/productservice-58fd9946c4-v8l4k   1/1     Running   4          27m

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/productservice   ClusterIP   10.108.110.250   <none>        8080/TCP   41m

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/productservice   1/1     1            1           41m

NAME                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/productservice-58fd9946c4   1         1         1       41m
~~~


#### others:
kubectl delete ns test-namespace

kubectl label pods pod/productservice-58fd9946c4-v8l4k new-label=productservice
kubectl apply -f deployment.yaml -n productservice

Docker commands
#docker build -f Dockerfile -t productservice .
#docker images
#docker run -p 8080:8080 productservice


