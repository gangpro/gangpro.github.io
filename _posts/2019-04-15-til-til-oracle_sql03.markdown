---
layout: post
title: '[TIL] 오라클 SQL - 단일행 함수'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 03:12:17 +0900
lastmod: 2019-04-15 03:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL 단일행 함수<br />

# 3장과 4장. 단일행 함수
> 3장과 4장 : 단일행 함수<br>

# 단일행 함수
* 문자 함수 
  - LOWER, UPPER, INITCAP
  - CONCAT, SUBSTR, LENGTH, INSTR, LPAD, RPAD, TRIM(LTRIM, RTRIM), REPLACE

* 숫자 함수
  - ROUND, TRUNC, MOD

* 날짜 함수
  - SYSDATE, MONTHS_BETWEEN, ADD_MONTHS, NEXT_DAY, LAST_DAY, ROUND, TRUNC, ...

* 변환 함수
  - TO_CHAR, TO_NUMBER, TO_DATE

* 일반 함수
  - NVL, NLV2, NULLIF, COALESCE
  - DECODE(vs CASE 표현식)

* dual table

* 참고 : <https://docs.oracle.com/cd/E18283_01/server.112/e17118/functions002.htm#CJAJHBIA>



## RPAD 함수
    select empno, ename, sal, ceil(sal/100)
    from emp;
    
    select empno, ename, sal, '*', ceil(sal/100)
    from emp;
    --방법1                                           --'*'자리에 채움 문자를 넣으면 된다.
    select empno, ename, sal, rpad('*', ceil(sal/100), '*') as stars
    from emp;
    
    select empno, ename, sal, ' ', ceil(sal/100)
    from emp;
    --방법2                                               --'*'자리에 채움 문자를 넣으면 된다.
    select empno, ename, sal, rpad(' ', ceil(sal/100) + 1, '*') as stars
    from emp;

## 문자함수 - LOWER, UPPER, INITCAP 함수
    select 'My name' a, 'My name' b, 'My name' c, 'My name' d
    from dual;
    
    select 'My name' a, upper('My name') b, lower('My name') c, initcap('My name') d
    from dual;

## 날짜 함수
    오라클은 Date를 내부적으로 7byte Nuberic으로 저장 : 20190327114325
    
    alter session set nls_date_format = 'yyyy-mm-dd hh24:mi:ss';
    select sysdate from dual;
    
    alter session set nls_date_format = 'yyyy-mm-dd';
    select sysdate from dual;

## to_char 함수 : 날짜 -> 문자
    --아래와 같이 날짜 기본 세팅 후 진행하자.
    alter session set nls_language = 'american';
    alter session set nls_territory = 'america';
### 
    --연 월 일 등   
    --format element로 format model 구성
    --1980 1980 1980
    select hiredate, to_char(hiredate, 'yyyy yyyy yyyy')
    from emp;
    
    --element -> 연, 분기, 월, 주, 일
    --1980 4 12 3 17
    select hiredate, to_char(hiredate, 'yyyy q mm w dd')
    from emp;
    
    --연, 분기, 월, 주, 일, 요일
    --1980 4 12 51 3 352 17 4 wed
    select hiredate, to_char(hiredate, 'yyyy q mm ww w ddd dd d dy')
    from emp;
    
    --NINETEEN EIGHTY-ONE / Nineteen Eight-One / nineteen eight-one
    select hiredate, to_char(hiredate, 'YEAR Year year')
    from emp;
    
    --fm : fill mode
    --Nineteen Eighty December Dec Wednesday Wed
    select hiredate, to_char(hiredate, 'fmYear Month Mon Day Dy')
    from emp;
    
    --17 seventeen 17th seventeenth seventeenth
    select hiredate, to_char(hiredate, 'dd ddsp ddth ddspth ddthsp')
    from emp;
###   
    --시, 분, 초
    --시스템 현재 날짜 
    --27-MAR-19 / 02 02 14 13 13
    select sysdate, to_char(sysdate, 'hh hh12 hh24 mi ss')
    from dual;
    
    --하루 중에 지나간 초 리턴(하루는 86400초)
    --27-MAR-19 / 51206
    select sysdate, to_char(sysdate, 'sssss')
    from dual;
    
    --27-MAR-19 / 오늘은 2019년 03월 27일입니다
    select sysdate, '오늘은 '||to_char(sysdate, 'yyyy')||'년 '||to_char(sysdate, 'mm')||'월 '||to_char(sysdate, 'dd')||'일입니다' as greeting
    from dual;
    
    --27-MAR-19 / 오늘은 2019년 03월 27일입니다.
    select sysdate, to_char(sysdate, '"오늘은" yyyy"년" mm"월" dd"일입니다."')
    from dual;
###
    -- 문제. 분기별 입사자 수 구하기
    
    select hiredate, to_char(hiredate, 'q')
    from emp;
    
    select to_char(hiredate, 'q') 분기, count(*) 입사자수
    from emp
    group by to_char(hiredate, 'q')
    order by 분기;

## decode 함수
* simple case expression, searched case expression
* sql 안에 함수를 넣을 수 있다.

## decode 예제(1)
* 문제1. 같은 값을 decode 함수로 다양하게 변환하기
###
    --emp table에서
    --deptno 컬럼과
    --deptno 컬럼과
    --deptno 컬럼과
    --deptno 컬럼 리턴
    select deptno, deptno, deptno, deptno
    from emp;
    
    
    
    --emp table에서
    --deptno 컬럼과
    --만약 decode(deptno)가 10이면 A, 20이면 B, 그밖에는 Z라는 의미이고 컬럼명은 ret1
    --deptno 컬럼과
    --deptno 컬럼을 리턴 
    select deptno, decode(deptno, 10, 'A', 20, 'B', 'Z') ret1, deptno, deptno
    from emp;
    
    
    
    --emp table에서
    --deptno 컬럼과
    --만약 decode(deptno)가 10이면 A, 20이면 B, 그밖에는 Z라는 의미이고 컬럼명은 ret1
    --만약 decode(deptno)가 10이면 A, 20이면 B, 그밖에는 빈칸이라는 의미이고 컬럼명은 ret2
    --deptno 컬럼을 리턴
    select deptno,
            decode(deptno, 10, 'A', 20, 'B', 'Z') ret1,
            decode(deptno, 10, 'A', 20, 'B') ret2,
            deptno
    from emp;
    
    
    
    --emp table에서
    --deptno 컬럼과
    --sal 컬럼과
    --만약 decode(deptno)가 10이면 A, 20이면 B, 그밖에는 Z라는 의미이고 컬럼명은 ret1
    --만약 decode(deptno)가 10이면 A, 20이면 B, 그밖에는 null이라는 의미이고 컬럼명은 ret2
    --만약 decode(deptno)가 10이면 sal*1.1값, 20이면 sal*1.2, 그밖에는 sal값이라는 의미이고 컬럼명은 ret3
    select deptno,
            decode(deptno, 10, 'A', 20, 'B', 'Z') ret1,
            decode(deptno, 10, 'A', 20, 'B') ret2,
            decode(deptno, 10, sal*1.1, 20, sal*1.2, sal) ret3
    from emp;
    
    

## decode 예제(2)    
* 문제2. 부서별 직무별 급여합을 쿼리하세요.(단, 결과는 매트릭스 형태로 나타내세요.)
###    
    --emp table에서
    --deptno 컬럼과
    --sal 컬럼과
    --sal 컬럼과
    --sal 컬럼과
    --sal 컬럼을 리턴
    select deptno, sal, sal, sal, sal
    from emp;
    
    
    
    --emp table에서
    --decode(deptno, 10, sal) deptno가 10이면 sal값 리턴, 10이 아니면 null 리턴, 컬럼명은 d10
    --sal 컬럼과
    --sal 컬럼을 리턴
    select deptno, sal, decode(deptno, 10, sal) d10, sal, sal
    from emp;
    
    
    
    --emp table에서 
    --deptno 컬럼과 
    --sal 컬럼과
    --deptno가 10이면 sal 대입 아니면 null 그리고 컬럼명은 d10
    --deptno가 20이면 sal 대입 아니면 null 그리고 컬럼명은 d20
    --deptno가 30이면 sal 대입 아니면 null 그리고 컬럼명은 d30
    select deptno,
           sal,
           decode(deptno, 10, sal) d10,
           decode(deptno, 20, sal) d20,
           decode(deptno, 30, sal) d30
    from emp;
    
    
    
    --emp table에서
    --sal 합계를 구하고 컬럼명은 total
    --deptno가 10이면 sal을 대입해서 합계를 구하고 컬럼명은 d10
    --deptno가 20이면 sal을 대입해서 합계를 구하고 컬럼명은 d20
    --deptno가 30이면 sal을 대입해서 합계를 구하고 컬럼명은 d30
    select 
           sum(sal) total,
           sum(decode(deptno, 10, sal)) d10,
           sum(decode(deptno, 20, sal)) d20,
           sum(decode(deptno, 30, sal)) d30
    from emp;
    
    
    
    --emp table에서
    --job 컬럼과
    --sal의 합계를 구하고 컬럼명은 total,
    --deptono가 10이면 sal을 대입한 후 합계를 구하고 컬럼명은 d10
    --deptono가 20이면 sal을 대입한 후 합계를 구하고 컬럼명은 d20
    --deptono가 30이면 sal을 대입한 후 합계를 구하고 컬럼명은 d30
    --job으로 그룹을 짓고
    --job으로 정렬(Default값은 ASC(오름차순)
    select job,
           sum(sal) total,
           sum(decode(deptno, 10, sal)) d10,
           sum(decode(deptno, 20, sal)) d20,
           sum(decode(deptno, 30, sal)) d30
    from emp
    group by job
    order by job;
    
    
    
    --emp table에서
    --job 컬럼과
    --sal의 합계를 구하고 컬럼명은 total, null값은 0으로 대체 
    --deptono가 10이면 sal을 대입한 후 합계를 구하고 컬럼명은 d10, null값은 0으로 대체 
    --deptono가 20이면 sal을 대입한 후 합계를 구하고 컬럼명은 d20, null값은 0으로 대체 
    --deptono가 30이면 sal을 대입한 후 합계를 구하고 컬럼명은 d30, null값은 0으로 대체 
    --job으로 그룹을 짓고
    --job으로 정렬(Default값은 ASC(오름차순)
    select job,
           nvl(sum(sal), 0) total,
           nvl(sum(decode(deptno, 10, sal)), 0) d10,
           nvl(sum(decode(deptno, 20, sal)), 0) d20,
           nvl(sum(decode(deptno, 30, sal)), 0) d30
    from emp
    group by job
    order by job;



## decode 예제(3)
* 문제3. sal이 2000미만이면 low, sal이 2000이상이면 high
* Simple case expression, searched case expression
###
    select empno, sal, sal
    from emp;
    
    select empno, sal, sal/2000
    from emp;
    
    --나눗셈을 한뒤 소수점 절삭
    select empno, sal, trunc(sal/2000)
    from emp;
    
    select empno,
           sal,
           decode(trunc(sal/2000), 0, 'low', 'high') as flag
    from emp;


## decode 함수, simple case 표현식, searched case 표현식
* decode function
* simple case expression
* searched case expression
###
    --decode 함수
    select empno,
           sal,
           decode(trunc(sal/2000), 0, 'low', 'high') as flag
    from emp;
    
    
    --simple case 표현식 
    select empno,
           sal,
           case trunc(sal/2000) when 0 then 'low'
                                else        'high' 
           end flag
    from emp;
    
    --searched case 표현식 
    select empno,
           sal,
           case when sal < 2000 then 'low'
                when sal >=2000 then 'high'
           end flag
    from emp;

## dual table
###
    desc sys.dual
    select * from dual;
    
    select empno, ename, 100*1.3+210 * from emp;
    select 100*1.3+210 * from emp;   --14건 
    select 100*1.3+210 * from dept;   --4건
    select 100*1.3+210 * from dual;   --1건
    
    참고 : https://docs.oracle.com/cd/E11882_01/server.112/e41084/queries009.htm#SQLRF20036




<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>