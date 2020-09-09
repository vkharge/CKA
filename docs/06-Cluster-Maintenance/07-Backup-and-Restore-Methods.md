# Backup and Restore Methods
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808228)
  
In this section, we will take a look at backup and restore methods

## Backup Candidates
 
 ![bc](../../images/bc.PNG)
 
## Resource Configuration
- Imperative way
  
  ![rci](../../images/rci.PNG)

- Declarative Way (Preferred approach)
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: myapp-pod
    labels:
      app: myapp
      type: front-end
  spec:
    containers:
    - name: nginx-container
      image: nginx
  ```
 ![rcd](../../images/rcd.PNG)
 
- A good practice is to store resource configurations on source code repositories like github.

  ![rcd1](../../images/rcd1.PNG)

## Backup - Resource Configs

 - But if someone created an object the imperative way without documenting that information anywhere. So a better approach to backing up resource configuration is to use query the kube-api server
 

  ```
  $ kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml (only for few resource groups)
  ```

- There are many other resource groups that must be considered. There are tools like **`ARK`** or now called **`Velero`** by Heptio that can do this for you. It can help in taking backups of your Kubernetes cluster using the Kubernetes API

  ![brc](../../images/brc.PNG)
  
## Backup - ETCD
- So, instead of backing up resources as before, you may choose to backup the ETCD cluster itself. 
- ETCD cluster is hosted on the master nodes. While configuring ETCD we specified a location where all the data would be stored, the data directory. That is the directory that can be configured to be backup by your backup tool. ETCD also comes with a built in snapshot solution.
  
  ![be](../../images/be.PNG)
  
- You can take a snapshot of the etcd database by using **`etcdctl`** utility snapshot save command.

  - You can take a snapshot of the ETCD database by using the etdctl utilities snapshot save command. Give the snapshot a name snapshot.db. A snapshot file is created by the name in the current directory.
  
  ```
  $ ETCDCTL_API=3 etcdctl snapshot save snapshot.db
  ```
  ```
  $  ETCDCTL_API=3 etcdctl snapshot status snapshot.db
  ```
  ![be1](../../images/be1.PNG)
  
## Restore - ETCD
- To restore etcd from the backup at later in time. First stop kube-apiserver service
  ```
  $ service kube-apiserver stop
  ```
- Run the etcdctl snapshot restore command.  Then run the etcdctl’s snapshot restore command with the path set to the path of the backup file which is the snapshot.db file. When ETCD restore from a backup, it initializes a new cluster configuration and configures the members of ETCD as new members to a new cluster. This is to prevent a new member from accidentally joining an existing cluster.

- Say for example, you use this backup snapshot to provision a new etcd-cluster for testing purposes. You don’t want the members in the new test cluster to accidentally join the production cluster. So during a restore you must specify a new cluster token and the same initial cluster configuration options specified in the original configuration file.

-  On running this command a new data directory is created. In this example at location /var/lib/etcd-from-backup.

- We then configure the ETCD configuration file to use the new cluster-token and data directory. The reload the service daemon and restart etcd service.

- Update the etcd service
- Reload system configs
  ```
  $ systemctl daemon-reload
  ```
- Restart etcd
  ```
  $ service etcd restart
  ```
  
  ![er](../../images/er.PNG)
  
- Start the kube-apiserver
  ```
  $ service kube-apiserver start
  ```
#### With all etcdctl commands specify the cert,key,cacert and endpoint for authentication.
```
$ ETCDCTL_API=3 etcdctl --endpoints=https://[127.0.0.1]:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/etcd-server.crt \
  --key=/etc/kubernetes/pki/etcd/etcd-server.key snapshot save /tmp/snapshot.db
```

  ![erest](../../images/erest.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/


 
