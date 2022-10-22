---
layout: post
title: "EKS에 Trino 구축하기"
tags: EKS Trino Kubernetes
---
해당 포스트는 **EKS에 Trino를 구축하는 과정**을 담았습니다🤗. 올해 4월쯤 진행한 내용을 기준으로 작성하여 최신 내용과 다른 부분이 있을 수 있어 이 점 유의해서 읽어주세요!
## 목차
1. [들어가며](#들어가며)

## 들어가며
본 내용을 시작하기에 앞서 **EKS와 Trino에 대해서 간략히 설명하자면** 다음과 같습니다.
<br/><br/>
<img src = "/post_images/eks-on-trino-part1/amazon_eks.png" width="400" height=auto>
**EKS란, Elastic Kubernetes Service**의 약자로, **AWS에서 제공하는 Kubernetes** 서비스입니다. 
**EKS**는 자체 컨트롤 플레인 또는 노드를 설치 및 운영할 필요 없이 사용할 수 있습니다.
<br/><br/>
<img src = "/post_images/eks-on-trino-part1/trino.png" width="400" height=auto>
**Trino**는 효율적이고 짧은 지연 시간으로 운영하기 위해 병렬화된 분산 쿼리 엔진입니다. 
또한 Tableau, Power BI, Superset 등과 같은 BI 도구와 함께 작동하는 ANSI SQL 호환 쿼리 엔진입니다.