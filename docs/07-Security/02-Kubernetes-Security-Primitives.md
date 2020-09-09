# Kubernetes Security Primitives
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808248)
  
In this section, we will take a look at kubernetes security primitives

## Secure Hosts
  
  Differen ways to secure host:
    - root access disabled, password based authentication disabled, 
    - Only SSH key based authentication to be made available. 
    

 ![sech](../../images/sech.PNG)
  
## Secure Kubernetes

- API Server is the central and core part of K8s
- CMD and API interfaces communicate with API Server  to sechedule a different tasks
- So that’s the first line of defense. Controlling access to the API server itself. 


- We need to make two types of decisions.
  - Who can access?
  - What can they do?
 
  ![seck](../../images/seck.PNG)
  
## Authentication
- Who can access the API Server is defined by the Authentication mechanisms.
  - There are different ways that you can authenticate to the API server. Starting with 
    - user IDs and passwords stored in a static file, 
    - usern and tokens
    - certificates
    - even integration with external authentication providers like LDAP.
    - service accounts


  
## Authorization
- Once they gain access to the cluster, what they can do is defined by authorization mechanisms.
- Authorization is implemented using 
  - RBAC Authorization Role Based Access Control, where users are associated to groups with specific permissions.
  - ABAC Authorization  Attribute Based Access Control
  - Node Authorization
  - WebHook Mode


## TLS Certificates
- All communication with the cluster, between the various components such as the ETCD Cluster, kube-controller-manager, scheduler, api server, as well as those running on the working nodes such as the kubelet and kubeproxy is secured using TLS encryption.

 ![tls](../../images/tls.PNG)
 
## Network Policies
What about communication between applications within the cluster?

- By default all PODs can access all other PODs within the cluster.
- You can restrict access between them using Network Policies. 

  ![np](../../images/np.PNG)
  
