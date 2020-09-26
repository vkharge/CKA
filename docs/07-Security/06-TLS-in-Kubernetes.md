# TLS in Kubernetes
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808256)
  
In this section, we will take a look at TLS in kubernetes

- The kubernetes cluster consist of a set of master and worker nodes. Of course all communication between these nodes need to be secure and must be encrypted. All interactions between all services and their clients need to be secure. For example an administrator interacting with the kubernetes cluster through the kubectl utility or via accessing the kubernetes API directly must establish secure TLS connection. Communication between all the components within the kubernetes cluste also need to be secured, so the two primary requirements are to have all the various services within the cluster to use server certificates and all clients to use client certificates to verify they are who they say they are.

#### The two primary requirements are to have all the various services within the cluster to user server certificates and all clients to use client certificates to verify they are who they say they are.
- Server Certificates for Servers
- Client Certificates for Clients

  ![tls](../../images/tls.PNG)
  
#### Let's look at the different components within the k8s cluster and identify the various servers and clients and who talks to who.

	
	```
	Server Components: apiserver [apiserver.crt, apiserver.key]
					   ETCD      [etcd.crt, etcd.key]
					   kubelet   [kubelet.crt, kubelet.key]

	Client Component: Admin Users[kubectl, API]  ---> apiserver
					  schedular   [schedular.crt, schedular.key]    ---> apiserver
					  controller  [controller.crt, controller.key]    ---> apiserver
					  proxy 	  [proxy.crt, proxy.key]    ---> apiserver
					  kubelet 	  [kubelet-client.crt, kubelet-client.key]    ---> apiserver


					  apiserver [apiserver-etcd-client.crt, apiserver-etcd-client.key]    	 ---> etcd
					  apiserver [apiserver-kubelet-client.crt, apiserver-kubelet-client.key]    	---> kubelet


	CA :  ca.crt , ca.key
    ```

- Kubernetes requires you to have one certificate authority for your cluster.

- In fact you can have more than one one for all the components in the cluster and another one specifically for ETCD. In that case the ETCD servers certificates and the ETCD servers client certificates, which in this case is the api-server client certificate will be all signed by the ETCD server CA.

![certs](../../images/certs.PNG)
  
