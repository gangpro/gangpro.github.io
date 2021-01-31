---
layout: post
title: '[oracle] 오라클 SQL - SET 연산자'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 08:12:17 +0900
lastmod: 2019-04-15 08:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL SET 연산자<br />

# 8장. SET 연산자(a.k.a 수직 조인)
> 8장 : SET 연산자(a.k.a 수직 조인)<br>

## 연산자의 종류
* UNION ALL : 합집합(중복 허용)
* UNION     : 합집합(중복 제거)
  - SELECT ...
  - UNION ALL
  - SELECT ...
  - UNION ALL
  - SELECT ...
* INTERSECT : 교집합(중복 제거)
* MINUS     : 차집합(중복 제거)
###
    예)
    A = {1, 1, 1, 2, 2, 3, 3, 3}
    B = {3, 3, 4, 4, 4, 5, 5}
    
    A union all B = {1, 1, 1, 2, 2, 3, 3, 3, 3, 3, 4, 4, 4, 5, 5}
    A union     B = {1, 2, 3, 4, 5}
    A intersect B = {3}
    A minus     B = {1, 2}
    
## UNION ALL
###
    select deptno, job, sum(sal)
    from emp
    group by deptno, job;
    
    select deptno, sum(sal)
    from emp
    group by deptno;
    
    select sum(sal)
    from emp;
    
    위의 3개를 아래와 같이 union all로 묶음
    
    select deptno, job, sum(sal)
      from emp
     group by deptno, job
     union all
    select deptno, null, sum(sal)
      from emp
     group by deptno
     union all
    select null, null, sum(sal)
      from emp
     order by 1, 2;

## 문제
* 집계, 소계, 총계를 쿼리하세요
###
    select deptno, job, sum(sal)
    from emp
    group by deptno, job
    union all
    select deptno, null, sum(sal)
    from emp
    group by deptno
    union all
    select null, null, sum(sal)
    from emp
    order by 1, 2;

## 문제
* 빠진 번호 찾기
###
    drop table t1 purge;

    create table t1
    as
    select level no
    from dual
    connect by level <= 100;

    delete from t1
    where no in (select trunc(dbms_random.value(1, 100)) 
                 from dual
                 connect by level <= 7);

    --정답 
    select level no         --원본 데이터
    from dual
    connect by level <= 100
    minus
    select no               --현재 데이터가 빠진 데이터
    from t1;






<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>