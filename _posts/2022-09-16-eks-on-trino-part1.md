---
layout: post
title: "EKS에 Trino 구축하기 #1"
tags: EKS Trino Kubernetes
---

## EKS란?
<img src = "/post_images/eks-on-trino-part1/amazon_eks.png" width="auto" height=auto>
EKS는 Elastic Kubernetes Service의 약자로, 
AWS에서 제공하는 Kubernetes 서비스이다. EKS는 자체 컨트롤 플레인 또는 노드를 설치 및 운영 할 필요 없이 사용할 수 있다.
<br/><br/>

## Kubernetes란?
Kubernetes(이하 k8s)는 컨테이너화된 서비스등을 관리하기 위한 오픈소스 플랫폼이다. k8s는 이식성이 있고, 확장가능하며 선언적 구성과 자동화 모두 용이하다. 
<br/><br/>

## Trino란?
Trino는 효율적이고 짧은 지연 시간으로 운영하기 위해 병렬화된 분산 쿼리 엔진이다. 또한 Tableau, Power BI, Superset 등과 같은 BI 도구와 함께 작동하는 ANSI SQL 호환 쿼리 엔진이다. 그 외에 Trino의 설명은 추후에 따로 자세히 다루겠음


