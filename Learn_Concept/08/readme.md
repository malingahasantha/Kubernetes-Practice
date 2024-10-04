# Deployment, Replication Controller and ReplicaSet

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