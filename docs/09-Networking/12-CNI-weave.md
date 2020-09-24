# CNI weave
  
  - Take me to [Lecture](https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests/lectures/9808283)

In this section, we will take a look at "CNI Weave in the Kubernetes Cluster"

- Agents or service are deployed on each cluster node and Each agent or peer stores a topology of the entire setup, that way they know the pods and their IPs on the other nodes.

- Weave creates its own bridge on the nodes and names it weave. Then assigns IP address to each network. 

- Remember that a single POD may be attached to multiple bridge networks. 

- For example you could have a pod attached to the weave bridge as well as the docker bridge created by Docker. What path a packet takes to reach destination depends on the route configured on the container. Weave makes sure that PODs get the correct route configured to reach the agent. And the agent then takes care of other PODs.

- When a packet is sent from one pod to another on another node, weave intercepts the packet and identifies that it's on a separate network. It then encapsulates this packet into a new one with new source and destination and sends it across the network.

- Once on the other side, the other weave agent retrieves the packet, decapsulates and routes the packet to the right POD.
  
## Deploy Weave

- Weave can be installed/deployed as a services or daemons on each node in the cluster
- Weave should be installed once the base kubernetes system is ready with nodes and networking configured correctly between the nodes and the basic control plan components are deployed 
- Most importantly the weave peers are deployed as a daemonset. A daemonset ensures that one pod of the given kind is deployed on all nodes in the cluster.

- Installing [weave net](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/) onto the Kubernetes cluster with a single command.

```
$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
serviceaccount/weave-net created
clusterrole.rbac.authorization.k8s.io/weave-net created
clusterrolebinding.rbac.authorization.k8s.io/weave-net created
role.rbac.authorization.k8s.io/weave-net created
rolebinding.rbac.authorization.k8s.io/weave-net created
daemonset.apps/weave-net created
```

## Weave Peers

```
$ kubectl get pods -n kube-system
NAME                                      READY   STATUS             RESTARTS   AGE
coredns-66bff467f8-894jf                  1/1     Running            0          52m
coredns-66bff467f8-nck5f                  1/1     Running            0          52m
etcd-controlplane                         1/1     Running            0          52m
kube-apiserver-controlplane               1/1     Running            0          52m
kube-controller-manager-controlplane      1/1     Running            0          52m
kube-keepalived-vip-mbr7d                 1/1     Running            0          52m
kube-proxy-p2mld                          1/1     Running            0          52m
kube-proxy-vjcwp                          1/1     Running            0          52m
kube-scheduler-controlplane               1/1     Running            0          52m
weave-net-jgr8x                           2/2     Running            0          45m
weave-net-tb9tz                           2/2     Running            0          45m
```

## View the logs of Weave Pod's

```
$ kubectl logs weave-net-tb9tz weave -n kube-system 
```

## View the default route in the Pod

```
$ kubectl run test --image=busybox --command -- sleep 4500
pod/test created

$ kubectl exec test -- ip route
default via 10.244.1.1 dev eth0
```


#### References Docs

- https://kubernetes.io/docs/concepts/cluster-administration/addons/
- https://www.weave.works/docs/net/latest/kubernetes/kube-addon/
