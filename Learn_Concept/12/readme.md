# Kubernetes - Daemonsets, Job and Cronjob

A DaemonSet is a deployment mechanism that ensures a single pod of a specific type runs on every node in a cluster. Whenever a new node is added to the cluster, It automatically deploys the pod on it, and when a node is removed, the pod will also be deleted. If the DaemonSet itself is removed, all the pods it created will also be deleted.

In ReplicaSet we are the one who decide how many replicas we want. Pods are spread across the cluster nodes. Some nodes may have no Pods, while others have multiple. 

DaemonSets are commonly used for tasks like running background services on every node, such as: 
* system maintenance, 
* log collection, 
* monitoring tools like Prometheus, or managing storage volumes.

### Key Characteristics:

* **Node-Level Deployment:** Unlike Deployments, which manage a fixed number of replicas, DaemonSets aim to run a single instance of the specified pod on each eligible node in the cluster.
* **Self-Healing:** If a DaemonSet pod on a node fails or the node itself is removed, the DaemonSet controller automatically creates a new pod on a replacement node.
* **Node-Level Services:** DaemonSets are often used for deploying node-level services, such as:
    * **Log collection agents:** (e.g., Fluentd, Logstash) to collect logs from all nodes.
    * **Monitoring agents:** (e.g., Prometheus Node Exporter) to gather node-level metrics.
    * **Network plugins:** To provide network functionality for the cluster.
    * **Storage daemons:** For managing local storage on nodes.

### How it Works:

1. **Creation:** When you create a DaemonSet, you define the desired pod specification.
2. **Pod Creation:** The DaemonSet controller analyzes the cluster nodes and creates a pod for the specified pod template on each eligible node.
3. **Maintenance:** The controller continuously monitors the cluster and ensures that:
    * A pod is running on each eligible node.
    * If a pod fails or a node is removed, a new pod is created on a replacement node.