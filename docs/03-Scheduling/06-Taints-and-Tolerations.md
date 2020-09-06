# Taints and Tolerations
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/10277934)
  
In this section, we will take a look at taints and tolerations.
- Pod to node relationship and how you can restrict what pods are placed on what nodes.
- Taints are set on Node
- Tolerations are added to PODS

#### Taints and Tolerations are used to set restrictions on what pods can be scheduled on a node. 
- Pods which are tolerant to the particular taint node will only get scheduled on that node.

  ![tandt](../../images/tandt.PNG)
  
## Taints
- Use **`kubectl taint nodes`** command to taint a node.

  Syntax
  ```
  $ kubectl taint nodes <node-name> key=value:taint-effect
  ```
 
  Example
  ```
  $ kubectl taint nodes node1 app=blue:NoSchedule
  ```
  
- The taint effect defines what would happen to the pods if they do not tolerate the taint.
- There are 3 taint effects
  - **`NoSchedule`** : Existing PODS will continue to run on the Node but new PODS with the matching toleration will only be created.
  - **`PreferNoSchedule`**
  - **`NoExecute`** : Existing PODS will be evicted[Killed] on the Node but new PODS with the matching toleration will only be created.
  
- Remember taints and toleration are only meant to restrict nodes from accepting certain PODS. When a Node is marked as taint, it can only accept those POD which are has a same toleration set but it does not guarantee that POD with the same toleration will always be placed on that Node. i.e When a toleration is added in POD defination, it does not guaranted that POD will be deployed on the taint Node only.

- If your requirement is to restrict a POD to certain nodes it is achieved through another concept called Ask node affinity

  
  ![tn](../../images/tn.PNG)
  
## Tolerations
   - Tolerations are added to pods and it can be added by adding a **`tolerations`** section in pod defination.
     ```
     apiVersion: v1
     kind: Pod
     metadata:
      name: myapp-pod
     spec:
      containers:
      - name: nginx-container
        image: nginx
     tolerations:
     - key: "app"
       operator: "Equal"
       value: "blue"
       effect: "NoSchedule"
     ```
    
  ![tp](../../images/tp.PNG)
    

#### Taints and Tolerations does not tell the pod to go to a particular node. Instead, it tells the node to only accept pods with certain tolerations.
- To see this taint, run the below command
  ```
  $ kubectl describe node kubemaster |grep Taint
  ```
 
 ![tntm](../../images/tntm.PNG)
  
     
#### K8s Reference Docs
- https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/

