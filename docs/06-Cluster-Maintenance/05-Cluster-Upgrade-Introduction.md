# Cluster Upgrade Introduction
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808227)
  
#### Is it mandatory for all of the kubernetes components to have the same versions?
- No, The components can be at different release versions.
- The components can be at different release versions since the cube API server is the primary component in the control plane and that is the component that all other components talked to. None of the other components should ever be at a version higher than the cube API server, the controller manager and scheduler can be at one version lower.

 - So if cube API server was at X control or managers and kube schedulers can be at X minus one and the kubellet and kube proxy components can be at two versions lower X minus two.
 
 - If cube API server was at 1.0 10,
    - the controller manager and scheduler could be at 1.0 10 or 1.0 9 
    - the kubelet and cube proxy could be at 1.0 8 or 1.0 9 or 1.0 10
    - none of them could be at a version higher than the cube API server like 1.0 11.
 
#### At any time, kubernets supports only up to the recent 3 minor versions
- The recommended approach is to upgrade one minor version at a time.

- Say all the components are on v1.10
- say there are v1.11, and v1.12 versions available in the market
- K8s support only up to the recent three minor versions, so v1.12 being latest K8s supports v1.12, v1.11, and v1.10
- So when v1.13 is released, only supported versions will be v1.13 , v1.12, and v1.11

- Before the release of v1.13, it is good time to upgarde the cluster to next version

- So how do we upgrade, do we upgrade directly from v1.10 to v1.13? NO
- The recommended approach is to upgrade one minor version at a time. i.e v1.10 -> v1.11 -> v1.12 -> v1.13
  
  ![up2](../../images/up2.PNG)
  
#### Options to upgrade k8s cluster
 
  ![opt](../../images/opt.PNG)
  
## Upgrading a Cluster
- Upgrading a cluster involves 2 major steps
 	- First you upgrade your master nodes
 	- Then upgrade the worker nodes
 	
- while the master is being upgraded the control plane components such as the API server scheduler and controller managers go down briefly, the master going down does not mean your worker nodes and applications on the cluster are impacted, all workloads hosted on the worker nodes continue to serve users as normal.

- Since the Master is down all management functions are down and you cannot access the cluster using cube control or other coordinates API, you cannot deploy new applications or delete or modify existing ones.

- If a POD fails, a new pod won't be automatically created.

- But as long as the nodes and the pods are up, your applications should be up and users will not be impacted. Once the upgrade is complete and the cluster is back up it should function normally.
  
#### There are different strategies that are available to upgrade the worker nodes
- One is to upgrade all at once. But then your pods will be down and users will not be able to access the applications.
 
 One is to upgrade all of them at once but then your PODS are down and users are no longer able to access the applications. Once the upgrade is complete the nodes are back up new PODS are scheduled and users can resume access. That's one strategy that requires downtime.
 
  ![stg1](../../images/stg1.PNG)
  
- Second one is to upgrade one node at a time. 

  The second strategy is to upgrade one node at a time. So going back to the state where we have our master upgraded and nodes waiting to be upgraded, we first upgrade the first node where the workloads move to the second and third node. Once the first note is upgraded and backed up, update the second node where the workloads move to the first and third nodes and finally the third node where the workloads are shared between the first and second. We can follow the same procedure to upgrade the nodes from v1.11 to v1.12 and then from v1.12 to v1.13
  
  
  ![stg2](../../images/stg2.PNG)
  
  
- Third one would be to add new nodes to the cluster

  A third strategy would be to add new node to the cluster nodes with newer software version. This is especially convenient if you're on a cloud environment where you can easily provision new nodes and decommission old ones nodes with the newer software version can be added to the cluster. Move the workload over to the new and remove the old node until you finally have all new nodes with the new software version.
  
  ![stg3](../../images/stg3.PNG)
  
## kubeadm - Upgrade master node
- kubeadm has an upgrade command that helps in upgrading clusters.
  ```
  $ kubeadm upgrade plan
  		- Current version of cluster, kubeadm tool version, latest stable major version and last stable version in the same series

		  - List the version of control plane components and the latest stable major version for the components

		  - It also tell once the cluster is upgraded, you must manually upgrade the kubelet version on all the nodes
		  ** kubeadm does not install or upgrade kubelets

		   - It give command to upgrade the cluster.
  ```
  ![kube1](../../images/kube1.png)
  
- Upgrade kubeadm from v1.11 to v1.12
  ```
  $ apt-get upgrade -y kubeadm=1.12.0-00
  ```
- Upgrade the cluster
  ```
  $ kubeadm upgrade apply v1.12.0
  ```
- If you run the 'kubectl get nodes' command, you will see the older version. This is beceause in the output of the command it is showing the versions of kubelets on each of these nodes registered with the API Server and not the version of API Server iteself  
  ```
  $ kubectl get nodes
  ```
  
  ![kubeu](../../images/kubeu.PNG)
  
- Upgrade 'kubelet' on the master node
  ```
  $ apt-get upgrade kubelet=1.12.0-00
  ```
- Restart the kubelet
  ```
  $ systemctl restart kubelet
  ```
- Run 'kubectl get nodes' to verify
  ```
  $ kubectl get nodes
  ```
  
  ![kubeu1](../../images/kubeu1.PNG)
 
## kubeadm - Upgrade worker nodes
  
- From master node, run 'kubectl drain' command to move the workloads to other nodes
  ```
  $ kubectl drain node-1
  ```
- Upgrade kubeadm and kubelet packages
  ```
  $ apt-get upgrade -y kubeadm=1.12.0-00
  $ apt-get upgrade -y kubelet=1.12.0-00
  ```
- Update the node configuration for the new kubelet version
  ```
  $ kubeadm upgrade node config --kubelet-version v1.12.0
  ```
- Restart the kubelet service
  ```
  $ systemctl restart kubelet
  ```
- Mark the node back to schedulable
  ```
  $ kubectl uncordorn node-1
  ```
  
  ![kubeu2](../../images/kubeu2.PNG)
  
- Upgrade all worker nodes in the same way

  ![kubeu3](../../images/kubeu3.PNG)
  

#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
- https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-upgrade/
  
  
  
