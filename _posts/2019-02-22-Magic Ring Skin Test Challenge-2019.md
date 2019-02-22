---
layout:     post   				    # 使用的布局（不需要改）
title:      Magic Ring Skin Test Challenge 				# 标题
subtitle:   OIP #副标题
date:       2019-02-22 				# 时间
author:     frankie 						# 作者
header-img: tag-bg-o.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - tech
    - CNN
    - computer vision
    - skin test
---

## Magic Ring Skin Test Challenge

## Background
SK-II has its proprietary ‘Magic Ring ™’ skin test carried out in its outlets where customers can take the skin test to determine their skin age against their biological age. Currently SK-II is using digital media advertising to drive up the number of customers taking this test and through online coupon booking on SK-II’s website.

## Challenges
There are two issues SK-II is currently facing:

(1) Increasing the number of customers taking this skin tests

(2) Reducing the drop-off between customers booking a skin test slot online and actually physically attending the skin test in a store

## Face Skin Test Approaches on market
### Smartphone based app solutions:
The main feature is to collect the full face image and then apply data(image) analysis accordingly including but not limited to:
  * Skin Color: the color comparison of specific areas with surroundings
  * Pore Size:  Pore detection and size calculation in interested areas
  * Acne: Color, size, pattern and area detection and recognition
  * Smoothness
  * Dark Circle
  * Comedo
  * Skin Sensitivity
  * ......
  
The main challenge in smartphone based method is the resource limitation in collecting the images, as per now, most of the apps are using the back-end cameras which have more pixels to capture the images with flash that provides better control on light and other conditions.

Case Study:
App Name: "你今天真好看" ("you looks so good today")

Utilization:
Taking a photo from back cameras under guidance.

Screenshot:

![Imgur](https://i.imgur.com/U6SL2nS.jpg)

Outcome:
A overall skin age score based on the user's skin condition and categorized level in specific dimension.

In terms of user experience, the test repeatability is vital to offer the consistent results for long term and frequent utilization. The other important factors are voice guidance on image capturing, result accuracy and visualization.

Limitations:
Inherent issue is unable to standardize the test procedures. In image capturing process, the light, angle, expression, location, background and distance from the face are all prone to bias and errors which leads to inaccurate results and later unsatisfied user experience.

### Dedicated skin test devices.
These kind of devices are design to solve the problems list above by putting the test in a constant and standard conditions and getting the quantized skin status. There are many of them, like VISIA and OBSERV. The principles behind are taking advantage of light reflection and scattering to complete the skin test as below:

![Imgur](https://i.imgur.com/O3LGmPy.jpg)

![Imgur](https://i.imgur.com/bUZgMim.jpg)

Since the various light itself is reflected and scattered at different levels, our tests are hugely dependent on signals from different light sources, which means different type of light is giving corresponding different signals as below:

![Imgur](https://i.imgur.com/tRMGhYv.jpg)

Based on this, the patterns are shown as below:

![Imgur](https://i.imgur.com/LkZWrWr.jpg)

And if more angles are added, the outcome is even more salient to detect the spots hidden in front images as below:

![Imgur](https://i.imgur.com/lC3iMUY.jpg)

Finally, in market, handed devices are also used for skin test purpose, and we believe current P&G ‘Magic Ring ™’ skin test is in this category. The principles behind are using various wave length light to get the corresponding images for further analysis. And each time, only one particular area is tested for better result.

![Imgur](https://i.imgur.com/Qf28hrd.jpg)

In summary, smartphone apps are convenient but limited solution, on the other hand, the professional skin test devices integrate the software and hardware to efficiently control the light conditions and better test results.

![Imgur](https://i.imgur.com/AkFp5Dn.jpg)

## Problem statement
How can we create a technology-enabled skin test at scale to make it more accessible for customers to take, thereby driving awareness of SK-II’s products and increasing their uptake by the target market?

## Requirements & Objectives
* Proof-Of-Concept (POC) demonstrating the technology-enabled solution that allows customers to access and perform the skin test at their convenience

* Solution must be scalable across SK-II’s global markets

* Ideally, solution to be hosted on a mobile device to increase

## POSSIBLE USE CASES
* The solution allows customers to perform the skin test independently at a convenient time and location to them i.e. from the comfort of their own home.

* The solution enables SK-II outlets as well as marketing and beauty counsellor teams to easily carry out the test on customers and gain data points in a non-store setting.

# Our Philosophy and Value
After our internal discussion, AIQ believes the best way to approach this problem is to use hybrid computer vision techniques, that is, the traditional local feature extracted based pattern recognition and modern convolutional neural network(CNN) based object detection and segmentation.

## Local feature based methods

This kind of method is particularly valuable in detection the common and regular shapes like circles, lines, rectangles(squares) and triangles.

![Imgur](https://i.imgur.com/PdguDul.jpg)

On the other hand, since we are working on face images, the traditional computer vision methods are also very useful in detecting the facial landmarks. In this case, the spotted pattern is easily associated with certain areas which is later used for skin test analysis.

![Imgur](https://i.imgur.com/EvWByAu.jpg)

So, in summary, traditional local feature based methods are universal and scalable across SK-II’s global markets. And luckily, we can borrow the experience from current magic ring tests and do not start from scratch but some fine tunes in mobile solution.

## Convolutional neural network(CNN) based methods

Although the above solution is excellent in terms of universal access and scalability, they do have some drawbacks. The main problems are they cannot detect irregular objects which are very common in skin images as below. So in order to tackle this, AIQ introduces the CNN based methods object detection and segmentation.

![Imgur](https://i.imgur.com/WKPHrKH.png)


In gist, the main Steps are as below:
1. Identify the business problems
2. Collect data
3. Explore the data
4. Prepare the data
5. Train&Test&Evaluate different models
6. Hyperparameter tuning
7. Present our solution
8. Launch, monitor, and maintain system

## Identify the business problems
1. Define the objective in business terms
  * [Further discussion with P&G needed on convertion rate etc.]
2. How will your solution be used?
  * The solution can be used by customers to complete skin test.
3. What are the current solutions/workarounds (if any)?
  * In store test
4. How should you frame this problem (supervised/unsupervised, online/offline, etc.)?
  * This is framed as supervised offline problem. Initially, face images are fed into the model and subsequently the software outputs automatically the result of the test. The output image shows detected/segmented objects to provide skin test scores.
  * The best solution is supposed to ideally detect all interested patterns in face images.
5. How should performance be measured?
  * Response time
  * Detection accuracy and Intersection over Union(IOU) ratio

    ![Imgur](https://i.imgur.com/j0ixMRv.png)
  * Classification accuracy

    ![Imgur](https://i.imgur.com/laJ9cwx.png)
  * Segmentation accuracy

    ![Imgur](https://i.imgur.com/JndGVQd.png)
  * Saliency map

    ![Imgur](https://i.imgur.com/qL1kcx4.png)
    ![Imgur](https://i.imgur.com/g2sre7l.png)
6. Is the performance measure aligned with the business objective?
  * [Further discussion with P&G needed]
7. What would be the minimum performance needed to reach the business objective?
  * [Further discussion with P&G needed]
8. What are comparable problems? Can you reuse experience or tools?
  * Yes, we did many similar projects in other domains such as skin disease classification/segmentation and object detection. Particularly, we delivered a Eczema detection app earlier this year to market as below:
  ![Imgur](https://i.imgur.com/sVPQa0u.png)
  ![Imgur](https://i.imgur.com/CC9k6hg.jpg)

9. Is human expertise available?
  * Not confirmed yet.
10. How would you solve the problem manually?
  * N.A.
11. List the assumptions you (or others) have made so far
  * Sufficient data volume for positive and negative responses
  * High quality dataset in terms of dimension and resolution, no blurring etc
  * Image dataset variety
  * Images of positive and negative cases are balanced
  * Images of each predefined pattern separately
12. Verify assumptions if possible
  * Data is as much as possible
  * Dataset is as comprehensive as possible
  * Data is in good quality
  * Data augmentation is easy to achieve
  * No ambiguous images
  * The number of images of positive and negative cases are similar

## Collect data
1. State the data characteristics you need
  * [Further discussion with P&G needed]
2. Agree on legal obligations
  * [Further discussion with P&G needed]
3. Create a workplace
  * Enough storage capacity: Cloud solution
  * Edge computing on mobile
4. Get data
  * [Further discussion with P&G needed]
5. Prepossess the data
  * Refine it on illumination, rotation and etc.
6. Ensure sensitive information is deleted or protected (e.g., anonymized)
  * [Further discussion with P&G needed]
7. Sample a test set, put it aside, and never look at it (no data snooping!)
  * [Further discussion with P&G needed]

## Explore the data
Note: try to get insights from a expert for these steps.
1. Create a copy of the data for exploration (sampling it down to a manageable size if necessary)
2. Create a Jupyter notebook to keep a record of data exploration
3. Study each attribute and its characteristics:
  * Name
  * Type (categorical, int/float, bounded/unbounded, text, structured, etc.)
  * % of missing values
  * Noisiness and type of noise (stochastic, outliers, rounding errors, etc.)
  * Possibly useful for the task?
  * Type of distribution (Gaussian, uniform, logarithmic, etc.)
4. For supervised learning tasks, identify the target attribute(s)
5. Visualize the data
6. Study the correlations between attributes
7. Study how we would solve the problem manually
8. Identify the promising transformations we may want to apply
9. Identify extra data that would be useful
10. Document what we have learned

## Prepare the data
Work on copies of the data (keep the original dataset intact)
Write functions for all data transformations we apply, for five reasons
  * So we can easily prepare the data the next time we get a fresh dataset
  * So we can apply these transformations in future projects
  * To clean and prepare the test set
  * To clean and prepare new data instances once our solution is live
  * To make it easy to treat our preparation choices as hyperparameters

1. Data cleaning
  * Missing values and outliers treatment, if necessary
2. Feature selection (optional)
  * Drop the attributes that provide no useful information for the task
3. Feature engineering, where appropriate
  * Discretize continuous features
  * Decompose features (e.g., categorical, date/time, etc.)
  * Variable transformation and creation, if necessary
  * Aggregate features into promising new features
4. Feature scaling: standardize or normalize features.

## Shortlist promising models

  * If the data is huge, we may want to sample smaller training sets so you can train many different models in a reasonable time (be aware that this penalizes complex models such as large neural nets or Random Forests)
  
  * Once again, we will try to automate these steps as much as possible

  1. Train many quick and dirty models from different categories (e.g., linear, naive Bayes, SVM, Random Forests, neural net, etc.) using standard parameters
  2. Measure and compare their performance
  
  * For each model, use N-fold cross-validation and compute the mean and stan‐ dard deviation of the performance measure on the N folds
    
  3. Analyze the most significant variables for each algorithm
  4. Analyze the types of errors the models make
  
  * What data would a human have used to avoid these errors?
    
  5. Have a quick round of feature selection and engineering
  6. Have one or two more quick iterations of the five previous steps
  7. Short-list the top three to five most promising models, preferring models that make different types of errors

## Hyperparameter tuning

  * We will want to use as much data as possible for this step, especially as we move toward the end of fine-tuning
  
  * As always automate what we can

  1. Fine-tune the hyperparameters using cross-validation
  
  * Treat our data transformation choices as hyperparameters, especially when we are not sure about them (e.g., should I replace missing values with zero or with the median value? Or just drop the rows?)
    
  * Unless there are very few hyperparameter values to explore, prefer random search over grid search. If training is very long, we may prefer a Bayesian optimization approach (e.g., using Gaussian process priors, as described by Jasper Snoek, Hugo Larochelle, and Ryan Adams)
    
  2. Try Ensemble methods. Combining our best models will often perform better than running them individually
  3. Once we are confident about our final model, measure its performance on the test set to estimate the generalization error

##  Present our solution
1. Document what we have done
2. Create a nice presentation
  * Make sure we highlight the big picture first
3. Retrain our models on a regular basis on fresh data (automate as much as possible)y our solution achieves the business objective
4. Don’t forget to present interesting points we noticed along the way
  * Describe what worked and what did not
  * List our assumptions and our system’s limitations
5. Ensure our key findings are communicated through beautiful visualizations or easy-to-remember statements (e.g., “the median income is the number-one predictor of housing prices”)

## LAUNCH!!!
1. Get our solution ready for production (plug into production data inputs, write unit tests, etc.)
2. Write monitoring code to check our system’s live performance at regular intervals and trigger alerts when it drops

  * Beware of slow degradation too: models tend to “rot” as data evolves
  
  * Measuring performance may require a human pipeline (e.g., via a crowdsourc‐ing service)
  
  * Also monitor our inputs’ quality (e.g., a malfunctioning sensor sending random values, or another team’s output becoming stale). This is particularly important for online learning systems
  
3. Retrain our models on a regular basis on fresh data (automate as much as possible)

## Easy access to SK-II outlets as well as marketing and beauty counsellor teams
In our previous project, we built a chatbot for easy access as below, so AIQ will customize similar solution for SK-II. In this context, we believe the customer conversion and retaining rate will be hugely increased.
![Imgur](https://i.imgur.com/hALEJ44.jpg)
![Imgur](https://i.imgur.com/8UFD8uP.jpg)
![Imgur](https://i.imgur.com/m4MwdjD.jpg)
![Imgur](https://i.imgur.com/FqQjspp.jpg)
![Imgur](https://i.imgur.com/hwDkA0j.jpg)
