# Storage Class

  - Take me to [Lectures](https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests/lectures/23710910)

In this section, we will take a look at **Storage Class**

- We discussed about how to create Persistent Volume and Persistent Volume Claim and We also saw that how to use into the Pod's volume to claim that volume space.
- We created Persistent Volume but before this if we are taking a volume from Cloud providers like GCP, AWS, Azure. We need to first create disk in the Google Cloud as an example. 
- We need to create manually each time when we define in the Pod definition file. that's called **Static Provisioning**. 

- With Cloud Storages:
    - First we need to create a disk on the cloud [google]
	  - Create the PV with the same disk name

		*** Every time an application requires storage, you have to first manually provisioned the desk on Google Cloud and then manually create a persistent volume definition file using the same name as that of the disk that you created. That's called static provisioning volumes.

#### Static Provisioning

![class-18](../../images/class18.PNG)


#### Dynamic Provisioning

- 		It would have been nice if the volume gets provisioned automatically when the application requires it, and that's where storage clauses come in.
- 		With storage classes, you can define a provisional such as Google storage that can automatically provision storage on Google Cloud and attach that to POD when a claim is made. That's called dynamic provisioning of volumes.
- So we no longer need the PV definition because the PV and any associated storage is going to be created automatically when the storage class is created.
- So remember that it still creates a PV. It's just that you don't have to manually create PVC anymore. It's created automatically by the storage class.

![class-19](../../images/class19.PNG)

- No we have a Storage Class, So we no longer to define Persistent Volume. It will create automatically when a Storage Class is created. It's called **Dynamic Provisioning**. 

```
sc-definition.yaml

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
   name: google-storage
provisioner: kubernetes.io/gce-pd
```

#### Create a Storage Class

```
$ kubectl create -f sc-definition.yaml
storageclass.storage.k8s.io/google-storage created
```

#### List the Storage Class

```
$ kubectl get sc
NAME             PROVISIONER            RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
google-storage   kubernetes.io/gce-pd   Delete          Immediate           false                  20s
```

#### Create a Persistent Volume Claim

```
pvc-definition.yaml

kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: myclaim
spec:
  accessModes: [ "ReadWriteOnce" ]
  storageClassName: google-storage       
  resources:
   requests:
     storage: 500Mi
```
```
$ kubectl create -f pvc-definition.yaml

```
#### Create a Pod

```
pod-definition.yaml

apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: frontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: web
  volumes:
    - name: web
      persistentVolumeClaim:
        claimName: myclaim
```
```
$ kubectl create -f pod-definition.yaml
```
#### Provisioner

![class-20](../../images/class20.PNG)

#### Kubernetes Storage Class Reference Docs

- https://kubernetes.io/docs/concepts/storage/storage-classes/
- https://cloud.google.com/kubernetes-engine/docs/concepts/persistent-volumes#storageclasses
- https://docs.aws.amazon.com/eks/latest/userguide/storage-classes.html
