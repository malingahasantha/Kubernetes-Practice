# Imperative VS Declarative | Pod in Kubernetes | YAML 

### Pod in Kubernetes

A pod is the smallest unit that exists in Kubernetes. It is similar to that of tokens in C or C++ languages. A specific pod can have one or more applications. The nature of pods is ephemeral, which means that in any case, if a pod fails, Kubernetes can and will automatically create a new replica or duplicate of the said pod and continue the operation. The pods have the capacity to include one or more containers based on the requirements. The containers can even be Docker containers. The pods in Kubernetes provide environmental dependencies, which include persistent storage volumes which means they are permanent and available to all pods in the cluster, and even configuration data that is required to run the container within the pod.

Pods in Kubernetes are like individual workers on a team. Each pod represents a specific task or process running in the cluster. They have their own unique address to communicate with other pods, storage space for saving data, and instructions on how to run their assigned job.

![pods in kubernetes](img/pod.png)

A pod in a Kubernetes cluster indicates a process that is currently operating, and a pod may contain one or more containers. All of those containers share a single IP address, as well as the pod’s storage, network, and any other requirements. A pod is a collection of one or more running containers, allowing for simple container movement within a cluster. 

### Imperative vs Declarative

Imperative configuration involves creating Kubernetes resources directly at the command line against a Kubernetes cluster. Declarative configuration defines resources within manifest files (It's like a configuration file) and then applies those definitions to the cluster.

![Imperative and Declarative ways use in kubernetes](img/01.png)

Create and run a particular image in a pod.

```
kubectl run NAME --image=image [--env="key=value"] [--port=port] [--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND] [args...]
```

Let's create an nginx pod using imperative way.

```
kubectl run nginx-pod --image=nginx:latest
```
Pod is directly don't go into the running state it goes into container creating state and then goes to running state.-

![Create a Pod using imperative way and list the current existing pods](img/02.png)

Let's create a pod using declarative way. For that we need to create a yaml file. yaml file extension can be ".yml as well as .yaml".

```
apiVersion: v1
kind: Pod
metadata: 
  name: nginx-pod-yaml
  labels: 
    env: demo-pod
spec: 
  containers: 
  - name: nginx-container
    image: nginx
    ports: 
    - containerPort: 80
```

In here ```metadata:``` and ```spec:``` are dictionary data type and ```containers:``` is a list data type.
![Create a Pod using declarative way and list the current existing pods](img/03.png)

We can use Execute a command in a container to go inside to container.

```
kubectl exec (POD | TYPE/NAME) [-c CONTAINER] [flags] -- COMMAND [args...]
```

```
kubectl exec -it nginx-pod-yaml -- sh
```
![use execute command to go inside the container](img/04.png)

#### Create your own yaml

We can write our imperative command and then we can output it in a yaml format.

We can use the ``` --dry-run=client ``` flag to preview the object that would be sent to our cluster, without really submitting it. It'll show you what will happen once run this command without applying it.

```
kubectl run nginx --image=nginx --dry-run=client
```

With this dry-run we can output this to a yaml file. 

```
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

This means share me the output of this in a yaml format. Once run this it will show us the yaml format of it.

![output of imperative command in yaml format](img/05.png)

With below command we can redirect this to a yaml file.

```
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod-new.yaml
```

![output of imperative command in to yaml file](img/06.png)

Then we can do the changers accordingly as per our requirements and our preferences. Then we can go ahead and apply this yaml.


Labels are really helpful to group our resources together. Let's say we have 100s of pods running and we want to know about all the pods are part of frontend application. We can just retrieve the information using labels tab. 

Let's check the labels of a particular pod.

![show labels of a particular pod](img/07.png)

Let's say we want to know on which node our pod is running. we can retrieve that with ```kubectl describe``` command as well. But with below command we can get all the extended information about a particular object.

```
kubectl get pods -o wide
```

![get all the extended information of a particular pod](img/08.png)

Same for the nodes we can use above commands to get the information about nodes as well.

```
kubectl get nodes
kubectl get nodes -o wide
```
![get the information of nodes](img/09.png)