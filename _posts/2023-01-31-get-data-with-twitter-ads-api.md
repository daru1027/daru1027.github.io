---
layout: post
title: "트위터 광고 API로 광고비 데이터 가져오기(Twitter Ads API)"
tags: API Twitter Data Ads
---
## 목차
1. [들어가며](#들어가며)
2. [Twitter Ads API 사용신청](#Twitter-Ads-API-사용신청)
3. [광고 데이터 구조 파악하기](#광고-데이터-구조-파악하기)
4. [Python 코드 작성 및 데이터 가져오기](#Python-코드-작성-및-데이터-가져오기)
5. [마무리하며](#마무리하며)
<br/><br/>
   
## 들어가며
이번 포스트에서는 트위터에서 제공하는 Twitter Ads API를 통해 광고비 데이터를 가져오는 내용에 대해 다루고자 합니다. 
보통은 서비스에서 제공하는 API를 자동화에 활용하는 등 운영 관련된 업무에 사용하곤 합니다. 
그렇기에 API를 통해 광고비 데이터를 가져온다는 것은 큰 목적이 느껴지지 않는 주제일 수 있습니다. 
하지만 다른 시선으로 바라봤을 땐 꽤 니즈가 있는 작업입니다. 
해당 작업으로 Paid 마케팅 집행 시 가장 효과적인 미디어 믹스를 짜는 데 도움이 되는 데이터를 제공할 수 있습니다. 
그럼 본격적으로 데이터를 제공하는 사람의 입장에서 Twitter Ads API를 사용하는 과정을 다뤄보도록 하겠습니다🙂.
<br/><br/>

## Twitter Ads API 사용신청
대부분의 매체가 그렇듯 API를 활용하려면 사용 신청을 해야 합니다. 
특히 광고처럼 프라이빗한 API의 경우 승인 절차가 더욱 복잡합니다. 
트위터의 경우 다른 매체에 비해 절차가 꽤 까다로운(?) 경향이 있습니다😢.

Twitter Ads API [공식문서](https://developer.twitter.com/en/docs/twitter-ads-api/getting-started)에 따르면 아래 3가지 절차를 언급합니다.
1. 개발자 계정 가입
2. 프로젝트와 앱 생성 및 앱의 키와 토큰을 보관
3. Ads API 액세스 요청
<br/><br/>
   
### 개발자 계정 가입
공식문서에 나오는 첫 번째 단계인 개발자 가입 절차는 아래 이미지와 같습니다. 
개발자 계정에 가입하는 과정은 크게 어렵지 않습니다. 
먼저 국가 선택 및 API를 사용하려는 목적을 선택하면 됩니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/a-twitter-developer-signup-1.png" width=auto height=auto>

다음 단계로 넘어가면 API와 관련한 정책 안내 및 약관 동의 절차를 진행합니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/a-twitter-developer-signup-2.png" width=auto height=auto>
<br/><br/>

### 프로젝트와 앱 생성 및 앱의 키와 토큰을 보관
가입을 완료했다면 두 번째 단계를 진행하면 됩니다. 
개발자 센터에서 프로젝트 생성을 진행합니다. 
먼저 프로젝트 이름을 지어줍니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/b-twitter-developer-project-1.png" width=auto height=auto>

그다음 API의 주 사용 목적을 선택합니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/b-twitter-developer-project-2.png" width=auto height=auto>

프로젝트에 대한 요약을 기재하는 절차입니다. 
크게 중요하진 않지만, 관련 중요한 내용을 적어두어 인수인계하는 등 프로젝트를 공유할 때 유용합니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/b-twitter-developer-project-3.png" width=auto height=auto>

바로 이어서 신규 앱을 해당 프로젝트 하위로 생성합니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/b-twitter-developer-project-4.png" width=auto height=auto>

배포할 환경을 선택합니다. 

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/b-twitter-developer-project-5.png" width=auto height=auto>

프로젝트와 마찬가지로 앱 이름을 지어줍니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/b-twitter-developer-project-6.png" width=auto height=auto>

다음으로 넘어가면 앱 생성이 완료됩니다. 
앱 생성이 완료되면서, API Key, API Key Secret, Token이 생성되며 노출됩니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/b-twitter-developer-project-7.png" width=auto height=auto>

키들과 토큰은 앱을 생성할 때 처음이자 마지막으로 보입니다. 
각 값을 잃어버렸을 때 앱 설정 내부에서 재발급을 받을 수 있지만, API를 사용할 때마다 매번 재발급을 받을 순 없기 때문에 키들과 토큰을 따로 보관해야 합니다. 
단 값들이 외부에 노출되지 않도록 보안에 신경 써서 보관해야 합니다.
<br/><br/>

### Ads API 액세스 요청
위의 두 단계는 일반적으로 Twitter API를 사용하기 위한 절차와 동일합니다. 
Ads API를 사용하기 위해선 추가로 액세스 요청을 진행해야 합니다. 
공식문서에 나와있는 액세스 신청 페이지로 넘어가면 아래와 같은 페이지가 나옵니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/c-twitter-developer-adsapi-1.png" width=auto height=auto>

첫 번째로 가장 기본적인 내용을 기재합니다. 
이미지에 나와 있는 Company handle에는 트위터 계정을 입력하면 됩니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/c-twitter-developer-adsapi-2.png" width=auto height=auto>

그다음 컨택포인트에 관한 내용을 기재합니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/c-twitter-developer-adsapi-3.png" width=auto height=auto>

광고 계정으로 운영하는 사업체에 대한 내용을 기재합니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/c-twitter-developer-adsapi-4.png" width=auto height=auto>

사업에 관한 상세한 내용을 입력하고, API를 이용하는 다른 플랫폼에 대한 내용도 선택합니다. 
다음 Client에 대한 내용을 기재합니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/c-twitter-developer-adsapi-5.png" width=auto height=auto>

트위터 광고 API를 통해 만들 제품에 대한 설명을 기재합니다. 
저의 경우 데이터를 제공할 목적으로 사용하기 때문에 Internal business system을 선택했습니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/c-twitter-developer-adsapi-6.png" width=auto height=auto>

마지막으로 동의 여부를 체크하고 제출하면 신청이 완료됩니다. 
신청이 완료되면 위에 작성한 컨택포인트 메일로 안내 메일이 발송됩니다. 
메일 내용에선, 영업일 기준 7일 이내에 트위터 광고 API 승인 결과가 나온다고 안내합니다. 
신청서에 별다른 문제가 없으면 보통 1~2일 안에 승인이 완료되었다는 메일을 받을 수 있습니다.
<br/><br/>

## 광고 데이터 구조 파악하기
### 광고 매니저 살펴보기
트위터 API 개발 문서를 살펴보기에 앞서 트위터 광고가 어떻게 운영되고 있고, 어떤 기준으로 데이터를 제공하고 있는지 살펴볼 필요가 있습니다. 
광고 매니저로 들어가면 아래 이미지와 같은 페이지를 볼 수 있습니다. 
광고를 운영 중이라면, 매니저 페이지에 필터 조건에 따른 비용에 관한 차트를 보여주고 광고 단위별로 비용, 노출, 클릭 등을 집계하여 표로 제공합니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/d-twitter-admin-1.png" width=auto height=auto>

위 사진을 자세히 살펴보면 캠페인, 광고 그룹, 광고 이 3개의 탭을 볼 수 있습니다. 
대부분의 매체는 어떤 소재(광고)로 어떤 그룹(광고 그룹)에 어떤 액션(캠페인)을 하게 할지에 대한 형태로 광고를 구성합니다. 
즉 광고는 광고 그룹에 포함되고, 광고 그룹은 캠페인에 포함됩니다. 
자세히 설명하자면, 매니저 페이지의 캠페인 탭은 동일한 캠페인으로 지정한 모든 광고 그룹 데이터를 합산한 데이터를 제공합니다. 
API가 제공하는 데이터 역시 광고 매니저와 같은 소스를 사용하기 때문에 이점을 고려하면서 개발 문서를 살펴보면 좋습니다.
<br/><br/>

### 트위터 개발자 문서 살펴보기
API로 데이터를 가져오기 위해선 요청할 때 인증이 필요합니다. 
이때 앞서 설명한 토큰과 키가 필요합니다. 
인증에 대한 내용은 이 [링크](https://developer.twitter.com/en/docs/twitter-ads-api/making-authenticated-requests)를 참고하면 됩니다.  

트위터 광고 API 개발 문서를 살펴보면, 광고 분석에 대한 문서가 있습니다. 
문서에서는 트위터에서 광고 중인 콘텐츠의 성과를 이해하는 데 도움이 되는 정보(데이터)를 제공한다고 언급하고 있습니다. 
데이터에는 노출 수, 클릭 수, 지출 등이 있습니다. 
문서를 자세히 읽어보면 광고 API에서 실적 지표를 검색하는 2가지 방법(synchronously, asynchronously)을 지원한다고 말합니다. 
저의 경우 동기식(synchronously) 방법을 사용했습니다. 
2가지 방법에 대한 차이는 문서에 나와 있는 내용을 자세히 읽어보기를 바랍니다.

Synchronous Analytics [문서](https://developer.twitter.com/en/docs/twitter-ads-api/analytics/api-reference/synchronous)에는 API 요청할 때 사용할 수 있는 파라미터와 파라미터의 내용을 설명합니다. 
Synchronous Analytics 리소스의 경우 안내하는 모든 파라미터가 필수조건입니다. 
파라미터 중에서 `entity`는 데이터를 검색할 엔터티 유형을 선택할 수 있는 파라미터입니다. 
앞서 광고 매니저를 살펴볼 때 데이터를 캠페인, 광고 그룹, 광고의 유형으로 나누었는데, `entity`의 값을 파라미터에 담아 요청하면 관련 리포트를 받을 수 있다는 것을 의미합니다. 
파라미터 설명 아래에는 요청 예시 및 응답 예시를 같이 안내하고 있습니다. 
요청 및 응답 예시는 API를 호출하는 양식을 살펴볼 수 있어 본격적으로 작업할 때 유용합니다.
<br/><br/>

### 주의할 점
`entity`에 대한 설명을 보면서 느껴졌겠지만, 개발자 문서와 광고 매니저에서 제공하는 용어나 마케터들이 사용하는 용어가 다르기도 합니다. 
담당자와 지속적인 커뮤니케이션을 통해 용어에 대한 싱크를 맞춰가면서 작업하면 훨씬 원활하게 작업할 수 있습니다.
<br/><br/>

## Python 코드 작성 및 데이터 가져오기
개발 문서를 살펴보면, 대부분의 설명을 Twurl을 바탕으로 설명하고 있습니다. 
Twurl은 curl과 비슷하지만, Twitter API용으로 특별히 조정된 커맨드 명령어 툴입니다. 
하지만 API 요청과 동시에 데이터를 직접 가공하기 위해선 커맨드 명령어 툴로는 불편함이 있습니다. 
따라서 API 호출 및 데이터 처리를 구조화해서 처리하기 위해 Python을 활용했습니다.
<br/><br/>

### 인증 요청 만들기
앞서 설명한 단계가 마무리되면, 이제 본격적으로 트위터 광고비 데이터를 가져올 수 있습니다.
광고비 데이터를 가져오기 위해선 Twitter Ads API 엔드포인트에 액세스해야 합니다. 
하지만 애플리케이션에선 인증된 요청만 허용하기 때문에 엔드포인트에 접근할 때 인증 요청도 같이 이뤄져야 합니다. 
인증 요청을 하는 방법은 이 링크에서 확인할 수 있습니다. 

데이터를 가져오기 위해 여러 파이썬 라이브러리가 필요하겠지만, 그중 인증과 관련된 라이브러리는 `requests_oauthlib` 입니다. 
이 라이브러리는 OAuth1 및 OAuth2 클라이언트를 구축하기 위한 사용하기 쉬운 파이썬 인터페이스를 제공합니다. 
첨부한 링크 내용에 따르면, 광고 API는 인증 OAuth 1.0A 헤더를 생성하는 작업이 필요하다고 언급합니다. 
따라서 `requests_oauthlib` 라이브러리의 OAuth1 와 OAuth2 중에서 OAuth1를 사용합니다. 
사용 예시는 아래와 같습니다.
```python
import requests
from requests_oauthlib import OAuth1

url = 'https://...'

client_key = '...'
client_secret = '...'
resource_owner_key = '...'
resource_owner_secret = '...'

headeroauth = OAuth1(
    client_key,
    client_secret,
    resource_owner_key,
    resource_owner_secret,
    signature_type='auth_header'
)

r = requests.get(url, auth=headeroauth)
```
예시를 간략히 설명하면, requests_oauthlib 라이브러리를 사용하여 OAuth1 헤더 인증을 생성하고 requests 요청할 때 auth인자로 같이 전달한다는 의미입니다. 
상세한 설명은 각 라이브러리의 Docs를 확인하면 좋습니다🙂.
<br/><br/>

### 데이터 요청 및 처리
이제 트위터 광고 API 엔드포인트로 액세스하여 데이터를 가져올 수 있습니다. 
위 인증 코드를 참고하여 앞선 문단에서 얘기한 Synchronous Analytics 데이터를 가져오는 로직을 예시 코드와 함께 설명하겠습니다.
```python
import requests
from requests_oauthlib import OAuth1

import json
```
먼저 라이브러리입니다. 
라이브러리는 인증 및 요청에 사용된 라이브러리에 더불어 데이터 가공에 사용되는 json 라이브러리를 더 사용합니다.
```python
consumer_key = '...'          #라이브러리 Docs의 client_key
consumer_secret = '...'       #라이브러리 Docs의 client_secret
access_token = '...'          #라이브러리 Docs의 resource_owner_key
access_token_secret = '...'   #라이브러리 Docs의 resource_owner_secret

account_id = '...'            #광고계정의 account_id
```
그다음은 토큰입니다. 
라이브러리 Docs와 트위터에서 언급하는 키와 토큰의 변수명이 달라 조금 생소할 수 있습니다. 
주석을 참고하여 발급한 키와 토큰을 변수에 적용하면 됩니다. 
관련해서 OS 환경변수를 활용하여 변수로 받는 등 좀 더 보안을 신경 써서 키와 토큰을 사용하는 방법도 있습니다.
```python
url = f'https://ads-api.twitter.com/12/stats/accounts/{account_id}'

headeroauth = OAuth1(
    consumer_key,
    consumer_secret,
    access_token,
    access_token_secret,
    signature_type='auth_header'
)

params = {
    "account_id": f"{account_id}",
    "start_time": "2023-01-30",
    "end_time": "2023-01-31",
    "entity": "PROMOTED_TWEET",
    "entity_ids": [
										"entity_id1", 
										"entity_id2",
										"entity_id3",
									],
    "granularity": "DAY",
    "metric_groups": "ENGAGEMENT,BILLING,WEB_CONVERSION",
    "placement": "ALL_ON_TWITTER",
}
```
다음으로 API 요청에 필요한 URL, OAuth1 헤더 인증, 파라미터입니다. 
URL은 개발문서의 Resource URL을 참고하면 됩니다. 
헤더 인증은 위에서 설명한 것처럼 트위터에서 제공하는 토큰을 가지고 `requests_oauthlib` 라이브러리로 생성하면 됩니다. 
여기서 자세히 살펴볼 부분은 파라미터입니다. 
개발문서에 사용할 수 있는 파라미터의 목록들이 있습니다. 
파라미터로 API 데이터 요청 시 필터 조건을 줄 수 있습니다. 
대략적인 사용 예시는 위 코드와 같습니다. 
파라미터에 대한 자세한 내용은 개발문서에 잘 나와 있으니 참고하면 좋습니다🙂.
```python
r = requests.get(url, params, auth=headeroauth)

data = r.json()['data']
```
마지막으로 데이터 요청 및 가공입니다. 
전 단계에서 준비한 URL, 헤더 인증, 파라미터를  `requests` 라이브러리로 요청합니다. 
요청에 사용되는 HTTP 메소드의 경우 앤드포인트마다 다를 수 있습니다. 
Synchronous Analytics의 경우 GET 메소드를 활용합니다. 
요청한 값을 변수에 저장하여 해당 변수를 확인해봤을 때 응답코드 200이 나오면 데이터 요청을 성공한 것입니다. 
응답코드 200이 나오면 `json` 라이브러리를 통해 json으로 읽을 수 있도록 변환합니다. 
json으로 변환한 값은 개발문서의 응답 예시와 같이 출력됩니다. 
응답 예시처럼 데이터가 잘 나온다면 이제 각자의 방식대로 데이터를 가공하면 됩니다. 
<br/><br/>

## 마무리하며
준비가 잘 되어있으면 API로 데이터를 가져오는 것은 어렵지 않습니다.
모든 작업이 그렇겠지만, 작업에 가장 큰 어려움은 커뮤니케이션의 부재에서 발생합니다. 
실제로 데이터를 필요로 하는 조직과 가공하는 조직의 이해도가 각기 달라 정말 필요한 값을 놓쳐 작업을 여러 번 하게 되는 등의 이슈가 발생할 수 있습니다. 
문서를 자세히 읽어보고 데이터를 필요로 하는 조직과 꾸준히 커뮤니케이션하면서 작업을 한다면 큰 어려움 없이 작업을 수행할 수 있습니다🙂.