![kubernetes-logo2-800x675](https://user-images.githubusercontent.com/31435126/49341150-d12c5d00-f68c-11e8-83ff-07619b73feea.png)

<h2>It is Kubernetes Tutorials.</h2>




https://kubernetes.io/ko/docs/tutorials/  <KOR.VER>


https://kubernetes.io/docs/tutorials/hello-minikube/ <ENG.VER>



# Kubernetes

Kubernetes (commonly stylized as K8s is an open-source container-orchestration system for automating deployment, scaling and management of containerized applicationsIt was originally designed by Google and is now maintained by the Cloud Native Computing Foundation. It aims to provide a "platform for automating deployment, scaling, and operations of application containers across clusters of hosts".It works with a range of container tools, including Docker, since its first release

<h2>History</h2>

Google Container Engine talk at Google Cloud Summit
Kubernetes (κυβερνήτης, Greek for "governor", "helmsman" or "captain") was founded by Joe Beda, Brendan Burns and Craig McLuckie who were quickly joined by other Google engineers including Brian Grant and Tim Hockin, and was first announced by Google in mid-2014. Its development and design are heavily influenced by Google's Borg system, and many of the top contributors to the project previously worked on Borg. The original codename for Kubernetes within Google was Project Seven, a reference to Star Trek character Seven of Nine that is a "friendlier" Borg. The seven spokes on the wheel of the Kubernetes logo are a nod to that codename.

Kubernetes v1.0 was released on July 21, 2015.Along with the Kubernetes v1.0 release, Google partnered with the Linux Foundation to form the Cloud Native Computing Foundation (CNCF)and offered Kubernetes as a seed technology. On March 6, 2018, Kubernetes Project reached ninth place in commits at GitHub, and second place in authors and issues to the Linux kernel

<h2>Pods</h2>
The basic scheduling unit in Kubernetes is a pod. It adds a higher level of abstraction by grouping containerized components. A pod consists of one or more containers that are guaranteed to be co-located on the host machine and can share resources Each pod in Kubernetes is assigned a unique IP address within the cluster, which allows applications to use ports without the risk of conflict. A pod can define a volume, such as a local disk directory or a network disk, and expose it to the containers in the pod. Pods can be managed manually through the Kubernetes API, or their management can be delegated to a controller.

<h2>Services</h2>
A Kubernetes service is a set of pods that work together, such as one tier of a multi-tier application. The set of pods that constitute a service are defined by a label selector. Kubernetes provides two modes of service discovery, using environmental variables or using Kubernetes DNS,. Service discovery assigns a stable IP address and DNS name to the service, and load balances traffic in a round-robin manner to network connections of that IP address among the pods matching the selector (even as failures cause the pods to move from machine to machine). By default a service is exposed inside a cluster (e.g. back end pods might be grouped into a service, with requests from the front-end pods load-balanced among them), but a service can also be exposed outside a cluster (e.g. for clients to reach frontend pods).

## Architecture ##

Kubernetes architecture diagram
Kubernetes follows the master-slave architecture. The components of Kubernetes can be divided into those that manage an individual node and those that are part of the control plane

## Kubernetes control plane (master) ##
The Kubernetes Master is the main controlling unit of the cluster, managing its workload and directing communication across the system. The Kubernetes control plane consists of various components, each its own process, that can run both on a single master node or on multiple masters supporting high-availability clusters.The various components of Kubernetes control plane are as follows:

## etcd ##
See also: Container Linux by CoreOS § Cluster infrastructure
etcd is a persistent, lightweight, distributed, key-value data store developed by CoreOS that reliably stores the configuration data of the cluster, representing the overall state of the cluster at any given point of time. Other components watch for changes to this store to bring themselves into the desired state.

## API server ##
The API server is a key component and serves the Kubernetes API using JSON over HTTP, which provides both the internal and external interface to Kubernetes. The API server processes and validates REST requests and updates state of the API objects in etcd, thereby allowing clients to configure workloads and containers across Worker nodes.

## Scheduler ## 
The scheduler is the pluggable component that selects which node an unscheduled pod (the basic entity managed by the scheduler) runs on, based on resource availability. Scheduler tracks resource use on each node to ensure that workload is not scheduled in excess of available resources. For this purpose, the scheduler must know the resource requirements, resource availability, and other user-provided constraints and policy directives such as quality-of-service, affinity/anti-affinity requirements, data locality, and so on. In essence, the scheduler’s role is to match resource "supply" to workload "demand".

## Controller manager ##
The controller manager is a process that runs core Kubernetes controllers like DaemonSet Controller and Replication Controller. The controllers communicate with the API server to create, update, and delete the resources they manage (pods, service endpoints, etc.)

## Kubernetes node (slave) ## 
The Node, also known as Worker or Minion, is a machine where containers (workloads) are deployed. Every node in the cluster must run a container runtime such as Docker, as well as the below-mentioned components, for communication with master for network configuration of these containers.

## Kubelet ##
Kubelet is responsible for the running state of each node, ensuring that all containers on the node are healthy. It takes care of starting, stopping, and maintaining application containers organized into pods as directed by the control plane.

Kubelet monitors the state of a pod, and if not in the desired state, the pod re-deploys to the same node. Node status is relayed every few seconds via heartbeat messages to the master. Once the master detects a node failure, the Replication Controller observes this state change and launches pods on other healthy nodes.[citation needed]
 
## Container ##
A container resides inside a pod. The container is the lowest level of a micro-service that holds the running application, libraries, and their dependencies. Containers can be exposed to the world through an external IP address. Kubernetes support Docker container since its first version, in July 2016 rkt container engine was added.

## Kube-proxy ##
The Kube-proxy is an implementation of a network proxy and a load balancer, and it supports the service abstraction along with other networking operation.It is responsible for routing traffic to the appropriate container based on IP and port number of the incoming request.

## cAdvisor ##
cAdvisor is an agent that monitors and gathers resource usage and performance metrics such as CPU, memory, file and network usage of containers on each node.

## Kubernetes Cloud Services ##
Kubernetes is offered as a service on multiple public clouds, including Amazon Web Services (EKS)(since June 2018 in the US East (N. Virginia) and US West (Oregon) Regions), Microsoft Azure,DigitalOcean (since May 2018), Google Kubernetes Engine (GKE) in Google Cloud Platform (at least since November 2014) IBM Cloud,and Alibaba Cloud.

## Comparison to Spring Cloud ##
Netflix open-sourced many of the tools that they developed, which are used to run their microservice-based infrastructure. Many of these tools have been popularized via the Spring Framework - they have been re-implemented as Spring-based tools under the umbrella of the Spring Cloud project. Prior to Kubernetes and its ecosystem of tools being developed, Spring Cloud was a popular library of tools used in microservices architectures to address many of the concerns that such applications had. The table  below, shows a comparison of an implementing feature from the Kubernetes ecosystem with an equivalent from the Spring Cloud world.






## Licensing ##
 
* https://www.wikipedia.org/
 
