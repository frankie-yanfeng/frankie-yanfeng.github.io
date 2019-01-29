---
layout:     post   				    # 使用的布局（不需要改）
title:      Dermatologist-level classification of skin cancer with deep neural networks 				# 标题
subtitle:   Paser Summary #副标题
date:       2019-01-29 				# 时间
author:     frankie 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - tech
    - paper
    - skin
    - CNN
---

# Paper Summary - Dermatologist-level classification of skin cancer with deep neural networks

## Topic: Skin cancer - automated classification of skin lesions using images.

## Motivation:
1. most common human malignancy
2. visually diagnosis
3. dermoscopic analysis

## Challenges:
1. fine-grained variability in the appearance of skin lesions.

2. insufficient data.

3. zoom, angle and lighting variability.

## Solutions:
1. CNN

2. data driven approach - 1.41 million pre-training and traning images make classification robust to photographic variability.

3. single network for both photographic and dermoscopic images.

4. partition diseases into fine-grained training classes.

## Dataset:
1. 129450 clinical images.

2. 2032 diseases.

## Performance:
1. dermatologists level competence.

2. 72.1 ~ 0.9% overall accuracy on three-class diseases partition (first-level nodes of taxonomy: benign lesion, malignant lesions and non-neoplastic) vs (65.56% & 66.0%) dermatologists accuracy.

3. 55.4 ~ 1.7% overall accuracy on nine-class disease partition (the second-level nodes) vs (53.3% & 55.0% ) same
dermatologists accuracy.

4. finer disease partition performs better than one trained directly on three or nine classes.

## Metric:
* binary classification - malignant vs benign

* sensitivity = (true positive) / positive

* specificity = (true negative) / negative

## Network and methods:
transfer learning = pretrained + retrained + tuning
1. GoogleNet Inception v3 CNN architecture.

2. transfer learning pretrained on ImageNet.

3. during inference, the CNN outputs a probability distribution over these fine
classes.

4. fine-tuning - retraining the last layers with own data & adjusting the parameters across all layers.

5. nine-fold cross-validation.

6. learning rate of 0.001.

7. decay factor of 16 every 30 epochs.

8. RMSProp with decay of 0.9, momentum of 0.9 and epsilon of 0.1.

9. tensorflow

10. data augmentation - a factor of 720. Each image is rotated randomly between 0 and 359. The largest upright inscribed rectangle is then cropped from the image, and is flipped vertically with a probability of 0.5.

11. Confusion matrices.

12. Saliency maps - Backpropagation is an
application of the chain rule of calculus to compute loss gradients for all weights
in the network. The loss gradient can also be backpropagated to the input data layer.
By taking the L1 norm of this input layer loss gradient across the RGB channels, the
resulting heat map intuitively represents the importance of each pixel for diagnosis.
As can be seen, the network fixates most of its attention on the lesions themselves
and ignores background and healthy skin.


## Visualization:
* t-SNE (t-distributed Stochastic Neighbour Embedding) of the last hidden layer representations in the CNN for given number classes.

## Dataset:
* combination of open-access dermatology repositories and privately collected data annotated by dermatologist.

## Data preparation:
* Blurry images and far-away images were removed from the test and validation sets, but were still used in training.

* dataset contains images corresponding to the same lesions but from multiple viewpoints, or multiple images of similar lesions on the same persion. -> useful training data, but not split between the training and validation sets.

* based on image metadata and nearest neighbour image retrieval with CNN features (last hidden layer), undirected graph is created to connect any pair of images that were determined to be similar. Connected components of this graph were not allowed to straddle the train/validation split and were randomly assigned to either train or validation.

* test sets are independent, high-quality repositories of biopsy-proven images.

* image size: 299 X 299 pixels.

* data augmentation to balance dataset.


Reference:
* https://cs.stanford.edu/people/esteva/nature/
* https://www.nature.com/articles/nature21056.epdf?author_access_token=8oxIcYWf5UNrNpHsUHd2StRgN0jAjWel9jnR3ZoTv0NXpMHRAJy8Qn10ys2O4tuPakXos4UhQAFZ750CsBNMMsISFHIKinKDMKjShCpHIlYPYUHhNzkn6pSnOCt0Ftf6
