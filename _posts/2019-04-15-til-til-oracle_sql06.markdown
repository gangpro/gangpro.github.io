---
layout: post
title: '[oracle] 오라클 SQL - 조인(Join)'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 06:12:17 +0900
lastmod: 2019-04-15 06:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL 조인(Join)<br />

# 6장. 조인(Join)
> 6장 : join<br>

## 조인(Join) 용어
    select *
    from emp, dept              -- 조인, Cartesian product, row 복제
    order by 1;
    
    select e.empno, e.name, d.*
    from emp e, dept d          --Join statement
    where e.deptno = d.deptno   --Join predicate
    and e.sal >= 1000           --Non-Join predicate
    and e.job like 'A%'         --Non-Join predicate
    and d.deptno = 20           --Non-Join predicate(Single row predicate)

## 조인(Join) 분류
* Oracle Syntax
  - Equi join : 등가 조인, 내부 조인 : 조인을 하는 기준이 같을 때
  - Nonequi join : 비등가 조인 : 조인을 하는 기준이 다를 때
  - Self join : 외부 조인 : 조건에 맞는 것 및 추가로 더 나오게 함
  - Outer join : 자체 조인 : from절에 같은 이름의 table이 두번 나오게
  
* SQL : 1999 표준 Syntax
  - Cross join : Cartesian product 생성
  - Natural join : 같은 이름의 컬럼을 모두 이용해서 Equi join
  - Join Using : 원하는 컬럼을 이용해서 Equi join
  - Join On
  - Outer join
  
* SQL 표준 Syntax ⊃ Oracle Syntax
  - Join On ⊃ Equi join, Nonequi join, Self join, Outer join




## Join
* Join을 잘하려면 from 처리를 잘해야한다.(정확한 값을 from에 넣어야 한다.)
* from 절에 테이블이 두 개 이상!
###
    --emp 테이블과 dept 테이블 조인
    
    --14행 emp 테이블 리턴
    select *
    from emp;
    
    --4행 dept 테이블 리턴
    select *
    from dept;
    
    --조인 처리된 54행(emp(14행) * dept(4행)) emp, dept 테이블 리턴
    select *
    from emp, dept --조인, Cartesian product, row 복제
    order by 1;
    
    --emp.deptno = dept.deptno이 True인 값만 리턴
    select *
    from emp, dept
    where emp.deptno = dept.deptno
    order by 1;

# 데이터의 상황을 확인하라 ≒ 냉장고를 살펴라
* 참고 : https://docs.oracle.com/cd/E18283_01/server.112/e10831/diagrams.htm#CIHGFFHI
###
    -- 모든 쿼리문 찾기
    select 'select * from '||tname||';' as 질의문
    from tab;

## Outer join option
    Oracle Syntax   
    --Kimberely (+) 
    --직원명이 다 나오고 부서는 null
    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e, departments d
    where e.department_id = d.department_id(+)
    order by 1;
    
    --190 Countracting(+)
    --부서는 다 나오고 직원명 null
    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e, departments d
    where e.department_id (+) = d.department_id
    order by 1;
 
    --왼쪽 오른쪽 다 나오게
    --직원 null, 부서 null 다 나오게
    --full outer join   
    --오라클 표준은 지원하지 않는다. 참고로 안씨 표준은 지원.
    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e, departments d
    where e.department_id (+) = d.department_id (+)
    order by 1;
    

### 
    SQL:1999 표준 Syntax    
    
    --Outer join option
    --Kimberely (+) 
    --직원명이 다 나오고 부서는 null
    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e LEFT OUTER JOIN departments d 
                     ON (e.department_id = d.department_id)
    order by 1;
    
    --190 Countracting(+)
    --부서는 다 나오고 직원명 null
    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e RIGHT OUTER JOIN departments d
                     ON (e.department_id = d.department_id)
    order by 1;
    
    --왼쪽 오른쪽 다 나오게
    --직원 null, 부서 null 다 나오게
    --full outer join
    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e FULL OUTER JOIN departments d
                     ON (e.department_id = d.department_id)
    order by 1;
###
 
    Oracle Syntax Outer Join 한걸음 더 살펴보기

    select e.empno, e.deptno, e.sal, d.deptno, d.dname
    from emp e, dept d
    where e.deptno(+) = d.deptno;

    select e.empno, e.deptno, e.sal, d.deptno, d.dname
    from emp e, dept d
    where e.deptno(+) = d.deptno
    and   e.sal >= 1500;           <- 엉터리

    select e.empno, e.deptno, e.sal, d.deptno, d.dname
    from emp e, dept d
    where e.deptno(+) = d.deptno
    and   e.sal(+) >= 1500;           <- 제대로
    
## Oracle Syntax 문제. 부서와 부서에 속한 사원들의 ...를 쿼리하세요.
    select * from departments;  -- 8 rows
    select * from employees;    --20 rows
    
    select *
    from employees, departments
    order by 1;                 --160 rows
    
    select *
    from employees, departments
    where department_id = department_id and SALARY >= 3000      --에러 발생
    order by 1;
    
    select *
    from employees, departments
    where employees.department_id = departments.department_id and employees.SALARY >= 3000 --가능 
    order by 1;
    
    select *
    from employees e, departments d
    where e.department_id = d.department_id and e.SALARY >= 3000 --가능 
    order by 1;
    
    select *                            -- equi join
    from employees e, departments d
    where e.department_id = d.department_id
    order by 1;
    
    select *
    from employees e, departments d
    where e.DEPARTMENT_ID = d.DEPARTMENT_ID and e.SALARY >= 3000 --가능 
    order by 1;

###
    --Outer join
    select
       e.employee_id, e.department_id, d.department_id
    from employees e, departments d 
    where e.department_id = d.department_id (+)
    order by 1;
    
    select
           e.employee_id, e.department_id, d.department_id
    from employees e, departments d 
    where e.department_id (+) = d.department_id
    order by 1;

###
    --추가 가공
    select *
      from employees e, departments d
     where e.DEPARTMENT_ID = d.DEPARTMENT_ID
       and e.salary >= 3000
     order by 1;
    
    select d.department_id,
           d.department_name,
           e.employee_id,
           e.job_id,
           e.salary
     from employees e, departments d
    where e.department_id = d.department_id
      and e.salary >= 3000
    order by 1;
     
    -- 
    select d.department_name,
           e.job_id,
           count(*) 인원수,
           sum(salary) 급여합,
           round(stddev(salary)) 급여표준편차
     from employees e, departments d
    where e.department_id = d.department_id
      and e.salary >= 3000
    group by d.department_name, e.job_id
    order by 1;





## Oracle Syntax 문제. 사원과 사원들의 급여 등급을 쿼리하세요.
    select * from salgrade;   --  5 rows
    select * from emp;        -- 14 rows
    
    select *
    from emp e, salgrade s
    order by 1;
    
    select *
    from emp e, salgrade s
    where e.sal >= s.losal and e.sal <= s.hisal   
    --where sal between losal and hisal 가능
    order by 1;
    
    
    --A          --non-equi join
    select s.grade, e.empno, e.job, e.sal
    from emp e, salgrade s
    where e.sal >= s.losal and e.sal <= s.hisal   
    order by s.grade, e.sal desc;
    
    --B
    select s.grade, 
           count(*) 인원수, 
           round(avg(e.sal)) 평균급여, 
           round(stddev(e.sal)) 급여표준편차
    from emp e, salgrade s
    where e.sal >= s.losal and e.sal <= s.hisal  
    group by s.grade
    order by s.grade;
    
    
    --A+B
    select a.grade, a.empno, a.job, a.sal, b.인원수, b.평균급여, b.급여표준편차
    from (select s.grade, e.empno, e.job, e.sal
            from emp e, salgrade s
           where e.sal >= s.losal and e.sal <= s.hisal   
           order by s.grade, e.sal desc) a,
         (select s.grade, 
                 count(*) 인원수, 
                 round(avg(e.sal)) 평균급여, 
                 round(stddev(e.sal)) 급여표준편차
            from emp e, salgrade s
           where e.sal >= s.losal and e.sal <= s.hisal  
           group by s.grade
           order by s.grade) b
    where a.grade = b.grade
    order by a.grade, a.sal desc;



## Oracle Syntax 문제. 7844 사원보다 많은 급여를 받는 사원?
    select *
    from emp e, emp t;  --같은 이름의 테이블이 2개 나오면 Self Join
    
    select *
    from emp e, emp t;  --14 * 14 = 196
    
    select *
    from emp e, emp t   --14 * 1 = 14
    where t.empno = 7844;
    
    select e.empno, e.sal, t.empno, t.sal
    from emp e, emp t          --Self Join
    where t.empno = 7844
    and e.sal > t.sal;  -- 7   --Non-Equi Join
    --위의 SQL은 Self Join과 Non-Equi Join 무엇일까??
    --결론은 Join은 이렇게 접근하는게 아니다.
    --문제 보고 필요한 Join을 넣어야 한다.
    --그럼 분류는 무슨 의미?가 아니라, 분류에 구애받지 않고 처리하면 된다.
    --그러면 이것저것 쓰게 된다.
    --join을 분류하는건 의미가 없다.



## Oracle Syntax 문제. 누적합 구하기
    ----A   ----B   ---누적합 
    7369	800      800
    7499	1600    2400
    7521	1250    3650
    
    drop table t1 purge;
    
    create table t1
    as
    select empno a, sal b 
    from emp 
    where rownum <= 3;
    
    select *
    from t1 a, t1 b;    --9 rows
    
    select *
    from t1 a, t1 b
    where a.a >= b.a
    order by a.a;       --6 rows
    
    select a.a, a.b, sum(b.b) 누적합
    from t1 a, t1 b     --self join
    where a.a >= b.a    --non-equi join
    group by a.a, a.b
    order by a.a        --3 rows / 정답 



## SQL:1999 표준 Syntax 문제들
## SQL:1999 표준 Syntax 문제. Cross join 예제를 만드세요
     select *
     from emp CROSS JOIN dept;

     위 쿼리를 오라클 방식을 재작성하면 아래와 같습니다

     select *
     from emp, dept;

## SQL:1999 표준 Syntax 문제.Natural join 예제를 만드세요

     desc dept
     desc emp

     select *
     from emp e NATURAL JOIN dept d;

     위 쿼리를 오라클 방식을 재작성하면 아래와 같습니다

     select *
     from emp e, dept d
     where e.deptno = d.deptno;

        -----
    
     desc departments  
     desc employees 

     select *
     from employees e NATURAL JOIN departments d;

     위 쿼리를 오라클 방식을 재작성하면 아래와 같습니다

     select *
     from employees e, departments d
     where e.manager_id = d.manager_id
     and   e.department_id = d.department_id;

     이와같이 Natural Join은 같은 이름의 컬럼을 모두 Equi join 조건으로
     활용하므로 같은 이름의 컬럼이 많은 테이블간 연결 및 Non-equi 조인 등에는
     활용할 수 없습니다.
      
## SQL:1999 표준 Syntax 문제.Join Using 예제를 만드세요.

     select *
     from employees e JOIN departments d
                      USING (department_id);

     Join Using은 Equi join만 가능합니다.

## SQL:1999 표준 Syntax 문제.부서와 부서에 속한 사원들의 ...를 쿼리하세요

    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e JOIN departments d
                     ON (e.department_id = d.department_id);

    --> Natural join으로 구현 불가
    --> Join Using으로   구현 가능

    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e JOIN departments d
                     ON (e.department_id = d.department_id)
    where e.salary >= 10000
    and   e.job_id like 'SA%';

      ----

    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e LEFT JOIN departments d
                     ON (e.department_id = d.department_id);

    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e RIGHT JOIN departments d
                     ON (e.department_id = d.department_id);

    select e.employee_id, e.department_id, d.department_id, d.department_name
    from employees e FULL JOIN departments d
                     ON (e.department_id = d.department_id);

## SQL:1999 표준 Syntax 문제.사원과 사원들의 급여 등급을 쿼리하세요

    select s.*, e.empno, e.sal
    from emp e JOIN salgrade s
               ON (e.sal >= s.losal and e.sal <= s.hisal);

    --> Natural join으로 구현 불가
    --> Join Using으로   구현 불가

## SQL:1999 표준 Syntax 문제.7844 사원보다 많은 급여를 받는 사원?
    
    select e.empno, e.sal, t.empno, t.sal
    from emp e JOIN emp t
               ON (e.sal > t.sal);

    --> Natural join으로 구현 불가
    --> Join Using으로   구현 불가

## SQL:1999 표준 Syntax 문제.누적합 구하기 

    select a.a, a.b, sum(b.b)
    from t1 a JOIN t1 b
              ON (a.a >= b.a)
    group by a.a, a.b
    order by a.a;

    --> Natural join으로 구현 불가
    --> Join Using으로   구현 불가



## Cartesian product는 항상 나쁘다? 
    select deptno, job, sum(sal) as sum_sal
    from emp
    group by deptno, job;
    
    select level no 
    from dual
    connect by level <= 3;
    
    select *
    from (select deptno, job, sum(sal) as sum_sal
        from emp
        group by deptno, job) a,
       (select level no 
        from dual
        connect by level <= 3) b
    order by b.no;
    
    select decode(b.no, 1, deptno, 2, deptno) deptno, job, sum_sal, no
    from (select deptno, job, sum(sal) as sum_sal
        from emp
        group by deptno, job) a,
       (select level no 
        from dual
        connect by level <= 3) b
    order by b.no;
    
    select decode(b.no, 1, deptno, 2, deptno) deptno, 
         decode(b.no, 1, job) job,
         sum_sal,
         no
    from (select deptno, job, sum(sal) as sum_sal
        from emp
        group by deptno, job) a,
       (select level no 
        from dual
        connect by level <= 3) b
    order by b.no, a.deptno;
    
    select decode(b.no, 1, deptno, 2, deptno) deptno, 
         decode(b.no, 1, job) job,
         sum(sum_sal)
    from (select deptno, job, sum(sal) as sum_sal
        from emp
        group by deptno, job) a,
       (select level no 
        from dual
        connect by level <= 3) b
    group by decode(b.no, 1, deptno, 2, deptno), 
           decode(b.no, 1, job)
    order by 1, 2;
    
    select decode(b.no, 1, deptno, 2, deptno) deptno, 
         decode(b.no, 1, job) job,
         sum(sum_sal)
    from (select deptno, job, sum(sal) as sum_sal
        from emp
        group by deptno, job) a
        CROSS JOIN
       (select level no 
        from dual
        connect by level <= 3) b
    group by decode(b.no, 1, deptno, 2, deptno), 
           decode(b.no, 1, job)
    order by 1, 2;



## Join 테스트 문제
       REGION_ID REGION_NAME                  사원수
      ---------- ------------------------- ---------
               1 Europe                            3
               2 Americas                         17 
               3 Asia                              0
               4 Middle East and Africa            0
    
      -> 부서가 없는 사원은 Americas로 포함시킬 것
    
      해답 1. SQL:1999 표준 Syntax 활용

    select r.region_id, r.region_name, employee_id
    from employees e JOIN departments d ON (nvl(e.department_id, 10) = d.department_id)
                     JOIN locations   l ON (d.location_id = l.location_id)
                     JOIN countries   c ON (l.country_id = c.country_id)
                     JOIN regions     r ON (c.region_id = r.region_id)
    order by r.region_id, employee_id;

    select r.region_id, r.region_name, employee_id
    from employees e JOIN departments d ON (nvl(e.department_id, 10) = d.department_id)
                     JOIN locations   l ON (d.location_id = l.location_id)
                     JOIN countries   c ON (l.country_id = c.country_id)
                     RIGHT JOIN regions     r ON (c.region_id = r.region_id)
    order by r.region_id, employee_id;

    select r.region_id, r.region_name, count(employee_id)
    from employees e JOIN departments d ON (nvl(e.department_id, 10) = d.department_id)
                     JOIN locations   l ON (d.location_id = l.location_id)
                     JOIN countries   c ON (l.country_id = c.country_id)
                     RIGHT JOIN regions     r ON (c.region_id = r.region_id)
    group by r.region_id, r.region_name
    order by r.region_id, r.region_name;

      해답 2. Oracle Syntax 활용

    select r.region_id, r.region_name, count(employee_id)
      from employees   e,
           departments d,
           locations   l,
           countries   c,
           regions     r
    where nvl(e.department_id (+), 10) = d.department_id
      and d.location_id(+) = l.location_id
      and l.country_id(+) = c.country_id
      and c.region_id (+) = r.region_id
    group by r.region_id, r.region_name
    order by r.region_id, r.region_name;

       ---

    select e.employee_id, c.region_id
      from employees   e,
           departments d,
           locations   l,
           countries   c
    where nvl(e.department_id , 10) = d.department_id
      and d.location_id = l.location_id
      and l.country_id = c.country_id;

    select *
    from (select e.employee_id, c.region_id
            from employees   e,
                 departments d,
                 locations   l,
                 countries   c
          where nvl(e.department_id , 10) = d.department_id
            and d.location_id = l.location_id
            and l.country_id = c.country_id) e, regions r
     where e.region_id(+) = r.region_id;

    select r.region_id, r.region_name, count(employee_id)
    from (select e.employee_id, c.region_id
            from employees   e,
                 departments d,
                 locations   l,
                 countries   c
          where nvl(e.department_id , 10) = d.department_id
            and d.location_id = l.location_id
            and l.country_id = c.country_id) e, regions r
     where e.region_id(+) = r.region_id
    group by r.region_id, r.region_name
    order by r.region_id, r.region_name;


## 데이터 연결 4가지 방법
* 단어 의미
  - 파티셔닝 : 데이터를 나누는 것
  - 수평 파티션(샤딩)
  - 수직 파티션

* 데이터 준비 

###
    drop table t1 purge;
    drop table t2 purge;
    
    --연결 고리가 있는 상태에서 나눠야 한다. 예를 들면 empno를 기준으로 잡아서.
    create table t1
    as
    select empno, ename
    from emp
    where empno <= 7788;
    
    create table t2
    as
    select empno, job
    from emp
    where empno <= 7788;

[1] 조인(곱셈연산)

    select * 
    from t1, t2;
    
    select * 
    from t1, t2
    where t1.empno = t2.empno;
    
    select t1.empno, t1.ename, t2.job
    from t1, t2
    where t1.empno = t2.empno;

[2] 서브쿼리

    --t1에 가서
    --     empno, ename,  t1.empno의 t2.job 리턴
    select empno, ename, (select job from t2 where empno = t1.empno) as job
    from t1;

[3] SET 연산자(덧셈연산)

    select empno, ename, null as job
    from t1
    union all
    select empno, null, job
    from t2
    order by 1, 2;
    
    select empno, max(ename), max(job)
    from (select empno, ename, null as job
          from t1
          union all
          select empno, null, job
          from t2
          order by 1, 2)
    group by empno
    order by empno;

[4] 사용자 정의 함수(plsql을 사용하여 함수 생성)

    --user function(uf) uf_get_t2_job 변수명( t1.empno의 타입을 a에 넣는다)
    --숫자가 들어와서 문자로 리턴
    --begin : t2에서 empno가 a인것을 job에서 찾아서 v_job에 넣고 
    --is : v_job t2.job%type에 넣고
    --return : v_job;으로 리턴
    create or replace function uf_get_t2_job(a t1.empno%type)
        return varchar2
    is
        v_job t2.job%type;
    begin
        select job into v_job
        from t2
        where empno = a;
        
        return v_job;
    end;
    /
    
    select empno, ename, uf_get_t2_job(empno) as job
    from t1;

## 읽어볼 것들 
* [Python] Pandas를 활용한 데이터프레임 병합
  - https://datascienceschool.net/view-notebook/7002e92653434bc88c8c026c3449d27b/

* [R] R 데이터 프레임 결합 : rbind(), cbind(), merge()
  - https://rfriend.tistory.com/51


 




<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>