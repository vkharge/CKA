# Certificate API
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808253)
  
In this section, we will take a look at how to manage certificates and certificate API's in kubernetes

## CA (Certificate Authority)
- The CA is really just the pair of key and certificate files that we have generated, whoever gains access to these pair of files can sign any certificate for the kubernetes environment.



- All the certificates are stored in Master Server, So the master node is also our CA server. 

- Consider new administrator joins the team:
	
	- New User generates his key and CSR
	- Sends the CSR to existing Admin
	- Admin Logins into the Master Node
	- Signs the CSR with the Root CA Certificate
	- Admin get the user.crt from the Master download
	- Admin forwards the user.crt to the new user
	- Now new user is able to access the K8S


	** The same process happens when the user certificate expires


- This is not recommended, Kubernetes has a built-in Certificates API that can do this for you. With the Certificates API, you now send a CertificateSigningRequest directly to kubernetes through an API call
	- New User generates his key and CSR
      - # openssl genrsa -out vk.key 2048
      - # openssl req -new -key vk.key -subj "/CN=vk" -out vk.csr 


	- Sends the CSR to existing Admin

	- Instead of login into Master Node, the admin calls/creates the CertificateSigningRequest Object
      - CSR Object is created using a manifest file
      - CSR Object requires the certificate in base64 format
      - # cat vk.csr  | base64

      - # kubectl create -f CSR.yaml

	- Request can be reviewed or approved by the admin using the kubeclt commands
	  - # kubectl get csr
	  - # kubectl certificate approve vk

	  - ** K8S signs the certificate with CA key pairs and generates a certificate for the user.


	- Once approved, this certificate can be extracted and send back to user
	  - # kubectl get csr vk -o yaml
	  - Look for status -> certificate , this certificate in base64 encoded format

	  - copy the certificate, # echo "<certificate contents>" | base64 --decode


	- Which kubernetes component is responsible for revewing and signing the certificate ?
	  - Controller-Manager is responsible for this.
	  - Contreoller-Manager has CSR-Aproving and CSR-Signing controllers in it

	  - Since the Controller-Manager performs the approving and signing of CSR, in its config, it has the CA Key and CA Cert


### Once the user csr is approved, how the user can access the cluster
 
 - As the CSR is approved, now the user has vk.key , vk.crt [converting the approved csr to crt], and ca.crt
 - Create a kubeconfig file
   
   ```
   # kubectl config view
   # kubectl config --kubeconfig=config set-cluster la-cluster --server=https://172.31.9.81:6443  --certificate-authority /home/cloud_user/ca.crt
   
   # kubectl config --kubeconfig=config set-credentials vk --client-certificate /home/cloud_user/vk.crt  --client-key /home/cloud_user/vk.key
   
   # kubectl config --kubeconfig=config set-context vk-la --user=vk --cluster=la-cluster --namespace=default
   
   # kubectl config view --kubeconfig=config
   
   # kubectl get no --kubeconfig=config --context vk-la
   Error from server (Forbidden): nodes is forbidden: User "vk" cannot list resource "nodes" in API group "" at the cluster scope
   ** New user does not have any permission on the cluster
   
   Default Context
   # kubectl config --kubeconfig=config use-context vk-la
   
   # kubectl get no --kubeconfig=config
   
   Copy the config file ~/.kube/config to make kubeconfig file as a default file
   
   # kubectl get no
  

#### Kubernetes has a built-in certificates API that can do this for you. 
- With the certificate API, we now send a certificate signing request (CSR) directly to kubernetes through an API call.
   
  ![csr](../../images/csr.PNG)
   
#### This certificate can then be extracted and shared with the user.
- A user first creates a key
  ```
  $ openssl genrsa -out jane.key 2048
  ```
- Generates a CSR
  ```
  $ openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr 
  ```
- Sends the request to the administrator and the adminsitrator takes the key and creates a CSR object, with kind as "CertificateSigningRequest" and a encoded "jane.csr"
  ```
  apiVersion: certificates.k8s.io/v1beta1
  kind: CertificateSigningRequest
  metadata:
    name: jane
  spec:
    groups:
    - system:authenticated
    usages:
    - digital signature
    - key encipherment
    - server auth
    request:
      <certificate-goes-here>
  ```
  $ cat jane.csr |base64 
  $ kubectl create -f jane.yaml
  ```
 ![csr1](../../images/csr1.PNG)
  
- To list the csr's
  ```
  $ kubectl get csr
  ```
- Approve the request
  ```
  $ kubectl certificate approve jane
  ```
- To view the certificate
  ```
  $ kubectl get csr jane -o yaml
  ```
- To decode it
  ```
  $ echo "<certificate>" |base64 --decode
  ```
  
  ![csr2](../../images/csr2.PNG)
  
#### All the certificate releated operations are carried out by the controller manager. 
- If anyone has to sign the certificates they need the CA Servers, route certificate and private key. The controller manager configuration has two options where you can specify this.

  ![csr3](../../images/csr3.PNG)
  
  ![csr4](../../images/csr4.PNG)
  
  
#### K8s Reference Docs
- https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/
- https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/
 
  


