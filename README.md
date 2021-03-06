# Kubernetes begineer

## Objectives

- Kubernetes Overview
- Containers - Docker
- Container Orchestration?
- Kubectl
- Kubernetes Concepts - [PODs](/pods/README.md) | [Deployment](/deployment) | [Services](/services) | [ReplicaSets Controllers](/replicasets-controllers)
- [Networking in Kubernetes](/networking)
- Kubernetes on Cloud - AWS/GCP

## Kubernetes Overview

Kubernetes also known as k8s was built by Google based on their experience running containers in productions. It is now an open source project and is arguably one of the best and most popular container orchestration.

## Container

Containers are completely isolated envrionments. As in they can have their own processes or services, their own networking interfaces, their own mounts just like virtual machines. Except they all share the same OS kernel.
Different types of containers LXC, LXD, LXCFS etc.
Setting up these containers environments is hard as they are very low level and that is where a docker offers a high level tool with several powerful functions making it really easy for end users.

## Container Orchestration

Automatically deploying and managing containers is know as Container Orchestration.
Kubernetes is just a container orchestration technology.
There are multiple such technologies available today such as:

1. Docker swarm from docker
2. MESOS from Apache
3. Kubernetes from google.

### Advantages of container orchestration

Application is now highly available as hardware failures to not bring your application down. Because you have multiple instance of the applications running on different nodes.
The user traffic is load balanced across the various containers.
When demand increases, deploy more instances of the applications seamlessly and within a matter of seconds.
Scale the number of underlying nodes up or down without having to take down the application.
And do all of these easily with a set of declarative object configuration files.

```
Kubernetes is a container orchestration technology used to orchestrate the deployment and management of hundreds and thousands of containers in a clustered environment.
```

## Kubernetes Architecture

Before heading into setting up a kubernetes cluster, It is important to understand some of the basic concepts.

This is to make sense of the terms that we will come across while setting up a kubernetes cluster.

### Node (Minions)

A node is a machine, physical or virtual on which kubernetes is installed. A node is a worker machine and that is where containers will be launched by kubernetes.

But what if the node on which your application is running fails? Well obviously our application goes down.So you need to have more than one nodes.

### Cluster

A cluster is a set of nodes grouped together.
This way even if one node fails you have your application still accessible from the other nodes.

Moreover, having multiple nodes can help in sharing load as well.

### Master

The master is the another node with kubernetes installed in it and is configured as a Master.

The master watches over the nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes.

When you install kubernetes on a system, you're actually installing the following components:

1. **API Server** acts as the front-end for kubernetes, The users, management devices, command line interfaces, all talk to the API server to interact with kubernetes cluster.

2. **etcd service** is a distributed reliable key value store used by kubernetes to store all data used to manage the cluster. When you have multiple nodes and multiple masters in your cluster, etcd stores all that infromation on all the nodes in the cluster in a distributed manner. etcd is responsible for implementing locks within the cluster to ensure that there are no conflicts between the **Masters**.

3. **Kubelet service** is the agent that runs on each node in the cluster. The agent is responsible for making sure that the containers are running on the nodes as expected.

4. **Container Runtime** is the underlying software that is used to run containers. In our case it happens to be docker.

5. **Controllers** are the brains behined orchestration. They are responsible for noticing and responding when nodes, containers or end pointes goes down. The controllers make decisions to bring up new containers in such cases.

6. **Schedulers** is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to nodes.

### Master vs Worker Nodes

- The worker node or minion as it is also known is where the containers are hosted. For example, Docker containers. And to run docker containers on a system, we need container runtime installed and that's where the container runtime falls. In our case it is docker and can be other alternatives available such as Rocket or Cryo.
- The master server has the kube API server and that is what makes it a master. Similarly, the worker nodes have the kublete agent that is responsible for interacting with a master to provide health information of the worker node and carry out actions requested by the master on the worker nodes.
- All the information gathered are stored in a key value store on the master. The key value store is based on the popular etcd framework.
- The master also has the control manager and the schedular.

## Kubectl

The kubetl is a utilites known as the kube command line tool or kubtectl or kube control.

- The kubectl tool is used to deploy and manage applications on a kubernetes cluster.
- To get cluster information.
- To get the status of other nodes in the cluster.
- To manage many other things.

### Commands

- The kubectl run command is used to deploy an application on the cluster.

  `kubectl run hello-world`
- The kubectl info command is used to view information about the cluster.

  `kubectl cluster-info`
- The kubectl get nodes command is used to list all the nodes part of the cluster.

  `kubectl get nodes`
- The kubectl get deployment command is used to list all the deployments.

  `kubectl get deployment`

- The kubectl expose deployment command is used to deploy the service on the cluster.
  
  `kubectl expose deployment balanced --type=LoadBalancer --port=8080`

- The kubectl delete services command is used to delete the service on the cluster.
  
  `kubectl delete services balanced`

- The kubectl delete deployment command is used to delete the deployment on the cluster.
  
  `kubectl delete deployment balanced`

- The kubectl get pods command is used to get the pod deployment info.
  
  `kubectl get pods`

## Kubernetes Concepts - [PODs](/pods/README.md) | ReplicaSets | Deployment | Services | Controllers

### YAML in kubernetes

Kubernetes uses Yaml file as inputs for the creation of objects such as POD's, replicas, deployment services, etc. All of these follow similar structure kubernetes definition file always contains 4 top levels fields:

- `apiVersion` is the version of kubernetes API used to create the objects. Depending on what we are trying to create we must use the right API version. It is of type string.
- `kind` refers to the type of object we are trying to create. It is of type string.
- `metadata` is data about the object like its name, labels etc. It is of type dictonary. Metadata properties are fixed. However, under labels you can have custom properties.
- `spec` is the actuall configuration of the kind such as containers(list)

> These are the top level or root level properties.
> These are also required fields in the configuration file.

  | Kind       | Version |
  | ---------- |:-------:|
  | POD        | v1      |
  | Service    | v1      |
  | ReplicaSet | apps/v1 |
  | Deployment | apps/v1 |
  

**pod-definition.yaml**

```
apiVersion: v1
kind: Pod
metadata:
  name: frontend-app
  labels:
      app: app
      type: frontend
spec:
  containers:
    - name: nginx-container
      image: nginx
```

Run command `kubectl create -f pod-definition.yaml` to create a pod.

### Controllers

- Controllers are the brain behined the kubernetes cluster.
- They are the process which monitor kubernetes objects and respond accordingly.
