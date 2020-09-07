# Multiple Schedulers 
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9815299)

In this section, we will take a look at multiple schedulers

- K8s has an algorithm that distributes pods across nodes evenly as well as takes into consideration the various conditions we specify through taints & tolerations and node affinity etc. But what if none of these satisfies your needs? 

- Kubernetes is highly extensible. You can write your own kubernetes scheduler program, package it and deploy it as the default scheduler or as an additional scheduler in the kubernetes cluster.

- In this case, your application can use your custom scheduler. Your kubernetes cluster can have multiple schedulers at the same time.

- When creating a POD or a Deployment you can instruct kubernetes to have the POD scheduled by a specific scheduler.

## Custom Schedulers
- You kubernetes cluster can schedule multiple schedulers at the same time.

  ![ms](../../images/ms.PNG)
  
## Deploy additional scheduler
- Download the binary
- We download the kube-scheduler binary and run it as a service with a set of options. One of the option is scheduler name. If not specified, It assumes in name of default scheduler. This kube-scheduler is the default scheduler.

  - To deploy an additional scheduler, you can use the same kube-scheduler binary or use one that you might have built for yourself, which makes more sense. In this case we are going to use the same binary to deploy the additional scheduler. This time we set the scheduler name to a custom name. This is important to differentiate the two schedulers and this is the name that we will be specifying in the pod definition file later on.

  ```
  $ wget https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler
  ```
  ![das](../../images/das.PNG)
  
## Deploy additional scheduler - kubeadm
   
- Let’s take a look at how it works with the kubeadm tool. The kubeadm tool deploys the scheduler as a POD. You can find the definition file it uses under the manifests folder. 

- We can create a custom scheduler by making a copy of the same file, and by changing the name of the scheduler. We set the name of the pod to my-custom-scheduler and we add a new option to the scheduler command to set a custom name for the scheduler.

- Finally an important option to look here is the leader-elect option. The leader-elect option is used when you have multiple copies of the scheduler running on different master nodes, in a High Availability setup where you have multiple master nodes with the kube-scheduler process running on both of them. 

- If multiple copies of the same scheduler are running on different nodes only one can be active at a time. That’s where the leader-elect option helps in choosing a leader who will lead scheduling activities.

- To get multiple schedulers working you must either set the leader-elect option to false, in case where you don’t have multiple masters. In case you do have multiple masters, you can pass in an additional parameter to set a lock object name. This is to differentiate the new custom scheduler from the default during the leader election process

- Once done, create the pod using the kubectl create command. Run the get pods command in the kube-system namespace and look for the new custom scheduler.
   
  ![dask](../../images/dask.PNG)
  
  - To create a scheduler pod
    ```
    $ kubectl create -f my-custom-scheduler.yaml
    ```
  
## View Schedulers
- To list the scheduler pods
  ```
  $ kubectl get pods -n kube-system
  ```

## Use the Custom Scheduler
- Create a pod defination file and add new section called **`schedulerName`** and specify the name of the new scheduler
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx
  spec:
    containers:
    - image: nginx
      name: nginx
    schedulerName: my-custom-scheduler
  ```
  ![cs](../../images/cs.png)
  
- To create a pod definaton
  ```
  $ kubectl create -f pod-defination.yaml
  ```
- To list pods
  ```
  $ kubectl get pods
  ```

## View Events
- To view events
  ```
  $ kubectl get events
  ```
  ![cs1](../../images/cs1.PNG)
  
## View Scheduler Logs
- To view scheduler logs
  ```
  $ kubectl logs my-custom-scheduler -n kube-system
  ```
  ![cs2](../../images/cs2.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/
  
