# Manual Scheduling
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9815303)
  
In this section, we will take a look at **`Manually Scheduling`** a **`POD`** on a node.

## How Scheduling Works
- What do you do when you do not have a scheduler in your cluster? [i.e the schedular service is down in the cluster or you do want to use the inbuild schedular]
  - You want to schedule the POD on a specific Node
  - Every POD has a field called NodeName that by default is not set. You don't typically specify this field when you create the manifest file, kubernetes adds it automatically.
  - Schedular identifies the Node on which the POD should be deployed
  - Once identified it schedules the POD on the node by setting the nodeName property to the name of the node by creating a binding object.
    ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: nginx
     labels:
      name: nginx
    spec:
     containers:
     - name: nginx
       image: nginx
       ports:
       - containerPort: 8080
     nodeName: node02
    ```
    ![sc1](../../images/sc1.png)
    
## No Scheduler
  - You can manually assign pods to node itself. Well without a scheduler, to schedule pod is to set **`nodeName`** property in your pod defination file while creating a pod.
  - nodeName can be set in pod defination file, and specify the pod defination file during the POD creation. 
    
    ![sc2](../../images/sc2.PNG)
    
  - What if the pod is already created and you want to assign the pod to a node? Kubernetes won’t allow you to modify the node Name property of a pod. So another way to assign a note to an existing pod is to create a binding object and send a post request to the pod binding API
    ```
    apiVersion: v1
    kind: Binding
    metadata:
      name: nginx
    target:
      apiVersion: v1
      kind: Node
      name: node02
    ```
    ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: nginx
     labels:
      name: nginx
    spec:
     containers:
     - name: nginx
       image: nginx
       ports:
       - containerPort: 8080
    ```
    
 - In the binding object you specify a target node with the name of the node. Then send a post request to the pods binding API with the data set to the binding object in a JSON format. 
    
    ![sc3](../../images/sc3.PNG)
    
    
K8s Reference Docs:
- https://kubernetes.io/docs/reference/using-api/api-concepts/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodename
    
    
   
