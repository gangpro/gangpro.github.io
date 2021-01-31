---
layout: post
title: '[TIL] 오라클 SQL - 복수행 함수'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 05:12:17 +0900
lastmod: 2019-04-15 05:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL 복수행 함수<br />

# 5장. 복수행 함수
> 5장 : group by & having<br>
> 복수행 함수 : AVG, SUM, COUNT, MAX, MIN, STDDEV, VARIANCE


## Group by절 이해
    --emp table에서 
    --job 컬럼과 sal 컬럼 리턴
    select job, sal
    from emp;

    --emp table에서
    --job을 그룹한 컬럼과 sal 컬럼의 합 리턴
    select job, sum(sal)
    from emp
    group by job;

    --emp table에서
    --deptno와 job을 그룹한 컬럼과 sal 컬럼의 합 리턴
    --deptno 정렬한 후 job으로 정렬
    select deptno, job, sum(sal)
    from emp
    group by deptno, job
    order by deptno, job;
###
    --가공한 결과에 의한 Group by
    --문제 입사일자의 월(Month)를 활용한 집계
    --emp table에서
    --hiredate 컬럼에서 'mm' month를 문자화해서 그룹한 후
    --월로 정렬해서 리턴
    select to_char(hiredate, 'mm') as 월, count(*) as 인원수
    from emp
    group by to_char(hiredate, 'mm')
    order by 월;

## group by절 관련 중요 문법 
    -- select 리스트에서 복수행 함수로 감싼 이외의 모든 컬럼은 반드시 group by절에 나타나야 한다. 단, literal은 예
    --* 복수행 함수를 감싼 컬럼(sum(sal)을 제외한 나머지 컬럼이 group by에 나와야한다.
    --에러  
    select job, sum(sal)
    from emp;함

    --리턴
    select job, sum(sal)
    from emp
    group by job;

    --literal은 예외로 리턴
    select '부서별 직무별' gubun, deptno, job, sum(sal)
    from emp
    group by deptno, job
    order by deptno, job;

## 5-5. 그룹 함수 유형
    select          sal from emp;       --결과값 : 14명
    select distinct sal from emp;       --결과값 : 12명
    
    select avg(sal)          from emp;  --결과값 : 2073...
    select avg(distinct sal) from emp;  --결과값 : 2064...
    
## 5-6. 
    --* 모든 그룹 함수는 null을 무시한다. 단 count(*) 예외 
    drop table t1 purge;
    
    create table t1 (no number);
    
    insert into t1 values (1000);
    insert into t1 values (1000);
    insert into t1 values (2000);
    insert into t1 values (2000);
    insert into t1 values (null);
    insert into t1 values (null);
    
    commit;
    
    select no, no, no
    from t1;
###
    --count(*)           --no 컬럼에 들어간 모든 값 개수(null포함) : 6  
    --count(no)          --no 컬럼에 들어간 데이터 개수           : 4   
    --count(distinct no) --no 컬럼에 중복값을 제거한 데이터 개수    : 2  
    select count(*), count(no), count(distinct no)
    from t1;
    
    --count(*)      --사원수라는 의미             : 6 
    --count(comm)   --커미션을 받는 사원 수라는 의미 : 4 
    select count(*), count(comm)
    from emp
    where deptno = 30;

## 5-11. 그룹 함수 및 null 값
    select          sal from emp;       --결과값 : 14명
    select distinct sal from emp;       --결과값 : 12명
    
    select avg(sal)          from emp;  --결과값 : 2073...
    select avg(distinct sal) from emp;  --결과값 : 2064...

    select comm,
           nvl(comm, 0)
    from emp;
    
    select avg(comm) a,           --커미션 받는 사원들의 평균 커미션 
           avg(nvl(comm, 0)) b    --전체 사원들의 평균 커미션
    from emp;

## 5-20. where절 vs having절
    --여기서 where을 쓸까? having을 쓸까? 좋은 방법은 where
    --ex) where : 대통령 선거시 A시 뺄것 먼저 선택 후 시군구 집계 
    --ex) having :대통령 선거 시군구 집계를 다 한 후에 A시만 빼
    --where
    select deptno, sum(sal)
    from emp
    where deptno != 20   --    !=   <>  ^=   3가지 가능  
    group by deptno;
    
    --having
    select deptno, sum(sal)
    from emp
    group by deptno
    having deptno != 20;


    --복수행 함수 적용 전 걸러낸다. 
    --문법 에러
    select deptno, sum(sal)
    from emp
    where sum(sal) < 10000
    group by deptno;

    --복수행 함수 적용 후 걸러낸다.
    --리턴 
    select deptno, sum(sal)
    from emp
    group by deptno
    having sum(sal) < 10000;
    
    --즉 where을 먼저 처리해보고 안되면 having을 쓰자.

    --where 절에는 그룹함수 사용 불가능.
    --having 절에 그룹함수 사용 가능.

## 5-26 그룹함수의 중첩
    --부서별 평균 급여 구하기
    select avg(sal)
    from emp
    group by deptno;
    
    --부서별 최대 평균 급여 구하기
    select max(avg(sal))
    from emp
    group by deptno;


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>