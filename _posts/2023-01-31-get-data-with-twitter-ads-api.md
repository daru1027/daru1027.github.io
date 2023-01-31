---
layout: post
title: "트위터 광고 API로 광고비 데이터 가져오기(Twitter Ads API)"
tags: API Twitter Data Ads
---
## 목차
1. [들어가며](#들어가며)
2. [Twitter Ads API 사용신청](#Twitter-Ads-API-사용신청)
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