---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 3				# 标题
subtitle:   k8s sharing #副标题
date:       2019-07-12				# 时间
author:     frankie 						# 作者
header-img: img/post-bg-keybord.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# The kubectl Command

  ![Imgur](https://i.imgur.com/I5Eh4HK.png)

* kubectl transfers command line entries into API calls, then send to the Kube apiserver within selected kubernetes cluster.

* kubectl must be configured with the location and credentials of a kubernetes cluster.

  ![Imgur](https://i.imgur.com/65zzLjn.png)

## The kubectl must be configured first

* Relies on a config file: $HOME/.kube/config

* Config file contains:
  * Target cluster name
  * Credentials for the cluster


* Current config: kubectl config view
 (configuration of the Kubectl command itself)

 ![Imgur](https://i.imgur.com/uc5SFO5.png)






## What is the difference between a pod and a container?
