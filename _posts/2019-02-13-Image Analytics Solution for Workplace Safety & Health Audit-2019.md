---
layout:     post   				    # 使用的布局（不需要改）
title:      Image Analytics Solution for Workplace Safety & Health 				# 标题
subtitle:   OIP Challenge #副标题
date:       2019-02-13 				# 时间
author:     frankie 						# 作者
header-img: img/post-bg-map.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - tech
    - recognition
    - project
    - CNN
---


# Image Analytics Solution for Workplace Safety & Health

## Context understanding:
1. The process of auditing involving Inspection of workplace is critical to assess how well the company's WSH management systems are implemented.

2. Auditors use a sampling approach, walking through a cross-section of a site, to visually identify potential
hazards and risks by taking photographs of positive safety & health findings as well as identified breaches of the regulation using a digital camera, and use these to build a report to the client. The report contains the photos, with each photo accompanied by the relevant regulation that has been breached.

## Pain points:
1. Intensive manual work
2. Time consuming -> expensive
3. Error prone

## Objective:
1. Increase the speed of production of the report -> be presented at the end of an inspection visit to the client, during the closing meeting.
2. Automation detecting positive and negative findings from a photo, and link it to the correct regulations with comments.

## Common WSH findings:
* Risk of falling from a height (for example, inadequate scaffolding)
* Risk from falling objects (for example, not wearing a helmet)
* Risk from electricity (for example, exposed wiring)
* Risk from machinery (for example, moving parts)

  ![Imgur](https://i.imgur.com/OLFeu2p.png)
  ![Imgur](https://i.imgur.com/qsnuWKq.png)
  ![Imgur](https://i.imgur.com/5VHE4sZ.png)

## PROBLEM STATEMENT
How might we use image recognition to increase the speed of reporting during workplace safety and health site inspections, while maintaining accuracy?

## Main steps
1. Frame the problem and look at the big picture.
2. Get the data.
3. Explore the data to gain insights.
4. Prepare the data to better expose the underlying data patterns to Computer Vision(Machine Learning) algorithms.
5. Explore many different models and short-list the best ones.
6. Fine-tune models and combine them into a great solution.
7. Present solution.
8. Launch, monitor, and maintain system.

## Frame the Problem and Look at the Big Picture
1. Define the objective in business terms.
  * Speed up the report generation, ideally within 15 minutes.
  * Automatically detect positive and negative findings.
  * Link the findings to correct regulation with comments.
  * Indicate the regulatory breach occurring.
  * Identify the regulation that is being breached from the photos (highlight).
  * Report generation.
  * Manual report amendment/edit.

2. How will your solution be used?
  * This solution will be used by auditors in workplace safety and health(WSH) auditing.

3. What are the current solutions/workarounds (if any)?
  * Current manual audit process
  ![Imgur](https://i.imgur.com/EuZou03.png)

4. How should you frame this problem (supervised/unsupervised, online/offline,
etc.)?
  * This is framed as supervised offline problem. Initially, the auditors take photos, and then the photos are automatically classified according to regulation categories, followed by objects detection. If necessary, the images are further segmented for scene understanding and further analysis.
  * The key solution is to properly define the scenarios and problems. For example, what are the typical problems and definitions of housekeeping risk? Are they framed by messy tubes on ground or twisted wires in outdoor. All these are determined with Concord Associates' expertise and help for thoroughly domain knowledge understanding.

5. How should performance be measured?
  * Response time: ~15 minutes
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
  * [Further discussion with Concord Associates needed]

7. What would be the minimum performance needed to reach the business objective?
  * [Further discussion with Concord Associates needed]

8. What are comparable problems? Can you reuse experience or tools?
  * Yes, we did many similar projects in other domains such as skin disease classification/segmentation and object detection.
  ![Imgur](https://i.imgur.com/sVPQa0u.png)
  ![Imgur](https://i.imgur.com/STf9H67.jpg)
  and even image captioning as long as the image dataset and corresponding descriptions are available.
  ![Imgur](https://i.imgur.com/DfcYG7n.jpg)

9. Is human expertise available?
  * Yes, the Concord Associates provides the human expertise if necessary.

10. How would you solve the problem manually?
  * At last step in generating the report, the auditor may choose manual report drafting or select interested images before report generating.

11. List the assumptions you (or others) have made so far.
  * Sufficient data volume - 600~1000 for each scenario (hazard)
  * High quality dataset in terms of dimension and resolution, no blurring etc
  * Image dataset variety
  * Images of positive and negative cases are balanced
  * Images of separated common articles, tools, vehicles as foreground
  * Images of separated common construction site as background
  * Images with context and without ambiguity, for example, below picture is very confusing if no background information is given. In such case, if human cannot differentiate it, then computer vision models definitely cannot do it as well.
  ![Imgur](https://i.imgur.com/9fsEgcL.png)

12. Verify assumptions if possible.
  * Data is as much as possible
  * Data is as different as possible
  * Data is in good quality
  * Data augmentation is easy to achieve
  * No ambiguous images
  * The number of images of positive and negative cases are similar.

## Get the Data
1. List the data you need and how much you need.
  * For each defined hazard, 600 to 1000 images are needed.

2. Find and document where you can get that data.
  * [Depending on Concord Associates, but AIQ may combine the public available dataset]

3. Check how much space it will take.
  * 100G to 200G, and dynamic change according to specific tasks.

4. Check legal obligations, and get authorization if necessary.
  * [Further discussion with Concord Associates needed]

5. Get access authorizations
  * [Further discussion with Concord Associates needed]

6. Create a workspace (with enough storage space).
  * Cloud Solution - compute engine & cloud storage
  * Edge computing on real-time video or other devices like Nvidia Jetson TX series (optional)

7. Get the data.
  * [Further discussion with Concord Associates needed]

8. Convert the data to a format you can easily manipulate (without changing the
data itself).
  * Image prepossessing on light, rotation and other conditions.

9. Ensure sensitive information is deleted or protected (e.g., anonymized).
  * [Further discussion with Concord Associates needed]

10. Check the size and type of data (time series, sample, geographical, etc.).
  * [Further discussion with Concord Associates needed]

11. Sample a test set, put it aside, and never look at it (no data snooping!).
  * [Further discussion with Concord Associates needed]

## Explore the Data
Note: try to get insights from a field expert for these steps.
1. Create a copy of the data for exploration (sampling it down to a manageable size
if necessary).
2. Create a Jupyter notebook to keep a record of data exploration.
3. Study each attribute and its characteristics:
  * Name
  * Type (categorical, int/float, bounded/unbounded, text, structured, etc.)
  * % of missing values
  * Noisiness and type of noise (stochastic, outliers, rounding errors, etc.)
  * Possibly useful for the task?
  * Type of distribution (Gaussian, uniform, logarithmic, etc.)
4. For supervised learning tasks, identify the target attribute(s).
5. Visualize the data.
6. Study the correlations between attributes.
7. Study how we would solve the problem manually.
8. Identify the promising transformations we may want to apply.
9. Identify extra data that would be useful.
10. Document what we have learned.  

## Prepare the Data

• Work on copies of the data (keep the original dataset intact).

• Write functions for all data transformations we apply, for five reasons:
  * So we can easily prepare the data the next time we get a fresh dataset
  * So we can apply these transformations in future projects
  * To clean and prepare the test set
  * To clean and prepare new data instances once our solution is live
  * To make it easy to treat our preparation choices as hyperparameters

1. Data cleaning:
  * Fix or remove outliers (optional).
  * Fill in missing values (e.g., with zero, mean, median…) or drop their rows (or
  columns).
2. Feature selection (optional):
  * Drop the attributes that provide no useful information for the task.
3. Feature engineering, where appropriate:
  * Discretize continuous features.
  * Decompose features (e.g., categorical, date/time, etc.).
  * Add promising transformations of features (e.g., log(x), sqrt(x), x^2, etc.).
  * Aggregate features into promising new features.
4. Feature scaling: standardize or normalize features.

## Short-List Promising Models
Notes:
  * If the data is huge, we may want to sample smaller training sets so you can train
many different models in a reasonable time (be aware that this penalizes complex
models such as large neural nets or Random Forests).
  * Once again, we will try to automate these steps as much as possible.

1. Train many quick and dirty models from different categories (e.g., linear, naive
Bayes, SVM, Random Forests, neural net, etc.) using standard parameters.
2. Measure and compare their performance.
  * For each model, use N-fold cross-validation and compute the mean and stan‐
dard deviation of the performance measure on the N folds.
3. Analyze the most significant variables for each algorithm.
4. Analyze the types of errors the models make.
  * What data would a human have used to avoid these errors?
5. Have a quick round of feature selection and engineering.
6. Have one or two more quick iterations of the five previous steps.
7. Short-list the top three to five most promising models, preferring models that
make different types of errors.

## Fine-Tune the System
Notes:
  * We will want to use as much data as possible for this step, especially as we move
toward the end of fine-tuning.
  * As always automate what we can.

1. Fine-tune the hyperparameters using cross-validation.
  * Treat our data transformation choices as hyperparameters, especially when
  we are not sure about them (e.g., should I replace missing values with zero or
  with the median value? Or just drop the rows?).
  * Unless there are very few hyperparameter values to explore, prefer random
  search over grid search. If training is very long, we may prefer a Bayesian
  optimization approach (e.g., using Gaussian process priors, as described by
  Jasper Snoek, Hugo Larochelle, and Ryan Adams).
2. Try Ensemble methods. Combining our best models will often perform better
  than running them individually.
3. Once we are confident about our final model, measure its performance on the
  test set to estimate the generalization error.  

## Present Our Solution
1. Document what we have done.
2. Create a nice presentation.
  * Make sure we highlight the big picture first.
3. Explain why our solution achieves the business objective.
4. Don’t forget to present interesting points we noticed along the way.
  * Describe what worked and what did not.
  * List our assumptions and our system’s limitations.
5. Ensure our key findings are communicated through beautiful visualizations or
easy-to-remember statements (e.g., “the median income is the number-one predictor of housing prices”).

## Launch!
1. Get our solution ready for production (plug into production data inputs, write
unit tests, etc.).
2. Write monitoring code to check our system’s live performance at regular intervals and trigger alerts when it drops.
  * Beware of slow degradation too: models tend to “rot” as data evolves.
  * Measuring performance may require a human pipeline (e.g., via a crowdsourc‐ing service).
  * Also monitor our inputs’ quality (e.g., a malfunctioning sensor sending random values, or another team’s output becoming stale). This is particularly
important for online learning systems.
3. Retrain our models on a regular basis on fresh data (automate as much as possible).
