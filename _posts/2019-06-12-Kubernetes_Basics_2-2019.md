---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 2				# 标题
subtitle:   k8s sharing #副标题
date:       2019-06-12 				# 时间
author:     frankie 						# 作者
header-img: img/post-bg-rwd.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# Kubernetes Concept

* Kubernetes Object model
* Declarative management


Kubernetes objects
Persistent entities representing the state of the cluster
* Object spec: Desired state described by us
* Object status: Current state described by Kubernetes

 ![Imgur](https://i.imgur.com/KvnTlWj.png)


Pod are the basic building block which shares the network namespace

  ![Imgur](https://i.imgur.com/piYaVqo.png)

  ![Imgur](https://i.imgur.com/kM2Vytd.png)

  ![Imgur](https://i.imgur.com/mPm4NN8.png)


## What is the difference between a pod and a container?


# Kubernetes Control Plane
Three nginx pods -> A controller object
![Imgur](https://i.imgur.com/5F9KfgA.png)


## Which master control plane component is the only one with which clients interact directly?
kube-controller-manager

kube-apiserver

etcd

kube-scheduler


## Which master control plane component is the cluster's database?
kube-controller-manager

kube-scheduler

kube-apiserver

etcd


## What is the role of the kubelet?
To interact with underlying cloud providers

To serve as Kubernetes’s agent on each node

To maintain network connectivity among the Pods in a cluster


# Google Kubernetes Engine Concepts
Kubeadm

![Imgur](https://i.imgur.com/FSXLNkR.png)

GKE: More about nodes
* Kubernetes doesn't create nodes. Cluster admins create nodes and add them to Kubernetes
* GKE manages this by deploying and registering Compute Engine instances as nodes.

## In GKE clusters, how are nodes provisioned?
As Compute Engine virtual machines

As abstract parts of the GKE service that are not exposed to GCP customers

## In GKE, how are masters provisioned?
As Compute Engine virtual machines

As abstract parts of the GKE service that are not exposed to GCP customers

## What is the purpose of configuring a regional cluster in GKE?
To allow applications running in the cluster to withstand the loss of a zone

To ensure that the cluster's workloads are isolated from the public Internet


# Kubernetes Object Management
All kubernetes object are identified by a unique name and a unique identifier.

manifest files:

![Imgur](https://i.imgur.com/ouUUyaY.png)

![Imgur](https://i.imgur.com/ZYLlswO.png)

apiVersion: which Kubernetes API version is used to create the object.
(protocol versioned -> backwards compatibility)

kind: which object

metadata: identify the object using name, unique id and an optional namespace


Best practice tip: use version control on YAML files: track and manage changes and to back out those changes. (recreate or restore clusters)


Object names
All objects are identified by a name, if an object is deleted, the name can be reused.

![Imgur](https://i.imgur.com/GACi9Kq.png)

![Imgur](https://i.imgur.com/eh7DlJK.png)

## why UID is needed?
No two objects will have the same UID throughout the life of a cluster

Labels:

![Imgur](https://i.imgur.com/NSHFMoP.png)

kubectl get pods -selector=app=nginx

![Imgur](https://i.imgur.com/tyt28Gr.png)
----> 200 nginx Pod

![Imgur](https://i.imgur.com/3TPfk3t.png)


Pods and Controller Object

![Imgur](https://i.imgur.com/BQZJQtI.png)


![Imgur](https://i.imgur.com/QnRZKkz.png)

![Imgur](https://i.imgur.com/B3TPkAW.png)

![Imgur](https://i.imgur.com/JACobW3.png)

![Imgur](https://i.imgur.com/SHRKJVo.png)

Kubernetes allows you to abstract a single physical cluster into multiple clusters known as namespaces

![Imgur](https://i.imgur.com/qxp0JAq.png)

![Imgur](https://i.imgur.com/xNcGtge.png)

![Imgur](https://i.imgur.com/wxT4YIs.png)

![Imgur](https://i.imgur.com/1znhfjD.png)

![Imgur](https://i.imgur.com/HU2wFi2.png)



Volume
* A directory that is accessible to all containers in Pod.
* Requirements of the Volume can be specified using Pod specification
* You must mount these Volumes specifically on each container within a Pod
* Set up Volumes using external storage outside of your Pods to provide durable storage



Services provide load-balanced access to specified Pods. There are three primary types of Services:

* ClusterIP: Exposes the service on an IP address that is only accessible from within this cluster. This is the default type.
* NodePort: Exposes the service on the IP address of each node in the cluster, at a specific port number.
* LoadBalancer: Exposes the service externally, using a load balancing service provided by a cloud provider.

In Google Kubernetes Engine, LoadBalancers give you access to a regional Network Load Balancing configuration by default. To get access to a global HTTP(S) Load Balancing configuration, you can use an Ingress object.

You will learn more about Services and Ingress objects in a later module in this specialization.


========================

This reading explains the relationship among several Kubernetes controller objects:

ReplicaSets
Deployments
Replication Controllers
StatefulSets
DaemonSets
Jobs
A ReplicaSet controller ensures that a population of Pods, all identical to one another, are running at the same time. Deployments let you do declarative updates to ReplicaSets and Pods. In fact, Deployments manage their own ReplicaSets to achieve the declarative goals you prescribe, so you will most commonly work with Deployment objects.

Deployments let you create, update, roll back, and scale Pods, using ReplicaSets as needed to do so. For example, when you perform a rolling upgrade of a Deployment, the Deployment object creates a second ReplicaSet, and then increases the number of Pods in the new ReplicaSet as it decreases the number of Pods in its original ReplicaSet.

Replication Controllers perform a similar role to the combination of ReplicaSets and Deployments, but their use is no longer recommended. Because Deployments provide a helpful "front end" to ReplicaSets, this training course chiefly focuses on Deployments.

If you need to deploy applications that maintain local state, StatefulSet is a better option. A StatefulSet is similar to a Deployment in that the Pods use the same container spec. The Pods created through Deployment are not given persistent identities, however; by contrast, Pods created using StatefulSet have unique persistent identities with stable network identity and persistent disk storage.

If you need to run certain Pods on all the nodes within the cluster or on a selection of nodes, use DaemonSet. DaemonSet ensures that a specific Pod is always running on all or some subset of the nodes. If new nodes are added, DaemonSet will automatically set up Pods in those nodes with the required specification. The word "daemon" is a computer science term meaning a non-interactive process that provides useful services to other processes. A Kubernetes cluster might use a DaemonSet to ensure that a logging agent like fluentd is running on all nodes in the cluster.

The Job controller creates one or more Pods required to run a task. When the task is completed, Job will then terminate all those Pods. A related controller is CronJob, which runs Pods on a time-based schedule.

Later modules in this specialization will cover these controllers in more depth.


## What is the purpose of a Service? Choose all that are true (2 correct answers)
To provide a load-balancing network endpoint for Pods

To allow you to choose how Pods are exposed

To allow you to put constraints on Pods' resource consumption

To provide a way to inspect and diagnose code running in a Pod


## If you are deploying applications in your Pods that need persistent storage, which controller type should you use?
StatefulSet

Deployment

DaemonSet

ReplicaSet


## You are designing an application, and you want to ensure that the containers are located as close to each other as possible, in order to minimize latency. Which design decision helps meet this requirement?

Place the containers in the same Namespace.

Give the containers the same labels.

Place the containers in the same Pod.

Place the containers in the same cluster.

## Which Kubernetes component does the kubectl command connect to in order to carry out operations on a cluster?

kube-controller-manager

kube-scheduler

kube-dns

kube-apiserver

## You need to ensure that the production applications running on your Kubernetes cluster are not impacted by test and staging deployments. Which features should you implement and configure to ensure that the resources for your production applications can be prioritized?

Configure labels for Test, Staging and Production and configure specific Kubernetes resource quotas for the Production Namespace.

Configure resource requests for Test, Staging and Production and configure specific Kubernetes resource quotas for the Production Namespace.

Configure Namespaces for Test, Staging and Production and configure specific Kubernetes resource quotas for the Production Namespace.

Configure Namespaces for Test, Staging and Production and configure specific Kubernetes resource quotas for the test and staging Namespaces.

## When configuring storage for stateful applications, what steps must you take to provide file system storage inside your containers for data from your applications that will not be lost or deleted if your Pods fail or are deleted for any reason?

You must create Volumes using local Storage on the Nodes and mount the Volumes inside your containers to provide durable storage.

You must mount NFS Volumes on each container in the Pod that requires durable storage.

You must create Volumes using network based storage to provide durable storage remote to the Pods and specify these in the Pods.

You must export the data from your applications to a remote service that preserves your data.

## You have a new logging and auditing utility that you need to deploy on all of the nodes within your cluster. Which type of controller should you use to handle this task?

Deployment.

DaemonSet

ReplicaSet

StatefulSet

## You want to deploy multiple copies of your application, so that you can load balance traffic across them. How should you deploy this application's Pods to the production Namespace in your cluster?

Create a Service manifest for the LoadBalancer that specifies the number of replicas you want to run.

Create a Deployment manifest that specifies the number of replicas that you want to run.

Deploy the Pod manifest multiple times until you have achieved the number of replicas required.

Create separate named Pod manifests for each instance of the application and deploy as many as you need.



Video Link:

https://youtu.be/oiN7xFylVDg

https://youtu.be/JkcQF5XOL2Y


Buy me a cup of coffee:

![Imgur](https://i.imgur.com/YbpJh2d.png)
