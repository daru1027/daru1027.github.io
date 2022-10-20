---
layout: post
title: "EKS에 Trino 구축하기 #1"
tags: EKS Trino Kubernetes
---
해당 포스트는 분산 쿼리 엔진인 Trino를 AWS EKS(Elastic Kubernetes Service) 클러스터 환경에 구축하는 내용을 담았습니다.
<br/><br/>
## EKS란?
<img src = "/post_images/eks-on-trino-part1/amazon_eks.png" width="256" height=auto>
EKS는 Elastic Kubernetes Service의 약자로, 
AWS에서 제공하는 Kubernetes 서비스입니다. EKS는 자체 컨트롤 플레인 또는 노드를 설치 및 운영할 필요 없이 사용할 수 있습니다.
<br/><br/>

## Kubernetes란?
<img src = "/post_images/eks-on-trino-part1/kubernetes.png" width="256" height=auto>
Kubernetes(이하 k8s)는 컨테이너화된 서비스 등을 관리하기 위한 오픈소스 플랫폼입니다. k8s는 이식성이 있고, 확장할 수 있으며 선언적 구성과 자동화 모두 용이합니다. 
<br/><br/>

## Trino란?
<img src = "/post_images/eks-on-trino-part1/trino.png" width="256" height=auto>
Trino는 효율적이고 짧은 지연 시간으로 운영하기 위해 병렬화된 분산 쿼리 엔진입니다. 또한 Tableau, Power BI, Superset 등과 같은 BI 도구와 함께 작동하는 ANSI SQL 호환 쿼리 엔진입니다.


