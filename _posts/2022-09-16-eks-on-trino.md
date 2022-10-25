---
layout: post
title: "EKS에 Trino 구축하기"
tags: EKS Trino Kubernetes
---
해당 포스트는 **EKS에 Trino를 구축하는 과정**을 담았습니다🤗. 올해 4월쯤 진행한 내용을 기준으로 작성하여 최신 내용과 다른 부분이 있을 수 있어 이 점 유의해서 읽어주세요!
## 목차
1. [들어가며](#들어가며)
2. [Helm을 사용하여](#helm을-사용하여)
3. [Trino 배포하기](#trino-배포하기)
<br/><br/>
   
## 들어가며
본 내용을 시작하기에 앞서 **EKS와 Trino에 대해서 간략히 설명하자면** 다음과 같습니다.
<br/><br/>

<img src = "/post_images/eks-on-trino/amazon_eks.png" width=auto height="200">

**EKS란, Elastic Kubernetes Service**의 약자로, **AWS에서 제공하는 Kubernetes** 서비스입니다. 
**EKS**는 자체 컨트롤 플레인 또는 노드를 설치 및 운영할 필요 없이 사용할 수 있습니다. 
또한 여러 **AWS 서비스와 통합되어 애플리케이션에 대한 확장성과 보안을 효과적으로 제공하는 장점**이 있습니다.
EKS에 대한 자세한 내용은 [공식문서](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/what-is-eks.html)를 참고하세요!🤗
<br/><br/>

<img src = "/post_images/eks-on-trino/trino.png" width=auto height="200">

**Trino**는 효율적이고 짧은 지연 시간으로 운영하기 위해 **병렬화된 분산 쿼리 엔진입니다.** 
또한 Tableau, Power BI, Superset 등과 같은 BI 도구와 함께 작동하는 ANSI SQL 호환 쿼리 엔진입니다.
**Trino**는 HDFS/Hive 기반으로 어려운 분석 쿼리를 가능하게 할 정도로 뛰어난 성능을 가지고 있습니다.
개인적으로 가장 큰 장점이라고 생각한 부분은, Trino라는 동일한 환경에서 **다양한 데이터 소스 쿼리를 처리할 수 있다는 점**입니다.
Trino가 지원하는 Connector는 스토리지, 관계형데이터베이스, NoSQL 상관없이 **Trino 내부적으로 처리할 수 있어 중앙 집중식 액세스 및 분석이 가능합니다.**
Trino에 대한 자세한 내용은 [공식문서](https://trino.io)를 참고하세요!🤗
<br/><br/>

## Helm을 사용하여
EKS에 Trino를 배포하는 도구로 **Helm**을 사용했는데, **Helm**을 설명하면 다음과 같습니다.
<br/><br/>

<img src = "/post_images/eks-on-trino/helm.png" width=auto height="200">

**Helm은 Kubernetes 애플리케이션 관리를 지원합니다.**
Helm 차트는 복잡한 애플리케이션도 유지 및 반복할 수 있는 배포기능을 제공합니다.
또한 이전 버전으로 쉽게 롤백하거나, 업그레이드하는 데 유용합니다.
**차트는 쉽게 생성할 수 있고, 버전 관리가 용이하며 공용 또는 개인 서버에서 간단히 공유 및 호스팅할 수 있습니다.**
Helm에 대한 자세한 내용은 [공식문서](https://helm.sh)에 상세히 적혀있습니다!🤗
<br/><br/>

EKS는 AWS서비스를 통해 클러스터 환경을 관리하는 것 외에 **Kubernetes와 거의 동일합니다.** 
따라서 **Kubernetes에 적용하는 도구들은 EKS에도 동일하게 적용할 수 있습니다.**
Helm도 마찬가지로 EKS에 동일하게 적용할 수 있으며, 애플리케이션을 배포할 때 유용하게 사용할 수 있습니다.
<br/><br/>

## Trino 배포하기
> 📢 배포 과정 설명에서 EKS 구축 과정 및 nginx-ingress-controller 세팅 과정 등 **세부적으로 필요한 환경 구축 과정은 생략했습니다.**

대략적인 설명은 마무리하고 Trino 배포 과정을 설명하자면 아래와 같습니다.
<br/><br/>

### 차트 저장소를 활용하여 배포하기
Helm Chart Repository(이하 차트 저장소)는 패키지형 차트를 저장하고 공유할 수 있는 장소입니다.
Trino 또한 공식 차트 저장소를 가지고 있고 당시 사용한 차트의 정보는 표와 같습니다.

|NAME|CHART VERSION|APP VERSION|CHART URL|
|---|---|---|---|
|trino/trino|0.5.0|372|https://trinodb.github.io/charts/|

## 애로사항 및 코드 리펙토링
차트 템플릿 구조가 어떻고, access-rule과 같이 특정 내용 적용할 수 없던 부분

## 최종배포

## 마무리