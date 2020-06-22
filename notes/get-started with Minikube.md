# Install kubernetes with Minikube
> host machine: macOS version:10.15.4 

## Check OS environment 
- run command below:
```shell 
sysctl -a | grep -E --color 'machdep.cpu.features|VMX'
```
- if you get result contain `VMX` with red color,it seems your OS environment correct. Example:
```
machdep.cpu.features: FPU VME DE PSE TSC MSR PAE MCE CX8 APIC SEP MTRR PGE MCA CMOV PAT PSE36 CLFSH DS ACPI MMX FXSR SSE SSE2 SS HTT TM PBE SSE3 PCLMULQDQ DTES64 MON DSCPL VMX SMX EST TM2 SSSE3 FMA CX16 TPR PDCM SSE4.1 SSE4.2 x2APIC MOVBE POPCNT AES PCID XSAVE OSXSAVE SEGLIM64 TSCTMR AVX1.0 RDRAND F16C
``` 

## Install kubectl
`kubectl` is a command line tool for control kubernetes cluster. 
- Run command below to install by Homebrew:
```shell
brew install kubectl
```

## Install hyperkit
`HyperKit` is a toolkit for embedding hypervisor capabilities in your application. It includes a complete hypervisor, based on xhyve/bhyve, which is optimized for lightweight virtual machines and container deployment. It is designed to be interfaced with higher-level components such as the VPNKit and DataKit. 
- Run command below to install by Homebrew:
```shell
brew install hyperkit
```

## Install Minikube
`Minikube` is a tool that runs a single-node Kubernetes cluster in a virtual machine on your personal computer. 
- Run command below to install by Homebrew:
```shell 
brew install minikube
```
- Add executable to path 
```shell 
sudo mv minikube /usr/local/bin
```

## Setup Minikube
- After install dependent components,We can setup Minikube cluster with command below:
```shell
minikube start --vm-driver=hyperkit
```
>because of the wall in China, sometimes we can't fetch the images,consult this issue: [Temporary Error: error getting ip during provisioning: IP address is not set](https://github.com/kubernetes/minikube/issues/7888)
- check cluster status
```shell
minikube status
```
- stop cluster with below command if you want:
```shell 
minikube stop
```

# Interacting with cluster 

## Dashboard
To access the Kubernetes Dashboard, run this command in a shell after starting Minikube to get the address:
```shell 
minikube dashboard
```

## Deployment basic command 
In kubernetes,`Pod` is a group of one or more Containers,tried together for the purpose of administration and networking. Use `kubectl` command to deploy a App and management it, in this case, the testing deployment service only with one `Pod`, also only one Container. Flow commands below to deploy `hello-node` to Minikube.

- create a deployment 
```shell 
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
```
- view the deployment 
```shell 
kubectl get deployments
```
- view the pod 
```shell 
kubectl get pods
```
- view the cluster event 
```shell
kubectl get events
```
- view the cluster configuration 
```shell 
kubectl config view
```

## service basic command
Service is a App who expose accessible interface in the kubernetes network.
By default, the Pod is only accessible by its internal IP address within the Kubernetes cluster. To make the hello-node Container accessible from outside the Kubernetes virtual network, you have to expose the Pod as a Kubernetes Service.

- expose the Pod to the public Internet 
```shell
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```
- view service 
```shell
kubectl get services
```

## CLean up environment 
- clean service 
```shell 
kubectl delete service hello-node
``` 
- clean deployment 
```shell 
kubectl delete deployment hello-node
``` 
- also can stop Minikube 
```shell 
minikube stop
```
- delete Minikube VM 
```shell
minikube delete
```

# Additional situation 
- can't setup with command `minikube start`.
```shell 
failed to get driver ip: getting IP: IP address is not set
```
that's issue probably because VPN using, consult with this GitHub link: https://github.com/kubernetes/minikube/issues/4206

# Reference 
- [install and setup kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos)
- [install minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- [Minikube tutorials](https://kubernetes.io/docs/tutorials/hello-minikube/)