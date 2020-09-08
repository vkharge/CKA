# Rolling Updates and Rollback
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808204)
  
In this section, we will take a look at rolling updates and rollback in a deployment

## Rollout and Versioning in a Deployment
  
  - when you first create a deployment it triggers a rollout, a new rollout creates a new deployment revision let's call it revision 1 
  
  - In the future when the application is upgraded meaning when the container version is updated to a new one a new rollout is triggered and a new deployment revision is created named revision 2. This helps us keep track of the changes made to our deployment and enables us to roll back to a previous version of deployment if necessary.

  ![rollv](../../images/rollv.PNG)
  
## Rollout commands
- You can see the status of the rollout by the below command
  ```
  $ kubectl rollout status deployment/myapp-deployment
  ```
- To see the history and revisions
  ```
  $ kubectl rollout history deployment/myapp-deployment
  ```
 
  ![rollc](../../images/rollc.PNG)
  
## Deployment Strategies
- There are 2 types of deployment strategies
  1. Recreate
      - Say for example you have five replicas of your web application instance deployed. One way to upgrade these to a newer version is to destroy all of these and then create newer versions of application instances meaning first destroy the five running instances and then deploy five new instances of the new application version.

      - The problem with this as you can imagine is that during the period after the older versions are down and before any newer version is up the application is down and inaccessible to users this strategy is known as the Recreate strategy.
      
      - This is not the default deployment strategy.
      
  2. RollingUpdate (Default Strategy)
  
      - In this we take down the older version and bring up a newer version one by one. This way the application never goes down and the upgrade is seamless.
      
      - This is the default deployment strategy.
  
  ![dst](../../images/dst.PNG)
  
## kubectl apply
- To update a deployment, edit the deployment and make necessary changes and save it. Then run the below command.
   - Update can be a image version change, no of replicas, labels modifications
  ```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
   name: myapp-deployment
   labels:
    app: nginx
  spec:
   template:
     metadata:
       name: myap-pod
       labels:
         app: myapp
         type: front-end
     spec:
      containers:
      - name: nginx-container
        image: nginx:1.7.1
   replicas: 3
   selector:
    matchLabels:
      type: front-end       
  ```
  ```
  $ kubectl apply -f deployment-defination.yaml
  ```
- Alternate way to update a deployment say for example for updating an image.
  ```
  $ kubectl set image deplyoment/myapp-deployment nginx=nginx:1.9.1
  ```
  ![ka](../../images/ka.PNG)
  
## Recreate vs RollingUpdate
  
  - 
  ![rcrl](../../images/rcrl.PNG)
  
## Upgrades
  - when a new deployment is created say to deploy five replicas. It first creates a Replicaset automatically, which in turn creates the number of PODs required to meet the number of replicas.

  - When you upgrade your application as we saw in the previous slide, the kubernetes deployment object creates a new replica set under the hood and starts deploying the containers there at the same time taking down the parts in the old replica set following a rolling update strategy.

  - This can be seen when you try to list the replicasets using the kubectl get replicasets command. Here we see the old replicaset with 0 PODs and the new replicaset with 5 PODs.
   
  ![up](../../images/up.PNG)
  
## Rollback

- Let's assume something's wrong with the new version of build. You used to upgrade so you would like to roll back your update.

- kubernetes deployments allow you to roll back to a previous revision to undo a change.

- run the command kubectl rollout undo followed by the name of the deployment. The deployment will then destroy the PODs in the new replicaset and bring the older ones up in the old replicaset. And your application is back to its older format.

- When you compare the output of the kubectl get replicasets command, before and after the rollback, You will be able to notice this difference before the rollback. The first replicaset had 0 PODs and the new replicaset had 5 PODs and this is reversed after the rollback is finished.

  
  ![rb](../../images/rb.PNG)
  
- To undo a change
  ```
  $ kubectl rollout undo deployment/myapp-deployment
  ```
  
## kubectl create
- To create a deplyoment
  ```
  $ kubectl create deployment nginx --image=nginx
  ```
## Summarize kubectl commands
```
$ kubectl create -f deployment-defination.yaml
$ kubectl get deployments
$ kubectl apply -f deployment-defination.yaml
$ kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
$ kubectl rollout status deployment/myapp-deployment
$ kubectl rollout history deployment/myapp-deployment
$ kubectl rollout undo deployment/myapp-deployment
```

![sum](../../images/sum.PNG)
 
#### K8s Reference Docs
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment
- https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment
