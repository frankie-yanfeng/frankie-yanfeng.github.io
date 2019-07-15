---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 6				# 标题
subtitle:   k8s sharing #副标题
date:       2019-07-15				# 时间
author:     frankie 						# 作者
header-img: img/12.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# Pod networking
## A Pod is a group of containers with shared storage and networking

IP per pod model

  ![Imgur](https://i.imgur.com/R7Vl9WU.png)

Because both containers share the same networking namespace, the two containers appears as through they are installed on the same machine. The nginx container will contact the legacy application by establishing a connection to local host on tcp port 8000.

  ![Imgur](https://i.imgur.com/QNL1cGr.png)

  ![Imgur](https://i.imgur.com/GCxF1vG.png)

  ![Imgur](https://i.imgur.com/Y6IZPkl.png)

  ![Imgur](https://i.imgur.com/Fjmrc1X.png)

Two pods communicate on the same node.

  ![Imgur](https://i.imgur.com/62xSR7J.png)

  ![Imgur](https://i.imgur.com/RQdicjb.png)

Pod IP is from the VPC (logically isolated networks that provide connectivity for resources you deploy within GCP. such as kubernetes clusters, Compute Engine instances, and App Engine)

AVPC can be composed of many different IP subnets in regions all around the world.

  ![Imgur](https://i.imgur.com/kD9XBHR.png)

  ![Imgur](https://i.imgur.com/b4jcz1Q.png)

  ![Imgur](https://i.imgur.com/UVbAsgK.png)

  ![Imgur](https://i.imgur.com/HgXKREH.png)

The pod's IP address are part of the address range called an alias IP. GKE automatically configures your VPC to recognize this range of IP address as an authorized secondary subnet of IP addresses.

As a result, the pod's traffic is permitted to pass the anti-spoofing filters on the network.

  ![Imgur](https://i.imgur.com/yAg0DDO.png)

Also, because each node maintains a separate IP address base for its pods, the nodes don't need to perform network address translation on the pod IP address. That means that the pods can directly connect to each other using their native IP addreses.

The traffic from your cluster is routed or peered inside GCP, but becomes now translated, at the node IP address if it has to exit GCP.


  ![Imgur](https://i.imgur.com/jxvautk.png)

  On GCP, Alias IPs allow you to configure additional secondary IP addresses or IP ranges on your Compute Engine VM instances. VPC-Native GKE clusters automatically create an Alias IP range to reserve approximately 4000 IP addresses for cluster-wide Services that you may create later.

# Services

  In an ever-changing container environment, Services give Pods a stable IP address and name that remains the same through updates, upgrades, scalability changes, and even Pod failures. Instead of connecting to a specific Pod, applications on Kubernetes rely on Services to locate suitable Pods and forward the traffic via those Services rather than directly to Pods.


  ![Imgur](https://i.imgur.com/5p8qzaf.png)

  ![Imgur](https://i.imgur.com/T32Z68c.png)

  ![Imgur](https://i.imgur.com/mVuyB4t.png)

  ![Imgur](https://i.imgur.com/3T6upM2.png)

## Finding Services (Service Discovery)

  There are several ways to find a Service in GKE

  * Environment Variables
  * Kubernetes DNS
  * lstio

  ![Imgur](https://i.imgur.com/MuDwCrI.png)

  Problems: For created pods, the environment variables does work, so no way to control them.

  ![Imgur](https://i.imgur.com/ylGeRGM.png)

  IP based solution to connect the service and pods

  ![Imgur](https://i.imgur.com/bqFttER.png)

  ![Imgur](https://i.imgur.com/kWFgIGJ.png)

  ![Imgur](https://i.imgur.com/C2jiIIz.png)

  For pod and service in the same namespace, use the short DNS name, otherwise, the namespace is needed for DNS solution.

  ![Imgur](https://i.imgur.com/KkbC6DY.png)

  ![Imgur](https://i.imgur.com/M77YFnS.png)

  ![Imgur](https://i.imgur.com/4GShwuk.png)

  ![Imgur](https://i.imgur.com/8U72AjO.png)

  ![Imgur](https://i.imgur.com/KrF3Ecg.png)

## Service Types and Load Balancers

    * ClusterIP
    * NodePort
    * Load balancer

  ![Imgur](https://i.imgur.com/CnFAGsc.png)

  ![Imgur](https://i.imgur.com/1nJULaL.png)

Internal communications within a cluster - ClusterIP

  ![Imgur](https://i.imgur.com/oUQYrBJ.png)

NodePort is built on top of cluster IP's service.

  ![Imgur](https://i.imgur.com/5Xi6ca6.png)

  ![Imgur](https://i.imgur.com/1JEzxRh.png)

## How load balancers work

  ![Imgur](https://i.imgur.com/R6duYmN.png)

  ![Imgur](https://i.imgur.com/QyFWslI.png)

  ![Imgur](https://i.imgur.com/VuhqtUu.png)

  ![Imgur](https://i.imgur.com/J9gJLo9.png)

  ![Imgur](https://i.imgur.com/X7iBlqE.png)

  ![Imgur](https://i.imgur.com/DQ4Ogfg.png)

  ![Imgur](https://i.imgur.com/hptKByu.png)

  ![Imgur](https://i.imgur.com/5X9Df0V.png)

## Ingress Resource

  ![Imgur](https://i.imgur.com/VWgT5li.png)

  ![Imgur](https://i.imgur.com/LqomQxW.png)

  ![Imgur](https://i.imgur.com/YYcMZlG.png)

  ![Imgur](https://i.imgur.com/DoXNq4z.png)

  ![Imgur](https://i.imgur.com/4vNocGJ.png)

  ![Imgur](https://i.imgur.com/vPTE0nF.png)

  ![Imgur](https://i.imgur.com/SM5ZGuU.png)

# Container-Native Load Balancing

  ![Imgur](https://i.imgur.com/7PvopbB.png)

  ![Imgur](https://i.imgur.com/qseS8xg.png)

  ![Imgur](https://i.imgur.com/KxpgsvG.png)

# Network Security

  ![Imgur](https://i.imgur.com/At87ohQ.png)

  ![Imgur](https://i.imgur.com/Jlc6GtF.png)

  ![Imgur](https://i.imgur.com/7ufudAh.png)

  ![Imgur](https://i.imgur.com/7ufudAh.png)

  ![Imgur](https://i.imgur.com/maDs7YX.png)

  
