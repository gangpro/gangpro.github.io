---
layout: post
title: '[oracle] 오라클 데이터 모델링 - 엔티티 모델 매핑'
subtitle: 
categories: til
tags: til oracle
comments: true
date: 2019-04-18 07:12:17 +0900
lastmod: 2019-04-18 07:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 데이터 모델링 - 엔티티 모델 매핑<br />

# 엔티티 모델 매핑
> Mapping the Entity Model


## Database Design : Relational Design
* Eyes Are Usually Red After Swimming(Studying)
```
    Initial database design

    1.Simple Entity     -> Table

    2.Attribtue         -> Column, Not null 설정, Sample data 입력

    3.UID(Relationship) -> Primary key

    4.Relationship      -> Foreign key -> 1 : M    -> Many측에 컬럼 추가 
                                       -> 1 : 1    -> Mandatory 1(+ unique 제약) or Wished 1(+ unique 제약)
                                       -> 61페이지 -> PERS_ID 컬럼 및 SPOUSE_ID 컬럼에 각각 unique 제약이 추가된다.

    5.Arc               -> Explicit Arc Design
                        -> Generic Arc Design 

    6.Subtype           -> Single Table Design     -> View로 단점 극복 가능
                        -> Separate Tables Design  -> View로 단점 극복 가능
                        -> Arc Implementation 
```

* Table Normalization
```
    Complete database design

    1.Referential integrity : on delete cacade, on delete set null

    2.Index

    3.View

    4.Denormalization

    5.Physical storage usage
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>