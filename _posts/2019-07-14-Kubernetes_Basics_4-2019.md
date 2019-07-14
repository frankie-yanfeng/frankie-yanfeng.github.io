---
layout:     post   				    # 使用的布局（不需要改）
title:      Kubernetes Concept 4				# 标题
subtitle:   k8s sharing #副标题
date:       2019-07-14				# 时间
author:     frankie 						# 作者
header-img: img/4.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - k8s
---

# Jobs and CronJobs

  * Unlike other Kubernetes controllers, jobs manages a task up to its completion rather than to an open-ended desired state. In a way, the desired state of a job is its completion.

## Video transcode task:

  ![Imgur](https://i.imgur.com/KJCwr1d.png)

  ![Imgur](https://i.imgur.com/WVGeboW.png)

  ![Imgur](https://i.imgur.com/POhdeLA.png)

  ![Imgur](https://i.imgur.com/ugDNUgK.png)

  ![Imgur](https://i.imgur.com/hQWFzIe.png)

## A non-parallel Job computing π to 2000 places
  ```
  apiVersion: batch/v1
  kind: Job
  metadata:
    name: pi
  spec:
    template:
      spec:
        container:
        - name: pi
        image: perl
        command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
    backoffLimit: 4
  ```

  ```
  kubectl apply -f [JOB_FILE]
  ```

  ```
  kubectl run pi --image perl --restart Never -- perl -Mbigum bpi -wle 'print bpi(2000)'
  ```

## The role of a non-parallel Jobs

  ![Imgur](https://i.imgur.com/qAigqFj.png)

  ![Imgur](https://i.imgur.com/e2qNJLG.png)

  ![Imgur](https://i.imgur.com/STVtfwS.png)

## Parallel Jobs
The parallel job type creates multiple pods that work on the same task at the same time. Parallel job types are specified by setting the spec.parallelism value for a job greater than one.

* Fixed task completion count
* Processing a work queue

### Parallel Job with fixed completion count

  ```
  apiVersion: batch/v1
  kind: Jobs
  metadata:
    name: my-app-job
  spec:
    completions: 3
    parallelism: 2
    template:
      spec:
  [...]
  ```

  ![Imgur](https://i.imgur.com/SPlVgOF.png)

  ![Imgur](https://i.imgur.com/zELixk7.png)

  ![Imgur](https://i.imgur.com/V2yTGPs.png)

  ![Imgur](https://i.imgur.com/vac9KeF.png)

### Parallel Job with a worker queue

  ```
  apiVersion: batch/v1
  kind: Jobs
  metadata:
    name: my-app-job
  spec:
    parallelism: 3
    template:
      spec:
  [...]
  ```

  ![Imgur](https://i.imgur.com/CBx66PH.png)

  ![Imgur](https://i.imgur.com/1NrUiDf.png)

  ![Imgur](https://i.imgur.com/osUxKC2.png)

## Inspecting a Job

  ```
  kubectl describe job [JOB_NAME]
  ```

  ```
  kubectl get pod -l [job-name=my-app-job]
  ```

## Scaling a Job

  ```
  kubectl scale job [JOB_NAME] --replicas [VALUE]
  ```

  ![Imgur](https://i.imgur.com/gu4IVWM.png)

## Deleting a Job

  ```
  kubectl delete -f [JOB_FILE]
  ```

  ```
  kubectl delete job [JOB_NAME]
  ```

  ```
  kubectl delete job [JOB_NAME] --cascade false
  ```


# CronJob

## Setting up a cron schedule under the CronJob spec

  ```
  apvVersion: batch/v1
  kind: CronJob
  metadata:
    name: my-app-job
  spec:
    schedule: "*/1 * * * *"
    startingDeadlineSeconds: 3600
    concurrencyPolicy: Forbid
    suspend: True
    successfulJobsHistoryLimit: 3
    failedJobsHistoryLimit: 1
    jobtemplate:
      spec:
        template:
          spec:
  [...]
  ```

## CronJobs can be managed using kubectl

```
Create a CronJob

kubectl apply -f [FILE]
```

```
Inspect a CronJob

kubectl describe cronjob [NAME]
```

```
Delete a CronJob

kubectl delete cronjob [NAME]
```
