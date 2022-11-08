---
layout: post
title: "EKS에 Trino 구축하기 w/ Helm"
tags: EKS Trino Kubernetes
---
해당 포스트는 **EKS에 Trino를 구축하는 과정**을 담았습니다🤗. 
올해 4월쯤 진행한 내용을 기준으로 작성하여 최신 내용과 다른 부분이 있을 수 있어 이 점 유의해서 읽어주세요!
<br/><br/>

## 목차
1. [들어가며](#들어가며)
2. [Kubernetes 패키지 매니저 Helm](#kubernetes-패키지-매니저-helm)
3. [Trino 배포하기](#trino-배포하기)
4. [마무리하며](#마무리하며)
5. [참고자료](#참고자료)
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

## Kubernetes 패키지 매니저 Helm
Helm이란 **Kubernetes 패키지 매니징 툴** 입니다.
EKS에 Trino를 배포하는 도구로 **Helm**을 사용했는데, **Helm**에 대해 자세히 살펴보면 다음과 같습니다.
<br/><br/>

<img src = "/post_images/eks-on-trino/helm.png" width=auto height="200">

**Helm은 Kubernetes 애플리케이션 관리를 지원합니다.**
Helm 차트는 복잡한 애플리케이션도 유지 및 반복할 수 있는 배포기능을 제공합니다.
또한 이전 버전으로 쉽게 롤백하거나, 업그레이드하는 데 유용합니다.
**차트는 쉽게 생성할 수 있고, 버전 관리가 용이하며 공용 또는 개인 서버에서 간단히 공유 및 호스팅할 수 있습니다.**
<br/><br/>

**Helm Chart Repository**(이하 차트 저장소)는 패키지형 차트를 저장하고 공유할 수 있는 장소입니다.
**Helm은 차트라는 패키징 형식**을 사용합니다. 따라서 차트에는 디렉리 내부의 파일 모음으로 구성되어있습니다. 
차트 저장소는 이러한 구성(Template)을 갖춰두고 배포되어있으며 이를 가져와 사용할 수 있다고 이해하면 쉽습니다. 
구성에 대한 예시는 아래와 같습니다.
```text
├── Chart.yaml
├── ...
├── templates
│   ├── NOTES.txt
│   ├── _helpers.tpl
│   └── *.yaml
└── values.yaml
```
구성 파일 중 중요한 2개를 짚고 넘어가면, `values.yaml` 파일과 `templates` 폴더입니다.
<br/><br/>

### values.yaml
공식 문서에 따르면 `values.yaml`파일은 **차트의 기본 구성 값**이라고 정의합니다.
`values.yaml`파일에는 `templates` 폴더에 구성된 파일들에 사용될 변수들이 모여있습니다.
<br/><br/>

### templates
Template를 구성하는 파일들이 정의되어있는 폴더입니다.
공식 문서 내용을 직역하면 value와 결합할 때 유효한 **Kubernetes 매니페스트 파일을 생성하는 템플릿 디렉터리**라고 정의하고 있습니다.
즉 `values.yaml`파일에서 정의한 변수 값을 통해 Kubernetes 매니페스트 파일을 생성합니다.
<br/><br/>

EKS는 AWS서비스를 통해 클러스터 환경을 관리하는 것 외에 **Kubernetes와 거의 동일합니다.** 
따라서 **Kubernetes에 적용하는 도구들은 EKS에도 동일하게 적용할 수 있습니다.**
마찬가지로 EKS에도 동일하게 Helm을 사용할 수 있으며, 애플리케이션을 배포할 때 유용하게 사용할 수 있습니다.
Helm에 대한 자세한 내용은 [공식문서](https://helm.sh)에 상세히 적혀있습니다!🤗
<br/><br/>

## Trino 배포하기
> 📢 배포 과정 설명에서 EKS 구축 과정 및 Helm 설치 과정 등 **세부적으로 필요한 환경 세팅에 관한 내용은 생략했습니다.**

대략적인 설명은 마무리하고, Trino 배포 과정을 설명하면 아래와 같습니다.
<br/><br/>

### 차트 저장소를 활용하여 배포하기
Trino 또한 공식 차트 저장소를 가지고 있고 당시 사용한 차트의 정보는 표와 같습니다.

|NAME|CHART VERSION|APP VERSION|CHART URL|
|:---:|:---:|:---:|:---:|
|trino/trino|0.5.0|372|https://trinodb.github.io/charts/|

해당 차트의 릴리즈를 관리하는 github 레포는 [여기](https://github.com/trinodb/charts)에서 확인할 수 있습니다.
<br/><br/>

#### 1. repo 추가
먼저 **Trino 차트 저장소에서 repo를 추가합니다.**
```bash
$ helm repo add trino https://trinodb.github.io/charts/
```
#### 2. repo 확인
아래 명령어를 통해 repo가 잘 추가됐는지 확인할 수 있습니다.
```bash
$ helm search repo trino/trino
```
#### 3. values.yaml 파일 내려받기
바로 `helm install` 명령어를 통해 Trino를 바로 배포할 수 있습니다. 
하지만 Trino에서 사용할 **카탈로그를 정의하여 배포하거나, worker 수를 조정하거나, Affinity rule을 적용하는 등 일부 커스터마이징이 필요합니다.** 
차트 Template은 `-f` 플래그를 통해 **외부** `values.yaml` **파일을 활용할 수 있습니다.**
<br/><br/>
먼저 `values.yaml` **파일을 내려받습니다**(git [repo](https://github.com/trinodb/charts)에서 복사해서 사용해도 괜찮습니다).
```bash
$ helm pull trino/trino
$ cd trino
$ ls
---
Chart.yaml  README.md   ci          templates   values.yaml
```
`values.yaml` 파일을 살펴보면, 아래와 예시와 같은 형식으로 값이 선언되어있습니다.
```yaml
image:
  repository: trinodb/trino
  # pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart version.
  tag: 375

server:
  workers: 2
  node:
    environment: production
    dataDir: /data/trino
    pluginDir: /usr/lib/trino/plugin
  log:
    trino:
      level: INFO
  config:
    path: /etc/trino
    http:
      port: 8080
    https:
      enabled: false
      port: 8443
      keystore:
        path: ""
~
```
#### 4. values.yaml 파일 수정
위에 언급한 것처럼 내려받은 `values.yaml` 파일을 수정하여 worker 수를 조정하는 등 **커스터마이징을 할 수 있습니다.**
Trino를 운영하면서 자주 변경하는 부분을 예시로 들면 아래와 같습니다.
* **Trino 카탈로그 추가/변경**
    ```yaml
    additionalCatalogs:
    #aws glue catalogs
      hive: |
        connector.name=hive
        hive.metastore=glue
        hive.metastore.glue.aws-access-key=AWS_ACCESS_KEY
        hive.metastore.glue.aws-secret-key=AWS_SECRET_KEY
        hive.metastore.glue.region=ap-northeast-2
        hive.metastore.glue.endpoint-url=https://glue.ap-northeast-2.amazonaws.com
    
    #mongodb catalogs
    ~
    
    ```
* **Trino Worker 조정**
    ```yaml
    server:
      workers: 2 #<--- change here!
    ```
* **Affinity rule 적용 예시**
    ```yaml
    affinity:
      podAntiAffinity:
        preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 100
            podAffinityTerm:
              labelSelector:
                matchExpressions:
                 - key: app
                   operator: In
                   values:
                     - trino
              topologyKey: "kubernetes.io/hostname"
    ```

#### 5. helm install trino
`-f` 플래그를 통해 외부 `values.yaml` 파일을 활용하여 Trino를 배포합니다.
```bash
$ helm install trino trino/trino -n [namespace name] -f values.yaml
```
<br/><br/>

### 로컬 환경에서 차트 템플릿을 통해 배포하기
필요한 기능 대부분은 위에 소개한 방법으로 커스터마이징하여 배포할 수 있습니다. 
하지만 **Trino 차트 버전** `0.5.0`**의 경우 몇몇 제약이 있었습니다.**
json-serde를 설치하기 위해 init Container를 추가하면서 emptyDir을 통해 메인 Container와 볼륨 마운팅을 할 필요가 있었는데, 이를 위에 소개한 방법으론 적용하기 어려운 부분이 있었습니다(지금 차트 버전에선 될지도...😂).
이처럼 차트 **템플릿에서 제공하는 기능 외에** 추가적인 환경 조정이 필요한 경우 아래와 같은 방법을 통해서 해결할 수 있습니다.
<br/><br/>

#### 1. helm pull 명령어를 통해 로컬에 차트 내려받기
위에 소개한 `values.yaml` 파일 수정을 위해 차트를 내려받은 것과 같은 방법으로 차트를 가져옵니다.
```bash
$ helm pull trino/trino
$ cd trino
$ ls
---
Chart.yaml  README.md   ci          templates   values.yaml
```
#### 2. templates 폴더 파일 수정
Trino의 경우 `templates` 폴더 안에 `deployment-coordinator.yaml`, `deployment-worker.yaml`과 같은 파일들이 있습니다.
각 파일들은 **Trino 서비스를 운영하기 위한 리소스들을 정의한 파일입니다.**
안에 변수를 받는 내용들이 있는데, 해당 부분이 `values.yaml`에서 기입한 내용으로 채워집니다.
`templates` 폴더 안에 있는 파일 내용을 수정하여 emptyDir를 적용하는 등 추가적인 환경을 구성할 수 있습니다.

#### 3. helm install trino
로컬에 내려받은 파일에서 templates 폴더를 빠져나와 `Chart.yaml` 파일과 같은 위계에서 아래 명령어를 통해 배포할 수 있습니다.
```bash
$ helm install trino . -n [namespace name] -f values.yaml
```
<br/><br/>

## 마무리하며
Trino에 대한 자세한 설명이나 EKS 설정 관련한 내용 등 추가적인 내용은 **다음 포스팅에서 따로 다뤄보도록 하겠습니다🤗.**
부족한 부분이 많지만 작은 지식이나마 누군가에게 도움이 되길 바랍니다!
<br/><br/>

## 참고자료
[EKS 공식문서](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/what-is-eks.html)
<br/>
[Trino 공식문서](https://trino.io)
<br/>
[Helm 공식문서](https://helm.sh)