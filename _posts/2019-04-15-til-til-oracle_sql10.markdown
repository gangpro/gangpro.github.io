---
layout: post
title: '[TIL] 오라클 SQL - Table'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 10:12:17 +0900
lastmod: 2019-04-15 10:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL Table<br />

# 10장. Table
> 10장 : Table<br>

## 개념 정리
* 테이블을 생성한다는 것은?
###
    - 과정 : 현실 세계 -> 데이터 모델링                -> 데이터베이스 구현 
                       - 선별, 정리                   - Create Database ~
                       - Logical    Modeling          Create User ~
                       - Relational Modeling          Create Table ~
                       - Physical   Modeling          ...
                       
    - Data Integrity(무결성) 유지를 끊임없이 고민해야 함.
      -> 데이터 무결성의 유일한 판단 기준은 비즈니스 룰임
      -> 테이블 생성시 무결성 제약 조건 설정
      -> PL/SQL을 이용해서 트리거(Trigger) 생성
      -> Application Code
      
    - 데이터 타입의 종류
      INT     : 정수형
      CHAR    : 고정길이 문자
      VARCHAR : 가변길이 문자
      TIME    : 시간
      DATE    : 날짜
      
###   
    - Data Type

    - 문자 -> col1 char(10)
           -> col1 varchar2(10)
      
    - 숫자 -> col1 number(4)     : 정수
           -> col1 number(10, 2) : 고정(fixed) 소수점
           -> col1 number        : 부동(float) 소수점

      cf.물리모델링시 Width가 없는 Number형을 쓰지 말아야 할 이유 

         https://gseducation.blog.me/20095938837

    - 날짜 -> date
           -> timestamp
           -> interval

    - 기타 - LOB -> CLOB
                 -> BLOB
                 -> BFile
           -> long
           -> raw


## 테이블 생성 및 DML
* Table Instance Chart?
###    
      Table Instance Chart?
      
      T_EMP Table                                 T_DEPT Table
      -----------                                 ------------
      empno   ename    sal    hp       deptno     deptno dname    loc
      nummber varchar2 number varchar2 number     number varchar2 vachar2
      ---------------------------------------     -----------------------
      
      drop talbe t_emp  purge;
      drop table t_dept purge;
                         
      
      create table t_dept(
      deptno number(2),     --정수(2자리)
       dname varchar2(10),   --문자(10byte)
         loc varchar2(10));
      
      create table t_emp(
       empno number(2),
       ename varchar2(10),
         sal number(10, 2),    --숫자 10자리, 소수점 2자리
          hp varchar2(11),
      deptno number(2));


    select * from tab
    where tname like 'T!_%' escape '!';
    
    desc t_dept
    desc t_emp
    
    insert into t_dept values(10, Marketing, Seoul);         --에러 : 문자에 작은따옴표 없음
    insert into t_dept values(10, 'Marketing', 'Seoul');     --삽입완료
    select * from t_dept;
    
    insert into t_dept values(10, 'IT', 'Masan');   --삽입완료 그러나 부서번호가 같기 때문에 좋지 않다
    
    alter table t_dept add unique(deptno);      --deptno 컬럼에 unique 제약 조건 삽입    --에러 발생 : 기존에 삽입해 놓은게 있기 때문에
    
    update t_dept set deptno = 20 where dname = 'IT';
    select * from t_dept;
    
    alter table t_dept add unique(deptno);      --성공
    select * from t_dept;
    
    insert into t_dept values(10, 'RD', 'Suwon');   --당연히 deptno에 unique 제약 조건이 있기 때문에 에러
    insert into t_dept values(30, 'RD', 'Suwon');   --삽입완료
    select * from t_dept;
    
    create sequence t_dept_deptno_seq       --번호표 발생기 같은것을 서버에 만든다고 생각하면 됨
    start with 40         --현재 10, 20, 30 있으니 40부터 한다.
    increment by 10       --10씩 증가
    maxvalue 1000;        --최대 1000까지
    
    insert into t_dept 
    values (t_dept_deptno_seq.nextval,'ACCOUNT', 'GJ');
    
    select t_dept_deptno_seq.currval    --시퀀스 상태 확인
    from dual;
    
    select * from t_dept;
    
    insert into t_dept 
    values (null,'SALES', 'GJ');        --unique 제약은 null을 허용한다.
    select * from t_dept;
    
    insert into t_dept 
    values (null,'RESEARCH', 'GJ');     --unique 제약은 null을 허용한다.
    select * from t_dept;
    
    commit;
    
    delete from t_dept;
    
    select * from t_dept;

* Table Instance Chart 수정?
###
      Table Instance Chart 수정?
    
      T_EMP Table                                 T_DEPT Table
      -----------                                 ------------
      empno   ename    sal    hp       deptno     deptno dname    loc
      nummber varchar2 number varchar2 number     number varchar2 vachar2
      중복  X  중복  O   중복  O  중복 X               중복  X 중복  O 
      NULL X  NULL X   NULL O  NULL X             NULL X NULL X
                       0이상
                                       정해진값
      ---------------------------------------     -----------------------


    --컬럼 레벨 제약조건 설정 문법
    drop table t_emp  purge;
    drop table t_dept purge;
                     
    
    create table t_dept(
    deptno number(2)     constraint t_dept_deptno_pk primary key,    --unique not null이라고 써도 되지만 기본키는 아니다.
     dname varchar2(10)  constraint t_dept_dname_nn  not null,       --constraint 제약조건 이름 추가(예: 제약조건 안지킬시 나오는 메시지)
       loc varchar2(10));                                            --만약 제약조건 이름 추가 안하면 SYS_n 이름으로 나온다. 
    
    create table t_emp(
     empno number(4)     constraint t_emp_empno_pk primary key,
     ename varchar2(10)  constraint t_emp_ename_nn not null,
       sal number(10, 2) constraint t_emp_sal_ck   check(sal >= 0),
        hp varchar2(11)  constraint t_emp_hp_uk    unique
                         constraint t_emp_hp_nn    not null,
    deptno number(2)     constraint t_emp_deptno_fk
                         references t_dept(deptno));    --참조 무결성
    
    insert into t_dept values (10, 'IT', 'Seoul');      --삽입완료 
    insert into t_dept values (10, 'SALES', 'Suwon');   --deptno PK로 에러
    insert into t_dept values (NULL, 'SALES', 'Suwon'); --deptno PK로 에러
    
    --테이블 만들 때 constraint 제약 조건명을 안정하면
    --위와 같은 에러 발생시 (ACE29.SYS_C007751) violated 이렇게 나온다.
    --그래서 알기 쉽게 constraint 제약 조건명을 정하면 
    --(ACE29.T_DEPT_DEPTNO_PK) violated 이렇게 오류 메시지가 나온다.
    --그래서 오류의 의미를 바로 알 수 있어서 좋다.
###
* 테이블 레벨 제약조건 설정 문법
### 
    --테이블 레벨 제약조건 설정 문법
    drop table t_emp  purge;
    drop table t_dept purge;
                     
    create table t_dept(
    deptno number(2),
     dname varchar2(10),
       loc varchar2(10),
    constraint t_dept_deptno_pk primary key(deptno),
    constraint t_dept_dname_nn  check(dname is not null));
    
    create table t_emp(
     empno number(2),
     ename varchar2(10),
       sal number(10, 2),
        hp varchar2(11),
    deptno number(2),
    constraint t_emp_empno_pk  primary key(empno),
    constraint t_emp_ename_nn  check(ename is not null),
    constraint t_emp_sal_ck    check(sal >= 0),
    constraint t_emp_hp_uk     unique(hp),
    constraint t_emp_hp_nn     check(hp is not null),
    constraint t_emp_deptno_fk foreign key(deptno) references t_dept(deptno));     --t_emp의 deptno값이 들어오면 t_dept의 deptno값
###
* 반드시 테이블 레벨 문법 제약 설정을 해야하는 경우?
###
    --cf. 반드시 테이블 레벨 문법 제약 설정을 해야하는 경우?
    --    두개 이상의 컬럼으로 하나의 제약을 생성할 경우. 반드시 테이블 레벨 문법 제약조건을 설정 해야한다.

    --주민등록번호 앞자리 6자리는 같은게 올 수 있다.
    --그럼 주민등록번호 뒷자리 7자리가 같을 수 있나? 가능하다. 
    --앞뒤 2개 합쳐서 유니크한 것
    --이럴때 사용
    
    create table t_simin(
        no number primary key,
       ju1 varchar2(6),
       ju2 varchar2(7),
    unique(ju1, ju2));    

## PL/SQL을 이용해서 트리거(Trigger) 생성
    drop table t1 purge;
    
    create table t1 (no number, name varchar2(10));
    
    --name 컬럼에 반드시 대문자 입력이라는 룰을 설정해야 함.
    --insert into t1 values (1000, 'john'); 이 실행되기 전에
    --begin @@@ end; 먼저 처리 후 insert@@@ 실행
    create or replace trigger t1_name_tri
    before insert or update of name on t1  --t1 name에 대한 update와 insert의 경우만 실행
    for each row
    begin
        :new.name := upper(:new.name);   --할당 연산자 : A를 new.name에 넣어라 
    end;
    /
    
    --자료입력
    insert into t1 values (1000, 'john');
    insert into t1 values (2000, 'alice');
    
    select * from t1;


 
## 테이블 관련 대표적 Meta data?    
    select table_name, num_rows, blocks
      from user_tables;
    
    select table_name, constraint_name, constraint_type, search_condition
      from user_constraints;


    select * from employees;
    
    --만약 primary key와 unique 제약조건을 걸면 
    --Oracle DBMS가 알아서 index를 만들어서 index를 이용해서 비교한다.
    select table_name, index_name
    from user_indexes
    order by table_name;
    --___PK, ___UK
    --PK와 UK는 제약조건을 만들기 위해 생성한거기 때문에 drop이 불가능하다. 

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>