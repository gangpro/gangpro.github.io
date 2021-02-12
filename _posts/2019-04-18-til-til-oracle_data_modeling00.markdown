---
layout: post
title: '[oracle] 오라클 데이터 모델링 - Intro'
subtitle: 
categories: til
tags: til oracle
comments: true
date: 2019-04-18 00:12:17 +0900
lastmod: 2019-04-18 00:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 데이터 모델링 - Intro<br />

# Intro
> Intro

## Data Modeling and Database Design
* 용어
  - Data Modeling?
  - Logical Modeling -> Relational Modeling -> Physical Modeling 
  - 개념모델링 -> 논리모델링 -> 물리모델링
  - Data Modeling -> Database Design

  - Data Modeling   : http://en.wikipedia.org/wiki/Data_modeling
  - Database Design : http://en.wikipedia.org/wiki/Database_Design

* 실습
  - [URL](http://me2.do/xMvTWFRe) : Modeling for a Small Database
  - [URL](http://me2.do/xeidjI0a) : Re-engineering Your Database Using Oracle SQL Developer Data Modeler 4.0 
  - [URL](http://me2.do/xpctClDk) : Sample Models and Scripts
  - [URL](http://me2.do/FDTFsJOh) : Online Demonstrations - Data Modeler
  - [URL](http://me2.do/GGRz4LjY) : Searching Models in Oracle SQL Developer Data Modeler 4.0

* SQL 활용 능력이 Database Design에 큰 영향을 줍니다.
  - [URL](http://me2.do/FWF1jH60) : 오라클 실습
  - [URL](http://me2.do/GkFy9CQD) : DW 매뉴얼 21장 이후 

## System Development Cycle  
```         
      Business Information Requirements     
         ↓       
      Data Modeling (Conceptual) : Basic, Advanced 
         ↓    
     (CRUD Matrix 및 여러 가지 질문으로 Data Model 검토)
         ↓    
      Database Design
         ↓    
     (Performance를 고려한 Database Design 수정 : PK 컬럼 조정, Denomalization 등)
         ↓      
      Database Build : DB 설치 등
```

## SQL Developer 접속
<img width="100%" alt="Screen Shot 2019-04-15 at 10 24 21 AM" src="https://user-images.githubusercontent.com/46523571/56102595-f4cfb000-5f68-11e9-82f2-cada40af46e7.png">

<br>
<br>
<br>

## View - Modeler - Browser창 생성
<img width="100%" alt="Screen Shot 2019-04-15 at 10 21 25 AM" src="https://user-images.githubusercontent.com/46523571/56102607-0d3fca80-5f69-11e9-9a1d-7c410579ddb1.png">

<br>
<br>
<br>

## File - Data Modeler - Save 저장
<img width="100%" alt="Screen Shot 2019-04-15 at 10 21 34 AM" src="https://user-images.githubusercontent.com/46523571/56102628-38c2b500-5f69-11e9-8b82-a6b36036e91f.png">
<img width="100%" alt="Screen Shot 2019-04-15 at 10 23 21 AM" src="https://user-images.githubusercontent.com/46523571/56102667-6b6cad80-5f69-11e9-8697-ceef011e7efa.png">

<br>
<br>
<br>

## 새로운 관계형 모델 생성
* 관계형 모델(Relational Models) 오른쪽 마우스 클릭 후 새로운 관계형 모델 생성
<img width="100%" alt="Screen Shot 2019-04-15 at 10 25 06 AM" src="https://user-images.githubusercontent.com/46523571/56102689-8fc88a00-5f69-11e9-8136-37c0c36de21f.png">
* 새로 생성한 Relational_2 - 물리적 모델(Pysical Models) 오른쪽 마우스 클릭 후 New 생성
<img width="100%" alt="Screen Shot 2019-04-15 at 10 25 15 AM" src="https://user-images.githubusercontent.com/46523571/56102690-90f9b700-5f69-11e9-837b-99710d89efbb.png">
* 'Oracle Database 11g' 물리적 모델 생성
<img width="100%" alt="Screen Shot 2019-04-15 at 10 25 22 AM" src="https://user-images.githubusercontent.com/46523571/56102692-922ae400-5f69-11e9-9b12-6ed202533fd8.png">
* 'SQL Server 2012' 물리적 모델 생성
<img width="100%" alt="Screen Shot 2019-04-15 at 10 25 34 AM" src="https://user-images.githubusercontent.com/46523571/56102693-935c1100-5f69-11e9-8e14-58598ad5a416.png">

<br>
<br>
<br>

## 새로운 개체 생성
* Logical Models 오른쪽 마우스 클릭 - 새 엔티티
<img width="100%" alt="Screen Shot 2019-04-15 at 11 10 47 AM" src="https://user-images.githubusercontent.com/46523571/56104622-775d6d00-5f73-11e9-8832-2ecd67dda25d.png">

```
      일반 - 이름: DEPT
      
      속성 - 아래 3개 추가
      이름 : deptno
      데이터 타입 : Logical
      소스 타입 : BIGINT
      Primary UID 체크
    
      이름 : dname
      데이터 타입 : Logical
      소스 타입 : CHAR
    
      이름 : loc
      데이터 타입 : Logical
      소스 타입 : CHAR
```

<img width="100%" alt="Screen Shot 2019-04-15 at 11 10 57 AM" src="https://user-images.githubusercontent.com/46523571/56104647-8e9c5a80-5f73-11e9-8d3c-3ee01d3904f4.png">
<img width="100%" alt="Screen Shot 2019-04-15 at 11 20 27 AM" src="https://user-images.githubusercontent.com/46523571/56104649-8fcd8780-5f73-11e9-8173-e65c288667cd.png">

```
      일반 - 이름: EMP
      
      속성 - 아래 3개 추가
      이름 : empno
      Primary UID 체크
    
      이름 : ename
      소스 타입 : CHAR
    
      이름 : sal
      소스 타입 : CHAR 
```

<img width="100%" alt="Screen Shot 2019-04-15 at 11 20 50 AM" src="https://user-images.githubusercontent.com/46523571/56107096-5ac73200-5f7f-11e9-8c77-7d3aaa4ba893.png">
<img width="100%" alt="Screen Shot 2019-04-15 at 1 06 46 PM" src="https://user-images.githubusercontent.com/46523571/56107155-a548ae80-5f7f-11e9-9822-8504c0a3691e.png">

<br>
<br>
<br>

## 관계 생성
* 새 1:N 단계 버튼 클릭
- DEPT 엔티티 클릭 후 EMP 엔티티 클릭하면 관계가 생성된다.

<img width="100%" alt="Screen Shot 2019-04-15 at 11 23 15 AM" src="https://user-images.githubusercontent.com/46523571/56104118-627fda00-5f71-11e9-84f4-a9382dd4f558.png">
<img width="100%" alt="Screen Shot 2019-04-15 at 11 24 14 AM" src="https://user-images.githubusercontent.com/46523571/56104122-69a6e800-5f71-11e9-9abc-83e4953413ad.png">
<img width="100%" alt="Screen Shot 2019-04-15 at 11 23 57 AM" src="https://user-images.githubusercontent.com/46523571/56104123-69a6e800-5f71-11e9-9629-7de004750b86.png">

<br>
<br>
<br>

## 표기법(Notation)
```

```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>