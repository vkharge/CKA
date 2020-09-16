# Cluster Architecture

  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808145)

In this section , we will take a look at the kubernetes Architecture at high level.
- 10,000 Feet Look at the Kubernetes Architecture

  ![Kubernetes Architecture](../../images/k8s-arch.PNG)
  
  ![Kubernetes Architecture 1](../../images/k8s-arch1.PNG)

```
Kubernetes Architecture:
=========================

	K8s cluster consist of set of nodes which may be physical or virtual deployed in on-premises or cloud infra to host applicatios in form of containers.


	K8s cluster clasifies 2 types of Nodes:


		- Manager Node: Responsible for Manage, Plan, Schedule, Monitor Nodes
			And also storing information about:
				- different nodes 
				- Number of containers on Nodes
				- Monitoring the containers on Nodes



		- Worker Node: Responsible for host applications As Container



	Kubernetes have  different componenets running on Master and Worker Nodes:


		Componenets running on master nodes are known as control plane components

		```
		+---------------------------------------------------------+           +--------------------------------------------+
		|  Manager                                                |           | +------------------+       Worker          |
		|                                                         |           | | kubelet          |                       |
		|                                                         |    +-------<+                  |                       |
		|  +------------------+           +-------------------+   |    |      | |                  |                       |
		|  |                  |           |                   |   |    |      | +------------------+                       |
		|  |  ETCD            |           |   kube-schedular  |   |    |      |                                            |
		|  |                  |           |                   |   |    |      | +-------------------+                      |
		|  |                  |           |                   |   |    |      | | kube-proxy        |                      |
		|  +----------+-------+           +--------+----------+   |    |      | |                   |                      |
		|             ^                            ^              |    |      | +-------------------+                      |
		|             |                            |              |    |      |                                            |
		|             |                            |              |    |      +--------------------------------------------+
		|             |                            |              |    |      |  Container Runtime                         |
		|         +---+----------------------------+------+       |    |      +--------------------------------------------+
		|         |                                       |       |    |
		|         |        kube-apiserver                 |       |    |
		|         |                                       +----------->+       +-------------------------------------------+
		|         |                                       |       |    |       | +-----------------+      Worker           |
		|         +-+------------------------------+------+       |    |       | | kubelet         |                       |
		|           |                              |              |    +--------<+                 |                       |
		|           |                              |              |            | |                 |                       |
		|           |                              |              |            | +-----------------+                       |
		|  +--------v---------+            +-------v-----------+  |            | +------------------+                      |
		|  |  Container       |            | kube-controller-  |  |            | | kube-proxy       |                      |
		|  |  Runtime         |            | manager           |  |            | |                  |                      |
		|  |                  |            |                   |  |            | +------------------+                      |
		|  |                  |            |                   |  |            |                                           |
		|  +------------------+            +-------------------+  |            +-------------------------------------------+
		|                                                         |            |   Container Runtime                       |
		|                                                         |            +-------------------------------------------+
		+---------------------------------------------------------+

		```




		On Manager [control plane components]:

			- etcd
				Etcd is a database that stores information in a key-value format. K8s stores all the information in the ETCD

			- kube-schedular
				scheduler identifies the right node to place a container on based on the containers resource requirement, worker node capacity, or any other policies ot constraints such as taints and toleration or node affinity rules on them.


			- controller-manager
				Controller takes care of different areas
				
				There are different types of controllers:
				
				- node-controller
					It takes care of nodes such as responsible for onboarding new nodes to the cluster, handling situation where nodes becomes unvailable or get destroyed 

				- replication-controller
					 the replication controller ensures that the desired number of containers are running at all times in the replication group


			- kube-apiserver
				The kube-apiserver is the primary management component of kubernetes.
				The kube-api server is responsible for :
					- orchestrating all operations within the cluster
					- It exposes the Kubernetes API which is used by externals users to perform management operations on the cluster as well as the various controllers to monitor the state of the cluster and make the necessary changes as required
					-  By the worker nodes to communicate with the server.

				kube-apiserver communicates with:
					- etcd
					- schedular
					- controllers
					- worker nodes

					And also provides interface to interact with external world


			- Container Runtime Engine:

				Entire management system or control plane components could be hosted in the form of containers. Such as DNS, DHCP, Networking components can be deployed in form of containerized application.
				So we need these software that can run containers and that's the container runtime engine.
				We can use any container runtime engine, but we will use docker as it most popular one.

				So we need to install ontainer runtime engine on all the nodes including the Master Nodes

		On Worker Node:

			- kubelet
				It is an agent that runs on each worker node in the cluster
				It listens for the instruction from the kube-apiserver and deploys or destroys the containers on the nodes
				kube-apiserver periodically fetches status report from the kubelet to monitor the status of nodes and containers running on it.



			- kube-proxy

				kube-proxy plays a important role when container application running on different worker nodes needs to communicate with each other
				Communication between worker nodes are enabled by another component that runs on the worker node known as the Kube-proxy service.
				The Kube-proxy service ensures that the necessary rules are in place on the worker nodes to allow the containers running on them to reach each other.


			- Container Runtime Engine:
```



K8s Reference Docs:
- https://kubernetes.io/docs/concepts/architecture/
