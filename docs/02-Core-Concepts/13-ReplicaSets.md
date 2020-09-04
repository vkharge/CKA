# ReplicaSets
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808168)

In this section, we will take a look at the below
- Replication Controller
- ReplicaSet

#### Controllers are brain behind kubernetes
They are the processes that monitor kubernetes objects and respond accordingly.

## What is a Replica and Why do we need a replication controller?

 - What if for some reason our application crashes and the pod fails. Users will no longer be able to access our application to prevent users from losing access to our application. We would like to have more than one instance or pod running at the same time. That way if one failed we still have our application running on the other one. The replication controller helps us run multiple instances of a single pod in the cabinet as cluster thus providing high availability
 
 - Even if you have a single pod the replication controller can help by automatically bringing up a new pod when the existing one fails. Thus the replication controller ensures that the specified number of parts are running at all times
 
 - Replication controller is used for scaling of PODS based on the load
 
 - For example in this simple scenario we have a single pod serving a set of users when the number of users increase. We deploy additional pod to balance the load across the two parts if the demand further increases and if we were to run out of resources on the first node we could deploy additional pods across the other nodes in the cluster. As you can see the replication controller spans across multiple nodes in the cluster. It helps us balance the load across multiple paths on different nodes as well as scale our application.
 
  ![rc](../../images/rc.PNG)
  
  ![rc1](../../images/rc1.PNG)
  
## Difference between ReplicaSet and Replication Controller
- **`Replication Controller`** is the older technology that is being replaced by a **`ReplicaSet`**.
- **`ReplicaSet`** is the new way to setup replication. [Recommended]

## Creating a Replication Controller

## Replication Controller Defination File
  
   ![rc2](../../images/rc2.PNG)
  
```
    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: myapp-rc
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
```
  - To Create the replication controller
    ```
    $ kubectl create -f rc-defination.yaml
    ```
  - To list all the replication controllers
    ```
    $ kubectl get replicationcontroller
    ```
  - To list pods that are launch by the replication controller
    ```
    $ kubectl get pods
    ```
    ![rc3](../../images/rc3.PNG)
    
## Creating a ReplicaSet
  
## ReplicaSet Defination File

   ![rs](../../images/rs.PNG)

```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     selector:
       matchLabels:
        type: front-end
 ```
#### ReplicaSet requires a selector defination when compare to Replicaton Controller.
   
  - Selector:
    - Selector is mandatory in ReplicaSet but not in ReplicationController. However there is one major difference between replication controller and replica set, replicaset  requires a selector definition, the selector section helps the replica set to identify what parts fall under it.
    
    - If we have provided the contents of the POD definition in the file itself in the template, it's because replicasset can also manage PODS that were not created as part of the replica at creation. Say for example the PODS created before the creation of the replica set that match labels specified in the selector. The replica set will also take those PODS into consideration when creating the replicas.
    
    - The matchLabels selector simply matches the labels specified under it to the labels on the POD. The replica set selector also provides many other options for matching labels that were not available in the replication controller.


  - To Create the replicaset
    ```
    $ kubectl create -f replicaset-defination.yaml
    ```
  - To list all the replicaset
    ```
    $ kubectl get replicaset
    ```
  - To list pods that are launch by the replicaset
    ```
    $ kubectl get pods
    ```
   
    ![rs1](../../images/rs1.PNG)
    
## Labels and Selectors
#### What is the deal with Labels and Selectors? Why do we label pods and objects in kubernetes?

  - Say we deployed three instances of our front end web application as three part. We would like to create a replication controller or replica set to ensure that we have three active parts at any time. You can use it to monitor existing PODS. In case they were not created the replicaset will create them for you.
  
  - The role of the replica set is to monitor the PODS and if any of them were to fail deploy new ones. The replica set is in fact a process that monitors the PODS.

  - How does the replicast know which PODS to monitor ? This is where labelling our PODS during creation comes in handy we could now provide these labels as a filter for replica set under the selector section we use to match labels filter and provide the same label that we used while creating the parts. This way the replica set knows which PODS to monitor, the same concept of labels and selectors is used in many other places throughout K8s

  ![labels](../../images/labels.PNG)
  
  - Consider we have three existing PODS that were created already and we need to create a replica set to monitor the PODS to ensure there are a minimum of three running at all times when the replication controller is created. It is not going to deploy a new instance of PODS as three of them with matching labels are already created. In that case do we really need to provide a template section in the type replica set specification. Since we are not expecting the replica set to create a new port on deployment yes we do. Because in case one of the PODS were to fail in the future the replicas that needs to create a new one to maintain the desired number of pods and for the replica set to create a new pod the template definition section is required let's now look at how we scaled the replica set.
  
## How to scale replicaset
- There are multiple ways to scale replicaset
  - First way is to update the number of replicas in the replicaset-defination.yaml defination file. E.g replicas: 6 and then run 
 ```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 6
     selector:
       matchLabels:
        type: front-end
```

  ```
  $ kubectl apply -f replicaset-defination.yaml
  ```
  - Second way is to use **`kubectl scale`** command.
  ```
  $ kubectl scale --replicas=6 -f replicaset-defination.yaml
  *** This will not update the replicas in the replicaset-defination.yaml
  ```
  - Third way is to use **`kubectl scale`** command with type and name
  ```
  $ kubectl scale --replicas=6 replicaset myapp-replicaset
    *** This will not update the replicas in the replicaset-defination.yaml
  ```
    - Fourth way is to use **`kubectl edit`** command with type and name
  ```
  $ kubectl edit replicaset myapp-replicaset
    *** This will not update the replicas in the replicaset-defination.yaml
  ```
  ![rs2](../../images/rs2.PNG)

#### K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
- https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/
