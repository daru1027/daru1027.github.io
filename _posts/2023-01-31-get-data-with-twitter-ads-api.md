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
개발자 계정에 가입하는 과정은 크게 어렵지 않고 국가 선택 및 API를 사용하려는 목적을 선택하면 됩니다.

<img src = "/post_images/2023-01-31-get-data-with-twitter-ads-api/twitter-developer-signup-1.png" width=auto height=auto>