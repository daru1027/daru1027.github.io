---
layout: post
title: "EKS에 Trino 구축하기"
tags: EKS Trino Kubernetes
---
해당 포스트는 **EKS에 Trino를 구축하는 과정**을 담았습니다🤗. 올해 4월쯤 진행한 내용을 기준으로 작성하여 최신 내용과 다른 부분이 있을 수 있어 이 점 유의해서 읽어주세요!
## 목차
1. [들어가며](#들어가며)
2. [애플리케이션 관리 도구 Helm](#애플리케이션-관리-도구-Helm)
<br/><br/>
   
## 들어가며
본 내용을 시작하기에 앞서 **EKS와 Trino에 대해서 간략히 설명하자면** 다음과 같습니다.
<br/><br/>
<img src = "/post_images/eks-on-trino-part1/amazon_eks.png" width="400" height=auto>
**EKS란, Elastic Kubernetes Service**의 약자로, **AWS에서 제공하는 Kubernetes** 서비스입니다. 
**EKS**는 자체 컨트롤 플레인 또는 노드를 설치 및 운영할 필요 없이 사용할 수 있습니다. 
또한 여러 **AWS 서비스와 통합되어 애플리케이션에 대한 확장성과 보안을 효과적으로 제공하는 장점**이 있습니다.
EKS에 대한 자세한 내용은 [공식문서](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/what-is-eks.html)를 참고하세요!🤗
<br/><br/>
<img src = "/post_images/eks-on-trino-part1/trino.png" width="400" height=auto>
**Trino**는 효율적이고 짧은 지연 시간으로 운영하기 위해 병렬화된 분산 쿼리 엔진입니다. 
또한 Tableau, Power BI, Superset 등과 같은 BI 도구와 함께 작동하는 ANSI SQL 호환 쿼리 엔진입니다.
**Trino**는 HDFS/Hive 기반으로 어려운 분석 쿼리를 가능하게 할 정도로 뛰어난 성능을 가지고 있습니다.
개인적으로 가장 큰 장점이라고 생각한 부분은, Trino라는 동일한 환경에서 **다양한 데이터 소스 쿼리를 처리할 수 있다는 점**입니다.
Trino가 지원하는 Connector는 스토리지, 관계형데이터베이스, NoSQL 상관없이 **Trino 내부적으로 처리할 수 있어 중앙 집중식 액세스 및 분석이 가능합니다.**
Trino에 대한 자세한 내용은 [공식문서](https://trino.io/)를 참고하세요!🤗
<br/><br/>

## 애플리케이션 관리 도구 Helm
Helm은 Kubernetes에서 실행되는 애플리케이션을 정의, 설치 및 업그레이드할 수 있는 방법을 제공합니다. 
Helm 차트에는 Kubernetes 애플리케이션의 인스턴스를 만드는 데 필요한 정보가 포함되어 있습니다. 구성은 차트 자체 외부의 values.yaml이라는 파일에 저장됩니다.

