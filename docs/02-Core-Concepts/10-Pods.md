# Pods
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808169)
  
In this section, we will take a look at PODS.
- POD introduction
- How to deploy pod?

#### Kubernetes doesn't deploy containers directly on the worker node.

  ![pod](../../images/pod.PNG)
  
#### Here is a single node kubernetes cluster with single instance of your application running in a single docker container encapsulated in the pod.

- What if the number of users accessing your application increase and you need to scale your application. You need to add additional instances of your web application to share the load. Now, were would you spin up additional instances? Do we bring up a new container instance within the same POD?
No. We create new POD altogether with a new instance of the same application. As you can see we now have two instances of our web application running on two separate PODs on the same kubernetes system or node.

- What if the user base further increases and your current node has no sufficient capacity. Well then you can always deploy additional PODs on a new node in the cluster. You will have a new node added to the cluster to expand the clusters physical capacity.

![pod1](../../images/pod1.PNG)

#### Pod will have a one-to-one relationship with containers running your application.

- To scale up you create new PODs and to scale down you delete existing PODs. You do not add additional containers to an existing pod to scale your application.

  ![pod2](../../images/pod2.PNG)
  
## Multi-Container PODs
- A single pod can have multiple containers except for the fact that they are usually not multiple containers of the **`same kind`**.

- But sometimes you might have a scenario were you have a helper container that might be doing some kind of supporting task for our web application such as processing a user entered data processing a file uploaded by the user etc. and you want these helper containers to live alongside your application container. In that case you can have both of these containers part of the same pod so that when a new application container is created. The helper is also created and when it dies the helper also dies. since they are part of the same POD. The two containers can also communicate with each other directly by referring to each other as ‘localhost’ since they share the same network namespace. Plus they can easily share the same storage space as well.

- We just need to define what containers a POD consists of and the containers in a POD by default will have access to the same storage, the same network namespace, and same fate as in they will be created together and destroyed together.

- However also note that multi-pod containers are a rare use-case
  
  ![pod3](../../images/pod3.PNG)
  
## Docker Example (Docker Link)
  
  ![pod4](../../images/pod4.PNG)
  
## How to deploy pods?
Lets now take a look to create a nginx pod using **`kubectl`**.

- To deploy a docker container by creating a POD.
  ```
  $ kubectl run nginx --image nginx
  ```

- To get the list of pods
  ```
  $ kubectl get pods
  ```

 ![kubectl](../../images/kubectl.PNG)

K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/pods/pod/
- https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-intro/


