---
layout: post
title: '[oracle] 오라클 SQL Fundamentals II'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 12:12:17 +0900
lastmod: 2019-04-15 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL 유저 액세스 제어, 스키마 객체 관리, 데이터 딕셔너리 뷰를 사용하여 객체 관리, 대형 데이터 집합 조작, 다른 시간대에서 데이터 관리, Subquery를 사용하여 데이터 검색, 정규식 지원<br />

# 12장. Oracle Database 11g: SQL Fundamentals II
> 유저 액세스 제어, 스키마 객체 관리, 데이터 딕셔너리 뷰를 사용하여 객체 관리, 대형 데이터 집합 조작, 다른 시간대에서 데이터 관리, Subquery를 사용하여 데이터 검색, 정규식 지원

## 1. 유저 액세스 제어
* 권한(Privilege) : 어떤 일을 할 수 있는 권리(right)
  - System privilege : DBA 관리
  - Object privilege : 객체 소유자 or DBA 관리자

<br>
<br>
<br>

## 2. 스키마 객체 관리
* 주요 내용
  - 테이블 수정 : 컬럼, 제약조건 등을 alter 명령으로 수정
  - 인덱스      : ..., FBI(Function Based Index), ...
  - 플래시백    : ..., flashback table, ...
  - 임시 테이블
  - External 테이블
###
    DELETE FROM EMP;

    COMMIT;

    SELECT /* FLASHBACK QUERY */ * 
    FROM EMP AS OF TIMESTAMP TO_TIMESTAMP('2019-01-21 17:16:51', 'YYYY-MM-DD HH24:MI:SS'));

      ---

    DROP TABLE EMP;

    FLASHBACK TABLE EMP TO BEFORE DROP;

<br>
<br>
<br>

## 3. 데이터 딕셔너리 뷰를 사용하여 객체 관리
* Meta Data를 쿼리로 확인하려면
  - Static Data Dictionary View
  - Dynamic Performance View

<br>
<br>
<br>

## 4. 대형 데이터 집합 조작
* 주요 내용
  - DML에 포함된 SELECT
  - Multitable Insert
  - Merge
  - Flashback Versions Query

<br>
<br>
<br>

## 5. 다른 시간대에서 데이터 관리
* 주요 내용
###
      - Time Zone
      - 날짜 관련 Data type : Date -> Timestamp
                                      Timestamp with time zone
                                      Timestamp with local time zone
                                      Interval Year to Month
                                      Interval Day to Second
      - Timestamp 관련 단일행 함수들 : ..., current_timestamp, ...

* 관련 함수 예제
###
      alter session set nls_date_format = 'DD-MON-RR HH24.MI.SS';
    
      select sysdate, systimestamp
      from dual;
    
      alter session set time_zone = '-05:00';
    
      select current_date, current_timestamp
      from dual;
    
      alter session set time_zone = '+00:00';
    
      select current_date, current_timestamp
      from dual;
    
      alter session set time_zone = '+03:00';
    
      select current_date, current_timestamp
      from dual;
    
      alter session set time_zone = '+09:00';
    
      select current_date, current_timestamp
      from dual;
###
      테이블 예제
    
      drop table t1 purge;
     
      create table t1 
      (c1 date,
       c2 timestamp);
    
      insert into t1
      values (sysdate, systimestamp);
    
        ---
    
      drop table t1 purge;
    
      create table t_news
      (news varchar2(30),
       c1   date,
       c2   Timestamp,
       c3   Timestamp with time zone,
       c4   Timestamp with local time zone);
    
      insert into t_news
      values ('Korea Win!', sysdate, sysdate, sysdate, sysdate);
    
      alter session set time_zone = '-05:00';
      select * from t_news;
    
      alter session set time_zone = '+00:00';
      select * from t_news;
    
      alter session set time_zone = '+03:00';
      select * from t_news;
    
      alter session set time_zone = '+09:00';
      select * from t_news;
###
      Interval 데이터 타입
    
      ~ Date      + Number = Date
      ~ Timestamp + Number = Date   <- 이 문제를 극복하기 위해 Interval 데이터 타입이 지원됨
    
      select c1, c1 + 1, c2, c2 + 1, c2 + interval '1' day
      from t_news;
    
      select c1,
             c1 + 4 + 13/24 + 36/(24*60) + 59/(24*60*60) "4일 13시간 36분 59초 뒤",
             c1 + interval '4 13:36:59' day to second "4일 13시간 36분 59초 뒤"
      from t_news;

<br>
<br>
<br>

## 6. Subquery를 사용하여 데이터 검색
* 주요 내용
  - Pair-wise subquery = Multiple column multiple row subquery
  - Scalar subquery
  - 상호관련 서브쿼리
  - Exists 연산자
  - Subquery Factoring

<br>
<br>
<br>

## 7. 정규식 지원
* Using Regular Expressions in Database Applications
  - https://docs.oracle.com/cd/E11882_01/appdev.112/e41502/adfns_regexp.htm#ADFNS1003
###
      [예제 1]
    
      DROP TABLE contacts;
    
      CREATE TABLE contacts (
        l_name    VARCHAR2(30),
        p_number  VARCHAR2(30)
        CONSTRAINT c_contacts_pnf
        CHECK (REGEXP_LIKE (p_number, '^\(\d{3}\) \d{3}-\d{4}$')));
    
      INSERT INTO contacts (p_number) VALUES('(650) 555-0100');    -- 성공
      INSERT INTO contacts (p_number) VALUES('(215) 555-0100');    -- 성공
    
      INSERT INTO contacts (p_number) VALUES('650 555-0100');      -- 실패
      INSERT INTO contacts (p_number) VALUES('650 555 0100');      -- 실패
      INSERT INTO contacts (p_number) VALUES('650-555-0100');      -- 실패
      INSERT INTO contacts (p_number) VALUES('(650)555-0100');     -- 실패
      INSERT INTO contacts (p_number) VALUES(' (650) 555-0100');   -- 실패
###
      [예제 2]
    
      DROP TABLE famous_people;
    
      CREATE TABLE famous_people (names VARCHAR2(20));
    
      INSERT INTO famous_people (names) VALUES ('John Quincy Adams');
      INSERT INTO famous_people (names) VALUES ('Harry S. Truman');
      INSERT INTO famous_people (names) VALUES ('John Adams');
      INSERT INTO famous_people (names) VALUES (' John Quincy Adams');
      INSERT INTO famous_people (names) VALUES ('John_Quincy_Adams');
    
      COLUMN "names after regexp" FORMAT A20
    
      SELECT names "names",
        REGEXP_REPLACE(names, '^(\S+)\s(\S+)\s(\S+)$', '\3, \1 \2')
          AS "names after regexp"
      FROM famous_people
      ORDER BY "names";



<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>