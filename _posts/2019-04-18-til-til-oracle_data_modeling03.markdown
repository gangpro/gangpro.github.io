---
layout: post
title: '[oracle] 오라클 데이터 모델링 - 관계 세부 설명, 정규화'
subtitle: 
categories: til
tags: til oracle
comments: true
date: 2019-04-18 03:12:17 +0900
lastmod: 2019-04-18 03:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 데이터 모델링 - 관계 세부 설명, 정규화<br />

# 관계 세부 설명, 정규화
> Relationships in Detail
> Normalize the Data Model


## Normalize the Data Model
* Normailize the Data Model : normalize / 표준화하다 / 표준적으로 하다
  - 정규화 또는 정상화(normalization)는 어떤 대상을 일정한 규칙이나 기준에 따르는 '정규적인' 상태로 바꾸거나, 비정상적인 대상을 정상적으로 되돌리는 과정을 뜻한다. 
  - 데이터를 일정한 규칙에 따라 변형하여 이용하기 쉽게 만드는 일. 
###  
    ~ History of Normalization 
    
      Normalization is a technique established by the originator of the relational model, E.F. Codd. The complete 
      set of normalization techniques, include "twelve rules" that databases need to follow in order to be described
      as truly normalized. 

      It is a technique that was created in support of relational theory, years before entity relationship modeling
      was developed. The entity relationship modeling process has incorporated many of the normalization techniques
      to produce a normalized entity relationship diagram.

      Two terms that have their origins in the normalization technique are still widely in use. One is normalized data,
      the other is denormalization.

      출처 : RDM9i 교재


## 정규화 방법
    정규화되지 않은 상태

       ↓ 제1정규화(1 Normalization) : repeating group 제거

    제1정규형(1 Normal Form) : repeating group 제거된 상태

       ↓ 제2정규화(2 Normalization) : 복합UID에 대한 부분 종속 제거

    제2정규형(2 Normal Form) : 복합UID에 대한 부분 종속 제거된 상태

       ↓ 제3정규화(3 Normalization) : Non-UID에 대한 종속 제거

    제3정규형(3 Normal Form) : Non-UID에 대한 종속 제거된 상태

* 정규화 되지 않은 상태
<img width="913" alt="Screen Shot 2019-04-17 at 10 23 51 AM" src="https://user-images.githubusercontent.com/46523571/56254256-03e66780-60fb-11e9-9ce1-3d97d001ee59.png">
* repeating group 제거, 복합 UID에 대한 부분 종속 제거
<img width="1409" alt="Screen Shot 2019-04-17 at 10 24 16 AM" src="https://user-images.githubusercontent.com/46523571/56254257-03e66780-60fb-11e9-9ffa-6d9002c67cdc.png">
* repeating group 제거, 복합 UID에 대한 부분 종속 제거, Non-UID에 대한 종속 제거
<img width="1577" alt="Screen Shot 2019-04-17 at 10 24 23 AM" src="https://user-images.githubusercontent.com/46523571/56254258-047efe00-60fb-11e9-88e7-e00af3a4cd82.png">


  - Resolve M:M Relationships
  - Model Subtypes
  - Model Exclusive Relationships
  - Model Hierachical Data
  - Model Recursive Relationships
  - Model Role Relationships
  - Model Data over Time  >> cf.Flashback Data Archive + Flashback Query
  - Model Complex relationships


 
## 연습문제 3-30. M : M 나눠보기
* 테이블 생성
<img width="100%" alt="Screen Shot 2019-04-16 at 5 23 32 PM" src="https://user-images.githubusercontent.com/46523571/56193865-9c81d680-606c-11e9-8053-e5d05e71c01c.png">
* SQL에 테이블 생성
<img width="100%" alt="Screen Shot 2019-04-16 at 5 23 59 PM" src="https://user-images.githubusercontent.com/46523571/56193836-8b38ca00-606c-11e9-8d68-26b504309eb0.png">
* ERD 모델 그려보기
<img width="100%" alt="Screen Shot 2019-04-16 at 5 22 21 PM" src="https://user-images.githubusercontent.com/46523571/56193709-3c8b3000-606c-11e9-98fe-98e60a13285e.png">


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>