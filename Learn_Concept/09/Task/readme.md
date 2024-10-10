## 09 Task

1. Create a Service named myapp of type ClusterIP that exposes port 80 and maps to the target port 80.

2. Create a Deployment named myapp that creates 1 replica running the image nginx:1.23.4-alpine. Expose the container port 80.

3. Scale the Deployment to 2 replicas.

4. Create a temporary Pod using the image busybox and run a wget command against the IP of the service.

5. Run a wget command against the service outside the cluster.

6. Change the service type so the Pods can be reached outside the cluster.

7. Run a wget command against the service outside the cluster.

8. Discuss: Can you expose the Pods as a service without a deployment?

9. Discuss: Under what condition would you use the service types LoadBalancer, node port, clusterIP, and external?



1. Create a Service named myapp of type ClusterIP that exposes port 80 and maps to the target port 80.

Create below yaml file to create ClusterIP and apply it.

```
apiVersion: v1
kind: Service
metadata: 
  name: myapp
  labels: 
    env: task-service
spec: 
  type: ClusterIP
  ports: 
  - port: 80
    targetPort: 80
  selector: 
    env: task
```

![apply yaml file](img/01.png)

![list the current services](img/02.png)