---
layout: post
title: '[oracle] 오라클 PL/SQL - 예외 처리'
subtitle: 
categories: til
tags: til oracle plsql
comments: true
date: 2019-04-16 08:12:17 +0900
lastmod: 2019-04-16 08:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 PL/SQL - 예외 처리<br />

# 8장. 예외 처리
> 예외 처리

## Error
      - Syntax error
      - Logic  error
      - Runtime error -> Oracle-defined Exception -> Predefined exception      -> [1] when 이름 then
        (Exception)                               -> Non-predefined exception  -> [2] 선언 -> 이름부여 -> 자동 발생 -> 처리
                                                                                  [3] when others then
                      -> User_defined Exception                                -> [4] 선언 -> raise로 수동 발생 -> 처리
                                                                                  [5] raise_application_error 프로시져      
     
      - Every Oracle error has a number, but exceptions must be handled by name.

     - An exception raised in a declaration        propagates immediately to the enclosing block.
       An exception raised in an exception handler propagates immediately to the enclosing block.

     - https://scidb.tistory.com/entry/PLSQL-면접문제


## Exception 처리 예제들

[1] when 이름 then

  (0) Predefined exception의 정체를 확인해 보세요
  
      1.파일을 여세요

        C:\Users\student> notepad c:\oraclexe\app\oracle\product\11.2.0\server\rdbms\admin\stdspec.sql
  
      2.다음으로 시작되는 파트를 찾으세요

        /********** Predefined exceptions **********/

  (1) 발생한 Exception을 제대로 처리하지 못하는 경우
  
      drop table t1 purge;
      create table t1 (no number);
    
      create or replace procedure p1(a number, b number)
      is
      begin
        insert into t1 values(1000);
        p.p(a/b);
      end;
      /
    
      exec p1(100, 2)
    
      select * from t1;    -> 입력된 1000이 확인됨
    
      rollback;
    
      exec p1(100, 0)
    
      select * from t1;    -> no rows selected
  
  (2) 발생한 Exception을 제대로 처리하는 경우

      create or replace procedure p1(a number, b number)
      is
      begin
        insert into t1 values(1000);
        p.p(a/b);
      exception
        when zero_divide then
          p.p('0으로 나눌 수 없습니다!');
        when others then
          p.p('에러 발생!');
      end;
      /
    
      exec p1(100, 0)
    
      select * from t1;  -> exception 발생 이전의 입력 데이터가 그대로 남아있음
    
      rollback;

[2] 선언 -> 이름부여 -> 자동 발생 -> 처리

      drop table t1 purge;
    
      create table t1 (no number not null);
    
      create or replace procedure p1(a number)
      is
        e_null exception;
        pragma exception_init(e_null, -1400);
      begin
        insert into t1 values(a);
      exception
        when e_null then
          p.p('null값 입력할 수 없습니다!');
      end;
      /
    
      exec p1(null)

### 

      create or replace package exception_pack
      is
        e_null exception;
        pragma exception_init(e_null, -1400);
      end;
      /
    
      create or replace procedure p1(a number)
      is
      begin
        insert into t1 values(a);
      exception
        when exception_pack.e_null then
          p.p('null값 입력할 수 없습니다!');
      end;
      /
    
      exec p1(null)

[3] when others then

      create or replace procedure p1(a number)
      is
      begin
        p.p('다른 문장이 많음');
        insert into t1 values(a);
      exception
        when others then
          p.p(sqlcode);
          p.p(sqlerrm);
      end;
      /
    
      exec p1(null)

[4] 선언 -> raise로 수동 발생 -> 처리

      create or replace procedure p1(e number)
      is
        v_sal emp.sal%type;
        e_low exception;
      begin
        p.p('중요한 작업들');
    
        select sal into v_sal
        from emp
        where empno = e;
    
        if v_sal < 3000 then
          raise e_low;
        end if;
    
        p.p(v_sal);    
      exception
        when e_low then
          p.p('급여가 너무 적어요!');
      end;
      /
    
      exec p1(7788)
      exec p1(7900)

[5] raise_application_error 프로시져
    
      create or replace procedure p1(a number)
      is
        v_sal emp.sal%type;
      begin
        select sal into v_sal
        from emp
        where empno = a; 
    
        if v_sal < 3000 then
          raise_application_error(-20100, '급여가 너무 적어요!');
        end if;
      end;
      /
    
      exec p1(7900)    --exception처리 했지만 비정상 종료 됨
###
      create or replace procedure p1(a number)
        is
          v_sal emp.sal%type;
    
          e_too_low exception;
          pragma exception_init(e_too_low, -20100);
      begin
          select sal into v_sal
          from emp
          where empno = a; 
    
          if v_sal < 3000 then
            raise_application_error(-20100, '급여가 너무 적어요!');
          end if;
      exception
          when e_too_low then
            p.p('급여를 확인해 보세요!');
      end;
      /
    
      exec p1(7900)    
      
###      
    cf. 트리거에서 에러가 발생하는 트리거를 유발한 작업이 실패한다

      - 첫번째 명령 프롬프트

      @KANG ~ $ sqlplus / as sysdba

      CREATE OR REPLACE TRIGGER ACE26_LOGON_TRI
      after LOGON ON ACE26.SCHEMA
      BEGIN
        IF SUBSTR(sys_context('USERENV', 'IP_ADDRESS'), 1, 9) in ('70.12.113', '70.12.114')  then
               RAISE_APPLICATION_ERROR ( -20002, 'IP '||ORA_CLIENT_IP_ADDRESS
                  || ' is not allowed to connect database!');
        END IF;
      END;
      /

      - 두번째 명령 프롬프트

      @KANG ~ $ sqlplus ace29/me@70.12.114.184:1521/xe

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>