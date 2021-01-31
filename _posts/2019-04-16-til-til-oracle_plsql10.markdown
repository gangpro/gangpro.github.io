---
layout: post
title: '[oracle] 오라클 PL/SQL - 패키지, 트리거'
subtitle: 
categories: til
tags: til oracle plsql
comments: true
date: 2019-04-16 10:12:17 +0900
lastmod: 2019-04-16 10:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 PL/SQL - 패키지, 트리거<br />

# 10장. 패키지, 트리거
> Package, Trigger

## package
* Package의 구조 이해
###    
      create or replace package pack1
      is
        v1 number;                                -- public variable
    
        procedure v2_setter(a number);            -- public subprogram
        function v2_getter return number; 
      end;
      /
    
      create or replace package body pack1
      is
        v2 number;                                -- private variable
    
        function x2 (a number) return number      -- private subprogram
        is
          function random_number return number    -- local subprogram
          is
          begin
            return round(dbms_random.value(1, 5));
          end;
        begin 
          return a*random_number;
        end;
      
        procedure v2_setter(a number)
        is
          v3 number;                           -- local variable
        begin
          v2 := x2(a);
        end;
    
        function v2_getter return number
        is
        begin
          return x2(v2);
        end;
      end;
      /

    
      (1) public variable 활용
    
        exec pack1.v1 := 9
        exec p.p(pack1.v1)
    
    
      (2) private variable 활용
    
        exec pack1.v2_setter(1000)
    
        exec p.p(pack1.v2)           -- 에러 : private 변수는 직접 이용할 수 없음
        exec p.p(pack1.v2_getter())  -- 성공






      

## 예제. getter, setter 처럼 만들어봅시다.      
    
      drop table t1 purge;
    
      create table t1
      as
      select * from emp;
    
      create or replace procedure t1_set_ename
      (a t1.empno%type,
       b t1.ename%type)
      is
      begin
        update t1
        set ename = b
        where empno = a;
      end;
      /
    
      create or replace function t1_get_ename
      (a t1.empno%type)
       return t1.ename%type
      is 
        v_ename t1.ename%type;
      begin
        select ename into v_ename
        from t1
        where empno = a;
    
        return v_ename;
      end;
      /
    
      select * from t1;
    
      exec t1_set_ename(7369, 'QUEEN')
    
      select * from t1;
    
      exec dbms_output.put_line(t1_get_ename(7369))
    
###
    
      create or replace package t1_pack
      is
        procedure t1_set_ename
        (a t1.empno%type,
         b t1.ename%type);
    
        function t1_get_ename
        (a t1.empno%type)
         return t1.ename%type;
      end;
      /
    
      create or replace package body t1_pack
      is
        procedure t1_set_ename
        (a t1.empno%type,
         b t1.ename%type)
        is
        begin
          update t1
          set ename = b
          where empno = a;
        end;  
    
        function t1_get_ename
        (a t1.empno%type)
         return t1.ename%type
        is 
          v_ename t1.ename%type;
        begin
          select ename into v_ename
          from t1
          where empno = a;
    
          return v_ename;
        end;
      end;
      /
    
      exec t1_pack.t1_set_ename(7369, 'PRINCE')
    
      select * from t1;
    
      exec dbms_output.put_line(t1_pack.t1_get_ename(7369))
    
<br>
<br>
<br>

## Trigger
* 트리거 유형
###
    - DML 발생시   ->   일반 DML 트리거   -> timing : before, after, instead of
                 ->   혼합 트리거
    - DDL 발생시
    - Event 발생시

    cf. 부록 G에 다양한 예제가 있습니다.

* for each row 키워드
  - 행 트리거 vs 문장 트리거


* 9-12. Mutating Error
  - cf. https://orapybubu.blog.me/40025296984


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>