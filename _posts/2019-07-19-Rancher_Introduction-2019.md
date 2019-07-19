---
layout:     post   				    # 使用的布局（不需要改）
title:      Rancher Introduction				# 标题
subtitle:   Technical Architecture(Version_2.1) #副标题
date:       2019-07-19				# 时间
author:     frankie 						# 作者
header-img: img/11.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
    - Rancher
---

# Rancher Introduction
## Technical Architecture: Version 2.1

### Motivation
Rancher container management platform is developed to address the need to manage containers in production.

### Timeline
Cattle - Rancher 1.0 - 2016 - container orchestration - Supporting Swarm, Mesos, and Kubernetes.

Rancher 2.0 - 2017 - k8s far outoaced other orchestrators - Rancher 2.0 focus solely on k8s technology.

### Problems encounted and Outcome
#### 2015 - k8s support - how to install and setup k8s cluster ->

button click cluster creation on any infrastructure, including public cloud, vSphere cluster, and bare metal servers ->

one of the most popular ways to launch k8s cluster

[Alternatives: kubeadm, KRIB, kops, Kubespray]

#### The early 2016 - numerous off-the-shelf and 3rd party installers for k8s ->

how to install and configure k8s is no longer the challenge ->

but how to operate and upgrade k8s cluster on an on-going basis. -> Rancher makes it easy to operate and upgrade k8s cluster and its associated etcd database.

#### The end of 2016 - Rancher noticed that the value of k8s operations software was rapidly diminishing due to
* 1 - open-source tools, such as Kubernetes
Operations (kops), have reached a level of maturity that made it easy for many organizations to operate
Kubernetes on AWS.

* 2 - Kubernetes-as-a-service started to gain popularity. A Google Cloud user,
for example, no longer needed to set up and operate their own clusters. They could use Google
Container Engine (GKE) instead.

#### June 2018 - AWS announced GA of Elastic Container Service for k8s (EKS)
k8s-as-a-service is also available from all major cloud providers. Unless they use VMware clusters and bare metal servers, DevOps teams will no longer need to operate k8s cluster themselves.

The only remaining challenge will be how to manage and utilize k8s cluster, which are available everywhere.

# Rancher 2.0: Built on Kubernetes
Rancher 2.0 is a complete container management platform built on Kubernetes with three major components.

  ![Imgur](https://i.imgur.com/nD2OFil.png)

## 1-RKE
RKE is an extremely simple, lightning fast Kubernetes installer that works everywhere. RKE is particularly
useful in standing up Kubernetes clusters on VMware clusters, bare metal servers, and VM instances on
clouds that do not yet support Kubernetes service.

RKE manages the complete lifecycle of Kubernetes clusters from initial install to on-going maintenance.
Rancher users can:
1. Automate VM instance provisioning on many clouds using Docker machine drivers.
2. Install Kubernetes masters, etcd nodes, and worker nodes.
3. Add or remove nodes in existing Kubernetes clusters.
4. Upgrade Kubernetes clusters to new versions.
5. Monitor the health of Kubernetes clusters.

## 2-Unified Cluster Management
Rancher 2.0 manages to use managed Kubernetes services such as GKE, EKS, and AKS, or provision
and operate RKE Kubernetes clusters on any cloud, virtualized, or bare metal infrastructure.

* centralized authentication

* authentication proxy - Active Directory credentials

* centralized access control policies built on top of centralized authentication - native RBAC

* Project - one or more Kubernetes namespaces
and their associated policies such as RBAC rules, resource quota, etc - multitenant self-service functionality of Kubernetes.

* [Coming] provide visibility into capacity and cost of underlying resources consumed by Kubernetes clusters.

## 3-Application Workload Management
Intuitive user interface for managing application workloads.

* The app catalog in Rancher 1.0 has been adapted to support Helm Charts.

* Helm is a powerful templating mechanism for deploying applications on k8s.

* Helm Charts lengthy documentation to understand variables and values {error-prone} -> exposing just the right set of variables and by guiding the user through the process.

* Works with any CI/CD system integrate with k8s like Jenkins, Drone, and GitLab. Rancher 2.0 additionally
includes a managed CI/CD service built on Jenkins. The Rancher 2.0 CI/CD service seamless integrate
with Rancher UI.

* Works with any monitoering and logging system that integrate with k8s like DataDog, Sysdig, and ELK. The Rancher 2.0 includes a managed Prometheus alert manager.

* [Coming] include a managed Prometheus service for
out-of-the-box monitoring.

# High-level Architecture
Rancher 2.0 software consists of two parts. The Rancher server components manages the entire
Rancher deployment. Rancher also deploys agent components into Kubernetes clusters and nodes.

  ![Imgur](https://i.imgur.com/6nYYR0i.png)

## Rancher Server Components  
### Rancher API Server
* Rancher API server is built on top of an embedded Kubernetes API server and etcd database.

* All Rancher specific resources that are being created using Rancher API, get translated to CRD (Custom Resource Definition) objects, with their lifecycle being managed by one or several Rancher controllers.

* Foundational layer for all controllers in the Rancher server.
  * User facing API schema generation with an ability to plug custom formatters and validators.
  * Controller interfaces generation for CRDs and native Kubernetes objects types
  * Object lifecycle management framework
  * Conditions management framework
  * Simplified generic controller implementation by encapsulating TaskQueue and SharedInformer
  logic into a single interface

### Management Controllers
activities happen at the Rancher server level, not specific to an individual cluster.

1. Configuring access control policies to clusters and projects
2. Managing pod security policy templates
3. Provisioning clusters by invoking the necessary Docker machine drivers and invoking Kubernetes
engines like RKE and GKE
4. Managing users – CRUD operations on users
5. Managing global-level catalog, fetch content of the upstream Helm repo, etc.
6. Managing cluster and project-level catalogs
7. Aggregating and displaying cluster stats and events
8. Managing of node drivers, node templates, and node pools
9. Managing cluster cleanup when cluster is removed from Rancher

### User Cluster Controllers
1. Managing workloads, which includes, for example, creating pods and deployments in each
cluster
2. Applying roles and bindings that are defined in global policies into every cluster
3. Propagating information from cluster to rancher server: events, stats, node info, and health
4. Managing network policies
5. Managing alerts, log aggregation, and CI/CD pipelines
6. Managing resource quota.
7. Propagating secrets down from Rancher server to individual clusters

User cluster controllers connect to API servers in GKE clusters directly, but tunnel through the cluster agent to connect to API servers in RKE clusters.

### Authentication Proxy
The authentication proxy proxies all Kubernetes API calls. It integrates with authentication services like local authentication, Active Directory, and GitHub.

On every Kubernetes API call, the authentication proxy authenticates the caller and sets the proper Kubernetes impersonation headers before forwarding the call to Kubernetes masters.

Rancher communicates with Kubernetes clusters using a service account.

The authentication proxy connects to API servers in GKE clusters directly, but tunnels through the cluster agent to connect to API servers in RKE clusters.

## Rancher Agent Components
Software components deployed in Kubernetes clusters managed by Rancher.
### Cluster Agents
The cluster agent opens a WebSocket tunnel back to Rancher server so that the user cluster controllers and authentication proxy can communicate with user cluster Kubernetes API server.

Note that only RKE clusters and imported clusters utilize the cluster agent to tunnel Kubernetes API. Cloud Kubernetes services like GKE already exposes API endpoint on the public Internet and therefore does not require the cluster agent to function as a tunnel.

Cluster agents serve two additional functions:
1. They serve as a proxy for other services in the cluster, like Rancher’s built-in alert, log
aggregation, and CI/CD pipelines. In fact, any services running in user clusters can be exposed
through the cluster agents. This capability is sometimes called “the magic proxy.”
2. During registration, cluster agents get service account credentials from the Kubernetes cluster
and send the service account credentials to the Rancher server.

### Node Agents
Node agents are primarily used by RKE to deploy the components during initial install and follow-on upgrades. Node agents, however, are deployed on cloud Kubernetes clusters like GKE even though they are not needed for Kubernetes install and upgrade. Node agents serve several additional functions for all clusters:
1. Fallback for cluster agents: if the cluster agent is not available for any reason, Rancher server will use node agent to connect to the Kubernetes API server.

2. Proxy for kubectl shell. Rancher server connects through node agents to tunnel the
kubectl shell in the UI. Node agent runs with more privileges than cluster agent, and that additional privilege is required to tunnel the kubectl shell.

### High Availability
Rancher server can be deployed on an existing Kubernetes cluster. In that case, the Rancher server is highly available as long as there the underlying Kubernetes cluster has no single point of failure.

### Scalability
#### Scalability of Kubernetes Clusters
As of Kubernetes version 1.6, A Kubernetes cluster can scale to 5,000 nodes and 150,000 pods. User can expect Rancher 2.0 to manage clusters up to that scale as well.

#### Scalability of Rancher Server
There is no inherent limit on how many Kubernetes clusters each Rancher server can manage. We do
not expect an issue for Rancher 2.0 to manage up to 1,000 clusters.
The real scalability limits of Rancher server are:

1. Total nodes across all clusters
2. Users and groups
3. Events collected from all clusters

Rancher server stores all the above entities in its own database. We will improve scalability along these dimensions over time to meet user needs.
