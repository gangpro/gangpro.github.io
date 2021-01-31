---
layout: post
title: '[oracle] 오라클 SQL - View, Sequence, Index, Synonym, 데이터 연결 4가지 방법'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 11:12:17 +0900
lastmod: 2019-04-15 11:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL View, Sequence, Index, Synonym, 데이터 연결 4가지 방법<br />

# 11장. View, Sequence, Index, Synonym, 데이터 연결 4가지 방법
> 11장 : 뷰(View), 시퀀스(Sequence), 인덱스(Index), 동의어(Synonym)<br>

## View
* Named Select 
* 뷰에 대한 질의는 Base Table에 대한 질의로 QT(Query Transformation)됨.(예외 많음)
* 집합의 무한 확장
* 종류
  - Simple View : 테이블에 있는 내용 그대로
  - Complex View : 테이블에 있는 내용 그대로 안 보여줌  (사용자가 이것저것 추가 한다는 의미)
  
## View - 집합의 무한 확장
    --
    create or replace view vu_numbers
    as
    select level as no, level*2 no2, level*5 no5, level*10 no10, chr(level+64) d
    from dual
    connect by level <= 10 ;
    
    select * from vu_numbers;  

## View - Simple View 1
    --create view 생성
    create or replace view v1
    as
    select empno, ename, sal
    from emp
    where deptno = 30;
    
    --create view가 만들어 졌는지 확인
    select view_name, text
    from user_views;
    
    --create view 만든거 사용하기
    select *
    from v1;
    
    --v1의 조건인 Deptno = 30 결과와 where sal >= 2500 2개를 처리한다. 
    --이를 view merging이라고 한다.
    select empno, enamne, sal
    from v1
    where sal >= 2500;

## View - Simple View 2
    --일부 내용만 보이게
    create or replace view vu_emp1
    as
    select empno, ename, job
    from emp;
    
    --급여 항목까지 보이게 
    create or replace view vu_emp2
    as
    select empno, ename, job, sal
    from emp;
    
    --검색 권한주기 
    grant select on vu_emp1 to public;
    grant select on vu_emp2 to public;
    
    --ace30에 대한 권한이 없기 때문에 에러 발생
    select * from ace30.emp;
    
    --뷰는 접근 가능
    select * from ace30.vu_emp1;
    select * from ace30.vu_emp2;
    
## View - Complex View
    create or replace view v2
    as
    select d.deptno   부서번호, 
           d.dname    부서이름, 
           count(*)   사원수, 
           max(e.sal) 최고급여, 
           min(e.sal) 최소급여
      from emp e, dept d
     where e.deptno = d.deptno
     group by d.deptno, d.dname;
    
    
    select * from v2;




## Sequence
* https://orapybubu.blog.me/40020473578

## 11-30. Sequence를 사용할 경우 값의 Gap이 발생할 수 있음
###
    --발생한 번호를 포함한 DML문 ROLLBACK 
    insert into t values (seq.nextval, ...);  --1
    insert into t values (seq.nextval, ...);  --2
    
    commit;
    
    insert into t values (seq.nextval, ...);  --3
    insert into t values (seq.nextval, ...);  --4
    insert into t values (seq.nextval, ...);  --5
    
    rollback;
    
    insert into t values (seq.nextval, ...);  --6
###
    --하나의 시퀀스를 여러 테이블에서 사용할 경우
    insert into t values (seq.nextval, ...); --1
    insert into t values (seq.nextval, ...); --2
                                                        insert into t values (seq.nextval, ...); --3
    insert into t values (seq.nextval, ...); --4    
                                                        insert into t values (seq.nextval, ...); --5
                                                        insert into t values (seq.nextval, ...); --6
###
    --Cache 설정으로 추출한 번호를 서버 종료로 잃어버리는 경우
    select * from user_sequences;


## Index
###
      - 이익          vs          손해
        검색속도 향상               검색속도 저하
        PK, UK 제약 강화           DML 속도 저하
        FK 관련 일부 Lock 해결      스토리지 소비

* 데이터 저장소의 데이터가 순서없이 저장되어 있어서
* 이를 극복하기 위해 만든 객체로서 rowid를 전문적으로 보관함.
###
    - rowid - pseudocolumn 컬럼 가운데 하나 -> https://docs.oracle.com/cd/B28359_01/server.111/b28286/pseudocolumns.htm#SQLRF0025
            - 64진법
            - 6(Object) 3(File) 6(Block) 3(Row) 구조
    
    - 오라클에서 데이터를 찾는 가장 빠른 방법은 rowid를 활용하는 것이다.
###
    --rowid를 포함한 모든 속성의 emp 테이블 리턴
    --WARD 직원의 rowid를 살펴보자
    select rowid, e.* from emp e;
    
    --WARD 사원 정보 가져오기 
    select *
    from emp
    where rowid = 'AAAFGdAABAAAMKpAAC';
###
    --인덱스 생성
    create index emp_job_idx
    on emp(job);
    
    --방금 만든 인덱스 살펴보기
    select job, rowid
    from emp
    order by 1, 2;
    
    select * from emp;

* 아래 쿼리를 수행할 경우 오라클의 Optimizer가 인덱스 사용 여부를 판단하며
* 인덱스를 사용할 경우 MANAGER라는 값으로 인덱스를 이용해서 적절한 rowid를 획득함
* 획득한 rowid로 테이블의 데이터를 찾아서 return하게 됨
###
    select *
    from emp
    where job = 'MANAGER';

## Synonym
* 하나의 객체(employees)에 여러개 synonym(es, sawon, ...) 사용 가능
###
    create synonym es for employees;
    
    select * from es;
    
    select * from employees;
    
    drop synonym es;

<br>
<br>
<br>

## 데이터 연결 4가지 방법
* [0] 데이터 준비
###
    drop table t1 purge;
    drop table t2 purge;
    
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

* [1] 조인
###
    select *
    from t1, t2;
    
    select *
    from t1, t2
    where t1.empno = t2.empno;
    
    select t1.empno, t1.ename, t2.job
    from t1, t2
    where t1.empno = t2.empno;

* [2] 서브쿼리
###
    select empno, ename, (select job from t2 where empno = t1.empno) as job
    from t1;

* [3] SET 연산자
###
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

* [4] 사용자 정의 함수
###
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
    
    col job format a20
    
    select empno, ename, uf_get_t2_job(empno) as job
    from t1;

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>