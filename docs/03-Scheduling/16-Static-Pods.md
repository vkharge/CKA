# Static Pods 
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/10352432)
  
 In this section, we will take a look at Static Pods
 
  - The kubelet relies on the kube-apiserver for instructions on what PODs to load on its node, which was based on a decision made by the kube-scheduler which was stored in the ETCD datastore. What if there was no kube-apiserver, and kube-scheduler and no controllers and no ETCD cluster. What if there was no master at all.

  - What if there is a standalone node(kubelet and docker installed) without a master, and how we can deploy a POD on such a standalone node. Well, the kubelet can manage a node independently. kubectl responsibility is to create a POD, so kube knows how to create a POD. But how the kubelet get the POD details since we dont have API-Server
  
  - In order to create a POD we need a POD defination yaml, But how do you provide a pod definition file to the kubelet without a kube-api server? You can configure the kubelet to read the pod definition files from a directory on the server designated to store information about pods.
  
  - The pods definition files can be placed in the /etc/kubernetes/manifests directory, the Kubelet periodically checks this directory for files, reads these files and creates pods on the host. Not only does it create the pod it can ensure that the pod stays alive.
  
  - If the application crashes, the kubelet attempts to restart it. 
  
  - If you make a change to any of the file within this directory, the kubelet recreates the pod for those changes to take effect.
  
  - If you remove a file from this directory the part is deleted automatically.
  
  - So these PODs that are created by the kubelet on its own without the intervention from the API server or rest of the kuberentes cluster components are known as Static PODs
  

#### How do you provide a pod defination file to the kubelet without a kube-api server?
- You can configure the kubelet to read the pod defination files from a directory on the server designated to store information about pods.

## Confgure Static Pod
- The designated directory can be any directory on the host and the location of that directory is passed in to the kubelet as an option while running the service.
  - The option is named as **`--pod-manifest-path`** and set to /etc/kubernetes/manifests
  
  ![sp](../../images/sp.PNG)
  
## Another way to configure static pod 
- Instead of specifying the option directly in the **`kubelet.service`** file, you could provide a path to another config file using the config option, and define the directoy path as staticPodPath in the file.

  ![sp1](../../images/sp1.PNG)

- Cluster setup using the kubeadm tool uses this approach

## View the static pods
- To view the static pods
  ```
  $ docker ps
  ```
  ![sp2](../../images/sp2.PNG)

#### The kubelet can create both kinds of pods - the static pods and the ones from the api server at the same time.

 - The way the kubelet works is it can take in requests for creating PODS from different inputs.

 - The first is through the POD definition files from the static pods folder, as we just saw.
 
 - The second, is through an HTTP API endpoint. And that is how the kube-apiserver provides input to kubelet. The kubelet can create both kinds of PODs – the staticpods and the ones from the api server - at the same time. 
 
 - In case of static PODS, is the API server aware of the static pods created by the kubelet?  Yes it is. If you run the kubectl get pods command on the master node, the static pods will be listed as any other pod. Well how is that happening? When the kubelet creates a static pod, if it is part of a cluster, it also creates a mirror object in the kubeapi server. What you see from the kube-apiserver is just a read only mirror of the pod. You can view details about the pod but you cannot edit or delete it like the usual parts. You can only delete them by modifying the files from the nodes manifest folder.
 
 - Note that the name of the part is automatically appended with the node name.
 
  ![sp3](../../images/sp3.PNG)

## Static Pods - Use Case

  - Since static pods are not dependent on the Kubernetes control plane, you can use static pods to deploy the control plane components itself as pods on a node. 
  
  - Start by installing kubelet on all the master nodes.
  - Then create pod definition files that uses Docker images of the various control plane components such as the api server, controller, etcd etc.
  - Place the definition files in the designated manifests folder. 
  - And kubelet takes care of deploying the control plane components themselves as PODs on the cluster.
  
  - This way you don't have to download the binaries configure services or worry about so the service is crashing.
  
  - If any of these services were to crash since it's a static pod it will automatically be restarted by the kubelet. Neat and simple. 
  - That’s how the kubeadmin tool set’s up a Kubernetes cluster. 
  
  ![sp4](../../images/sp4.PNG)
  
  ![sp5](../../images/sp5.PNG)
  
## StaticPods vs DaemonSets

   ![spvsds](../../images/spvsds.PNG)
  

#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/
