---
layout: post
title: '[oracle] 오라클 PL/SQL - 실행문 작성'
subtitle: 
categories: til
tags: til oracle plsql
comments: true
date: 2019-04-16 03:12:17 +0900
lastmod: 2019-04-16 03:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 PL/SQL - 실행문 작성<br />

# 3장. 실행문 작성
> SQL Statement in Blocks

## Lexical unit(어휘 단위)
A line of PL/SQL text contains groups of characters knows as lexical units:

- 구분자(Delimiter) : simple and compound symbols
- 식별자(Identifier) : which include reserved words
- 리터럴(Literal) :
- 주석(Comment) : 

## 3-16.
- Scope(변수의 범위)
- Visibility(변수의 가시성)

## 3-18. 
- qualifier(수식자)

## 3-19. 
* overloading vs overriding
* 형제 간의 관계 vs 부모 간의 관계
###
      create or replace package p
      is
        procedure p(a number);
        procedure p(a date);
        procedure p(a varchar2);
      end;
      /
    
      create or replace package body p
      is
        procedure p(a number)
        is
        begin
          dbms_output.put_line(a);
        end;
    
        procedure p(a date)
        is
        begin
          dbms_output.put_line(a);
        end;
    
        procedure p(a varchar2)
        is
        begin
          dbms_output.put_line(a);
        end;
      end;
      /  
    
      exec p.p(100)
      exec p.p('100')
      exec p.p(sysdate)
    
       ---
    
      <<outer>>
      DECLARE
        v_sal     NUMBER(7,2) := 60000;
        v_comm    NUMBER(7,2) := v_sal * 0.20;
        v_message VARCHAR2(255) := ' eligible for commission';
      BEGIN 
    
        DECLARE
          v_sal        NUMBER(7,2) := 50000;
          v_comm       NUMBER(7,2) := 0;
          v_total_comp NUMBER(7,2) := v_sal + v_comm;
        BEGIN 
          v_message := 'CLERK not'||v_message;
    
          p.p(v_message);         -- Q1 : CLERK not eligible for commission
          p.p(v_comm);            -- Q2 : 0
          p.p(outer.v_comm);      -- Q3 : 12000
    
          outer.v_comm := v_sal * 0.30; 
        END;
    
        v_message := 'SALESMAN, '||v_message;
    
        p.p(v_total_comp);     -- Q4 : 에러
        p.p(v_comm);              -- Q5 : 15000 
        p.p(v_message);           -- Q6 : SALESMAN, CLERK not eligible for commission
    
      END;
      /

## 3-22. PL/SQL 연산자

    1 + (-2)


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>