---
layout: post
title: '[oracle] 오라클 데이터 모델링 - 엔티티, 속성 관계'
subtitle: 
categories: til
tags: til oracle
comments: true
date: 2019-04-18 01:12:17 +0900
lastmod: 2019-04-18 01:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 데이터 모델링 - 엔티티, 속성 관계<br />

# 엔티티, 속성 관계 소개
> Introduction to Entities, Attributes, and Relationships

## Entity (개체)
* "Entity" Is something that exists separately from other things and has a clear identity of its own. (by [네이버 영영 사전](https://endic.naver.com/enkrEntry.nhn?sLn=en&entryId=6b7ba26f876245b5a9d9cfe1df2e4b43&query=Entity))
  - Is a thing of significance.
  - Must have attributes.(2개 이상 유무)
  - Must have multiple instances.(2개 이상 유무)
  - Must have a Unique Identifier(UID 유무).

* Entity를 찾는 순서
  - 0.무엇을 이용해서 엔티티를 찾을 것인가? 장표(보고서), 업무기술서, 인터뷰, 예전 시스템, 현장조사, DFD 등
  - 1.업무에 중요한 명사 찾기
  - 2.Entity 이름 짓기 
  - 3.Attribute 찾기 
  - 4.Instance 찾기 
  - 5.UID 찾기
  - 6.그림으로 그리기 

* 엔티티 검증의 대표적인 기법 : CRUD Matrix(CRUD : Create, Read, Update, Delete) 등
* 엔티티 검토용 질문 

## Relationship (관계)
* Relationship을 찾는 순서
  - 1.Existence : Relationship Matrix
  - 2.Name      : Relationship Matrix 
  - 3.Optionality : Must be(실선)(반드시), May be(점선)(있어도 되고 없어도 되고)
  - 4.Degree    : "1:1", "1:M", "M:M"
  - 5.Validate  : 관련자들 앞에서 큰소리로 떠들기

* Relationship Syntax
###
    Each entity1   must be     relationship name    one or more        entity2s.
    Each entity1   may be      relationship name    one or more        entity2s.
    Each entity1   must be     relationship name    one and only one   entity2.
    Each entity1   may be      relationship name    one and only one   entity2.
 
                [optionality]        [name]           [Degree]

    cf.A degree of 0 is addressed by may be.

## 연습문제
    -- 1-45. Practice : Read and Comment
    Each PERSON must be born in one or more TOWN(s)
    Each TOWN may be birthplace of one and only one PERSON
    
    Each PERSON must be living in one or more TOWN
    Each TOWN may be home town of one or more PERSON(s)
    
    Each PERSON may be visitor of one or more TOWN(s) 
    Each TOWN must be visited by one or more PERSON(s)
    
    Each PERSON may be mayor of one and only one TOWN
    Each TOWN may be with mayor one and only one PERSON

* Relationship Types : 1:1 or 1:M or M:M


## Attribute (속성)
* Is a quality or feature that someone or something has. (by 네이버 영영사전)
* Attribute 찾는 순서
  - 1.Attribute 후보를 찾아서 Entity에 표현하세요.
  - 2.더 작은 단위의 Attribute로 나눌 수 있으면 나누기(원자단위로 쪼개기)  -> (ex: 주소(국가, 시군구, 읍면동 등등 나누는 것) 
  - 3.Single valued Attribute인가요?            -> A repeated attribute indicates a missing entity!!! (예를 들어, 휴대전화가 2개라면 두개의 컬럼을 사용해야한다.)
  - 4.Derived Attribute는 아니겠지요?
  - 5.정말 Attribute인가요? 혹시 Entity가 아닌가요?  -> If an attribute has their own attributes, then it is an entity!!!
  - 6.필수 속성인가 판단하기. 

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>