---
layout: post
title: '[oracle] 오라클 SQL - DML, TCL, 읽기 일관성 및 Lock'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 09:12:17 +0900
lastmod: 2019-04-15 09:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL DML, TCL, 읽기 일관성 및 Lock<br />

# 9장. DML, TCL, 읽기 일관성 및 Lock
> 9장 : DML, TCL<br>

## DML : INSERT, UPDATE, DELETE, MERGE
    drop table t1 purge;
    
    create table t1
    as
    select empno a, ename b, sal c
    from emp
    where 1 = 2;
    
    select * from t1;
    
    insert into t1 values(1000, 'Korea', 267.34);
    insert into t1 values(1001, 'States');        --에러, 컬럼 3개인데 insert 2개하면 에러 발생
    insert into t1(a, b) values(1001, 'States');  --성공 암시적, 2개만 insert하고 싶을 때 t1에 컬럼명을 입력하면 한다.
    insert into t1 values(1002, 'England', null); --성공 명시적, 2개만 insert하고 싶을 때 세번째 컬럼에 null을 추가하면 된다.
    
    select * from t1;
    
    insert into t1 select empno, ename, sal from emp;   --select(db내부에 있는 정보)의 결과를 insert에
    
    select * from t1;

## merge
    drop talbe t1 purge;
    drop table t2 purge;
    
    create table t1 as select empno, ename, sal, deptno from emp where rownum <= 8;

    create table t2 as select empno, ename, sal, deptno from emp;
    
    update t2
    set sal = sal * 1.1
    where rownum <= 8;
    
    select * from t1;
    select * from t2;
    
    문제. t2 table을 이용해서 t1에 update 또는 insert
    
    select * from t1;
    
    MERGE INTO t1
    USING t2
    ON (t1.empno = t2.empno)
    WHEN MATCHED THEN          --같은게 있으면 update
        UPDATE SET t1.ename = t2.ename, t1.sal = t2.sal
    WHEN NOT MATCHED THEN      --같은게 없으면 insert
        INSERT (t1.empno, t1.ename, t1.sal, t1.deptno)
        VALUES (t2.empno, t2.ename, t2.sal, t2.deptno) 
    
    select * from t1;


## 9-16 Update문
* implicit query : https://gseducation.blog.me/20125786704

## 9-25 Delete vs Truncate vs Drop
                            ROLLBACK          공간 반납
    delete from t1;             O                X
    truncate table t1;          X             최초 크기만 남기고 반납(공장초기화 느낌)
    drop table t1;              X             몽땅 반납

## TCL : COMMIT, ROLLBACK, SAVEPOINT
    drop table t1 purge;
    
    create table t1 (no number, name varchar2(10));
    
    insert into t1 (no) values (1000);
    insert into t1 (no) values (2000);
    
    update t1 set name = 'Java' where no = 2000;
    
    savepoint s1;    --savepoint : 마킹하는 것
    
    insert into t1 (no) values (3000);
    insert into t1 (no) values (4000);
    
    savepoint s2;
    
    insert into t1 (no) values (5000);
    insert into t1 (no) values (6000);
    
    rollback to s2; --s2 만날때까지 취소 그위는 살아있다.(기준은 rollback에서 위로 올라간다.) 결과적으로 1000~4000은 살아있고 5000~6000은 삭제
    
    commit;
    
    select * from t1;

## 9-29 읽기 일관성, Lock 그리고 Deadlock
    [SESSION 1]                             [SESSION 2]
    
    -> 읽기 일관성
    
    drop table t_books purge;
    
    create table t_books
    (no number, name varchar2(20));
    
    insert into t_books
    values (1000, 'Java');
    
    insert into t_books
    values (2000, 'SQL');
    
    select * from t_books;
                                            select * from t_books;  --no rows selected
    
    
    commit;                                 
                                            select * from t_books;  --data 보임
    
    
###
    -> Lock(은행 전화걸었을 때와 같은 느낌)
    
    update t_books
    set name = 'Python'
    where no = 1000;
    
    select * from t_books;
    
                                            select * from t_books;                                        
    
                                            update t_books          --왼쪽이 lock상태이므로 처리가 안됨
                                            set name = 'Python'
                                            where no = 1000;
                                            --|끝없이 기다려야하는 상태...
    
    rollback;                               --왼쪽에서 rollback 하는 순간 lock걸렸던 오른쪽이 풀림.           
    
    
###
    -> DeadLock
    
    update t_books
    set name = 'Python'
    where no = 1000;
    
    
                                            update t_books
                                            set name = 'Scalar'
                                            where no = 2000;
    
    update t_books
    set name = 'Unix'
    where no = 2000;
    --|wait 상태
                                            update t_books
                                            set name = 'R'
                                            where no = 1000;
                                          --|wait 상태
                                          --|3초 뒤에 왼쪽 상태 lock 풀리면서 ORA-00060 에러 뜸.
                        
    
    ORA-00060: 
    deadlock detected
    while waiting for resource
    --나는 lock가 풀렸지만 오른쪽이 lock 상태이므로
    --rollback 또는 commit 처리 해줘야
    --오른쪽 lock이 풀린다.
    
    rollback;


## 종료할 때 주의사항
* sql에서 exit하고 끄면 처리하던 DML이 commit 처리 된 후 종료
* sql에서 x버튼을 누르고 끄면 처리하던 DML이 rollback 처리 된 후 비정상 종료 됨.

## DDL : CREATE, ALTER, DROP, RENAME, TRUCATE, COMMENT
    drop table t1 purge;
    drop table tab1 purge;
    
    create table t1
    (empno number,
    ename varchar2(10));
    
    insert into t1
    select empno, ename from emp where rownum <= 3;
    
    --sal, deptno 컬럼 추가
    alter table t1 add(sal number(10, 2), deptno number(2));
    --ename의 길이 변경
    alter table t1 modify(ename varchar2(6), deptno default 10);
    --제약조건 추가
    alter table t1 add constraint t1_empno_pk primary key(empno);
    --not null 형태로 추가 할 수 없으므로 에러 발생
    alter table t1 add not null(ename);
    --not null 형태로 추가하고 싶다면 modify만 가능
    alter table t1 modify (ename not null);
    
    select * from t1;
    
    rename t1 to tab1;
    
    select * from tab1;
    
    truncate table tab1;
    
    select * from tab1;
    
    --테이블에 주석 달기 
    comment on table tab1 is '테스트 프로젝트용';
    
    --컬럼에 주석 달기
    comment on column tab1.empno is '사번';
    comment on column tab1.ename is '사원이름';
    
    select * from user_tab_comments;
    select * from user_col_comments;

## 

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>