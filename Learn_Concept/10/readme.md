
# Kubernetes – Namespaces

Kubernetes Namespace is a mechanism that enables you to organize resources. It is a logical partitioning mechanism used to divide a single Kubernetes cluster into multiple virtual clusters. It is like a virtual cluster inside the cluster. A namespace isolates the resources from the resources of other namespaces. It allows you to organize and manage resources such as Pods, Services, and ConfigMaps, in isolated environments. Namespaces provide a way to create boundaries within the same cluster, making it possible to manage large-scale deployments, multi-team environments, and various application stages (development, testing, production) efficiently.

Each Kubernetes resource is created in a namespace, and resources in different namespaces are logically separated. This means that resources in one namespace, such as a Pod or Service, won’t interact with resources in another unless explicitly allowed. Namespaces are particularly useful in scenarios where multiple teams or applications share the same Kubernetes cluster, helping to prevent name collisions and allowing for more fine-grained control over resource limits, security, and access control.

Kubernetes comes with some default namespaces like:

* ```default```: The default namespace for resources without a specified namespace.
* ```kube-system```: Reserved for system components of Kubernetes.
* ```kube-public```: Public information that can be accessed across the cluster.

By using namespaces, teams can manage large Kubernetes environments more effectively, ensuring a clean separation between different projects, environments, or departments.

We can list the current namespaces with below commands.

```
kubectl get ns
```
or
```
kubectl get namespace
```

![List current namespaces](img/01.png)

The resources we are creating without specifying any namespace are go in to the default namespace.

```
kube-node-lease      Active   37d
kube-public          Active   37d
kube-system          Active   37d
```

Above namespaces are used by kubernetes for it's own purpose. If we want to check what's inside these namespaces, we can use below command.

```
kubectl get all --namespace=kube-system
```

![Inside the namespace](img/02.png)

* First couple of lines have control plane components.
* Then we have kube-dns service. kube-dns service is responsible for resolving your IPS to the host name within the cluster.
* Then we have deamon sets. 
* Next we can see deployments and then we can see replicaset.


There are multiple ways to create namespaces. First let's go with the declarative way.

```
apiVersion: v1
kind: Namespace
metadata: 
  name: demo-namespace
```

we don't have to specify spec and these three fields will be sufficient, so we just have to provide the API version name and a kind which is ```Namespace```. Let's apply it with below command.

```
kubectl apply -f .\namespace.yaml
```

![Namespace created](img/03.png)

We can delete the namespace with below command.

```
kubectl delete ns/demo-namespace
```

![delete Namespace](img/04.png)


Now, let's create the namespace imperative way. This way is pretty simple and straight forward. Below is the command.

```
kubectl create namespace demo-namespace
```

![imperative way to create Namespace](img/05.png)


Now, let's create deployment in the ```demo-namespace``` using imperative command.

```
kubectl create deployment nginx-ns-demo --image=nginx --namespace=demo-namespace
```

or

```
kubectl create deploy nginx-ns-demo --image=nginx -n demo-namespace
```

![create deployment in the Namespace](img/06.png)

If we run ```kubectl get deploy``` command without specifying a namespace, it will show the deployments in default namespace. So if we want to list the deployments in a specified namespace, we have to run the command specifying the namespace.

```
kubectl get deploy --namespace=demo-namespace
```

![list the deployments in a Namespace](img/07.png)

So everytime if you have to interact with a particular namespace, like whether you are getting any resources from that namespace or you are adding or deleting any resources from that namespace you have to specify the namespace name. 

Now, let's check the communication between two namespaces. To do that let's create a new deployment in the default namespace and try to communicate between pod inside ```demo-namespace``` and pod inside default namespace.

Let's create a new deployment inside default namespace with below command. Here we don't need to specify the namespace since we are creating the deployment in the default namespace.

```
kubectl create deploy nginx-test --image=nginx
```

I have split the terminal and left side is demo-namespace and right side is default namespace. Let's get the ip addresses of pods from both namespaces with below command.

```
kubectl get pods -n demo-namespace -o wide
```
```
kubectl get pods -o wide
```

![create new deployments inside default namespace](img/08.png)

Go to inside the pods with ```exec``` command. 

```
kubectl exec -it nginx-ns-demo-5987d8686c-dn6ps -n demo-namespace -- sh
```
```
kubectl exec -it nginx-test-5b77bfd686-zhqvl -- sh
```

![get the ip addresses of pods inside both namespaces](img/09.png)

Now let's run ```curl <ip address>``` command to access the particular pod. It is working. We can access another pod inside another namespace.

![access the pods from pods inside both namespaces](img/10.png)