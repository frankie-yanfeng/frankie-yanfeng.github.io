---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 10				# 标题
subtitle:   k8s sharing #副标题
date:       2019-07-22				# 时间
author:     frankie 						# 作者
header-img: img/2.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# Google Kubernetes Engine (GKE) Logging and Monitoring

* Use Stackdriver to monitor and manage the availability and performance
* Locate and inspect Kubernetes logs
* Perform forensic analysis of logs
* Monitor performance
* Create probes for wellness checks on live applications

## Stackdriver

  ![Imgur](https://i.imgur.com/oVPT7Kb.png)

  ![Imgur](https://i.imgur.com/tWMRf3q.png)

  ![Imgur](https://i.imgur.com/ymz1GjM.png)

  ![Imgur](https://i.imgur.com/o7AWylg.png)

  Metrics represents the bottleneck, which event shows the errors.

  ![Imgur](https://i.imgur.com/FukuICc.png)

  ![Imgur](https://i.imgur.com/9oTsBT4.png)

  ![Imgur](https://i.imgur.com/fowdXA4.png)

  ![Imgur](https://i.imgur.com/hyyxW5q.png)

## Logging

  ![Imgur](https://i.imgur.com/LIMI5SS.png)

  GKE automatically streams it's logs to Stackdriver to give you better visibility into the events and activity of your cluster.

  ![Imgur](https://i.imgur.com/sKYKwQ4.png)

## Kubernetes native logging

  Logs from Kubernetes system components such as Kubelets and KubeProxy are stored in each node's file system in its /var/log directory.

  ![Imgur](https://i.imgur.com/sCDQ3EY.png)

## Logging with Kubernetes and Stackdriver

  ![Imgur](https://i.imgur.com/H5U9RFJ.png)

  ![Imgur](https://i.imgur.com/6KyoGpq.png)

  ![Imgur](https://i.imgur.com/Y2t2VZo.png)

  ![Imgur](https://i.imgur.com/UKSer4r.png)

  ![Imgur](https://i.imgur.com/nYzY9aq.png)

  ![Imgur](https://i.imgur.com/MZboWvd.png)

  ![Imgur](https://i.imgur.com/0CHTpMH.png)

## Monitoring
  Service reliability hierarchy
  ![Imgur](https://i.imgur.com/fcSSoYE.png)

  ![Imgur](https://i.imgur.com/dI49p3W.png)

  ![Imgur](https://i.imgur.com/oyvtVjn.png)

  ![Imgur](https://i.imgur.com/Pd2MExH.png)

  ![Imgur](https://i.imgur.com/xeE0lst.png)

  ![Imgur](https://i.imgur.com/j295tkq.png)

  ![Imgur](https://i.imgur.com/v3ln9CE.png)

## Probes

  Service dependencies

  ![Imgur](https://i.imgur.com/6Mnh0bn.png)

  ![Imgur](https://i.imgur.com/wJGixDw.png)

  ![Imgur](https://i.imgur.com/WXzQ75S.png)

  ![Imgur](https://i.imgur.com/s11EyNe.png)

  If the request returns with a code range from 200 to 400, kubelet considers the container healthy.

  ![Imgur](https://i.imgur.com/w1C8oQj.png)

  ![Imgur](https://i.imgur.com/vk9ttLD.png)

  
