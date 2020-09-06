# Node Selectors
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/10277935)

In this section, we will take a look at Node Selectors in Kubernetes

  - Consider you have a three node cluster of which two are smaller nodes with lower hardware resources and one of them is a larger node configured with higher resources. You would like to dedicate the data processing workloads that require higher horsepower to the larger node as that is the only node that will not run out of resources in case the job demands extra resources. However, in the current default setup, any pods can go to any nodes. So Pod which requires higher resources in this case may very well end up on nodes which has lower resouces and this is not desired.

- To solve this, We can set a limitation on the pods so that they only run on particular nodes.

- There are two ways to do this.
  - Using Node selectors which is the simple and easier method.
  - Node Affinity


# Node Selector

#### We add new property called Node Selector to the spec section and specify the label.
- A label is assigned to the Node such size=large
- A nodeSelector property is added with the node label in the POD spec which tell where the pod should be deployed

- The scheduler uses these labels to match and identify the right node to place the pods on.
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
![nsel](../../images/nsel.PNG)
  
- To label nodes

  Syntax
  ```
  $ kubectl label nodes <node-name> <label-key>=<label-value>
  ```
  Example
  ```
  $ kubectl label nodes node-1 size=Large
  ```
  
![ln](../../images/ln.PNG)
  
- To create a pod defination
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
  ```
  $ kubectl create -f pod-defination.yml
  ```
  
![nsel](../../images/nsel.PNG)
  
## Node Selector - Limitations
- We used a single label and selector to achieve our goal here. But what if our requirement is much more complex.

- Consider we have a requirement where we want to place a POD on the nodes which has labels Large or Medium, or place the POD on any Node that are not small. We cannot achieve this using node selector.
  
![nsl](../../images/nsl.PNG)
 
- For this we have **`Node Affinity and Anti Affinity`**
  
#### K8s Reference Docs
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector





