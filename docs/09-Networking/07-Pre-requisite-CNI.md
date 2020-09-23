# Pre-requisite CNI

  - Take me to [Lecture](https://kodekloud.com/courses/539883/lectures/9808284)

In this section, we will take a look at **Pre-requisite Container Network Interface(CNI)**

## CNI Solutions

- So we take all of these ideas from the different solutions and move all the networking portions of it into a single program or code And since this is for the bridge network we call it bridge.

- So we created a program or a script that performs all the required tasks to get the container attached to a bridge network.

![cni1](../../images/cni1.PNG)

![cni2](../../images/cni2.PNG)

- For example you could run this program using its name bridge and specify that you want to add this container to a particular network namespace. The bridge program takes care of the rest so that the container runtime environments are relieved of those tasks.

- For example, whenever rkt or kubernetes creates a new container, they call the bridge plugin and pass the container id and namespace to get networking configured for that container.

![net-7](../../images/net7.PNG)

- So what if you wanted to create such a program for yourself? May be for a new networking type. If you where doing so. hat arguments and commands should it support. How do you make sure the program you create will work correctly with these run times. How do you know container run times like kubernetes or rkt will invoke your program correctly. That’s where we need some standards defined. A standard that defines , how a programe should look, how container runtime will invoke them so that everyone can adhere to a single set of standards and develop solutions that work across runtime. That’s where container network interface comes in. The CNI is a set of standards that define how programs should be developed to solve networking challenges in a container runtime environment. The programs are referred to as plugins.


- CNI defines how the plugin should be developed and how container run times should invoke them.
- CNI defines a set of responsibilities for container run times and plugin.
- For container runtimes CNI specifies that it is responsible for creating a network namespace for each container
- It should then identify the networks the container must attach to container runtime must then invoke the plugin.

- When a container is created using the ADD command and also invoke the plugin when the container is deleted using the Del command.
	It also specifies how to configure in network plugin on the container runtime environment using a JSON file.
  
- Any runtime should be able to work with any plugin.

- ***** Docker does not implements CNI standard, it has its own CNM (Container Network Model) standard
```
K8s uses CNI standard, how is docker used under K8s?
		For this 
		- K8s create a docker container without any network configuration
		  # docker run --network=none nginx

		- Invoke the the bridge plugin. 
		  # bridge add <cid> <ns>


		That is pretty much how kubernetes does it
```

- When kubernetes creates docker containers it creates them on the none network. It then invokes the configured CNI plugins who takes care of the rest of the configuration.

## Third Party Network Plugin Providers

- [Weave](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-installation)
- [Calico](https://docs.projectcalico.org/getting-started/kubernetes/quickstart)
- [Flannel](https://github.com/coreos/flannel/blob/master/Documentation/kubernetes.md)
- [Cilium](https://github.com/cilium/cilium)


## To view the CNI Network Plugins

- CNI comes with the set of supported network plugins. 

```
$ ls /opt/cni/bin/
bridge  dhcp  flannel  host-device  host-local  ipvlan  loopback  macvlan  portmap  ptp  sample  tuning  vlan
```




#### References Docs

- https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/


