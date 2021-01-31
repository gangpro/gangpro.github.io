---
layout: post
title: '[TIL] 오라클 SQL - 데이터 제한 및 정렬'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 02:12:17 +0900
lastmod: 2019-04-15 02:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL 데이터 제한 및 정렬<br />

# 2장. 데이터 제한 및 정렬
> 2장 : where & order by<br>

# WHERE 절 이해
* 후보행(candidate row)을 검증해서 True, False, Null을 리턴하는 절인데 where절이 True 리턴해야 후보행이 리턴된다.

## 기본
    --기존에 있는 table 삭제(연습을 위해 기존것 삭제)
    drop table t1 purge;

    --emp 테이블에서 empno, sal, deptno을 가져와서 t1 테이블로 만들기
    create table t1
    as
    select empno, sal, deptno
    from emp;

    --t1 테이블 리턴
    select *
    from t1;

    --t1 테이블에서 sal이 1500 이상인 것을 empno, sal, sal*1.1로 리턴
    select empno, sal, sal*1.1
    from t1
    where sal >= 1500;

    --t1 테이블에서 sal이 1500 이상이고 deptno가 20인 것을 empno, sal, sal*1.1로 리턴
    select empno, sal, sal*1.1
    from t1
    where sal >= 1500 and deptno = 20;

    --emp 테이블 리턴
    select *
    from emp;

    --emp 테이블에서 이름이 A로 시작해서 empno, ename, sal, job, deptno, ename을 리턴
    select empno, ename, sal, job, deptno, ename
    from emp
    where ename like 'A%';

    --emp 테이블에서 이름이 A로 시작해서 empno, enmae, ename, ename, deptno을 리턴(1)
    --substr(ename, 1, 1) : ename 첫번째 부터 한칸
    --substr(ename, 2) : ename 두번째부터 끝까지
    select empno, ename, substr(ename, 1, 1), substr(ename, 2), deptno
    from emp
    where ename like 'A%';

    --emp 테이블에서 이름이 A로 시작해서 empno, enmae, ename, ename, deptno을 리턴(2)
    --substr(ename, 1, 1) 첫글자 : 컬럼명이 첫글자 라고 나온다
    --substr(ename, 2) 나머지 : 컬럼명이 나머지 라고 나온다
    select empno, ename, substr(ename, 1, 1) 첫글자, substr(ename, 2) 나머지, deptno
    from emp
    where ename like 'A%';











## 2-8. 비교 연산

## between A and B 연산자 사용
###
    select *
    from emp
    where empno between 7500 and 8000;
    
    select *
    from emp
    where empno >= 7500 and empno <= 8000;

## 연산자 우선순위에 따라 (AND 수행 후 OR 수행)
    --emp table에서 empno가 7500이상이고 empno가 8000이하이고 ename이 A로 시작하거나
    --ename이 B로 시작하는 모든 컬럼 리턴
    select *
    from emp
    where empno >= 7500 
        and empno <= 8000
        and ename like 'A%'
         or ename like 'B%' 
    
    --
    select *
    from emp
    where empno between 7500 and 8000
        and (ename like 'A%' or ename like 'B%' )
        and comm is null;

## in 연산자 사용    
        
    --에러남
    select *
    from emp
    where empno between 7500 and 8000
        and (ename like 'A%' or ename like 'B%' )
        and comm is null
        and mgr = (7788, 7698) -- 1:1 비교를 해야하는데 에러남

    --가능 
    select *
    from emp
    where empno between 7500 and 8000
        and (ename like 'A%' or ename like 'B%' )
        and comm is null
        and mgr in (7788, 7698) -- =이 아닌 in을 쓰면 가능함 -- 1개 in 여러개 
    
    --최종 정리
    select empno, ename, mgr, sal, comm
    from emp
    where empno between 7500 and 8000
        and  (ename like 'A%' or ename like 'B%' )
        and   comm is null
        and   mgr in (7788, 7698) -- =이 아닌 in을 쓰면 가능함 

## where 추가 기능 
    --empno 컬럼에 값이 있는 행 리턴 
    select * from emp where empno = empno;
    
    --comm 컬럼에 값이 있는 행 리턴 
    select * from emp where comm = comm;
###
    --말이 안되지만 에러 발생이 아닌
    --False인 조건이라 하나도 리턴이 되지 않는다.
    --항상 False인 조건으로 쿼리 
    select * from emp where 1 = 2;  
    
    
    --t2 테이블은 만들었지만
    --항상 False인 조건으로 쿼리한 결과를 이용해서 테이블로 생성 
    create table t2
    as
    select * from emp where 1 = 2;
###    
    --항상 True인 조건 
    --120>20 True 이니 emp table 데이터 다 나옴 
    select * from emp where 120 > 20;
    
    --항상 False인 조건 : 문자 데이터를 비교하는 방식 이기 때문에 
    --문자 120에서 1과 문자 20에서 2를 비교하니 false
    select * from emp where '120' > '20';
###    
    --데이터 타입 변환 함수 to_number('문자')
    select * 
    from emp 
    where to_number('120') > to_number('15');
###
    alter session set nls_language = 'american';
    alter session set nls_territory = 'america';
    
    select empno, ename, mgr, sal, comm
    from emp
    where empno between 7500 and 8000
        and (ename like 'A%' or ename like 'B%' )
        and comm is null
        and mgr in (7788, 7698) -- =이 아닌 in을 쓰면 가능함 
###    
    -- Month만 나오게(첫글자 대문자고 나머지는 소문자면 리턴도 똑같이
    select empno, hiredate, to_char(hiredate, 'Month')
    from emp;
###    
    -- 12월에 입사한 사람 리턴
    -- rtrim() 오른쪽 공백 지워
    -- 값 1개 리턴 됨
    select empno, ename, hiredate, job, sal, deptno
    from emp
    where rtrim(to_char(hiredate, 'Month')) = 'December'
        and deptno in (10, 30)
        and (job like '%C%' or job like '%K%')
        and sal > 500 and sal <= 2000;      -- between은 이상이하만 가능하므로 여기서 쓰면 안된다
    
    -- 13개 리턴
    select empno, ename, hiredate, job, sal, deptno
    from emp
    where not (rtrim(to_char(hiredate, 'Month')) = 'December'
        and deptno in (10, 30)
        and (job like '%C%' or job like '%K%')
        and sal > 500 and sal <= 2000);      -- between은 이상이하만 가능하므로 여기서 쓰면 안된다
    

## 2-12. LIKE 연산자를 사용하여 패턴 일치
    --emp table과
    --empno 컬럼과 
    --ename이 반드시 다섯글자인 ename 컬럼 리턴 
    select empno, ename
    from emp
    where ename like '_____';


## 2-13. Escape 옵션 예제 
    --휴지통에 넣는 느낌으로 삭제 
    drop table t1;
    
    --shift+del 느낌으로 삭제
    --기존에 있는 t1 table 삭제 
    drop table t1 purge;
    
    --숫자형식의 col1과 문자형태의 col2를 포함한 t1 table 만들기
    create table t1(col1 number, col2 varchar2(10));
    
    --1~4행 값 넣기 
    insert into t1 values (1000, 'AAA');
    insert into t1 values (2000, 'ABA');
    insert into t1 values (3000, 'ACA');
    insert into t1 values (4000, 'A_A');
    
    commit;
    
    select * from t1;
    
    
    --like 뒤에 나왔기 때문에 % _ 다 와일드카드이다.
    --그래서 A_A만 리턴 불가능.
    select * from t1 where col2 like '%A_A%';
    
    --escape '!'다음에 나오는 단어는 데이터야
    select * from t1 where col2 like '%A!_A%' escape '!';


## 2-20. 우선 순위 규칙 
    --상식적 수준으로 처리
    --산술(a+b) 연결(||) 비교(=) 순으로 처리 
    where a + b || c = d
    --단 and로 연결된 조건이 or로 연결된 조건에 비해 먼저 처리된다.











# order by절 이해
* order by

## name으로 정렬, as로 정렬, 위치값으로 정렬.
    --오름차순(ASC, Default)
    select empno, ename, sal as salary from emp order by sal;      --name
    select empno, ename, sal as salary from emp order by salary;   --as
    select empno, ename, sal as salary from emp order by 3;        --position

    --내림차순(DESC)
    select empno, ename, sal as salary from emp order by sal desc;      --name
    select empno, ename, sal as salary from emp order by salary desc;   --as
    select empno, ename, sal as salary from emp order by 3 desc;        --position

## 두개 이상의 조건으로 정렬
    --deptno 오름차순으로 정렬 
    select deptno, empno, sal from emp order by deptno;

    --deptno 오름차순으로 정렬 후 sal 내림차순 적용 
    select deptno, empno, sal from emp order by deptno, sal desc;

    --select 리스트에 없는 컬럼으로 정렬
    select empno, ename, job from emp order by sal desc;

    --ex) 게시판에 성적순으로 이름을 나열하지만 성적자체는 노출되지 않게 할때 사용 등.

## null 정렬
    --null 값은 가장 큰 값으로 취급 됨(오름차순)
    select empno, comm from emp order by comm asc;

    --null 값은 가장 큰 값으로 취급 됨(오름차순) 하지만 null값을 처음에 표시하게 끔 리턴.
    select empno, comm from emp order by comm asc nulls first;

    --null 값은 가장 큰 값으로 취급 됨(내림차순)
    select empno, comm from emp order by comm desc;

    --null 값은 가장 큰 값으로 취급 됨(내림차순) 하지만 null값을 마지막에 표시하게 끔 리턴.
    select empno, comm from emp order by comm desc nulls last;




## 가공한 결과에 의한 order by
* 문제. 사원이름이 짧은 사원 길이 구해서 리턴 
###
    select empno, ename, length(ename)
    from emp;

    select empno, ename
    from emp
    order by length(ename);
###
* 문제. 사원이름이 짧은 사원부터 ABC 정렬
###
    select empno, ename
    from emp
    order by length(ename), ename;

* 문제. 입사일자의 월(Month)를 활용한 정렬 
###
    select empno, hiredate, to_char(hiredate, 'mm')
    from emp;
    
    select empno, hiredate
    from emp
    order by to_char(hiredate, 'mm'), hiredate;

##

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>