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

## Connecting to a Google Kubernetes Engine (GKE) cluster

```
gcloud container clusters \
  get-credentials [CLUSTER_NAME] \
  --zone [ZONE_NAME]
```

## Explaining kubectl syntax

  ![Imgur](https://i.imgur.com/qABiDQO.png)

  ![Imgur](https://i.imgur.com/HeSTsyk.png)

  ![Imgur](https://i.imgur.com/xIC0tvE.png)

  ![Imgur](https://i.imgur.com/4m0MgOc.png)


# Introspection

  ![Imgur](https://i.imgur.com/9k9qYGL.png)


  ```
  kubectl get pods
  ```

  ![Imgur](https://i.imgur.com/Og7TLjY.png)

  CrashLoopBackOff means that the pod isn't configured correctly.


  ```
  kubectl describe pod [POD_NAME]
  ```

  ![Imgur](https://i.imgur.com/KmTF7cR.png)

  ```
  kubectl exec [POD_NAME] -- [command]
  ```

  ![Imgur](https://i.imgur.com/C72Ypgy.png)

  ```
  kubectl exec -it [POD_NAME] -- [command]
  ```

  ![Imgur](https://i.imgur.com/geNEJBw.png)

  Pass terminal's standard input (stdin) to the container.

  Display the container's standard output (stdout) in your terminal window.

  Uses the flags -i and -t (or -it).


  ```
  kubectl logs [POD_NAME]
  ```

  ![Imgur](https://i.imgur.com/VYYIvqF.png)
