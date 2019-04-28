![kubernetes-logo2-800x675](https://user-images.githubusercontent.com/31435126/49341150-d12c5d00-f68c-11e8-83ff-07619b73feea.png)

<h2> It is Kubernetes Tutorials. </h2>




* https://kubernetes.io/ko/docs/tutorials/  <KOR.VER>


* https://kubernetes.io/docs/tutorials/hello-minikube/ <ENG.VER>



<h1> Kubernetes </h1>

Kubernetes (commonly stylized as K8s is an open-source container-orchestration system for automating deployment, scaling and management of containerized applicationsIt was originally designed by Google and is now maintained by the Cloud Native Computing Foundation. It aims to provide a "platform for automating deployment, scaling, and operations of application containers across clusters of hosts".It works with a range of container tools, including Docker, since its first release

<h2>Install kubernetis using kubeadm on Centos 7</h2>
 
 
1. Install the docker on the entire server.

```

# yum install -y yum-utils device-mapper-persistent-data lvm2
# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# yum install docker-ce
# systemctl start docker && systemctl enable docker

```

2. Preparing to install kubeadm.

* Change SELinux settings to permissive mode.

```
# setenforce 0
# sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

```

* Iptable settings.

```
# cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
$ sysctl --system

```
* Disable firewalld.

```
# systemctl stop firewalld
# systemctl disable firewalld

```
* Swap off

Turn off swap:

```
# swapoff -a

```

Comment out the following code in the / etc / fstab file:

```

#/dev/mapper/centos-swap swap                    swap    defaults        0 0

```

Restart the server:

```
# reboot

```

3. Preparing for cooking
Cubanetis YUM Repository Settings:

```
# cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF

```

Installing kubeadm:

```
# yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
# systemctl enable kubelet && systemctl start kubelet

```

4. Installing the master component
Initialize master node with kubeadm init command

Initialize the master node using the kubeadm init command. The --pod-network-cidr option fits the CNI (Container Network Interface) to  be used.

```

# kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=172.16.1.100

[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config


You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/
  
You can now join any number of machines by running the following on each node
as root:


  kubeadm join 172.16.1.100:6443 --token yrc47a.55b25p2dhe14pzd1 --discovery-token-ca-cert-hash sha256:2a7a31510b9a0b0da1cf71c2c29627b40711cdd84be12944a713ce2af2d5d148

```


Setting Environment Variables

If you run kubectl using the root account, set the following environment variables.

```

# export KUBECONFIG=/etc/kubernetes/admin.conf

```

* CNI installation

```
# kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml

```

* Confirm master execution.

```

# kubectl get pods --all-namespaces
NAMESPACE     NAME                                 READY   STATUS    RESTARTS   AGE
kube-system   coredns-86c58d9df4-78jbg             1/1     Running   0          9m3s
kube-system   coredns-86c58d9df4-q7mwf             1/1     Running   0          9m3s
kube-system   etcd-k8s-master                      1/1     Running   0          13m
kube-system   kube-apiserver-k8s-master            1/1     Running   0          13m
kube-system   kube-controller-manager-k8s-master   1/1     Running   0          13m
kube-system   kube-flannel-ds-amd64-zv8nc          1/1     Running   0          3m11s
kube-system   kube-proxy-xj7hg                     1/1     Running   0          14m
kube-system   kube-scheduler-k8s-master            1/1     Running   0          13m

```

5. Installing the node components.

```
# kubeadm join 172.16.1.100:6443 --token yrc47a.55b25p2dhe14pzd1 --discovery-token-ca-cert-hash sha256:2a7a31510b9a0b0da1cf71c2c29627b40711cdd84be12944a713ce2af2d5d148

```

```
[root@k8s-master ~]# kubectl get nodes
NAME         STATUS     ROLES    AGE   VERSION
k8s-master   Ready      master   18m   v1.13.2
k8s-node1    NotReady   <none>   30s   v1.13.2

```

```
[root@k8s-master ~]# kubectl get nodes
NAME         STATUS     ROLES    AGE     VERSION
k8s-master   Ready      master   21m     v1.13.2
k8s-node1    Ready      <none>   3m28s   v1.13.2
k8s-node2    NotReady   <none>   15s     v1.13.2

```

* You can check the cluster information by running the kubectl cluster-info command.

```

[root@k8s-master ~]# kubectl cluster-info
Kubernetes master is running at https://172.16.1.100:6443
KubeDNS is running at https://172.16.1.100:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

```

6. Test the cluster

```

# kubectl run kubia --image=luksa/kubia --port 8080 --generator=run-pod/v1
replicationcontroller/kubia created

```

* This command runs a kernel pod on the cluster using the image of the locker (luksa / kubia). Note that the luksa / kubia image is a simple web server that responds to hostnames.

```

[root@k8s-master ~]# kubectl expose rc kubia --type=LoadBalancer --name kubia-http
service/kubia-http exposed

```

* Let's look at the service information generated by the kubectl get services command. In my test environment, the LoadBalancer type service uses 10.101.195.144 as the cluster IP.

```
[root@k8s-master ~]# kubectl get services
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP      10.96.0.1        <none>        443/TCP          3h22m
kubia-http   LoadBalancer   10.101.195.144   <pending>     8080:31701/TCP   15s

```

* Connect to the 8080 port using this cluster IP on the server where you installed the Coubertetis cluster. If the result is similar to the following, the cubenet is operating normally.

```

# curl 10.101.195.144:8080
You've hit kubia-ckh8w

```

<h2>History</h2>

Google Container Engine talk at Google Cloud Summit
Kubernetes (κυβερνήτης, Greek for "governor", "helmsman" or "captain") was founded by Joe Beda, Brendan Burns and Craig McLuckie who were quickly joined by other Google engineers including Brian Grant and Tim Hockin, and was first announced by Google in mid-2014. Its development and design are heavily influenced by Google's Borg system, and many of the top contributors to the project previously worked on Borg. The original codename for Kubernetes within Google was Project Seven, a reference to Star Trek character Seven of Nine that is a "friendlier" Borg. The seven spokes on the wheel of the Kubernetes logo are a nod to that codename.

Kubernetes v1.0 was released on July 21, 2015.Along with the Kubernetes v1.0 release, Google partnered with the Linux Foundation to form the Cloud Native Computing Foundation (CNCF)and offered Kubernetes as a seed technology. On March 6, 2018, Kubernetes Project reached ninth place in commits at GitHub, and second place in authors and issues to the Linux kernel

<h2>Pods</h2>
The basic scheduling unit in Kubernetes is a pod. It adds a higher level of abstraction by grouping containerized components. A pod consists of one or more containers that are guaranteed to be co-located on the host machine and can share resources Each pod in Kubernetes is assigned a unique IP address within the cluster, which allows applications to use ports without the risk of conflict. A pod can define a volume, such as a local disk directory or a network disk, and expose it to the containers in the pod. Pods can be managed manually through the Kubernetes API, or their management can be delegated to a controller.

<h2>Services</h2>
A Kubernetes service is a set of pods that work together, such as one tier of a multi-tier application. The set of pods that constitute a service are defined by a label selector. Kubernetes provides two modes of service discovery, using environmental variables or using Kubernetes DNS,. Service discovery assigns a stable IP address and DNS name to the service, and load balances traffic in a round-robin manner to network connections of that IP address among the pods matching the selector (even as failures cause the pods to move from machine to machine). By default a service is exposed inside a cluster (e.g. back end pods might be grouped into a service, with requests from the front-end pods load-balanced among them), but a service can also be exposed outside a cluster (e.g. for clients to reach frontend pods).

<h2> Architecture </h2>

Kubernetes architecture diagram
Kubernetes follows the master-slave architecture. The components of Kubernetes can be divided into those that manage an individual node and those that are part of the control plane

<h2>Kubernetes control plane (master)</h2>
The Kubernetes Master is the main controlling unit of the cluster, managing its workload and directing communication across the system. The Kubernetes control plane consists of various components, each its own process, that can run both on a single master node or on multiple masters supporting high-availability clusters.The various components of Kubernetes control plane are as follows:

<h2> etcd </h2>
See also: Container Linux by CoreOS § Cluster infrastructure
etcd is a persistent, lightweight, distributed, key-value data store developed by CoreOS that reliably stores the configuration data of the cluster, representing the overall state of the cluster at any given point of time. Other components watch for changes to this store to bring themselves into the desired state.

<h2> API server </h2>
The API server is a key component and serves the Kubernetes API using JSON over HTTP, which provides both the internal and external interface to Kubernetes. The API server processes and validates REST requests and updates state of the API objects in etcd, thereby allowing clients to configure workloads and containers across Worker nodes.

<h2> Scheduler </h2> 
The scheduler is the pluggable component that selects which node an unscheduled pod (the basic entity managed by the scheduler) runs on, based on resource availability. Scheduler tracks resource use on each node to ensure that workload is not scheduled in excess of available resources. For this purpose, the scheduler must know the resource requirements, resource availability, and other user-provided constraints and policy directives such as quality-of-service, affinity/anti-affinity requirements, data locality, and so on. In essence, the scheduler’s role is to match resource "supply" to workload "demand".

<h2> Controller manager </h2>
The controller manager is a process that runs core Kubernetes controllers like DaemonSet Controller and Replication Controller. The controllers communicate with the API server to create, update, and delete the resources they manage (pods, service endpoints, etc.)

<h2> Kubernetes node (slave) </h2> 
The Node, also known as Worker or Minion, is a machine where containers (workloads) are deployed. Every node in the cluster must run a container runtime such as Docker, as well as the below-mentioned components, for communication with master for network configuration of these containers.

<h2> Kubelet </h2>
Kubelet is responsible for the running state of each node, ensuring that all containers on the node are healthy. It takes care of starting, stopping, and maintaining application containers organized into pods as directed by the control plane.

Kubelet monitors the state of a pod, and if not in the desired state, the pod re-deploys to the same node. Node status is relayed every few seconds via heartbeat messages to the master. Once the master detects a node failure, the Replication Controller observes this state change and launches pods on other healthy nodes.[citation needed]
 
<h2> Container </h2>
A container resides inside a pod. The container is the lowest level of a micro-service that holds the running application, libraries, and their dependencies. Containers can be exposed to the world through an external IP address. Kubernetes support Docker container since its first version, in July 2016 rkt container engine was added.

<h2> Kube-proxy </h2>
The Kube-proxy is an implementation of a network proxy and a load balancer, and it supports the service abstraction along with other networking operation.It is responsible for routing traffic to the appropriate container based on IP and port number of the incoming request.

<h2> cAdvisor </h2>
cAdvisor is an agent that monitors and gathers resource usage and performance metrics such as CPU, memory, file and network usage of containers on each node.

<h2> Kubernetes Cloud Services </h2>
Kubernetes is offered as a service on multiple public clouds, including Amazon Web Services (EKS)(since June 2018 in the US East (N. Virginia) and US West (Oregon) Regions), Microsoft Azure,DigitalOcean (since May 2018), Google Kubernetes Engine (GKE) in Google Cloud Platform (at least since November 2014) IBM Cloud,and Alibaba Cloud.

<h2> Comparison to Spring Cloud </h2>
Netflix open-sourced many of the tools that they developed, which are used to run their microservice-based infrastructure. Many of these tools have been popularized via the Spring Framework - they have been re-implemented as Spring-based tools under the umbrella of the Spring Cloud project. Prior to Kubernetes and its ecosystem of tools being developed, Spring Cloud was a popular library of tools used in microservices architectures to address many of the concerns that such applications had. The table  below, shows a comparison of an implementing feature from the Kubernetes ecosystem with an equivalent from the Spring Cloud world.






<h2> Licensing </h2>
 
* https://www.wikipedia.org/
 
