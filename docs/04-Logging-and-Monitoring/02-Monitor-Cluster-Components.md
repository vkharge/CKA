# Monitor Cluster Components
  - Take me to [Video Tutuorials](https://kodekloud.com/courses/539883/lectures/9808186)
  
In this section, we will take a look at monitoring kubernetes cluster

#### How do you monitor resource consumption in kubernetes? or more importantly, what would you like to monitor?

 -  Node level metrics such as the number of nodes in the cluster, how many of them are healthy as well as performance metrics such as CPU, Memory, network and disk utilization.

 - POD level metrics such as the number of PODs, and performance metrics of each POD such as the CPU and Memory consumption on them.
 
 - So we need a solution that will monitor these metrics store them and provide analytics around this data
 
 - Kubernetes does not come with a full featured built-in monitoring solution. However, there are a number of open-source solutions available today, such as the Metrics-Server, Prometheus, Elastic Stack, and proprietary solutions like Datadog and Dynatrace. 
 
  ![mon](../../images/mon.PNG)
 
## Heapster vs Metrics Server
- Heapster was one of the original projects that enabled monitoring and analysis features for kubernetes 
- Heapster is now deprecated and a slimmed down version was formed known as the **`metrics server`**.


  ![hpms](../../images/hpms.PNG)
  
## Metrics Server
- You can have one metrics server per kubernetes cluster, the metric server retrieves metrics from each of the kubernetes nodes and pods, aggregates them and stores them in memory. 
- Note that the metric server is only an in memory monitoring solution and does not store the metrics on the desk and as a result you cannot see historical performance data.

- For that you must rely on one of the advanced monitoring solutions

  ![ms1](../../images/ms1.PNG)

#### How are the metrics generated for the PODs on these nodes?

-  Kubernetes runs an agent on each node known as the kubelet, which is responsible for receiving instructions from the kubernetes API master server and running PODs on the nodes.

- The kubelet also contains a subcomponent known as as cAdvisor or Container Advisor. cAdvisor is responsible for retrieving performance metrics from pods, and exposing them through the kubelet API to make the metrics available for the Metrics Server.

- 
  ![ca](../../images/ca.PNG)
  
## Metrics Server - Getting Started

  ![msg](../../images/msg.PNG)
  
- Clone the metric server from github repo
  ```
  $ git clone https://github.com/kubernetes-incubator/metrics-server.git
  ```
- Deploy the metric server
  ```
  $ kubectl create -f metric-server/deploy/1.8+/
  ```
  - This command deploys a set of pods, services and roles to enable metrics server to poll for performance metrics from the nodes in the cluster.
  
- View the cluster performance

 - Once deployed, give the metrics-server some time to collect and process data. Once processed, cluster performance can be viewed by running the command kubectl top node. This provides the CPU and Memory consumption of each of the nodes. 
  ```
  $ kubectl top node
  ```
  
- View performance metrics of pod

 - Use the kubectl top pod command to view performance metrics of pods in kubernetes. 
  ```
  $ kubectl top pod
  ```
  
  ![view](../../images/view.PNG)
  
  
