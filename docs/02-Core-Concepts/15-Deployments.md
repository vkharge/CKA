# Deployments
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808165)



In this section, we will take a look at kubernetes deployments

#### Deployment is a kubernetes object. 

 - Deployments helps to achieve following things:
    - Deploy multiple instances of application in production environment
    - Whenever newer version of the application is available, upgrade the existing version of application with the new version without downtime
    - Providing rolling updates, upgrade one instance of the application at a time or based on the configured instances
    - Providing rollback option, if the upgraded application is not working as expected you can immediately rollback to the previous or specific version
    - Scaling of the application instances based on the user load
    - Pausing the application environment, make necessary changes to the environment and rollout the changes
 
 - The deployment provides us with the capability to upgrade the underlying instances seamlessly using rolling updates, undo changes, and pause and resume changes as required.
  
 ![deployment](../../images/deployment.PNG)
  
#### How do we create deployment?

```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: myapp-deployment
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
- Once the file is ready, create the deployment using deployment defination file
  ```
  $ kubectl create -f deployment-defination.yaml
  ```
- To see the created deployment
  ```
  $ kubectl get deployment
  ```
- The deployment automatically creates a **`ReplicaSet`**. To see the replicasets
  ```
  $ kubeclt get replicaset
  ```
- The replicasets ultimately creates **`PODs`**. To see the PODs
  ```
  $ kubectl get pods
  ```
    
  ![deployment1](../../images/deployment1.PNG)
  
- To see the all objects at once
  ```
  $ kubectl get all
  ```
  ![deployment2](../../images/deployment2.PNG)
  
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/
- https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
