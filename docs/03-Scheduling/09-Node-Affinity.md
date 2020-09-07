# Node Affinity
  - Take me to the [Video Tutorial](https://kodekloud.com/courses/539883/lectures/10277936)
  
In this section, we will talk about "Node Affinity" feature in kubernetes.

#### The primary feature of Node Affinity is to ensure that the pods are hosted on particular nodes.
- With **`Node Selectors`** we cannot provide the advance expressions such as and , or , not expressions
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   nodeSelector:
    size: Large
  ```
  ![ns-old](../../images/ns-old.PNG)
  
  - Node affinity features provides advanced features to limit POD placement on the node.
  
  - In the below case, we can place a POD on the node which has a label size=Large or size=Medium
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: In
              values: 
              - Large
              - Medium
  ```
  ![na](../../images/na.PNG)
  
  
 - In the below case, we can place a POD on the node which is not a small [ size=Small]

  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: NotIn
              values: 
              - Small
  ```
  ![na1](../../images/na1.PNG)
  
  
 - In the below case, we can place a POD on the node which is has a label size irrespective of the label value
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: Exists
  ```
  
  ![na2](../../images/na2.PNG)
  

- when the pods are created. These rules are considered and the pods are placed onto the right nodes.

- what if node affinity could not match a node with a given expression. In this case what if there are no nodes with a label called size?

- Say we had the labels configured on the Node and the pods are schedule and What if someone changes the label on the node at a future point in time. Will the pod continue to stay on the Node.?

- This is solved by the Node affinity type

## Node Affinity Types
- Available
  - requiredDuringSchedulingIgnoredDuringExecution
  - preferredDuringSchedulingIgnoredDuringExecution
- Planned
  - requiredDuringSchedulingRequriedDuringExecution
  - preferredDuringSchedulingRequiredDuringExecution
  
  ![nat](../../images/nat.PNG)
  
## Node Affinity Types States
  
  - During Scheduling: state where a pod does not exist and is created for the first time. when a pod is first created the affinity rules specified are considered to place the pods on the right note.
  
  - What if the nodes with matching labels are not available? If you select the required type which is the first one the scheduler will mandate that the pod be placed on a node with a given affinity. Rules if it cannot find one, the pod will not be scheduled. This type will be used in cases where the placement of the pod is crucial. If a matching node does not exist the pod will not be scheduled.
  
  - Let's say the pod placement is less important than running the workload itself. In that case you could set it to preferred and in cases where a matching node is not found. The scheduler will simply ignore node affinity rules and place the card on any available note.
  
  - During Execution: The second part of the property or the other state is during execution, during execution is the state where a POD has been running and a change is made in the environment that affects node affinity such as a change in the label of a node.
  
  - For example say an administrator removed the label we said earlier called size equals large from the node. Now what would happen to the pods that are running on the Node. Two types of node affinity available today has this value set too ignored which means pods will continue to run and any changes in node affinity will not impact them once they are scheduled.
  
  - The new types expected in the future only have a difference in the during execution phase, a new option called required during execution is introduced which will evict any pods that are running on nodes that do not meet affinity rules. In the earlier example, a pod running on the large node will be evicted or terminated if the label large is removed from the node.

  ![nats](../../images/nats.PNG)
  
  ![nats1](../../images/nats1.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/
- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
