# Basic concepts and components

## What Kubernetes is?
Kubernetes is a open source platform for manage containerized service, it provide much functional operations to manage container cluster such as control, monitoring, audit etc. Kubernetes come from Google with over 15 years of Google's experience, and now it has a large, rapidly growing ecosystem and community.

## Features
Kubernetes provide much features for managing containerized applications, and keep highly-availability automatic. It's a convenient approaches to create distributed system, take careof scaling and failover applications, deployment and maintenance efficiently. 

### Service discovery and load balancing 
Containers can expose server address as a DNS name or IP, if traffic to a container is high, Kubernetes is able to load balancing and distribute network to keep service available.

### Storage orchestration
Allow multiple mount target storage system automatically, such as local disk, publish cloud provider, etc.

### Automatic rollout and rollback 
Describe desired state for deployed containers by Kubernetes objects, then it can change current state to desired state automatically at a controlled rate.

### Automatic bin packing 
Describe desired resource used(CUP,Memory,etc) of containers by Kubernetes objects, it can fit onto nodes with best practice.

### Self-healing 
When containers restart/deployment fail/replace/killed, Kubernetes doesn't respond user-defined health check. It schedule scheme all mutely and automatically.

### Secret and configuration management 
Allow to store sensitive information such as token/password/keys, it can deploy and update secrets and application configurations with out rebuild container images, and separate with stack configuration.

## Components
A Kubernetes cluster consist of series components that represent the control plane and a set of machine nodes.
- Control plane main responsibility is make global decision about the cluster such as scheduling, and detecting/responsing the cluster event.
- Node components provide basic runtime environment, should running at each node, maintain pods running and network.
> Diagram of main components tied together:
![imgage](https://d33wubrfki0l68.cloudfront.net/7016517375d10c702489167e704dcb99e570df85/7bb53/images/docs/components-of-kubernetes.png)

### kube-apiserver
Expose Kebernetes API, is the front end for the Kubernetes control plane. It's designed to scale horizontally itself by deploying more instances. Learn more about kube-apiserver: [go official documentation](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)

### etcd
`etcd` is a consistent and highly availability key-value store used as Kubernetes' backing store for all cluster. It is implemented complete `Raft` algorithm to keep strong-consistent, important in service register and discovery. Learn more about etcd: [go official documentation](https://etcd.io/docs/)

### kube-scheduler
Nodes resource schedule, watch for newly created Pods with no assigned node, select a node for them to run on appropriately.

### kube-controller-manager 
Include a series components controller running as a separate process.
- Node controller: noticing and responding when nodes go down.
- Replication controller: maintain the correct number of pods for each application.
- End point controller: populates the endpoint object.
- Service account & token controller: Create new account and API access token for new namespace.

### cloud-controller-manager 
Link cluster to specify cloud provider's API, and separate withe cloud platform. Only used when specific to particular cloud provider.

### kubelet
An agent running at every node, ensure containers are running in a pod. Only manage containers which created by Kubernetes.

### kube-proxy
Running on each node, is a network proxy. It maintains network rules on nodes, control input/out network gateway just like Firewalls in OS.

### Container runtime
Is a software support containers running, series kind of containers are supported: Docker/containerd/CRI-O and any implementation of the [Kubernetes CRI](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md)

### Addons
Use system resources to implement cluster features, with `kube-system` namespace. typical addons below as:
- DNS: cluster dns server, [learn more](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)
- Dashboard: web-based UI for Kubernetes clusters, [learn more](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
- Monitoring: monitoring time-series metrics with web UI, [learn more](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-usage-monitoring/)
- Logging: record cluster-level logs and provide search/browsing interface, [learn more](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
