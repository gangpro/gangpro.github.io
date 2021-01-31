---
layout: post
title: '[oracle] 오라클 PL/SQL intro'
subtitle: 
categories: til
tags: til oracle plsql
comments: true
date: 2019-04-16 00:12:17 +0900
lastmod: 2019-04-16 00:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 PL/SQL intro<br />

# intro
> 0장 PL/SQL 살펴보기

## Program?
* 해야 할 일을 미리 기술해 놓은 것.
* 인간의 언어
###
    인간의 언어
    
        ↓ translate(사람)
    
    프로그래밍 언어 : C, Java, SQL, PL/SQL, HTML, ... 
    
        ↓ translate(소프트웨어)

      기계어

## PL/SQL
* PLSQL = https://en.wikipedia.org/wiki/PL/SQL
* SQL =  https://en.wikipedia.org/wiki/SQL#Procedural_extensions
* Pascal -> Ada -> PL/SQL
* SQL(Manipulating power) + 3GL(Processing power) = PL/SQL
* SQL 엔진 + PL/SQL 엔진
* 할당 연산자 vs 비교 연산자
* function은 SQL에 포함시킬 수 있다는 점에서 procedure에 비해 유리한 점 있음

* 구조
###
    Block Structured Language -> Anonymous block
                              -> Named block      : procedure, function, package, trigger, object, ...
###
    --Anonymous block
    declare     --옵션
        선언부
    begin       --필수 키워드
        실행부
    exception   --옵션
        예외처리부
    end;        --필수 키워드
    /           --종결

###
    --Named block
    SOMETHING   --옵션
        선언부
    begin       --필수 키워드
        실행부
    exception   --옵션
        예외처리부
    end;        --필수 키워드
    /           --종결


## Anonymous Block 예제들
    --plsql을 사용하여 Hello World 출력
    set serveroutput on     --화면 출력 기능 툴
    
    begin
        dbms_output.put_line('Hello World!');   --오라클 서버에 요청해서 화면에 출력
    end;
    /
###
    --
    begin
        for i in 1..10 loop
            dbms_output.put_line(i||'번째, Hello World!');
        end loop;
    end;
    /
###
    --begin 안에 select문, v_sal 변수를 넣기 위해 declare에 v_sal 변수 선언
    declare
        v_sal number;
    begin
        select sal into v_sal
        from emp
        where empno = 7788;
        
        dbms_output.put_line(v_sal);
    end;
    /

## Named Block 예제들
    --plsql을 사용하여 Hello World 출력
    create or replace procedure p1      --dbms()....을 p1에 저장
    is
    begin
        dbms_output.put_line('Hello World!');
    end;
    /
###
    col object_name format a30
    
    select object_name, object_type
    from user_objects
    where object_type in ('PROCEDURE', 'FUNCTION', 'PACKAGE')
    order by 2, 1;
    
    col name format a30
    col text format a80
    
    select name, text
    from user_source;
    
    execute p1
###
    --p1
    create or replace procedure p1(a number)
    is
        v_sal number;
    begin
        select sal into v_sal
        from emp
        where empno = a;
        
        dbms_output.put_line(v_sal);
    end;
    /
    
    desc p1
    
    exec p1(7788)   --7788사원의 월급이 리턴 = 3000
    
    ###
    --p2
        create  or replace procedure p2(a number)
        is
        begin
          for i in 1..a loop
            dbms_output.put_line(i||'번째, Hello world!');
          end loop;
        end;
        /
    
        desc p2
    
        exec p2(100)
    ###
    --p3
    create or replace procedure p3(a number)
    is
        v_sal number;
    begin
        select sal into v_sal
        from emp
        where empno = a;
        
        dbms_output.put_line(v_sal);
    end;
    /
    
    exec p3(7788)
    exec p3(7900)


## 문제. 부서번호를 입력하면 평균급여를 리턴하는 프로시져를 생성할 것
    set serveroutput on
    select * from emp;
    
    create or replace procedure emp_avg_sal(a number)   --헤더부분(변수(매개변수 데이터타입))
    is
        v_avg_sal number;                               --선언부(변수명 데이터타입)
    begin
        select avg(sal) into v_avg_sal
        from emp
        where deptno = a;
        
        dbms_output.put_line(round(v_avg_sal, 2));
    end;
    /
    
    show errors
    
    exec emp_avg_sal(10)
    exec emp_avg_sal(30)
###
    --emp_avg_sal 프로시져에 out 매개변수를 사용할 경우
    --첫번째 문장
    set serveroutput on     --세션이 종료 된 후 다시 시작하면 무조건 On 해야함.
    
    create or replace procedure emp_avg_sal(a in number, b out number)      --a 들어오는 매개변수, b 나가는 매개변수 
    is
    begin
        select round(avg(sal), 2) into b
        from emp
        where deptno = a;
    end;
    /
    
    show errors
    
    --두번째 문장
    create or replace procedure emp_sal_compare(a number)
    is
        v_sal     emp.sal%type;
        v_deptno  emp.deptno%type;
        v_avg_sal number;
    begin
        select sal, deptno into v_sal, v_deptno
        from emp
        where empno = a;
        
        emp_avg_sal(v_deptno, v_avg_sal);
        
        if v_sal > v_avg_sal then
            dbms_output.put_line('소속부서 평균 급여보다 급여 큼');
        elsif v_sal < v_avg_sal then
            dbms_output.put_line('소속부서 평균 급여보다 급여 적음');
        else
            dbms_output.put_line('소속부서 평균 급여보다 급여 같음');
        end if;
    end;
    /
    
    --첫번째 문장과 두번째 문장을 활용
    exec emp_sal_compare(7788)
    exec emp_sal_compare(7900)
###
    --emp_avg_sal 프로시져를 함수로 변경할 경우
    drop procedure emp_avg_sal;

    create or replace function emp_avg_sal (p_deptno emp.deptno%type)
      return number
    is
      b number;
    begin                                       --함수는 begin과 end 안에 return이 필요 
      select round(avg(sal), 2) into b
      from emp
      where deptno = p_deptno;

      return b;
    end;
    /
###
    - function은 SQL에 포함시킬 수 있다는 점에서 
      procedure에 비해 유리한 점 있음
 
    select deptno, emp_avg_sal(deptno) avg_sal
    from dept;

    create or replace procedure emp_sal_compare(a number)
    is
      v_sal     emp.sal%type;
      v_deptno  emp.deptno%type;
    begin
      select sal, deptno into v_sal, v_deptno
      from emp
      where empno = a;

      if v_sal > emp_avg_sal(v_deptno) then
        dbms_output.put_line('소속 부서 평균 급여보다 급여 큼');
      elsif v_sal < emp_avg_sal(v_deptno) then
        dbms_output.put_line('소속 부서 평균 급여보다 급여 적음');
      else
        dbms_output.put_line('소속 부서 평균 급여보다 급여 같음');
      end if;
    end;
    /

    show errors

    exec emp_sal_compare(7788)
    exec emp_sal_compare(7900)


## 문제. 급여가 높은 사원의 사번을 출력하는 함수를 만드세요.
    --함수
    drop procedure emp_sal_compare;
    
    create or replace function emp_sal_compare(p_first_empno emp.empno%type, p_second_empno emp.empno%type) --매개변수 앞에는 일반적으로 p_NAME
        return emp.empno%type
    is
        v_first_sal emp.sal%type;
        v_second_sal emp.sal%type;
    begin
        select sal into v_first_sal 
          from emp 
         where empno = p_first_empno;
         
        select sal into v_second_sal 
          from emp 
         where empno = p_second_empno;
        
        if v_first_sal > v_second_sal then
            return p_first_empno;
            
        elsif v_first_sal < v_second_sal then
            return p_second_empno;
            
        else
            return 0;
        end if;    
    end;
    /
    
    show error



    --함수 테스트용 조인 문장
    select w.empno, e.empno
      from emp w, emp e
     where w.empno = 7788 and e.empno != w.empno;
    
    select w.empno, e.empno, emp_sal_compare(w.empno, e.empno) as winner
      from emp w, emp e
     where w.empno = 7788 and e.empno != w.empno;
###
    --cf. 함수없이 그냥 SQL로 해결하면 이렇습니다.

    select w.empno, e.empno, case when w.sal > e.sal then w.empno
                                  when w.sal < e.sal then e.empno
                                  else                        0 
                              end as winner               
      from emp w, emp e
     where w.empno = 7788
       and e.empno != w.empno;

## 할당 연산자 vs 비교 연산자
              C, Java     Basic, PowerScript      Pascal, PL/SQL
    할당 연산자  A = B             A = B                 A := B
    비교 연산자  A == B            A = B                 A = B

## DML을 PL/SQL 프로그램 Unit을 이용해서 구현
    drop table t1 purge;
    
    create table t1
    as
    select empno, ename, sal, job
    from emp
    where 1 = 2;
    
    create or replace procedure t1_insert_proc(a number, b varchar2, c number, d varchar2)
    is
    begin
        if c <= 1000 then
            dbms_output.put_line('입력 실패! 급여를 확인하세요');
        else
            insert into t1
            values(a, b, c, d);
        end if;    
    end;
    /
    
    show errors     --오타 발생시 이유를 알 수 있음
    
    exec t1_insert_proc(1000, 'Tom', 200, 'MANAGER');


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>