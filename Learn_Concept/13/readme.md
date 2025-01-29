# Static Pods, Manual Scheduling, Labels, and Selectors in Kubernetes


![kubernetes sample architecture diagram](img/01.png)

Above image is a kubernetes sample architecture diagram with the control plane on the left with the control plane components and kubelets, kubeproxy and some workloads on the right side running on the worker nodes. When we have multiple workloads running in either of the nodes or if we have multiple nodes as well, Schuler decides which pod has to go on which node. (scheduler responsible for deciding which pod has to go on which node). Let's assume we have provisioned a new pod and we have sent the request to create a new pod. This request will go to API server, API server creates an entry in the HCD database to create the pod, and then scheduler checks to which particular node out of the available nodes has to schedule this particular pod on. Because schedular is monitoring this particular pod or every single pod that is running or that is yet to run on the server. It is continuously monitoring it. This is based on uh the scheduling algorithm and various other factors. It basically sends that request to the kubelet. like it provides the details to the API server and API server sends the request to the kubelet on that particular node. Instruct the kubelet to run this pod on this particular node and the pod will be provisioned. Then kubelet will send the request back to API server and it will update the entry in the HCD database. And then it will send the response back to the client let's saying that your pod has been created. So the main component uh that is involved in this scheduling is scheduler. Scheduler is the one who takes the decision and send the request to kubelet to provision the pod or any pod.

Schedular as along with API server and other components (Controller Manager, HCD Database) is a control plane component.


