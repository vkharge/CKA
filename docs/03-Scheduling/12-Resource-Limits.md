# Resource Limits
  - Take me to [Video Tutorials](https://kodekloud.com/courses/539883/lectures/9816620)
  
In this section we will take a look at Resource Limits

#### Let us take a look at 3 node kubernetes cluster.
- Each node has a set of CPU, Memory and Disk resources available.
- Whenever a POD is placed on a Node, it consumes resources available to that node. The scheduler takes into consideration, the amount of resources required by a POD and those available on the Nodes. 
-  If the node has no sufficient resources, the scheduler avoids placing the POD on that node. Instead places the POD on one where sufficient resources are available
- If there is no sufficient resources available on any of the nodes, kubernetes holds the scheduling the pod. You will see the pod in pending state. If you look at the events, you will see the reason as insufficient CPU.
  
  ![rl](../../images/rl.PNG)
  
## Resource Requirements
- By default, K8s assume that a pod or container within a pod requires **`0.5`** CPU and **`256Mi`** of memory. This is known as the **`Resource Request` for a container**. For the POD to pick up those defaults you must have first set those as default values for request and limit by creating a LimitRange in that namespace.
  ```
    apiVersion: v1
    kind: LimitRange
    metadata:
      name: mem-limit-range
    spec:
      limits:
      - default:
          memory: 512Mi
        defaultRequest:
          memory: 256Mi
        type: Container
  ```
  https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/
  
  
  ```
    apiVersion: v1
    kind: LimitRange
    metadata:
      name: cpu-limit-range
    spec:
      limits:
      - default:
          cpu: 1
        defaultRequest:
          cpu: 0.5
        type: Container
  ```
  https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/
  
  
  https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource
  
  ![rr](../../images/rr.PNG)
  
- If your application within the pod requires more than the default resources, you need to set them in the pod defination file.

  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: simple-webapp-color
    labels:
      name: simple-webapp-color
  spec:
   containers:
   - name: simple-webapp-color
     image: simple-webapp-color
     ports:
      - containerPort:  8080
     resources:
       requests:
        memory: "1Gi"
        cpu: "1"
  ```
  ![rr-pod](../../images/rr-pod.PNG) 
  
- You can specify any value as low as 0.1, 0.1 CPU can also be expressed as 100m were m stands for milli. You can go as low as 1m, but not lower than that.
- 1 count of CPU is equivalent to 1 vCPU. That’s 1 vCPU in AWS, or 1 Core in GCP or 1 Core in Azure or 1 Hyperthread.
- You could request a higher number of CPUs for the container, provided your Nodes are sufficiently funded. 


- Similarly, with memory you could specify 256 Mebibyte using the Mi suffix. Or specify the same value in Memory like this. Or use the suffix G for a gigabyte.
- Note the difference between G and Gi. G is Gigabyte and it refers to a 1000 Megabytes, whereas Gi Gi refers to Gibibyte and refers to 1024 Mebibyte. The same applies to megabyte and kilobytes.

   
## Resources - Limits
- Container running on a node in the docker world, a docker container has no limit to the resources it can consume on a Node. Say a container starts with 1 vCPU on a Node, it can go up and consume as much resource as it requires, suffocating the native processes on the node or other containers of resources.

- By default, k8s sets resource limits to 1 vCPU and 512Mi of memory
  
  ![rsl](../../images/rsl.PNG)
  
- You can set the resource limits in the pod defination file.
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: simple-webapp-color
    labels:
      name: simple-webapp-color
  spec:
   containers:
   - name: simple-webapp-color
     image: simple-webapp-color
     ports:
      - containerPort:  8080
     resources:
       requests:
        memory: "1Gi"
        cpu: "1"
       limits:
         memory: "2Gi"
         cpu: "2"
  ```
  ![rsl1](../../images/rsl1.PNG)
  
#### Note: Remember Requests and Limits for resources are set per container in the pod.
  
## Exceed Limtis
- what happens when a pod tries to exceed resources beyond its limits?

   ![el](../../images/el.PNG)
   
   - In case of the CPU, kubernetes throttles the CPU so that it does not go beyond the specified limit. A container cannot use more CPU resources than its limit. 
   
   - However, this is not the case with memory. A container CAN use more memory resources that its limit. A container CAN use more memory resources that its limit. So if a pod tries to consume more memory than its limit constantly, the POD will be terminated.
   
  
#### K8s Reference Docs:
- https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
  
  
