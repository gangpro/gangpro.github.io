---
layout: post
title: '[TIL] 오라클 SQL intro'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 00:12:17 +0900
lastmod: 2019-04-15 00:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL intro<br />

# 0장. intro
> 0장 : Oracle SQL intro<br>

## New Connection 
* Oracle SQL 생성하기

## SQL 개발환경 2가지
* 기본 도구는 Oracle SQL Developer
<img width="1680" alt="Screen Shot 2019-03-25 at 1 25 59 PM" src="https://user-images.githubusercontent.com/46523571/54895253-0c83bd80-4f02-11e9-8027-2c3ead7bd4c1.png">

* SQL *plus 명령행 인터페이스
<img width="844" alt="Screen Shot 2019-03-25 at 5 13 43 PM" src="https://user-images.githubusercontent.com/46523571/54904259-65168300-4f21-11e9-8428-5201e3fa6344.png">


## 우리는 기본 도구 Oracle SQL Developer을 사용
## 1. New Database Connection  클릭

<img width="1680" alt="Screen Shot 2019-03-25 at 9 29 07 AM" src="https://user-images.githubusercontent.com/46523571/54888468-b5212580-4ee0-11e9-99ff-d84f3fba9cb4.png">

## 2. New / Select Database Connection
- 강사님 server 접속, 나는 guest
- Connection Name : ace29
- User name : ace29
- Password : 

- Hostname : Server
- Port : 1521
- SID : xe

* Server 접속 성공시 아래와 같이 'Status : Success' 라고 뜬다.

<img width="1680" alt="Screen Shot 2019-03-25 at 10 02 32 AM" src="https://user-images.githubusercontent.com/46523571/54888957-206cf680-4ee5-11e9-91c3-522139f3a013.png">

## 3. sql 세팅
* creobjects.sql 파일을 ace29 sql 문에 클릭 드래그 한다.

<img width="1680" alt="Screen Shot 2019-03-25 at 9 38 33 AM" src="https://user-images.githubusercontent.com/46523571/54888620-f960f580-4ee1-11e9-83cf-3e854bf7f31c.png">

* creobjects.sql 파일 전체를 선택한 후 Run Script(F5)를 실행한다.

<img width="1680" alt="Screen Shot 2019-03-25 at 9 39 48 AM" src="https://user-images.githubusercontent.com/46523571/54888681-86a44a00-4ee2-11e9-8bde-fc5233a0a828.png">

<img width="1680" alt="Screen Shot 2019-03-25 at 10 04 07 AM" src="https://user-images.githubusercontent.com/46523571/54888998-69bd4600-4ee5-11e9-9cf1-1dfc54ec4e3b.png">

<img width="1680" alt="Screen Shot 2019-03-25 at 10 04 20 AM" src="https://user-images.githubusercontent.com/46523571/54889013-8194ca00-4ee5-11e9-952d-8c3877f1cb21.png">


* demobld.sql 파일을 ace29 sql 문에 클릭 드래그 한다.

<img width="1680" alt="Screen Shot 2019-03-25 at 9 38 45 AM" src="https://user-images.githubusercontent.com/46523571/54888678-74c2a700-4ee2-11e9-9611-15424b3fd92c.png">

* demobld.sql 파일 전체를 선택한 후 Run Script(F5)를 실행한다.

<img width="1680" alt="Screen Shot 2019-03-25 at 9 39 55 AM" src="https://user-images.githubusercontent.com/46523571/54888685-915edf00-4ee2-11e9-8e47-034f4ecd567c.png">

<img width="1680" alt="Screen Shot 2019-03-25 at 10 06 45 AM" src="https://user-images.githubusercontent.com/46523571/54889049-bd2f9400-4ee5-11e9-8932-adc4459f33e7.png">

<img width="1680" alt="Screen Shot 2019-03-25 at 10 06 57 AM" src="https://user-images.githubusercontent.com/46523571/54889060-d2a4be00-4ee5-11e9-967d-2324da8bd648.png">




* ace29 Tables가 생성된 걸 알 수 있다. 

<img width="1680" alt="Screen Shot 2019-03-25 at 10 08 37 AM" src="https://user-images.githubusercontent.com/46523571/54889079-f9fb8b00-4ee5-11e9-88c2-329dc16122db.png">


<br>
<br>
<br>
<br>
<br>

## Data Import

### 0단계 : 원하는 데이터를 구할 것
국도교통부 실거래가 공개시스템 <br>
http://rtdown.molit.go.kr/# <br>
예제 파일은 data 폴더에 있음.<br>

### 1단계 : 데이터를 담을 수 있는 자료구조를 만들 것
* ace29.sql에서 작성을 한다.
###
    create table apt
    (시군구   varchar2(30),
    번지     varchar2(10),
    본번     varchar2(10),
    부번     varchar2(10),
    단지명   varchar2(100),
    전용면적 varchar2(10),
    계약년월 varchar2(10),
    계약일   varchar2(10),
    거래금액 varchar2(10),
    층       varchar2(4),
    건축년도 varchar2(4),
    도로명   varchar2(10));
<img width="1680" alt="Screen Shot 2019-03-25 at 10 10 19 AM" src="https://user-images.githubusercontent.com/46523571/54889113-36c78200-4ee6-11e9-93cb-e97e057204ee.png">

* APT 테이블 구조를 만든 후 왼쪽 탭 Connection - Refresh 해야 APR 테이블이 생성된걸 볼 수 있다.

<img width="1680" alt="Screen Shot 2019-03-25 at 10 11 11 AM" src="https://user-images.githubusercontent.com/46523571/54889135-5494e700-4ee6-11e9-9822-fd3a0e732014.png">

### 2단계 : 익숙한 툴을 이용해서 데이터를 Import 할 것

 - 지금은 SQL Developer의 데이터 임포트 기능 활용함

* 데이터 import 하기

* 왼쪽 탭 - ace29 - tables - APT에서 오른쪽 마우스 클릭 후 import

<img width="1680" alt="Screen Shot 2019-03-25 at 10 12 44 AM" src="https://user-images.githubusercontent.com/46523571/54889187-a9d0f880-4ee6-11e9-96f1-2fc612faf95b.png">

* File - Browse - 데이터 로딩할 파일 선택

<img width="1680" alt="Screen Shot 2019-03-25 at 10 13 09 AM" src="https://user-images.githubusercontent.com/46523571/54889213-c79e5d80-4ee6-11e9-8e9d-6c1116302db7.png">

* Header, Skip Rows 등을 통해 불러올 데이터 선별 - Next

<img width="1680" alt="Screen Shot 2019-03-25 at 10 16 35 AM" src="https://user-images.githubusercontent.com/46523571/54889265-16e48e00-4ee7-11e9-9333-cbe8c9abc229.png">

* Import Method - Next

<img width="1680" alt="Screen Shot 2019-03-25 at 10 17 23 AM" src="https://user-images.githubusercontent.com/46523571/54889296-3bd90100-4ee7-11e9-9c99-a4864f1f699e.png">

* Choose Columns - Next 

<img width="1680" alt="Screen Shot 2019-03-25 at 10 17 40 AM" src="https://user-images.githubusercontent.com/46523571/54889311-4b584a00-4ee7-11e9-9846-3d1c61b9e9f9.png">

* Column Definition - Match By - position

<img width="1680" alt="Screen Shot 2019-03-25 at 10 18 43 AM" src="https://user-images.githubusercontent.com/46523571/54889329-6460fb00-4ee7-11e9-9fff-5eb63082f11a.png">

* Finish

<img width="1680" alt="Screen Shot 2019-03-25 at 10 19 27 AM" src="https://user-images.githubusercontent.com/46523571/54889346-7f336f80-4ee7-11e9-8f02-a0638a07936a.png">

* Import Data 오류. 이유는 컬럼 길이가 안 맞아서. No를 누른다.

<img width="1680" alt="Screen Shot 2019-03-25 at 10 20 00 AM" src="https://user-images.githubusercontent.com/46523571/54889369-95d9c680-4ee7-11e9-9974-5a8559fe7ac0.png">


### 데이터 길이가 다른 경우 해결 방법
    alter table apt modify(시군구   varchar2(40));
    alter table apt modify(거래금액 varchar2(20));
    alter table apt modify(도로명   varchar2(30));

<img width="1680" alt="Screen Shot 2019-03-25 at 10 22 37 AM" src="https://user-images.githubusercontent.com/46523571/54889423-ed783200-4ee7-11e9-88f7-f5444cfb6e6b.png">

* 왼쪽 탭 - ace29 - tables - APT에서 오른쪽 마우스 클릭 후 import
* File - Browse - 데이터 로딩할 파일 선택
* Header, Skip Rows 등을 통해 불러올 데이터 선별 - Next
* Import Method - Next
* Choose Columns - Next 
* Column Definition - Match By - position
* Finish

* 데이터 로딩 성공

<img width="1680" alt="Screen Shot 2019-03-25 at 10 25 30 AM" src="https://user-images.githubusercontent.com/46523571/54889511-58296d80-4ee8-11e9-8212-8e5d91973eab.png">




### 3단계 : 탐색적 데이터 분석 해볼 것
###
      select * 
      from apt;

<img width="1680" alt="Screen Shot 2019-03-25 at 9 23 28 AM" src="https://user-images.githubusercontent.com/46523571/54888320-ab4af280-4edf-11e9-8bae-3331387681bd.png">

###
      select * 
      from apt
      where 단지명 like '%센트레빌%';

<img width="1680" alt="Screen Shot 2019-03-25 at 9 24 30 AM" src="https://user-images.githubusercontent.com/46523571/54888345-d1709280-4edf-11e9-9f0c-0718f8a30417.png">

###
      select 단지명, 전용면적, MAX(거래금액), MIN(거래금액)
      from apt
      group by 단지명, 전용면적;

<img width="1680" alt="Screen Shot 2019-03-25 at 9 25 14 AM" src="https://user-images.githubusercontent.com/46523571/54888358-e9e0ad00-4edf-11e9-8ab8-96dd5ed8a2e0.png">

## 테이블 삭제 방법 
###
    drop table APT PURGE;

<img width="1680" alt="Screen Shot 2019-03-25 at 10 43 17 AM" src="https://user-images.githubusercontent.com/46523571/54890489-f0295600-4eec-11e9-92bf-00bd794b0f2f.png">

<img width="1680" alt="Screen Shot 2019-03-25 at 10 45 22 AM" src="https://user-images.githubusercontent.com/46523571/54890496-f7506400-4eec-11e9-9fda-0aaa60355836.png">

<br>
<br>
<br>
<br>
<br>

## Data Export
### 

* ace29.sql에서 EMP 테이블 export

<img width="1680" alt="Screen Shot 2019-03-25 at 11 00 02 AM" src="https://user-images.githubusercontent.com/46523571/54890643-ac831c00-4eed-11e9-8fc2-532cf370936b.png">

* Source/Destication

<img width="1680" alt="Screen Shot 2019-03-25 at 11 03 30 AM" src="https://user-images.githubusercontent.com/46523571/54890665-bc9afb80-4eed-11e9-869d-529a50e46bba.png">

* Specipy Data

<img width="1680" alt="Screen Shot 2019-03-25 at 11 05 32 AM" src="https://user-images.githubusercontent.com/46523571/54890720-f53ad500-4eed-11e9-9505-1d11ab6303db.png">

* Export Summary

<img width="1680" alt="Screen Shot 2019-03-25 at 11 06 30 AM" src="https://user-images.githubusercontent.com/46523571/54890758-1ac7de80-4eee-11e9-868f-b572a7ed9d70.png">

* Export 완료

<img width="1680" alt="Screen Shot 2019-03-25 at 11 07 48 AM" src="https://user-images.githubusercontent.com/46523571/54890800-4c40aa00-4eee-11e9-9477-deb85689654c.png">

* Export한 파일은 data 폴더에 있음. 



## 참고
    --환경 설정 값 변경
    show all
    
    --시스템 껐다 키면 리셋된다
    set linesize 200
    set pagesize 40
    
    
    
    --반복적인 일을 할때
    --c.sql 파일이 만들어 진다
    ed c
    --메모장에 적은 후 저장하면 된다
    clear screen
    --실행
    @c.sql
    --


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>