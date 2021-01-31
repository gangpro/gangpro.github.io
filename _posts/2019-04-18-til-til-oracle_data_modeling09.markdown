---
layout: post
title: '[oracle] 오라클 데이터 모델링 - 데이터베이스 설계 고려 사항'
subtitle: 
categories: til
tags: til oracle
comments: true
date: 2019-04-18 09:12:17 +0900
lastmod: 2019-04-18 09:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 데이터 모델링 - 데이터베이스 설계 고려 사항<br />

# 데이터베이스 설계 고려 사항
> Database Design Considerations

## Performance를 고려한 Database Design 수정 
  - [데이터베이스 설계와 구축] 8장에서...
  - 정규화를 통한 성능 향상
  - 반정규화를 통한 성능 향상
  - PK 순서 조정을 통한 성능 향상
  - FK 인덱스 생성을 통한 성능 향상
  - 이력모델의 구분과 기능성 컬럼을 통한 성능 향상
  - 슈퍼타입/서브타입 구분을 통한 성능 향상
  - 효율적인 채번 방법 사용을 통한 성능 향상
  - 컬럼 수가 많은 테이블의 1:1 분리를 통한 성능 향상
  - 대용량 테이블의 파티셔닝 적용을 통한 성능 향상
  - CHAR 형식에서 개발 오류 제거를 통한 성능 향상
  - 복잡한 데이터 모델 단순화를 통한 성능 향상
  - 일관성있는 데이터타입과 길이를 통한 성능 향상
  - 분산 환경 구성을 통한 성능 향상


## Table과 Segment의 관계
                          table   segment
    
      - Heap-organized      1       1  
      - Partitioned         1       N  -> http://me2.do/FwuEFNqU, http://me2.do/GcUzbjjr
      - Clustered           N       1  -> http://me2.do/5MfRssu5, http://me2.do/5oCnOOHZ, http://me2.do/5oCnQdjn(실습)        
      - Index-organized     1       2  -> http://me2.do/5ROboyz1, http://me2.do/xlJAs890(실습)
     
      - MView -> http://me2.do/GkFy9CQD
###
     -- CLUSTER 예제

     set autot off

     drop cluster bank_cluster including tables cascade constraints;

     create cluster bank_cluster
     (bank_number varchar2(30))
     size 1k
     tablespace users;

     create index bank_cluster_idx
     on cluster bank_cluster;   

     create table bank
     (bank_number varchar2(30),
      bank_name   varchar2(30))
     cluster bank_cluster(bank_number);

     create index bank_pk_idx 
     on bank(bank_number);

     alter table bank
     add constraint bank_pk primary key(bank_number);

     create table account
     (account_number varchar2(30),
      bank_number    varchar2(30),
      account_owner varchar2(30))
     cluster bank_cluster(bank_number);

     create index account_pk_idx
     on account(account_number, bank_number);

     alter table account
     add constraint account_pk primary key(account_number, bank_number);

     set autot on

     select /*+ rule */ * 
     from account a, bank b
     where a.bank_number = b.bank_number;

     -----------------------------------------
     | Id  | Operation             | Name    |
     -----------------------------------------
     |   0 | SELECT STATEMENT      |         |
     |   1 |  NESTED LOOPS         |         |
     |   2 |   TABLE ACCESS FULL   | BANK    |
     |   3 |   TABLE ACCESS CLUSTER| ACCOUNT |
     -----------------------------------------
###
     -- IOT 예제

     create table iot
     (c1 number,
      c2 number,
      c3 number,  
        primary key(c1, c2))
      organization index;
   
     set autot on

     select * from iot
     where c1 > 0;

     create index iot_c3_idx 
     on iot(c3);

     select c3 from iot
     where c3 > 0;

     select * from iot
     where c3 > 0;

     --> http://www.orafaq.com/wiki/Index-organized_table

## With Check Option
* View?
  - Named Select!
  - View에 대한 질의는 Base Table에 대한 질의로 transformation 됩니다. (예외 많음)
###    
      drop table t1 purge;
      
      create table t1 
      as
      select empno, 
             ename, 
             case when rownum < 10  then sal           end as month_sal,
             case when rownum >= 10 then trunc(sal/4)  end as week_sal, 
             case when rownum < 10  then 'R' else 'IR' end as gubun,
             deptno 
      from emp;
    
      create or replace view sawon_r
      as
      select empno, ename, month_sal, gubun, deptno from t1
      where gubun = 'R';
    
      create or replace view sawon_ir
      as
      select empno, ename, week_sal, gubun, deptno from t1
      where gubun = 'IR';
    
      select * from sawon_r;
      select * from sawon_ir;
    
      insert into sawon_r values (9999, 'JAMES', 400, 'IR', 20);
    
      select * from t1;  --> 9999 사원은 IR인데 month_sal에 value가 입력되어 문제이다.
      
      delete from t1 where empno = 9999;
    
        ↓↓
    
      create or replace view sawon_r
      as
      select empno, ename, month_sal, gubun, deptno from t1
      where gubun = 'R'
      with check option constraint sawon_r_gubun_ck;
    
      create or replace view sawon_ir
      as
      select empno, ename, week_sal, gubun, deptno from t1
      where gubun = 'IR'
      with check option constraint sawon_ir_gubun_ck;
      
      insert into sawon_r  values (9999, 'JAMES', 400, 'IR', 20);    --> 에러 : ORA-01402: view WITH CHECK OPTION where-clause violation
      insert into sawon_ir values (9999, 'JAMES', 400, 'IR', 20);    --> 성공
    
      select * from t1;  --> 9999 사원이 제대로 입력되었음을 확인할 수 있다.
    
      select constraint_name, table_name, constraint_type
      from user_constraints
      where table_name like 'SAWON%'
      or    table_name like 'TV%'   
      or    table_name like 'EMPL%'   ;


## 참고 자료
* Partitioned Outer Join : http://me2.do/5Fc6guTF 
###
      select ...
      from days d LEFT OUTER JOIN sales s PARTITION BY (prod_id) ON (조인조건)
  
* Between 이해
###    
      drop table t1 purge;
    
      create table t1 
      (name varchar2(10),
       price number,
       start_date varchar2(8),
       end_date varchar2(8));
    
      insert into t1 values ('ABC', 1000, '19980101', '20020331');
      insert into t1 values ('ABC', 2000, '20020401', '20091210');
      insert into t1 values ('ABC', 3000, '20091211', '');
      insert into t1 values ('MOP', 2000, '20120101', '20131231');
      insert into t1 values ('MOP', 2500, '20140101', '');
    
      select * from t1;
    
      select * from t1
      where '20120630' between start_date and nvl(end_date, '99991231');

* Flashback Data Archive + Flashback Query

* 객체이름 정하기
###    
      http://orapybubu.blog.me/40045704831
      http://me2.do/xV90URJ3 : Oracle SQL Reserved Words
    
      create table "2015 sales" (no number);
    
      drop table tab1 purge;
    
      create table tab1 (no number);
      create view  tab1 as select no from tab1;  -- 에러
      create index tab1 on tab1(no);
      alter table tab1 add constraint tab1 primary key(no);
    
      create table substr (substr varchar2(30));
      select substr, substr(substr, 1, 2)
      from substr;

* MView 

  http://docs.oracle.com/cd/E11882_01/server.112/e25554/toc.htm

* 물리모델링시 Width가 없는 Number형을 쓰지 말아야 할 이유 
  
  http://gseducation.blog.me/20095938837

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>