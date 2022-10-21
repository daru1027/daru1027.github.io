---
layout: post
title: "EKS에 Trino 구축하기 #1"
tags: EKS Trino Kubernetes
---
해당 포스트는 분산 쿼리 엔진인 **Trino**를 클러스터 환경인 **AWS EKS(Elastic Kubernetes Service)**에 구축하는 내용을 담았습니다. 
내용에 들어가기 전 배경을 설명하는 포스트로 디테일 한 내용은 다음 포스트부터 다룹니다😀.
<br/><br/>
## 시작하기에 앞서
### EKS란?
<img src = "/post_images/eks-on-trino-part1/amazon_eks.png" width="400" height=auto>
**EKS**는 Elastic Kubernetes Service의 약자로, AWS에서 제공하는 Kubernetes 서비스입니다. 
EKS는 자체 컨트롤 플레인 또는 노드를 설치 및 운영할 필요 없이 사용할 수 있습니다.
<br/><br/>

### Kubernetes란?
<img src = "/post_images/eks-on-trino-part1/kubernetes.png" width="400" height=auto>
**Kubernetes(이하 k8s)**는 컨테이너화된 서비스 등을 관리하기 위한 오픈소스 플랫폼입니다. 
k8s는 이식성이 있고, 확장할 수 있으며 선언적 구성과 자동화 모두 용이합니다. 
<br/><br/>

### Trino란?
<img src = "/post_images/eks-on-trino-part1/trino.png" width="400" height=auto>
**Trino**는 효율적이고 짧은 지연 시간으로 운영하기 위해 병렬화된 분산 쿼리 엔진입니다. 
또한 Tableau, Power BI, Superset 등과 같은 BI 도구와 함께 작동하는 ANSI SQL 호환 쿼리 엔진입니다.
<br/><br/>

## 구축 배경
**EKS**에 **Trino**를 구축하게 된 이유로 크게 2가지가 있습니다.
<br/><br/>
기존에는 **AWS EMR** 클러스터 환경에 **Trino**를 구축하여 운영했습니다.
클러스터 환경을 쓰는 만큼 **오토스케일링(Autoscaling)**은 상상만으로도 즐거운 기술입니다. 
하지만 **Trino**의 리소스 사용량은 일반적인 방법으론 **CloudWatch**로 트래킹하기 어려웠고(나만 어려웠던 걸지도..?😵‍), **CloudWatch** 기반으로 오토스케일링을 하는 **EMR**은 변동적인 쿼리 사용량에 맞춰 오토스케일링하기 어렵습니다.
<br/><br/>
또한 **EMR**은 서비스 업데이트를 진행할 때 배포하는 데 시간이 소요되어 업무 환경에 영향을 끼치게 됩니다. 
급히 Catalog 변경사항을 반영할 일이 있더라도 **EMR** 리소스를 회수하고 다시 배포하기까지 쿼리 엔진을 제공할 수 없게 됩니다.
<br/><br/>
그 밖에도 **Trino**를 **EKS**에 구축해 사용할 이유는 많지만, 자세한 내용은 다음에 다뤄보겠습니다😀.
<br/><br/>