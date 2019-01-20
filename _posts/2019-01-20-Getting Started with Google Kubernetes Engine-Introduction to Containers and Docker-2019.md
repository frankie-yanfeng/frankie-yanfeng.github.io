---
layout:     post   				    # 使用的布局（不需要改）
title:      Getting Started with Google Kubernetes Engine 				# 标题
subtitle:   Introduction to Containers and Docker #副标题
date:       2019-01-20 				# 时间
author:     frankie 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - tech
    - k8s
    - docker
---

![Imgur](https://i.imgur.com/V5gOmM7.png)
# Introduction to Containers
Kubernetes
Container Engine


Containers



Before:
Building applications on individual servers

![Imgur](https://i.imgur.com/9Iq2Fly.png)

Dedicated server:
Application code
Dependencies
Kernel
Hardware
->
Deployment ~months, Low utilization and not portable

![Imgur](https://i.imgur.com/xLoAOJY.png)
![Imgur](https://i.imgur.com/tpSBZGE.png)
![Imgur](https://i.imgur.com/NSgr6cE.png)

Virtual machine:
Application code
Dependencies
Kernel
-----
Hardware + hypervisor
->
Deployment ~days(mins), Improved utilization hypervisor specific
Problem:
Cannot install the same application or multiple versions of the same application into one single VM: dependency errors, conflicts
=> many virtual machines

![Imgur](https://i.imgur.com/m9TxEqM.png)


Container
Application code
Dependencies
Kernel + Container runtime
Hardware
->
Deployment ~mins(sec), portable, very efficient

![Imgur](https://i.imgur.com/5QXXp6F.png)


Benefits:
Code works the same everywhere:
* Across dev, test, and prod

* Across bare-metal, VMs, and cloud

* Packaged apps speed development:

* Agile creation and deployment

* Continuous integration/deliver
* Single file copy

Path to microservices:
* Introspectable, isolated, and elastic

![Imgur](https://i.imgur.com/7V9b5Sz.png)


Containers use a layered file system with only the top layer writable

![Imgur](https://i.imgur.com/mKQMDZl.png)

Containers promote smaller shared images


In the real world you’ll push and pull your image from a registry

# Introduction to Docker

![Imgur](https://i.imgur.com/Z7CR3RN.png)

![Imgur](https://i.imgur.com/Z7wpm1m.png)

![Imgur](https://i.imgur.com/o2JdkzK.png)

![Imgur](https://i.imgur.com/p4Y7aXi.png)
