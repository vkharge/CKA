# Kubernetes Software Versions
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808230)
  
In this section, we will take a look at various kubernetes releases and versions

#### We can see the kubernetess version that we installed
```
$ kubectl get nodes
```
![kgn](../../images/kgn.PNG)

#### Let's take a closer look at the version number
- It consists of 3 parts
  - First is the major version
  - Second is the minior version
  - Finally, the patch version
  
  ![mmp](../../images/mmp.PNG)
  
#### Kubernetes follows a standard software release versioning procedure
- You can find all kubernetes releases at https://github.com/kubernetes/kubernetes/releases

  ![r1](../../images/r1.PNG)
  
  ![r2](../../images/r2.PNG)
  
#### Downloaded package has all the kubernetes components in it except **`ETCD Cluster`** and **`CoreDNS`** as they are seperate projects.

-  You can find all the releases in the releases page of the Kubernetes Github repository. Download the kubernetes.tar.gz file and extract it to find executables for all the kubernetes components. The downloaded package when extracted has all the control plane components in it.

- All of them of the same version.

- Remember that there are other components within the control plane that do not have the same version numbers. The ETCD cluster and CoreDNS servers have their own versions as they are separate projects. 


 ![r3](../../images/r3.PNG)
 
#### References

 - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/release/versioning.md
 - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/api-machinery/api-group.md
 - https://blog.risingstack.com/the-history-of-kubernetes/
 - https://kubernetes.io/docs/setup/release/version-skew-policy/
 
 - https://kubernetes.io/docs/concepts/overview/kubernetes-api/
 - https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md
 - https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md
 
 






