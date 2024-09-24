## Replicaset
* Create a new Replicaset based on the nginx image with 3 replicas
* Update the replicas to 4 from the YAML
* Update the replicas to 6 from the command line

![Create ReplicaSet using yaml file](img/rscreated1.png)
![](img/rscreated2.png)
![Update the replicas to 6 from the command line](img/rsscaled.png)

## Deployment
1. Create a Deployment named nginx with 3 replicas. The Pods should use the nginx:1.23.0 image and the name nginx. The Deployment uses the label tier=backend. The Pod template should use the label app=v1.
2. List the Deployment and ensure the correct number of replicas is running.
3. Update the image to nginx:1.23.4.
4. Verify that the change has been rolled out to all replicas.
5. Assign the change cause "Pick up patch version" to the revision.
6. Scale the Deployment to 5 replicas.
7. Have a look at the Deployment rollout history.
8. Revert the Deployment to revision 1.
9. Ensure that the Pods use the image nginx:1.23.0.

Answers as follows:

1. Create a Deployment named nginx with 3 replicas.  
![Create a Deployment named nginx with 3 replicas.](img/deployment_1.png)

2. List the Deployment and ensure the correct number of replicas is running.
![List the Deployment and ensure the correct number of replicas is running.](img/deployment_1.1.png)

3. Update the image to nginx:1.23.4. 
4. Verify that the change has been rolled out to all replicas.
![Update the image to nginx:1.23.4.](img/deployment_2.png)

5. Assign the change cause "Pick up patch version" to the revision.
![Assign the change cause "Pick up patch version" to the revision.](img/change_cause_5.png)

6. Scale the Deployment to 5 replicas.
![Scale the Deployment to 5 replicas.](img/scale_deployment.png)

7. Have a look at the Deployment rollout history.
![Have a look at the Deployment rollout history.](img/look_deployment_history.png)

8. Revert the Deployment to revision 1.
![Revert the Deployment to revision 1.](img/revert_deployment.png)

9. Ensure that the Pods use the image nginx:1.23.0.
![Ensure that the Pods use the image nginx:1.23.0.](img/pods_use_previous_version.png)