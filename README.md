# Kubernetes
This repo is for learning about Kubernetes. **Kubernetes**, aka K8s, is an open source system for automating deployment, scaling, and management of containerized applications.

## Introduction
**Kubernetes** is a container orchestration technology used to orchestrate the deployment and management of 100s and 1000s of containers in a clustered environment.

When you install Kubernetes on a system, you are actually installing the following components.
+ `API server` -- It acts as the front-end for kubernetes. The users, management devices, Command line interfaces all talk to the API server to interact with the kubernetes cluster.
+ `etcd Key store` -- It is a distributed reliable key-value store used by kubernetes to store all data used to manage the cluster. Think of it this way, when you have multiple nodes and multiple masters in your cluster, etcd stores all that information on all the nodes in the cluster in a distributed manner.
+ `scheduler` -- It is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to Nodes.
+ `controllers` -- They are the brain behind orchestration. They are responsible for noticing and responding when nodes, containers or endpoints goes down. The controllers makes decisions to bring up new containers in such cases.
+ `container runtime` -- underlying software that is used to run containers. For example, docker.
+ `kubelet` -- agent that runs on each node in the cluster.

## YAML
YAML, which stands for *YAML Ain't Markup Language,* is a human-readable data serialization language. It is commonly used to create configuration files and works well with any programming language. Kubernetes uses YAML as inputs for creating of objects such as PODs, replicaSets, deployments, services etc.

### Dictionary/Map
~~~
Banana: 
    Calories: 105
    Fat: 0.4 g
    Carbs: 27 g
~~~

### Array/List
~~~
Fruits:
    - Orange
    - Apple
    - Banana
~~~

### Creating Dictionary in Dictionary
~~~
color: Blue
Model:
    Name: Corvette
    Year: 1995
Transmission: Manual
Price: $20,000
~~~

## Kube Command-line use
+ `kubectl cluster-info` -- is used to view information about the cluster.

+ `kubectl get nodes` -- provides a quick overview of the nodes present in your Kubernetes cluster. It allows you to see each node’s current status, availability, and resource allocation.

+ `kubectl version` -- version of Kubernetes running on the nodes.

+ `kubectl get nodes -o wide` -- flavor and version of operating system on which the Kubernetes nodes are running.

+ `kubectl get pods -o wide` -- Provides a detail overview od the pods present in our kubernetes cluster.

+ `kubectl describe pod webapp` -- Describes the pod with name *webapp*.

+ `kubectl delete pod webapp` -- Delete the pod with name *webapp*.

## Pods
Kubernetes does not deploy containers directly on the worker nodes. The containers are encapsulated into a Kubernetes object known as **PODs**. A *POD* is a single instance of an application. A *POD* is the smallest object, that you can create in kubernetes.
Each container is encapsulated in POD. Multiple such PODs are deployed using replication controllers or replicasets.

+ `kubectl run nginx --image nginx` -- Deploys a docker container by creating a POD. So, it first creates a POD automatically and deploys an instance of the *nginx* docker image. The application image, in this case the *nginx* image, is downloaded from the docker hub repository.

+ `kubectl get pods` -- Lists of available PODs.

We can do POD creation using the following YAML config format:

`pod-definiton.yml`
~~~
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
~~~

Once the above POD definition file is created, we can run 
+ `kubectl create -f pod-definiton.yml` OR 
+ `kubectl apply -f pod.yaml`

We can get the details of the PODs using `kubectl describe pod myapp-pod`

## Controllers
Controllers are the brain behind kubernetes. They are the process that monitor the kubenertes objects and act accordingly.

### Replication controller
A ReplicationController ensures that a specified number of pod replicas are running at any one time. In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is always up and available.

We now can define the replication controller using YAML config format.
`rc-definiton.yml`
~~~
apiVersion: v1
kind: ReplicationController
metadata:
    name: myapp-rc
    labels:
        app: myapp
        type: front-end
spec:
    template:
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                type: front-end
        spec:
            containers:
                - name: nginx-container
                  image: nginx
    
    replicas: 3
~~~

Once the above replication controller definition file is created, we can run 
`kubectl create -f rc-definiton.yml`.

To see the created replication controller, run `kubectl get Replicationcontroller`

### ReplicaSet
ReplicaSet is pretty much same as replication controller except for some minor differences. We now can define the replicset using YAML config format.

`replicaset-definiton.yml`
~~~
apiVersion: apps/v1
kind: myapp-replicaset
metadata:
    name: myapp-rc
    labels:
        app: myapp
        type: front-end
spec:
    template:
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                type: front-end
        spec:
            containers:
                - name: nginx-container
                  image: nginx
    
    replicas: 3
    selector: 
        matchLabels:
            type: front-end
~~~

Once the above replicaset definition file is created, we can run `kubectl create -f replicaset-definiton.yml`.

+ To see the created replicset, run `kubectl get replicaset`.

+ To delete the replicaset, run `kubectl delete replicaset myapp-replicaset` OR `kubectl delete rs myapp-replicaset`.

+ To replace/update the replicaset, run `kubectl replace -f replicaset-definition.yml`.

+ To scale the replicaset(increasing the no. of replicas), run `kubectl scale rs new-replica-set --replicas=5`.

+ To edit the replicaset, run `kubectl edit rs myapp-replicaset`.


### Deployment
We can create the deployment component(kubernets object) by defining the below file:

`deployment-definiton.yml`
~~~
apiVersion: apps/v1
kind: Deployment
metadata:
    name: myapp-deployment
    labels:
        app: myapp
        type: front-end
spec:
    template:
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                type: front-end
        spec:
            containers:
                - name: nginx-container
                  image: nginx
    
    replicas: 3
    selector: 
        matchLabels:
            type: front-end
~~~

Once the above deployment definition file is created, we can run 
`kubectl create -f deployment-definiton.yml`.

+ To see the created replicset, run `kubectl get deployments`.

## Minikube
Single note cluster, often called as local kubernetes. We can run the following commands for minikube:

+ `minikube start` -- Starting the minikube cluster

+ `minikube stop` -- Stopping the minikube cluster

+ `minikube pause` -- Pausing the minikube cluster

+ `minikube unpause` -- Unpausing the minikube cluster


## References
1. https://uklabs.kodekloud.com/courses/labs-kubernetes-for-the-absolute-beginners-hands-on/ (Hands on labs on Kubernetes)
2. https://kubernetes.io/docs/tasks/tools/ (Install Kubectl)
3. https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Fx86-64%2Fstable%2Fbinary+download (Install Minikube)
4. https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/ (Understanding Replication controllers)
