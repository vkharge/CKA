# OS Upgrades
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808229)
  
In this section, we will take a look at os upgrades.

#### If the node was down for more than 5 minutes, then the pods are terminated from that node
  
  - Consider a case if one of the node goes down, then users accessing the green pod are impacted as that was the only pod running the green application.
  
  - If the node came back online immediately, then the kubectl process starts and the pods come back online.
  
  - However, if the node was down for more than 5 minutes, then the pods are terminated from that node. 
  - If the PODs where part of a replicaset then they are recreated on other nodes. 
  
  - The time it waits for a pod to come back online is known as the pod eviction timeout and is set on the controller manager with a default value of five minutes. So whenever a node goes offline, the master node waits for upto 5 minutes before considering the node dead.
  
  - When the node comes back on line after the pod eviction timeout it comes up blank without any pods scheduled on it.

  - Since the blue pod was part of a replicaset, it had a new pod created On another node. However since the green pod was not part of the replica set it's just gone.
  
  - Thus if you have maintenance tasks to be performed on a node if you know that the workloads running on the Node have other replicas and if it's okay that they go down for a short period of time. And if you're sure the node will come back on line within five minutes you can make a quick upgrade and reboot. However you do not for sure know if a node is going to be back on line in five minutes.
  
  - So there is a safer way to do it. You can purposefully drain the node of all the workloads so that the workloads are moved to other nodes in the cluster. Well technically they are not moved. When you drain the node the pods are gracefully terminated from the node that they're on and recreated on another.

  - The node is also cordoned or marked as unschedulable. Meaning no pods can be scheduled on this node until you specifically remove the restriction.
  
  - Now that the pods are safe on the others nodes, you can reboot the first node. When it comes back online it is still unschedulable. You then need to uncordon it, so that pods can be scheduled on it again. 
  
  - Now, remember the pods that were moved to the other nodes, don’t automatically fall back. If any of those pods where deleted or if new pods were created in the cluster, Then they would be created on this node.
  
  - Apart from drain and uncordon, there is also another command called cordon. Cordon simply marks a node unschedulable. Unlike drain it does not terminate or move the pods on an existing node. It simply makes sure that new pods are not scheduled on that node. 

  ![os](../../images/os.PNG)
  
- You can purposefully **`drain`** the node of all the workloads so that the workloads are moved to other nodes.
  ```
  $ kubectl drain node-1
  
  $ kubectl drain node-1 --ignore-daemonsets
  *** Since it cannot delete the daemonsets managed pod such as funnel , kube-proxy, so we have to ignore the daemonsets
  
  $ kubectl drain node-1 --ignore-daemonsets --force
  *** By default, it will not allow to delete the system pods such as daemonsets and standalone pods running on the Node when the node is being marked for drain.
  *** Since there are standalone pods too that are not managed by RS, RC, Job, Daemonset, Statefull, use --force option  to delete the pods.
  *** Once the node is marked drain, the standalone pods will deleted and lost forever
  ```
- The node is also cordoned or marked as unschedulable.
- When the node is back online after a maintenance, it is still unschedulable. You then need to uncordorn it.
  ```
  $ kubectl uncordorn node-1
  ```
- There is also another command called cordorn. Cordorn simply marks a node unschedulable. Unlike drain it does not terminate or move the pods on an existing node.

  ![drain](../../images/drain.PNG)
  
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
