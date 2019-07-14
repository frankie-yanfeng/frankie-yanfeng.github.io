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


## Deployments
  ```
  Replicaset
  ```
  ![Imgur](https://i.imgur.com/bWhOf4c.png)

  ![Imgur](https://i.imgur.com/zcysGD7.png)

  Typical statefulless application is web front end.

  ```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: my-app
  spec:
    replicas: 3
    template:
      metadata:
        labels:
          app: my-app
        spec:
          containers:
          - name: my-app
            image: gcr.io/demo/my-app:1.0
            ports:
            - containerPort: 8080
  ```

  ![Imgur](https://i.imgur.com/9vNWtGs.png)

## There are three ways to create a Deployment

  ```
  kubectl apply -f [DEPLOYMENT_FILE]
  ```

  ```
  kubectl run [DEPLOYMENT_NAME] \
  --image [IMAGE]:[TAG] \
  --replicaset 3 \
  --labels [KEY]=[VALUE] \
  --port 8080 \
  --generator deployment/apps.v1 \
  --save-config
  ```

  ```
  GKE workloads menu in the GCP console.
  ```

### Use kubectl to inspect your Deployment
  ```
  kubectl get deployment [DEPLOYMENT_NAME]
  ```

  ![Imgur](https://i.imgur.com/Ph2tF0S.png)

  ```
  kubectl get deployment [DEPLOYMENT_NAME] -o yaml > this.yaml
  ```

### Use the 'describe' command to get detailed info
  ```
  kubectl describe deployment [DEPLOYMENT_NAME]
  ```

  ![Imgur](https://i.imgur.com/Yezl3jN.png)

  Or use the GCP Console

## Scaling a Deployment manually

  ```
  kubectl scale deployment [DEPLOYMENT_NAME] -replicas=5
  ```

  ```
  kubectl autoscale deployment [DEPLOYMENT_NAME] --min=5 --max=15
  --cpu=percent=75
  ```

  ```
  --horizontal-pod-autoscaler-downscale-delay
  ```
  wait period before performing another scale down action

## Updating deployments
  ```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: my-app
  spec:
    replicas: 3
    template:
      spec:
        containers:
        - name: my-app
          images: gcr.io/demo/my-app:1.0
          ports:
          - containerPort:8080
  ```

  ```
  kubectl apply -f [DEPLOYMENT_FILE]
  ```

  ```
  kubectl set image deployment
  ```

  ```
  kubectl edit deployment/[DEPLOYMENT_NAME]
  ```

## Rolling updates
  ```
  [...]
  kind: deployment
  spec:
    replicas: 10
    strategy:
      type: RollingUpdate
      rollingUpdate:
        maxSurge: 5
        maxUnavailable: 30%
  [...]
  ```

## Blue/green deployment strategy
A blue/green deployment strategy is useful when you want to deploy a new version of an application and also ensure that application services remain available while the deployment is updated.

  ![Imgur](https://i.imgur.com/c9tyyRR.png)


```
service with old deployment

[...]
kind: service
spec:
  selector:
    app: my-app
    version: v1
[...]
```

```
new deployment applied

kubectl apply -f my-app-v2.yaml

kubectl patch service my-app-service -p '{"spec":{"selector":{"version":"v2"}}}
```

  ![Imgur](https://i.imgur.com/2x8ip7x.png)

## Canary deployment

  ![Imgur](https://i.imgur.com/v1aCqCl.png)

  ![Imgur](https://i.imgur.com/Uxggu01.png)

  ![Imgur](https://i.imgur.com/Clzm3EO.png)

  ```
  keypoint: no version number in the label

  [...]
  kind: service
  sepc:
    selector:
      app: my-app
  [...]
  ```

  ```
  kubectl apply -f my-app-v2.yaml
  ```

  ```
  kubectl scale deployment/my-app-v2 -replicas=10
  ```

  ```
  kubectl delete -f my-app-v1.yaml
  ```

## Rolling back a Deployment
  ```
  kubectl rollout undo deployment [DEPLOYMENT_NAME]
  ```

  ```
  kubectl rollout undo deployment [DEPLOYMENT_NAME] --to-revision=2
  ```

  ```
  kubectl rollout history deployment [DEPLOYMENT_NAME] --revision=2
  ```

  Clean up Policy:
  * Default: 10 revision
  * Change: .spec.revisionHistoryLimit

## Deployment has three different lifecycle states

  ![Imgur](https://i.imgur.com/Yg4MInc.png)

## Pausing a Deployments
  ```
  kubectl rollout pause deployment [DEPLOYMENT_NAME]
  ```

  ```
  kubectl rollout resume deployment [DEPLOYMENT_NAME]
  ```

  ```
  kubectl rollout status deployment [DEPLOYMENT_NAME]
  ```

## Deleting a Deployment

  ```
  kubectl delete deployment [DEPLOYMENT_NAME]
  ```
