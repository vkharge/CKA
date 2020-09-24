# IPAM weave

  - Take me to [Lecture](https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests/lectures/9808295)

- IP Address Management in the Kubernetes Cluster

	- This section covers how are the virtual bridge networks in the nodes assigned an IP subnet. And how are the PODS assigned an IP.

	- Where is this information stored and who is responsible for ensuring there are no duplicate IP is assigned ?

	- It is the responsibility of the CNI plugin used. CNI plug is the network solution provider to take care of assigning IP to the containers.

	- CNI comes with two built in plugins to which you can outsource this task to:
		- host local plugin : For IP Address Management
		  *** Every network solution provider uses different bridge network IP Address Subnet

![net-3](../../images/net3.PNG)


- How weaveworks Manages IP addresses in the Kubernetes Cluster 

![net-4](../../images/net4.PNG)


## References Docs

- https://www.weave.works/docs/net/latest/kubernetes/kube-addon/
- https://kubernetes.io/docs/concepts/cluster-administration/networking/ 
