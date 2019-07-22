---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 9				# 标题
subtitle:   k8s sharing #副标题
date:       2019-07-18				# 时间
author:     frankie 						# 作者
header-img: img/8.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# Access Control and Security in Kubernetes and Google Kubernetes Engine (GKE)

* Understand Kubernetes authentication an authorization
* Define Kubernetes roles and role bindings for accessing resources in namespaces
* Define Kubernetes cluster roles and cluster role bindings for accessing cluster-scoped resources
* Define Kubernetes pod security policies to only allow pods with specific security-related attributes to run
* Understand the structure of Cloud IAM
* Define IAM roles and policies for Google Kubernetes Engine cluster administration

## Authentication and Authorization

  ![Imgur](https://i.imgur.com/6USZufR.png)

  * Google Cloud platform service account
  * Kubernetes service account

### Normal users are managed outside of Kubernetes, which relies on external identity services.

user accounts are defined through Google's identity service.
  * consumer Google accounts
  * accounts in G Suite domains

flexible service:
  * define identity directly
  * mirror accounts from existing directory service such as Active Directory
  * group users

### K8s Service accounts

  ![Imgur](https://i.imgur.com/Do5HAff.png)

  Kubernetes service accounts provide an identity for processes in a pod and used to interact with Kubernetes cluster.

  GCP service account are managed by Google Cloud identity, not Kubernetes and you use them when you want GCP resources to have an identity that's tied to an application or virtual machine not a human being.

  In Kubernetes, every namespace has a default Kubernetes service account.

  ### There are two main ways to authorize in GKE:


  * Cloud IAM is the access control system for managing GCP resources. It lets you grant permissions to users to perform operations at the project and cluster levels (outside Kubernetes clusters).

  * Kubernetes RBAC gives you access control inside your Kubernetes cluster at the cluster level and the namespace level. It lets you create fine tune rules in granular access to resources within the cluster.

  Users use Cloud IAM to define who can view or change the configuration of your GKE clusters. And you'll use Cloud RBAC to define who can view or change Kubernetes objects inside those cluster.

  ![Imgur](https://i.imgur.com/dBByalY.png)

  ![Imgur](https://i.imgur.com/dqwQpUT.png)

## Cloud IAM
How you use Cloud IAM policies and roles to control who can interact with and manage GKE clusters.

  ![Imgur](https://i.imgur.com/YUikbeb.png)

  Use Cloud IAM to grant permissions to members, till up and perform certain operations on certain resources. GCP permissions are grouped into roles based on common user flows.

  permissions can't be individually assigned to members. Instead, members are assigned roles. Every operation on a GCP resource is performed using an API call, for which access is controlled using a permission.

  ![Imgur](https://i.imgur.com/xyGSGR3.png)

  ![Imgur](https://i.imgur.com/7ubPqTG.png)

## Types of IAM roles

  ![Imgur](https://i.imgur.com/Pjk9hVK.png)

  ![Imgur](https://i.imgur.com/7Jd3jYd.png)

  ![Imgur](https://i.imgur.com/ANIN6Ow.png)

## Kubernetes RBAC
In GKE envirtonments, RBAC extends Cloud IAM security by offering control over Kubernetes resources within the cluster, supplementing the control provided directly by Cloud IAM, which allow you to control access of the GKE and cluster level.

  ![Imgur](https://i.imgur.com/RnkTiYp.png)

  * A subject is a set of users or processes that can make requests to the Kubernetes API.
  * A resource is a set of Kubernetes API objects such as a pod, deployment, service or persistent volume.
  * A verb is a set of operations that can be performed on resources such as get, watch, create, and describe.

  ![Imgur](https://i.imgur.com/O2NGwEN.png)

  Roles and RoleBindings can be applied at the cluster or namespace level.

  ![Imgur](https://i.imgur.com/U9pNsCI.png)

  In Kubernetes, there are two types of roles: Role and ClusterRole.

  RBAC Roles are defined at the namespace level, and RBAC ClusterRoles are defined at the cluster level.

  ![Imgur](https://i.imgur.com/kW4vB7s.png)

  ![Imgur](https://i.imgur.com/m5G8ydY.png)

  ![Imgur](https://i.imgur.com/DVBEKWr.png)

## Attaching roles

  ![Imgur](https://i.imgur.com/mkNPD8o.png)

  Subject kind can be users, groups, or service accounts.

  ![Imgur](https://i.imgur.com/0QL7dyc.png)

  ![Imgur](https://i.imgur.com/ZTpsE9L.png)

  ![Imgur](https://i.imgur.com/F8d05Bs.png)

  ![Imgur](https://i.imgur.com/lQp9M39.png)

  ![Imgur](https://i.imgur.com/AKekZgs.png)

## Kubernetes Control Plane Security

  ![Imgur](https://i.imgur.com/R7nAuDo.png)

  Secure communications between the master and nodes at a cluster relies on the shared root of trust provided by the certificates issued by the CA.

  ![Imgur](https://i.imgur.com/cbaJULN.png)

  The Kubernetes API server and the kubelets use secured network communications protocols, namely TLS and SSH.

  ![Imgur](https://i.imgur.com/Xn5mcDj.png)

  ![Imgur](https://i.imgur.com/QRbQ6Yj.png)

  ![Imgur](https://i.imgur.com/DujWOgg.png)

  ![Imgur](https://i.imgur.com/GgKgbpD.png)

  ![Imgur](https://i.imgur.com/AcXxCD2.png)

  ![Imgur](https://i.imgur.com/WB1hCdZ.png)

## Pod Security
  How to control what sorts of things of pod, or more specifically the containers inside a pod are allowed to do.

  In a Linux system, zero is the privileged root user's user ID.

  ![Imgur](https://i.imgur.com/rvEcIai.png)

  ![Imgur](https://i.imgur.com/ASdXtoO.png)

  ![Imgur](https://i.imgur.com/yf3TCYq.png)

## The pod security policy

  ![Imgur](https://i.imgur.com/2CWHd0Y.png)

  ![Imgur](https://i.imgur.com/ASSnjrm.png)

  The pod security policy admission controller acts on the creation and modification of pods, and determines whether the pod should be admitted based on the requested security context and the available pod security policies. Note that these policies are enforced during the creation or updated of a pod, but security context is enforced by the Container Runtime.

## Applying pod security policies

  After you've defined a pod security policy, you need to authorize it. Otherwise it'll prevent any Pod from being created. You can authorize a policy using Kubernetes Role-Based Access Control. Here, a cluster role allows the pod security policy called demo PSP to be used.

  ![Imgur](https://i.imgur.com/4kF8vAX.png)

  ![Imgur](https://i.imgur.com/X0JO3oR.png)

  Two subjects for the role binding are specified. The first is a group containing all service accounts within the demo name-space. The other is a specific service account in the demo name space. The role binding can grant permission to the creator of the pod which might be a deployment, replica set or other template controller or to the created pods service account. Note that granting the controller access to the policy would grant access for all pods created by that controller. So the preferred method for authorizing policies is to grant access to the pods service account.

  ![Imgur](https://i.imgur.com/DAh4mV4.png)

  ![Imgur](https://i.imgur.com/jy743E2.png)

  
