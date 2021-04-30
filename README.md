
### Docker Download from
https://www.docker.com/products/docker-desktop


### Minikube Install from
https://minikube.sigs.k8s.io/docs/start/


### minikube steps:
~~~
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
~~~
kubectl delete ns test-namespace

kubectl label pods pod/productservice-58fd9946c4-v8l4k new-label=productservice
kubectl apply -f deployment.yaml -n productservice
~~~
### Docker commands
~~~
#docker build -f Dockerfile -t productservice .
#docker images
#docker run -p 8080:8080 productservice

~~~

### Advance:
~~~
kubectl create namespace productservice
kubectl config set-context --current --namespace=productservice
kubectl create -f deployment.yaml -n productservice (change replicas=2  if already deployment is there run "kubectl apply -f deployment.yaml -n productservice")
kubectl expose deployment productservice --type=LoadBalancer --name=productservice  --port=8080 (if already there delete using "kubectl delete service productservice")
kubectl describe services productservice
kubectl autoscale deployment productservice --cpu-percent=60 --min=1 --max=10
kubectl get hpa
kubectl get pods --output=wide
kubectl edit deploy productservice

minikube tunnel (any issue just run)
~~~
## Load Test using busybox
~~~
kubectl run -i --tty load-generator --image=busybox /bin/sh  
while true; do wget -q -O- http://productservice:8080/token/getAdminToken; done

jmeter -JSERVICE_HOST=127.0.0.1 -Jprotocol=http -Jport=8080 -JthreadCount=300 -JloadTime=120 -JstartUpTime=30 -JtestEnv=127.0.0.1 -n -t C:\Users\Dhananjay\Desktop\Kubernetes\CICD_PCF_Spring_Boot_With_JWT_Security-master\CICD_PCF_Spring_Boot_With_JWT_Security-master\jmeter_script.jmx -l .\target\TestResult.csv -e -o .\target\Results-TestResult

~~~

## login inside pod:
~~~
kubectl exec -it pod/productservice-58fd9946c4-kwc6k bash
~~~

