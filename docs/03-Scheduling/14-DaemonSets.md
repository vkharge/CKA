# DaemonSets
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9815302)

In this section, we will take a look at DaemonSets.

#### DaemonSets are like replicasets, as it helps in to deploy multiple instances of pod. But it runs one copy of your pod on each node in your cluster.
  
  - Whenever a new node is added to the cluster a replica of the pod is automatically added to that node and when a node is removed the pod is automatically removed.
  
  ![ds](../../images/ds.PNG)
  
## DaemonSets - UseCases

  ![ds-uc](../../images/ds-uc.PNG)
  
  - The kubeproxy component can be deployed as a daemon set in the cluster.
  ![ds-uc-kp](../../images/ds-uc-kp.PNG)
  
  - Networking solutions like weave net requires an agent to be deployed on each node in the cluster.
  ![ds-ucn](../../images/ds-ucn.PNG)
  
## DaemonSets - Defination
- Creating a DaemonSet is similar to the ReplicaSet creation process.
- For DaemonSets, we start with apiVersion, kind as **`DaemonSets`** instead of **`ReplicaSet`**, metadata and spec. 
  ```
  apiVersion: apps/v1
  kind: Replicaset
  metadata:
    name: monitoring-daemon
    labels:
      app: nginx
  spec:
    selector:
      matchLabels:
        app: monitoring-agent
    template:
      metadata:
       labels:
         app: monitoring-agent
      spec:
        containers:
        - name: monitoring-agent
          image: monitoring-agent
  ```
  
  ```
  apiVersion: apps/v1
  kind: DaemonSet
  metadata:
    name: monitoring-daemon
    labels:
      app: nginx
  spec:
    selector:
      matchLabels:
        app: monitoring-agent
    template:
      metadata:
       labels:
         app: monitoring-agent
      spec:
        containers:
        - name: monitoring-agent
          image: monitoring-agent
  ```
  ![dsd](../../images/dsd.PNG)
  
- To create a daemonset from a defination file
  ```
  $ kubectl create -f daemon-set-defination.yaml
  ```

## View DaemonSets
- To list daemonsets
  ```
  $ kubectl get daemonsets
  ```
- For more details of the daemonsets
  ```
  $ kubectl describe daemonsets monitoring-daemon
  ```
  ![ds1](../../images/ds1.PNG)
  
## How DaemonSets Works

- set the nodeName property on the pod to bypass the scheduler and get the pod placed on a node directly. So that’s one approach. 

  On each pod, set the nodeName property in its specification before it is created and when they are created, they automatically land on the respective nodes. So that’s how it used to be until kubernetes version v1.12. 

  ![ds2](../../images/ds2.PNG)
  
- From v1.12 onwards, the Daemon set uses the default scheduler and node affinity rules that we learned in one of the previous lectures to schedule pods on nodes.

#### K8s Reference Docs
- https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/#writing-a-daemonset-spec
