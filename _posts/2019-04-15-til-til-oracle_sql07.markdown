---
layout: post
title: '[oracle] 오라클 SQL - 서브쿼리'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 07:12:17 +0900
lastmod: 2019-04-15 07:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL 서브쿼리<br />

# 7장. 서브쿼리
> 7장 : 서브쿼리란? <br>
> 다른 SQL에 포함된 SELECT!

## 서브쿼리 분류
* Single colunmn Single row subquery : >, >=, <, <=, = ... (왼쪽, 오른쪽 값이 1개일 때)
* Single colunmn Multiple row subquery : in(왼쪽 값 1개, 오른쪽 값 여러개)
* Multiple column Multiple row subquery : (pair-wise subquery)
* 
* Inline view : from절의 select
* 
* Correlated subquery : 상호관련 서브쿼리(내부 테이블에 외부 컬럼이 들어와있는 경우)


## 문제
* 7782 사원보다 급여를 많이 받으면서 7902 사원과 같은 직무를 수행하는 사원?
```
    --하나 하나 찾아서 처리하는 방법
    select sal
      from emp
     where empno = 7782;     --2450
    
    select job
      from emp
     where empno = 7902;     --ANALYST
    
    select empno, ename, job, sal
      from emp
     where sal > 2450
       and job = 'ANALYST';
```

```
    --조인으로 처리하는 방법
    select e.empno, e.ename, e.job, e.sal
      from emp e, emp a, emp b
     where a.empno = 7782
       and b.empno = 7902
       and b.sal > a.sal
       and e.job = b.job;
```

```
    --서브쿼리로 처리하는 방법
    --하나하나 처리하는 것에서 
    --A
    select sal
      from emp
     where empno = 7782;
    --B
    select job
      from emp
     where empno = 7902;
    --A+B
    select empno, ename, job, sal
      from emp
     where sal > (A)
       and job = (B);

    --정리하면 이렇게
    select empno, ename, job, sal
      from emp
     where sal > (select sal
                    from emp
     where empno = 7782)
       and job = (select job
                    from emp
                   where empno = 7902);
```

## [참고] Query Transformation 기능을 갖고 있다.
* where ename like 'SCOTT'             --> like의 의미가 무의미하기 때문에 오라클에서는 where ename = 'SCOTT' 으로 자동 변경
* where sal between 1000 and 2000      --> where sal >= 1000 and sal <= 2000
* where job in ('ANALYST', 'MANAGER')  --> where job = 'ANALYST' OR 'MANAGER'
* 그 밖에 서브쿼리를 날리면 조인으로 자동 처리된다. 등등등~

## 문제
* 회사의 평균 급여보다 많은 급여를 받으면서 7788 사원보다 빨리 입사한 사원을 쿼리하세요. 단 서브쿼리 문법을 활용하도록 하세요.
```
    --정답
    select empno, sal, hiredate
      from emp
     where sal > (select avg(sal) from emp)
       and hiredate < (select hiredate from emp where empno = 7788);

    -- +@ 인원수 구하기 
    select count(*)
      from emp
     where sal > (select avg(sal) from emp)
       and hiredate < (select hiredate from emp where empno = 7788);

    -- +@ 쭉쭉 추가할 수 있다.
    select deptno, job, count(*)
      from emp
     where sal > (select avg(sal) from emp)
       and hiredate < (select hiredate from emp where empno = 7788)
     group by deptno, job
     order by deptno, job;
```

## 문제
* 최저 급여자의 ename, sal을 쿼리하세요.
```
    --정답
    select empno, ename, sal
      from emp
     where sal = (select min(sal) from emp);
```

## 문제 
* EMPNO, ENAME, SAL, "회사평균급여"를 쿼리하세요. 단, 서브쿼리 문법을 활용하도록 하세요.
* (Scalar subquery 사용법을 눈여겨 볼 것)
```
    select empno, ename, sal, (select avg(sal) from emp) 평균급여
      from emp;
```

## 문제
* EMPNO, ENAME, SAL, "소속부서평균급여"를 쿼리하세요. 단, 서브쿼리 문법을 활용하도록 하세요.
* (Scalar subquery + 상호관련 서브쿼리 사용법을 눈여겨 볼 것)
```
    select empno, ename, sal, deptno, (select avg(sal) 
                                         from emp
                                        where deptno = e1.deptno) 소속부서평균급여
      from emp e1;
```

## 문제
* EMPNO, ENAME, SAL, "소속부서평균급여"를 쿼리하세요. 단, 서브쿼리 문법을 활용하도록 하세요.
* (Scalar subquery 사용법을 눈여겨 볼 것)
```
    select empno, ename, deptno
      from emp e1
     order by (select loc
                 from dept
                where deptno = e1.deptno);
    
    이 문제를 조인으로 해결하면 아래와 같습니다.
    
    select e.empno, e.ename, e.deptno
      from emp e, dept d
     where e.deptno = d.deptno
     order by d.loc;
```

## 문제
* 근무하는 사원이 있는 부서의 deptno, dname, loc를 쿼리하세요.
* (하나의 문제를 다양한 방법으로 해결하는 것을 전하는 예제임. 그런데 특히 상호관련 서브쿼리, Exists 연산자 사용법을 눈여겨 볼 것)
```
    --에러
    --= 왼쪽 값 1개 오른쪽 값 1개 그래서 에러
    select deptno, dname, loc
      from dept
     where deptno = (select distinct deptno from emp);  --서브쿼리 안에서는 distinct를 안 써도 중복값을 제거하고 리턴한다.
    
    --정답(일반 쿼리)
    --in 왼쪽 값 1개 오른쪽 값 여러개
    select deptno, dname, loc
      from dept
     where deptno in (select distinct deptno from emp);  --서브쿼리 안에서는 distinct를 안 써도 중복값을 제거하고 리턴한다.
    
    --정답(상호 관련 서브쿼리 처리)
    --근무하는 사원이 있는 부서를 상호 관련 서브쿼리로 해결
    select deptno, dname, loc
      from dept d
     where 0 < (select count(*) from emp where deptno = d.deptno);
    --full scan
    --수만명 중에 수천명을 찾아서 구하는 방식
    --한번이라도 팔린적 있는 물건
    
    --+@ exists 사용(상호관련 서브쿼리+Exists)
    select deptno, dname, loc
      from dept d
     where exists (select 'x' from emp where deptno = d.deptno);   --관습적으로 'x'   or    1을 넣는다.
    --dept scan, emp 부분 scan.
    --존재 유무를 파악할 때 exists를 사용하는게 좋다.
```

## 문제
* 부하직원이 있는 사원을 쿼리하세요.
* (Exists 연산자 사용법을 눈여겨 볼 것)
```
    --내 사원번호가 MGR에 있어야 = 부하직원이 있는 
    select empno, ename, job, sal
      from emp
     where empno in (select mgr from emp);
    
    select empno, ename, job, sal
      from emp e1
     where exists (select 'x' from emp where mgr = e1.empno);
```

* 5명 이상 근무하는 부서를 상호 관련 서브쿼리로 해결하세요.
* (상호관련 서브쿼리 사용법을 눈여겨 볼 것)
```
    select deptno, dname, loc
      from dept d
     where 5 <= (select count(*) from emp where deptno = d.deptno);
```

* 부하 직원이 3명 이상인 사원을 쿼리하세요.
* (상호관련 서브쿼리 사용법을 눈여겨 볼 것)
```
    select empno, ename, job, sal
      from emp e1
     where 3 <= (select count(*) from emp where mgr = e1.empno);
```

## 문제
* empno, ename, sal, 급여의 누적합을 쿼리 하세요. 단 서브 쿼리 문법을 활용하도록 하세요.
* (상호관련 서브쿼리 사용법을 눈여겨 볼 것)
```
    --정답
    select empno, ename, sal, (select sum(sal) from emp where empno <= e1.empno) as 누적합
      from emp e1;
```

## 문제
* empno, ename, hiredate, "먼저입사한사원수"를 쿼리 하세요. 단 서브 쿼리 문법을 활용하도록 하세요.
* (상호관련 서브쿼리 사용법을 눈여겨 볼 것)
```
    --정답
    select empno, ename, hiredate, (select count(hiredate) from emp where hiredate < e1.hiredate) as 먼저입사한사원수
      from emp e1
     order by hiredate
```

## 문제
* Single coulumn Multiple row subquery와 부등호 연산을 이해하도록 하세요.
```
    drop table t1 purge;
    drop table t2 purge;
    
    create table t1 (col1 number);
    
    insert into t1 values (1000);
    insert into t1 values (2000);
    insert into t1 values (3000);
    
    commit;
    
    create table t2 (col1 number);
    
    insert into t2 values (1500);
    insert into t2 values (2500);
    
    commit;
    
    select * from t1;
    select * from t2;
    
    --부등호 연산자는 1:1 관계를 비교함 그래서 t1(3행), t2(2행)이므로 오류가 발생 
    select * from t1
    where col1 > (select col1 from t2);
    
    --subquary에 any를 쓰면 비교 가능
    select * from t1
    where col1 > any (select col1 from t2);
    
    --크다 any, 서브쿼리에서 값을 여러개 리턴할때 서브쿼리에 MIN으로 비교해서 리턴하면 된다.
    select * from t1
    where col1 > (select min(col1) from t2);
    
    --작다 any, 서브쿼리에서 값을 여러개 리턴할때 서브쿼리에 MAX으로 비교해서 리턴하면 된다.
    select * from t1
    where col1 < (select min(col1) from t2);
    
    --subquary에 all을 쓰면 비교 가능
    select * from t1
    where col1 > all (select col1 from t2);
    
    --모두보다 크다, 서브쿼리에서 값을 여러개 리턴할때 서브쿼리에 MAX으로 비교해서 리턴하면 된다.
    select * from t1
    where col1 > (select max(col1) from t2);
    
    --모두보다 작다, 서브쿼리에서 값을 여러개 리턴할때 서브쿼리에 MIN으로 비교해서 리턴하면 된다.
    select * from t1
    where col1 < (select min(col1) from t2);
```

## 문제
* Multiple coulumn Multiple row subquery 예제를 이해하도록 하세요.
```
    drop table t1 purge;
    drop table t2 purge;

    create table t1 (col1 number, col2 varchar2(10));

    insert into t1 values (100, 'A');
    insert into t1 values (100, 'B');
    insert into t1 values (200, 'A');
    insert into t1 values (200, 'B');

    create table t2 (col1 number, col2 varchar2(10));

    insert into t2 values (100, 'A');
    insert into t2 values (200, 'B');

    commit;

    select * from t1;
    select * from t2;
 
    -> non-pair-wise subquery

       select * 
         from t1
        where col1 in (select col1 from t2)  /* 100, 200 */
          and col2 in (select col2 from t2); /* A, B */

    -> pair-wise subquery
 
       select * 
         from t1
        where (col1, col2) in (select col1, col2 from t2);    /* (100, A), (200, B) */
```

## 문제
* Subquery Factoring을 이해하도록 합시다.
```
    --본문을 시작하기 전에 미리 query를 만들어서 시작.
    --문장을 단순하게 만든다.
    --서브쿼리 결과를 임시집합으로 생성해서 성능을 개선함.

      with a as (select deptno, avg(sal) dept_avgsal from emp group by deptno), --부서별 평균급여  
           b as (select avg(sal) emp_avg_sal from emp)                          --급여별 평균급여
    select e.empno, e.ename, e.sal, a.dept_avgsal, b.emp_avg_sal
      from emp e, a, b
     where e.deptno = a.deptno
       and a.dept_avgsal = (select max(dept_avgsal) from a);
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>