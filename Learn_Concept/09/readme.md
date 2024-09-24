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

Target port is the port which is application pod is listning on. This is not exposed externally. We are only expose NodePort externally. Node port will redirect the traffic to Target port.

If I am an external user and if I am accessing the application on the web, I will access on the NodePort (30001). But our application will be listning target port (Port 80).

If I am a pod that want to access the frontend application and if I am a backend port, than I will access the port(port where specify on the service/port 80) these two ports are same, but they use to do different tasks.

![Target Port Explained.](img/02.png)
![Port Explained](img/03.png)