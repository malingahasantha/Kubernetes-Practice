
# Kubernetes – Multi Container Pod | Sidecar vs Init Container

A multi-container Pod in Kubernetes is a Pod that contains more than one container running within it. While a typical Pod often runs a single container, Kubernetes allows multiple containers to run within the same Pod, enabling them to share resources and communicate with each other efficiently.

### Characteristics of Multi-Container Pods
1. Shared Resources:
    * Containers in the same Pod share the same network namespace, which means they can communicate using localhost and share the same IP address.
    * They also share the Pod's storage volumes, enabling them to access shared data.

2. Tight Coupling:
    * Containers within a Pod are tightly coupled and are meant to work together to form a single unit of execution.

3. Lifecycle and Scheduling:
    * All containers in a Pod are managed together. They are scheduled on the same node and share the same lifecycle (started, stopped, or restarted together).

Let's say we have a pod running on your kubernetes cluster and it has one app container which is (let's say nginx) Now we can have additional containers to support this app container. these containers could be an init container an init container or this could be a sidecar container. 
    * init container: initialization container that actually runs before the app container to do certain tasks.
    * sidecar container: sidecar container that runs all the time with the app container and it provides certain input or it takes certain output from the app container it is also referred to as an helper container.

When the Pod start, the first container that will be started is the init container. so as soon as the init container completes and it finishes its work then only the app container will be started. So starting of app container is totally dependent on the init container and it works uh according to the app container. So whenever the app container restarts stops init container goes down with it. So it works in conjunction with the app container and it shares the resources of the Pod. Let's say we have allocated some memory, some CPU, some storage, to the Pod, all those resources will be shared among all the containers inside that Pod.

![multi container Pod diagram](img/01.png)