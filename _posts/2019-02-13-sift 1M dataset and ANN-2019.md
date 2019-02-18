---
layout:     post   				    # 使用的布局（不需要改）
title:      sift 1M dataset and ANN 				# 标题
subtitle:   approximate nearest neighbor #副标题
date:       2019-02-18 				# 时间
author:     frankie 						# 作者
header-img: post-bg-keybord.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - tech
    - ANN
    - sift
    - hash
---

## Datasets for approximate nearest neighbor search

[sift 1M Datasets](http://corpus-texmex.irisa.fr/)

After searching a while, I found above link which hosts the datasets for sift 1M and other related datasets for ANN.

The link provides the script (fvecs_read.m) to read the data whose dimension is 128 each. Below are the results after dimension reduction into two (10,000 from 1M for illustration purpose):

![Imgur](https://i.imgur.com/hjufIXA.jpg)

![Imgur](https://i.imgur.com/zzbUDK8.jpg)

![Imgur](https://i.imgur.com/crfcBCG.jpg)

The vector is 128 integers as below, while our 128 dimensional sift vectors in center file is float type.

![sift 128](https://i.imgur.com/wNGYRUQ.png)

The next step is to build vocabulary (index) or hash in different ways, for examples, right now, we are using kgraph for approximate nearest neighbour search, which is a graph construction based approach.

The other methods are as below:
1. KD tree - Annoy
2. Hash (Local Sensitive Hashing) - LSHash & FALCONN
3. Vector Quantization (Product Quantization) (ANN) - FAISS, NMSLIB, etc.

[ANN-benchmarks](https://github.com/erikbern/ann-benchmarks) provides the algorithm comparisions for reference. In our context, we are focusing on sift-128-euclidean, which is shown as below:

![Imgur](https://i.imgur.com/IwybXoH.png)

The next move is to verify the algorithms starting from falconn.
