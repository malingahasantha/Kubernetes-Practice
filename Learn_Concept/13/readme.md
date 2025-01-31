# Static Pods, Manual Scheduling, Labels, and Selectors in Kubernetes


![kubernetes sample architecture diagram](img/01.png)

Above image is a kubernetes sample architecture diagram with the control plane on the left with the control plane components and kubelets, kubeproxy and some workloads on the right side running on the worker nodes. When we have multiple workloads running in either of the nodes or if we have multiple nodes as well, Schuler decides which pod has to go on which node. (scheduler responsible for deciding which pod has to go on which node). Let's assume we have provisioned a new pod and we have sent the request to create a new pod. This request will go to API server, API server creates an entry in the HCD database to create the pod, and then scheduler checks to which particular node out of the available nodes has to schedule this particular pod on. Because schedular is monitoring this particular pod or every single pod that is running or that is yet to run on the server. It is continuously monitoring it. This is based on uh the scheduling algorithm and various other factors. It basically sends that request to the kubelet. like it provides the details to the API server and API server sends the request to the kubelet on that particular node. Instruct the kubelet to run this pod on this particular node and the pod will be provisioned. Then kubelet will send the request back to API server and it will update the entry in the HCD database. And then it will send the response back to the client let's saying that your pod has been created. So the main component uh that is involved in this scheduling is scheduler. Scheduler is the one who takes the decision and send the request to kubelet to provision the pod or any pod.

Schedular as along with API server and other components (Controller Manager, HCD Database) is a control plane component. It has to be running all the time and and it is running as a pod. Let's check it with below command.

```
kubectl get pods -n kube-system |findstr scheduler
```

![list the pods in kube-system namespace](img/02.png)

Here ```findstr``` is equal to ```grep``` command in linux. 

So we can see ```kube-scheduler-cka-cluster3-control-plane``` is running as a pod. Scheduler is responsible for scheduling the pods, but if schedular itself is a pod, who's responsible for running it?

Reason for that is: there is a concept in kubernetes call ```static pods```. 

**Static pods are a special type of pod that runs directly on a specific node without being managed by the Kubernetes Control Plane (API Server, Scheduler, etc.). Instead, they are managed by the kubelet on that node. Unlike Pods that are managed by the control plane, Static Pods are always bound to one Kubelet on a specific node.**

control plane components so these static PS are the control plane components that are not managed byul so schul is not responsible for scheduling these type of BS but who does that cuet does that so we have cuet running on this node as well and cuet is responsible for checking that if we have any manifest of for these of the pods and if it finds it in a particular directory it will spin up the Pod so let me show you how it works so if we go back okay let's see on which node it is running although we


