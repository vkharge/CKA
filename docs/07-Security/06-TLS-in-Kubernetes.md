# TLS in Kubernetes
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808256)
  
In this section, we will take a look at TLS in kubernetes

- The kubernetes cluster consist of a set of master and worker nodes. Of course all communication between these nodes need to be secure and must be encrypted. All interactions between all services and their clients need to be secure. For example an administrator interacting with the kubernetes cluster through the kubectl utility or via accessing the kubernetes API directly must establish secure TLS connection. Communication between all the components within the kubernetes cluste also need to be secured, so the two primary requirements are to have all the various services within the cluster to use server certificates and all clients to use client certificates to verify they are who they say they are.

#### The two primary requirements are to have all the various services within the cluster to user server certificates and all clients to use client certificates to verify they are who they say they are.
- Server Certificates for Servers
- Client Certificates for Clients

  ![tls](../../images/tls.PNG)
  
#### Let's look at the different components within the k8s cluster and identify the various servers and clients and who talks to who.

  ![certs](../../images/certs.PNG)
  
