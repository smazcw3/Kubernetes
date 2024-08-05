# Kubernetes
This repo is for learning about Kubernetes.

## Introduction
**Kubernetes** is a container orchestration technology used to orchestrate the deployment and management of 100s and 1000s of containers in a clustered environment.

When you install Kubernetes on a System, you are actually installing the following components.
+ `API server` -- It acts as the front-end for kubernetes. The users, management devices, Command line interfaces all talk to the API server to interact with the kubernetes cluster.
+ `etcd Key store` -- It is a distributed reliable key-value store used by kubernetes to store all data used to manage the cluster. Think of it this way, when you have multiple nodes and multiple masters in your cluster, etcd stores all that information on all the nodes in the cluster in a distributed manner.
+ `scheduler` -- It is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to Nodes.
+ `controllers` -- They are the brain behind orchestration. They are responsible for noticing and responding when nodes, containers or endpoints goes down. The controllers makes decisions to bring up new containers in such cases.
+ `container runtime` -- underlying software that is used to run containers. For example, docker.
+ `kubelet` -- agent that runs on each node in the cluster.


## Kube Command-line use
+ `kubectl cluster-info` -- is used to view information about the cluster.
+ `kubectl get nodes` -- provides a quick overview of the nodes present in your Kubernetes cluster. It allows you to see each node’s current status, availability, and resource allocation.

+ `kubectl version` -- version of Kubernetes running on the nodes.

+ `kubectl get nodes -o wide` -- flavor and version of operating system on which the Kubernetes nodes are running.


## References
1. https://uklabs.kodekloud.com/courses/labs-kubernetes-for-the-absolute-beginners-hands-on/ (Hands on labs on Kubernetes)
