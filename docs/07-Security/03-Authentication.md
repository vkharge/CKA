# Authentication
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808250)
  
In this section, we will take a look at authentication in a kubernetes cluster

## Accounts

  ![auth1](../../images/auth1.PNG)
  
 - Throughout this section we will discuss how to secure our cluster by securing the communication between internal components and securing management access to the cluster through authentication and authorization mechanisms.
 
 
#### Different users that may be accessing the cluster security of end users who access the applications deployed on the cluster is managed by the applications themselves internally.

 ![acc1](../../images/acc1.PNG)
 
- So, we left with 2 types of users
  - Humans, such as the Administrators and Developers
  - Robots such as other processes/services or applications that require access to the cluster.
  

  ![acc2](../../images/acc2.PNG)
  
- All user access is managed by apiserver and all of the requests goes through apiserver.
 
  ![acc3](../../images/acc3.PNG)
  
## Authentication Mechanisms
- There are different authentication mechanisms that can be configured.

  ![auth2](../../images/auth2.PNG)
  
## Authentication Mechanisms - Basic
  
  ![auth3](../../images/auth3.PNG)
  
## kube-apiserver configuration
- If you set up via kubeadm then update kube-apiserver.yaml manifest file with the option.
  
  ![auth4](../../images/auth4.PNG)
  
## Authenticate User

- To authenticate using the basic credentials while accessing the API server specify the username and password in a curl command.
  ```
  $ curl -v -k http://master-node-ip:6443/api/v1/pods -u "user1:password123"
  ```
  ![auth5](../../images/auth5.PNG)
  
- We can have additional column in the user-details.csv file to assign users to specific groups.

  ![auth6](../../images/auth6.PNG)
  
## Note
 
 ![note](../../images/note.PNG)
  


```
Article on Setting up Basic Authentication
Setup basic authentication on kubernetes

    Note: This is not recommended in a production environment. This is only for learning purposes.

Follow the below instructions to configure basic authentication in a kubeadm setup.

Create a file with user details locally at /tmp/users/user-details.csv

    # User File Contents
    password123,user1,u0001
    password123,user2,u0002
    password123,user3,u0003
    password123,user4,u0004
    password123,user5,u0005


Edit the kube-apiserver static pod configured by kubeadm to pass in the user details. The file is located at /etc/kubernetes/manifests/kube-apiserver.yaml


    apiVersion: v1
    kind: Pod
    metadata:
      name: kube-apiserver
      namespace: kube-system
    spec:
      containers:
      - command:
        - kube-apiserver
          <content-hidden>
        image: k8s.gcr.io/kube-apiserver-amd64:v1.11.3
        name: kube-apiserver
        volumeMounts:
        - mountPath: /tmp/users
          name: usr-details
          readOnly: true
      volumes:
      - hostPath:
          path: /tmp/users
          type: DirectoryOrCreate
        name: usr-details


Modify the kube-apiserver startup options to include the basic-auth file


    apiVersion: v1
    kind: Pod
    metadata:
      creationTimestamp: null
      name: kube-apiserver
      namespace: kube-system
    spec:
      containers:
      - command:
        - kube-apiserver
        - --authorization-mode=Node,RBAC
          <content-hidden>
        - --basic-auth-file=/tmp/users/user-details.csv

Create the necessary roles and role bindings for these users:


    ---
    kind: Role
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      namespace: default
      name: pod-reader
    rules:
    - apiGroups: [""] # "" indicates the core API group
      resources: ["pods"]
      verbs: ["get", "watch", "list"]
     
    ---
    # This role binding allows "jane" to read pods in the "default" namespace.
    kind: RoleBinding
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      name: read-pods
      namespace: default
    subjects:
    - kind: User
      name: user1 # Name is case sensitive
      apiGroup: rbac.authorization.k8s.io
    roleRef:
      kind: Role #this must be Role or ClusterRole
      name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
      apiGroup: rbac.authorization.k8s.io

Once created, you may authenticate into the kube-api server using the users credentials

curl -v -k https://localhost:6443/api/v1/pods -u "user1:password123" 
```

  
  
#### K8s Reference Docs
- https://kubernetes.io/docs/reference/access-authn-authz/authentication/ 
  
  
