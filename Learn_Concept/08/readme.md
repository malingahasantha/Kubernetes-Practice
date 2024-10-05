# Deployment, Replication Controller and ReplicaSet

### Replication Controller

One thing that we should make sure when running an application is we should make sure that user does not get an empty response even if the application crashes. 

Since we are using an orchestration system, it should have the ability to auto-heal the application or automatically spin up a new pod when an existing pod crashes. This ensures minimal downtime and seamless user experience.

The replication controller plays a key role in this by automatically creating a new pod (if specified) whenever a pod crashes. For high availability, it keeps multiple replicas of the same pods running simultaneously. This ensures that if one pod fails, a replica can seamlessly take over, preventing any disruption to the system.

![Replication Controller Intro](img/01.png)

Let's assume that it makes sure our application is highly available all the time because now we are not redirecting traffing to just one pod and we are redirecting traffic to replication controller. Replication controller has the load balancing logic and it responsible for redirecting the traffic to one of the active pad. Pods are running behind the replication controller and replication controller is responsible for redirecting traffic to one of the active pod. it has different mechanism like, it will check which pod is active or which pod is healthy and it will redirect the traffic using this load balancing algorithm to which it finds the most healthy pods.

Let's say if we have specified replicas as three. So it will make sure that we always have three replicas running, we always have ruuning three instances of that pod running all the time. So if one failes it spin up one and if two failes it spin up two pods. 

Let's say if we increase the resources and a particular node does not have enough resources. Because what pods are doing is they share the resources from the node. like CPU, memory, storage...etc everything shares from the node. let's assume it reaches to a point where the particular node is running out of the capacity. In that case what we can do is we can spin up a new node and we provision pod in the new node. Because this replication controller we are using can span multiple nodes. 

![Replication Controller redirect traffic to pods and it can span](img/02.png)

Let's create replication controller yaml file.

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
  labels: 
    env: demo
spec:
  template:
    metadata: 
      labels: 
        env: demo
      name: nginx
    spec:
      containers:
      - image: nginx
        name: nginx-rcpod
  replicas: 3 
```

We can apply above yaml using below command.

```
kubectl apply -f replication_controller.yaml
```
![Create replication Controller](img/03.png)

If we want to check the specification of the replication controller or the pods we can use below command.

```
kubectl describe pod nginx-rc-2mgsp
```

It will show you all the information about the pod, we mentioned in the command. If we does not specify a name, then it will show you all the information about all the pods in the node.

![describe pod](img/04.png)


### ReplicaSet

Different between Replication Controller and ReplicaSet is replication controller will only be used to manage the resources of the pods that were created as part of that particular replica controller, but with  ReplicaSet we can manage the existing pods as well, that were not part of the particular ReplicaSet. That is done with the help of another field called selector. Inside the selector we match labels. Match the labels of the pods that want to be part of the particular ReplicaSet.

* Replication Controller is the legacy version. Replica Set is the never verison. 
* Replication Controller is only be use to manage the resources the pods that were created as part of that Replica Controller.
* But with Replica Set we can manage the existing pods as well.
* We do that with the help of the another field called selector. Inside that we do match labels. 
* So we match the labels of the pods that we want to be part of this perticular replica set.

As an example, let's define environment as demo inside the matchLabels. Every pod that is running with this particular label will now be managed by the ReplicaSet. 

Let's create ReplicaSet using below yaml file.

```
apiVersion: apps/v1   # group/version - here we have to specify the group as well
kind: ReplicaSet
metadata: 
  name: nginx-rs
  labels:
    env: demo
spec: 
  template:
    metadata:
      labels: 
        env: demo
      name: nginx
    spec:
      containers:
      - image: nginx
        name: nginx-rspod
  replicas: 3
  selector: 
    matchLabels: 
      env: demo   
```

Apply the yaml file using below command.

```
kubectl apply -f replicaSet.yaml
```

![create ReplicaSet](img/05.png)

![list the existing pods](img/06.png)

Here you can see three pods I have highlited in green color are pods which already part of the replication controller.

Let's delete the replication controller with below command.

```
kubectl delete rc/nginx-rc
```

![delete replication controller](img/07.png)

* If we need to change the count of replicas, we can do the change in yaml file "replica: 5" and save and apply the change again. Then it will get apply the change make the change instantly. 

* Another way of do this change is instead of updating the yaml file you can do the change live object. Instead of the updating yaml file we can use below command. It will open a live object yaml and we can edit and update the live object yaml file and save it. As soon as we close the yaml file it will show us the message as in the below image. It will update it automatically. we don't need to apply it again.

```
kubectl edit rs/nginx-rs
```

![replicaSet edit command](img/08.png)

![live object yaml file](img/09.png)

Now we can see pod count is updated as we update above.

![list existing pods](img/10.png)

* Another way is update the pods count is scale command. we can use scale command to update pods count as below.

![scale command](img/11.png)


