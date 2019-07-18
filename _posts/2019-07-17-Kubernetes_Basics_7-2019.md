---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 7				# 标题
subtitle:   k8s sharing #副标题
date:       2019-07-17				# 时间
author:     frankie 						# 作者
header-img: img/14.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# Volumes
## Kubernetes offers storage abstraction options

  ![Imgur](https://i.imgur.com/QnNzMw8.png)

  ConfigMap & emptyDir

  ![Imgur](https://i.imgur.com/5pendt0.png)

  ![Imgur](https://i.imgur.com/3FospxV.png)

# Volume types

  ![Imgur](https://i.imgur.com/5pEyh4N.png)

  ![Imgur](https://i.imgur.com/eicUlLc.png)

  ![Imgur](https://i.imgur.com/hdFHWJa.png)

  ![Imgur](https://i.imgur.com/bhROtHW.png)

  ![Imgur](https://i.imgur.com/egejkda.png)

  ![Imgur](https://i.imgur.com/p6olsbb.png)

  ![Imgur](https://i.imgur.com/MDA6k7w.png)

  ![Imgur](https://i.imgur.com/DKFTfOo.png)

  ![Imgur](https://i.imgur.com/Sntq5Re.png)

  ![Imgur](https://i.imgur.com/GAvntT4.png)

  ![Imgur](https://i.imgur.com/92nrGih.png)

  ![Imgur](https://i.imgur.com/ipwxEKu.png)

  ![Imgur](https://i.imgur.com/CE3sLXY.png)

## PersistentVolumes abstraction has two components

  ![Imgur](https://i.imgur.com/KgrYgqT.png)

  ![Imgur](https://i.imgur.com/htfuPsU.png)

  ![Imgur](https://i.imgur.com/Lv7Q1Gt.png)

  ![Imgur](https://i.imgur.com/Tgl1a72.png)

  ![Imgur](https://i.imgur.com/DpaTays.png)

  ![Imgur](https://i.imgur.com/8Whs2Lf.png)

  ![Imgur](https://i.imgur.com/SjJh14s.png)

## AccessModes determine how the Volume will read or write

  ReadWriteOnce mounts the volume as readwrite to a single node.

  ![Imgur](https://i.imgur.com/M2ML7yk.png)

  ReadOnlyMany mounts a volume as read-only to many nodes.

  ![Imgur](https://i.imgur.com/3xtnGL6.png)

  ReadWriteMany mounts volumes as readwrite to many nodes.

  ![Imgur](https://i.imgur.com/lMsXk6v.png)

  GCP persistent disks do not support ReadWriteMany

  NFS supports the ReadWriteMany access mode.

  ![Imgur](https://i.imgur.com/S0goNvS.png)

  ![Imgur](https://i.imgur.com/qkAi5YE.png)

## An alternative option is Dynamic Provisioning

  ![Imgur](https://i.imgur.com/yephDWx.png)

  ![Imgur](https://i.imgur.com/MCP0HIf.png)

  ![Imgur](https://i.imgur.com/3tyPGRe.png)

  
