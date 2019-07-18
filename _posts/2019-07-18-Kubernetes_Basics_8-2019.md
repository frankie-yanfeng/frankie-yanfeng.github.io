---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 8				# 标题
subtitle:   k8s sharing #副标题
date:       2019-07-18				# 时间
author:     frankie 						# 作者
header-img: img/5.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# StatefulSets
Stateful sets are useful for stateful applications. Unlike deployments, stateful sets maintain a persistent identity for each pod. Each pod in a stateful set maintains a persistent identity and has an ordinal index with the relevant pod name, a stable hostname and stably identified persistent storage that is linked to the ordinal index.

  ![Imgur](https://i.imgur.com/W2iJqKL.png)

  An ordinal index is just a unique sequential number that is assigned to each pod in the stateful set. This number defines the pods position in the sets sequence of pods.

  ![Imgur](https://i.imgur.com/QC78xHi.png)

  ![Imgur](https://i.imgur.com/viLaBia.png)

  ![Imgur](https://i.imgur.com/e6kfTAg.png)


  Stateful sets require a service to control their networking. Sometimes you may not want or need load balancing and a single service IP. In this case, a headless service is created by specifying none for the cluster IP in the service definition as shown here on the left.

  ![Imgur](https://i.imgur.com/242Z0ss.png)

  ![Imgur](https://i.imgur.com/4fqxcAj.png)

  ![Imgur](https://i.imgur.com/PGE6Plf.png)

  ![Imgur](https://i.imgur.com/jkSedJH.png)

# ConfigMaps

  ConfigMaps decouple configuration from Pods. This means that you don't have to enter the same information across multiple parts specifications. This prevents config drift.

  ![Imgur](https://i.imgur.com/EUrNWcn.png)

## Creating a ConfigMaps

  ```
  kubectl create configmap [NAME] [DATA]
  ```

  ![Imgur](https://i.imgur.com/Vmbb3dF.png)

  ![Imgur](https://i.imgur.com/tv2djFc.png)

  ![Imgur](https://i.imgur.com/QibEjRK.png)

  ![Imgur](https://i.imgur.com/c01DG5A.png)

  ![Imgur](https://i.imgur.com/BArPASR.png)

  ![Imgur](https://i.imgur.com/GLZvrnf.png)

  ![Imgur](https://i.imgur.com/JkJTcv0.png)

  ![Imgur](https://i.imgur.com/Wxw9AoS.png)

  ![Imgur](https://i.imgur.com/gsVJ8wM.png)

  Each nodes Kubelet periodically syncs with ConfigMap
  to keep the ConfigMap volume updated. When a ConfigMap volume is already mounted and the source ConfigMap is changed, the projected keys are eventually updated. It is on the order of seconds or minutes. If you have a piece of configuration data that will change more rapidly than that, you should probably implement a micro-service to provide its value to Pods rather than using a ConfigMap.

  ![Imgur](https://i.imgur.com/pyA2YNu.png)

# Secrets

  Just like ConfigMaps, Secrets pass information to pods. Secrets let you manage sensitive information in their own control plane. Secrets also helped to ensure Kubernetes doesn't accidentally output these data to logs.

  ![Imgur](https://i.imgur.com/qXYgd0Z.png)

  ![Imgur](https://i.imgur.com/Omlo8w2.png)

  Values are in base64 encoded strings.

  ![Imgur](https://i.imgur.com/QTFEUFI.png)

  ![Imgur](https://i.imgur.com/cD1jlpr.png)

  ![Imgur](https://i.imgur.com/376WFI8.png)

  ![Imgur](https://i.imgur.com/OlfVDYu.png)

  ![Imgur](https://i.imgur.com/jd8ZcuH.png)
