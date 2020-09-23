# CNI in Kubernetes

  - Take me to [Lecture](https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests/lectures/9808289)

In this section, we will take a look at **Container Networking Interface (CNI) in Kubernetes**

## Configuring CNI

- The CNI plugin must be invoked by the kubelet component within Kubernetes that is responsible for creating containers. Because that component must then invoke the appropriate network plugin after the container is created.

- CNI plugin is invoked by the kubeleton each node

- The CNI plugin is configured in the kubelet service on each node in the cluster. 

![net-1](../../images/net1.PNG)


- Check the status of the Kubelet Service

```
$ systemctl status kubelet.service
```

## View Kubelet Options

```
$ ps -aux | grep kubelet
```

## Check the Supportable Plugins 

- The CNI bin directory has all the supported CNI plugins as executables. Such as the bridge, dhcp, flannel etc. 
- To check the all supportable plugins available in the `/opt/cni/bin` directory.

```
$ ls /opt/cni/bin

```

## Check the CNI Plugins

- The CNI config directory has a set of configuration files. This is where kubelet looks to find out which plugin needs to be used. In this case it finds the bridge configuration file. If there are multiple files here, It will choose the one in alphabetical order. 

- To check the cni plugins which kubelet needs to be used.

```
ls /etc/cni/net.d

```

## Format of Configuration File  

- The isGateway defines whether the bridge network interface should get an IP address assigned so it can act as a gateway. 
- The ipMasquerade defines if a NAT rule should be added for IP masquerading. The IPAM section defines IPAM configuration. 

![net-2](../../images/net2.PNG)


#### References Docs

- https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/
- https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/
