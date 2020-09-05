# Namespaces
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808159)
  
In this section, we will take a look at **`Namespaces`**

So far in this course we have created **`Objects`** such as **`PODs`**, **`Deployments`** and **`Services`** in our cluster. Whatever we have been doing we have been doing in a **`NAMESPACE`**.

- When the cluster is first set up, kubernetes creates a set of pods and services for its internal purpose such as those required by the networking solution, dns services and etc. To isolate these from the user and to prevent you from accidentally deleting or modifying these services, kubernetes creates them under another namespace created at cluster startup.

- When a K8s cluster is installed/deployed, certain namespaces are created automatically by K8s and those are .:
  - kube-system [In this namespace different K8s services run]
  - kube-public [In this namespace different K8s resoucres required for users are run]
  - default [This is namespace where user can deploy its own custome services]
  
-  When you use a kubernetes cluster for enterprise or production purposes you may want to consider the use of namespace as you can create your own namespaces as well.

- Each namespaces has set of polices that defines who can do what. You can also assign quota of resources to each of these named spaces that each namespace is guaranteeda certain amount and does not use more than it's allowed limit.

- To refer a service named 'vdr-svc' in 'dev' namespace:
  
  - from its won namespace: directly we can use service name such as 'vdr-svc'
  
  - from different namespaces:  'vdr-svc.dev.svc.cluster.local'
    When a service is created, a dns entry is automatically created in this format
     - vdr-svc : Name of the service
     - dev : Name of the namespace
     - svc : Subdomain for the service
     - cluster.local : Default domain name of the K8s cluster
    

- This namespace is the **`default`** namespace in kuberntes. It is automatically created when kubernetes is setup initially.

  ![ns](../../images/ns.PNG)
 
- You can create your own namespaces as well.

  ![ns3](../../images/ns3.PNG)
  
- To list the pods in default namespace
  ```
  $ kubectl get pods
  ```
- To list the pods in another namespace. Use **`kubectl get pods`** command along with the **`--namespace`** flag or argument.
  ```
  $ kubectl get pods --namespace=kube-system
  ```
  ![ns8](../../images/ns8.PNG)
  
- Here we have a pod defination file, when we create a pod with pod-defination file, the pod is created in the default namespace.
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
     app: myapp
     type: front-end
spec:
  containers:
  - name: nginx-container
    image: nginx
 ```
  ```
  $ kubectl create -f pod-defination.yaml
  ```
- To create the pod with the pod-defination file in another namespace, use the **`--namespace`** option.
  ```
  $ kubectl create -f pod-defination.yaml --namespace=dev
  ```
  ![ns9](../../images/ns9.PNG)

- If you want to make sure that this pod gets you created in the **`dev`** env all the time, even if you don't specify in the command line, you can move the **`--namespace`** defination into the pod-defination file.
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  namespace: dev
  labels:
     app: myapp
     type: front-end
spec:
  containers:
  - name: nginx-container
    image: nginx
 ```
  
  ![ns10](../../images/ns10.PNG)
  
- To create a new namespace, create a namespace defination as shown below and then run **`kubectl create`**
```
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

  ```
  $ kubectl create -f namespace-dev.yaml
  ```
  Another way to create a namespace
  ```
  $ kubectl create namespace dev
  ```
  ![ns11](../../images/ns11.PNG)
  
- By default, we will be in a **`default`** namespace. To switch to a particular namespace permenently run the below command.
  ```
  $ kubectl config set-context $(kubectl config current-context) --namespace=dev
  ```
- To view pods in all namespaces
  ```
  $ kubectl get pods --all-namespaces
  ```
  ![ns12](../../images/ns12.PNG)
  
- To limit resources in a namespace, create a resource quota. To create one start with **`ResourceQuota`** defination file.
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```
  ```
  $ kubectl create -f compute-quota.yaml
  ```
  ![ns13](../../images/ns13.PNG)
 
## Set Namespace to context ad commands help




 - kubectl config help
```
	$ kubectl config --help

	Available Commands:

	  current-context Displays the current-context
	  delete-context  Delete the specified context from the kubeconfig
	  get-contexts    Describe one or many contexts
	  rename-context  Renames a context from the kubeconfig file.
	  set-context     Sets a context entry in kubeconfig
	  use-context     Sets the current-context in a kubeconfig file
	  

	  delete-cluster  Delete the specified cluster from the kubeconfig
	  get-clusters    Display clusters defined in the kubeconfig
	  set-cluster     Sets a cluster entry in kubeconfig


	  set-credentials Sets a user entry in kubeconfig
	  set             Sets an individual value in a kubeconfig file
	  unset           Unsets an individual value in a kubeconfig file
	  view            Display merged kubeconfig settings or a specified kubeconfig file
```

- Every context have a cluster associated with it:
	
	- To get all contexts in K8s
	```
	$ kubectl config get-contexts
	CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
	*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin
   ```

	-  To get all custers names
	```
	$ kubectl config get-clusters
	NAME
	kubernetes
	```

	-  To get current context name
	```
	$ kubectl config current-context
	kubernetes-admin@kubernetes
	```	

	- To set a default namespace for a specific context
	  - Create two namespaces:
	  ```
	  $ kubectl get namespaces
		NAME              STATUS   AGE
		default           Active   16m
		kube-node-lease   Active   16m
		kube-public       Active   16m
		kube-system       Active   16m


	  $ kubectl create namespace test
	  $ kubectl create namespace dev

	  $ kubectl get namespaces
		NAME              STATUS   AGE
		default           Active   17m
		dev               Active   4s
		kube-node-lease   Active   17m
		kube-public       Active   17m
		kube-system       Active   17m
		test              Active   12s


      $ kubectl config get-contexts
		CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
		*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin


      
      $ kubectl config set-context kubernetes-admin@kubernetes --namespace=dev


      $ kubectl config get-contexts
		CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
		*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   dev


	  $ kubectl config set-context $(kubectl config current-context) --namespace test
	  OR
	  $ kubectl config set-context `kubectl config current-context` --namespace dev


      $ kubectl config get-contexts
		CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
		*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   test
	  ``` 



K8s Reference Docs:
- https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
- https://kubernetes.io/docs/tasks/administer-cluster/namespaces-walkthrough/
- https://kubernetes.io/docs/tasks/administer-cluster/namespaces/
- https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/
- https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/
  
  
