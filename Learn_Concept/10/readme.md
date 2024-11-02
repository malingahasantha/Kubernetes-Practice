
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