---
layout: post
title: '[oracle] 오라클 데이터 모델링 - 엔티티, 속성 관계 세부 사항'
subtitle: 
categories: til
tags: til oracle
comments: true
date: 2019-04-18 02:12:17 +0900
lastmod: 2019-04-18 02:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 데이터 모델링 - 엔티티, 속성 관계 세부 사항<br />

# 엔티티 및 속성 세부 사항
> Entities and Attributes in Detail

## 수퍼 타입과 서브 타입
* 상호 베타적 Attributes
* 수퍼 타입은 모든 속성과 관계를 상속한다.
* 서브 타입은 단독으로 존재 하지 않는다.
* 서브 타입은 
* 테이블 -> ERD -> 테이블로 바꿔보기
<img width="100%" alt="Screen Shot 2019-04-16 at 11 27 21 AM" src="https://user-images.githubusercontent.com/46523571/56177771-c5897380-603a-11e9-8a48-a7f98416c87b.png">
<img width="100%" alt="Screen Shot 2019-04-16 at 11 27 27 AM" src="https://user-images.githubusercontent.com/46523571/56177769-c4f0dd00-603a-11e9-8774-d85bb92c0777.png">
<img width="100%" alt="Screen Shot 2019-04-16 at 11 34 40 AM" src="https://user-images.githubusercontent.com/46523571/56178031-a3442580-603b-11e9-9918-921d5f3093a4.png">

<br>
<br>
<br>

## 수퍼 타입과 서브 타입(추가)
* 조금더 보충하면 ERD에서 테이블로 바꾸는 방식은 3가지가 있다.
![Screen Shot 2019-04-16 at 1 26 27 PM](https://user-images.githubusercontent.com/46523571/56182060-42244e00-604b-11e9-93bc-3bcba744b9c4.png)
![Screen Shot 2019-04-16 at 1 26 17 PM](https://user-images.githubusercontent.com/46523571/56182061-42244e00-604b-11e9-82d2-c98cd70d78ec.png)
![Screen Shot 2019-04-16 at 1 26 09 PM](https://user-images.githubusercontent.com/46523571/56182062-42244e00-604b-11e9-941c-c3954e90cc96.png)

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>