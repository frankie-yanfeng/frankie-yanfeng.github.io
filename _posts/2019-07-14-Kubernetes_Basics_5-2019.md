---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 5				# 标题
subtitle:   k8s sharing #副标题
date:       2019-07-14				# 时间
author:     frankie 						# 作者
header-img: img/13.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# Cluster scaling

## Scaling a cluster from the GCP Console

A node pool, is a subset of node instance within a cluster. They all have the same configuration. Node pools use a NodeConfig specification.

## Scaling a cluster using the gcloud command
```
gcloud container clusters resize projectdemo --node-pool default-pool --size 6
```
## Scale up a cluster with autoscaling

  ![Imgur](https://i.imgur.com/79VMVLl.png)

  ![Imgur](https://i.imgur.com/CCuy9Zk.png)

## Downscaling

  ![Imgur](https://i.imgur.com/pkc69um.png)

  ![Imgur](https://i.imgur.com/OUeXAZY.png)

  ![Imgur](https://i.imgur.com/K4mgzVs.png)

  ![Imgur](https://i.imgur.com/SQTHuot.png)


## Setting a node pool size

  ![Imgur](https://i.imgur.com/RlkWJMd.png)

  ![Imgur](https://i.imgur.com/UcXWpRv.png)


# Controlling pod placement

## Controlled scheduling

  * labels and taints on nodes
  * node affinity rules
  * toleration in deployment specification

  ![Imgur](https://i.imgur.com/afa1UTF.png)

  ![Imgur](https://i.imgur.com/RcjHSvv.png)

  ![Imgur](https://i.imgur.com/bk2ywYu.png)

  If the nodes labels are changed, running pods are not affected. Node selector is only used during pod scheduling.

## Affinity and anti-affinity

  ![Imgur](https://i.imgur.com/EYcqDJO.png)

  ![Imgur](https://i.imgur.com/dgl0TiI.png)

  ![Imgur](https://i.imgur.com/IZZrV4j.png)

  ![Imgur](https://i.imgur.com/jhzXofz.png)

  ![Imgur](https://i.imgur.com/EJrpivb.png)

## Pod placement

  ![Imgur](https://i.imgur.com/TkUCMem.png)

  ![Imgur](https://i.imgur.com/TE15mKm.png)

## Taints allow a node to repel Pods

  * affinity and anti-affinity rules on Pods.
  * taints on nodes

  ![Imgur](https://i.imgur.com/rxcq3Ip.png)

  ![Imgur](https://i.imgur.com/WMFK6Ta.png)

  ![Imgur](https://i.imgur.com/2ptqWER.png)

  ![Imgur](https://i.imgur.com/xUlDYPI.png)

  ![Imgur](https://i.imgur.com/7mjV1mM.png)

## How to get software

1. Build it yourself, and supply your own YAML

2. Use Helm to install software into your cluster

  ![Imgur](https://i.imgur.com/Q6LttZw.png)

  ![Imgur](https://i.imgur.com/vxdMybG.png)

3. Use GCP Marketplace to install both open-source and commercial software
