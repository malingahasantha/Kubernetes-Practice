# Kubernetes Services - ClusterIP vs NodePort vs Loadbalancer vs External

## Services

Front end application can be accesible from the outside of the cluster, outside the node. can access publically by all the users.
User access the service between front end and user. That service will then request front end pod on which the service is exposed and it gives the response back to user.
Sameway another service will be use to access front end to the back end. 
Sameway this service or any other service will be use to get the data or write something to a external data source. 

![Services Explained.](img/01.png)


### Dirfferent services in Kubernetes
* Cluster IP
* NodePort
* External Names
* Loadbalancers

### NodePort

NodePort is the port where the application exposed (Application will exposed to a perticular port). (range for the NodePort is 30000 - 32767)

targetPort is the port which is application pod is listning on. This is not exposed externally. We are only expose NodePort externally. Node port will redirect the traffic to targetPort .

If I am an external user and if I am accessing the application on the web, I will access on the NodePort (30001). But our application will be listning targetPort (Port 80).

If I am a pod that want to access the frontend application and if I am a backend port, than I will access the port(port where specify on the service/port 80) these two ports are same, but they use to do different tasks.

![Target Port Explained.](img/02.png)
![Port Explained](img/03.png)

Describe the service-We can get explanation about service using below command and we can find apiVersion and kind with below command.
```
kubectl explain service
```

We can use below commands to get the existing services
```
kubectl get svc
```
or
```
kubectl get service
```
![Get the existing services](img/04.png)

Now our application should be accessible. The way we have created the cluster if it would have been a kubernetes cluster, or any other on premises cluster, this should have been working till now. But since we are using kind, and the way it works in kind cluster, it does not expose your port to the outside world. So, we have to do an extra work only for the kind cluster. It is port mapping. 

You can map extra ports from the nodes to the host machine with "extraPortMappings:"

Applying below commands in a yaml file, we can create a new clsuter with "extraPortMappings:", That's how it expose locally and that's how our application will be accessible.
Since we cannot change the existing cluster in kind we might have to create a new cluster.
You can read documentation for more information : https://kind.sigs.k8s.io/docs/user/quick-start/

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 30001
    hostPort: 30001
- role: worker
- role: worker
```

Use below command to create the new cluster.
```
kind create cluster --config kind.yaml --name cka-cluster3
```
![Get list of created nodes](img/05.png)

Now let's create the deployment. Let's get a copy of the previous desployment in 08 and apply it here.

![Created the deployment](img/06.png)

Let's see the list of pods created in deployment.
![Get list of created pods](img/07.png)

Now let's apply nodeport.yaml which we have created.

```
apiVersion: v1
kind: Service
metadata: 
  name: nodeport-svc
  labels: 
    env: demo-service
spec: 
  type: NodePort
  ports: 
  - nodePort: 30001
    port: 80
    targetPort: 80
  selector: 
    env: demo
```
![Apply the nodeport.yaml file](img/08.png)

Let's see the existing service.
![Get list of created pods](img/09.png)

With below command we can get more details about the nodeport. It shows labels, IPs, Port, TargetPort, NodePort...etc
```
kubectl describe svc nodeport-svc
```
![Describe the nodeport](img/10.png)

Our main goal is to make the service accessible over port 30001. Now if you run,
```
curl.exe localhost:30001
``` 
Since we are running kind in windows we need to run above command as "curl.exe", but if we are running it on mac or linux we can run it as below without ".exe",
```
curl localhost:30001
``` 
![run curl.exe localhost:30001](img/11.png)

We can try it in our browser as well now.
![run localhost:30001 in browser](img/12.png)

What we did is,
* we re-created the cluster because it is a kind cluster, kind cluster does not expose nodeport externally. So we had to do one extra task creating a new cluster which is with port mapping.
* We created cluster with one control-plane and two worker nodes
* Then we add below part, which says extraPortMapping on the container port 30001, to the host port 30001. That will expose our container port 30001 externally on the host. Through it we can access our application running on the local host.
* Then we created the deployment.
* Then we created the service, our service is now exposed.
```
  extraPortMappings:
  - containerPort: 30001
    hostPort: 30001
```

When we are running our cluster not on kind, by following the steps we did, our service should be accessible on the node that it is running.
For example, let's get a pod and describe the pod to find the node that pod is running. 
![get a pod and describe the pod](img/13.png)

Bydefauld our application should be accessible on above ip and port. But because this is a kind cluster it is exposed on localhost. That is the only difference.
```
172.18.0.2:30001
```