## 10 Task

1. Create two namespaces and name them ns1 and ns2

2. Create a deployment with a single replica in each of these namespaces with the image as nginx and name as deploy-ns1 and deploy-ns2, respectively.

3. Get the IP address of each of the pods (Remember the kubectl command for that?)

4. Exec into the pod of deploy-ns1 and try to curl the IP address of the pod running on deploy-ns2.

5. Your pod-to-pod connection should work, and you should be able to get a successful response back.

6. Now scale both of your deployments from 1 to 3 replicas.

7. Create two services to expose both of your deployments and name them svc-ns1 and svc-ns2.

8. exec into each pod and try to curl the IP address of the service running on the other namespace.

9. This curl should work.

10. Now try curling the service name instead of IP. You will notice that you are getting an error and cannot resolve the host.

11. Now use the FQDN of the service and try to curl again, this should work.

12. In the end, delete both the namespaces, which should delete the services and deployments underneath them.



### Answers:

### 1. Create two namespaces and name them ns1 and ns2

With below imperative commands we can create namespaces.

```
kubectl create namespace ns1
kubectl create namespace ns2
```

Also we can create namespaces with creating yaml file for each namespace and applying them. 

```
apiVersion: v1
kind: Namespace
metadata:
  name: ns1
---
apiVersion: v1
kind: Namespace
metadata:
  name: ns2
```

```
kubectl apply -f .\<file_name>.yaml
```

![create 2 namespaces](img/01.png)