---
layout: post
title: '[oracle] 오라클 PL/SQL - PL/SQL 변수 선언'
subtitle: 
categories: til
tags: til oracle plsql
comments: true
date: 2019-04-16 02:12:17 +0900
lastmod: 2019-04-16 02:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 PL/SQL - PL/SQL 변수 선언<br />

# 2장. PL/SQL 변수 선언
> 2장 PL/SQL 변수 선언

## 그릇
* 변수(Variable) = 그릇(변경할 수 있는)
* 상수(Constant) = 그릇(변경이 안 되는) = 특수한 변수
* 매개변수(Parameter) = 그릇(이웃집과 나눌 수 있는)
```
    declare
        v_sal number;                    --변수 : 한줄 의미 : 메모리 할당, 그 메모리에 별명 부여(변수명), 데이터 타입 결정, 실제 데이터
        v_tax constant number := 0.013;  --상수 : 한줄 의미 : 메모리 할당, 그 메모리에 별명 부여(변수명), 데이터 타입 결정, 실제 데이터 - 상수는 반드시 초기화 해야한다.
    begin
    end;
    /
```

```
    create or replace function tax(a number)    --매개변수
        return number
    is
        v_sal number := a;                      --변수 v_sal, 매개변수 a     
        v_tax constant number := 0.013;         --상수
    begin
        return v_sal * v_tax;
    end;
    /
    
    select empno, job, sal, tax(sal)
      from emp
     where job in('MANAGER', 'SALESMAN');
```

## SELECT문 유형에 따른 변수 선언 6가지
* 이미지 참고<br>
![190402 PLSQL select_수업참고](https://user-images.githubusercontent.com/46523571/55386456-789d9b80-556a-11e9-8cec-0e65d5c6066f.png)

[1] 한 개 리턴
```
    set serveroutput on
    
    create or replace procedure p1(k number)
    is
        v_sal emp.sal%type;
    begin
        select sal into v_sal
          from emp
         where empno = k;
        
        dbms_output.put_line(k||' 사원의 급여는 '||v_sal||'입니다.');
    end;
    /
    
    exec p1(7788)
```

[2] 한 행 전부 리턴 
```
    set serveroutput on
    
    create or replace procedure p1(k number)
    is
        r emp%rowtype;    --emp%rowtype : 테이블 컬럼의 구조 그대로 데이터 타입을 갖는다.
    begin
        select * into r
          from emp
         where empno = k;
         
        dbms_output.put_line(r.empno);  --출력 방식1
        dbms_output.put_line(r.ename);
        
        dbms_output.put_line(r.empno||' ' ||r.ename);   --출력 방식2
    end;
    /
    
    exec p1(7788)
```

[3] 하나의 행에서 중 일부 값만 리턴
```
    set serveroutput on
    
    create or replace procedure p1(k number)
    is
        TYPE rt IS RECORD
        (a emp.ename%type, 
         b emp.job%type, 
         c emp.sal%type);     --필드명은 일반적으로 컬럼명으로 작성
        
        r rt;   --rt란 데이터 타입은 r변수를 따른다.
    begin
        select ename, job, sal into  r      --여기서 r은 PL/SQL Record(Struct)라고 한다.
          from emp
         where empno = k;
         
         dbms_output.put_line(r.a);
         dbms_output.put_line(r.b);
         dbms_output.put_line(r.c);
    end;
    /
    
    exec p1(7788)
```

```
    --조금더
    --body가 없는 package로 만들면 활용성 증가
    crete or replace package pack1
    is
        TYPE rt IS RECORD
        (a emp.ename%type, 
         b emp.job%type, 
         c emp.sal%type);
    end;
    /

    create or replace procedure p1(k number)
    is
        r pack1.rt;
    begin
        select ename, job, sal into  r
          from emp
         where empno = k;
         
         dbms_output.put_line(r.a);
         dbms_output.put_line(r.b);
         dbms_output.put_line(r.c);
    end;
    /

    exec p1(7788)
```

```
      cf. 뷰를 활용할 경우 이렇게 구현할 수 있음

      create or replace view v1
      as
      select ename, job, sal
      from emp;

      create or replace procedure p1(k number)
      is
        r v1%rowtype;
      begin
        select ename, job, sal into r
        from emp
        where empno = k;
 
        dbms_output.put_line(r.ename||' '||r.job||' '||r.sal);
      end;
      /

      exec p1(7788)
```
    
[4] 같은 종류의 값을 여러개 리턴(배열변수 준비해야함)
```
    set serveroutput on

    create or replace procedure p1(k number)
    is
        TYPE t1 IS TABLE OF emp.sal%type
            INDEX BY PLS_INTEGER;            --datatype
        s t1;                                --t1의 데이터 타입을 따르는 변수 s
    begin                                    --여기서 s는 PL/SQL Table(Array)라고 한다.
        select sal BULK COLLECT INTO s       --여러개의 값을 s에 넣으니깐 into가 아닌 BULK COLLECT INTO를 써야한다.
          from emp
         where deptno = k;
         
         dbms_output.put_line(s.first);      --인덱스의 첫번째 값
         dbms_output.put_line(s.last);       --인덱스의 마지막 값
         
         FOR i IN s.first .. s.last LOOP
            dbms_output.put_line(s(i));
         END LOOP;    
    end;
    /
    
    exec p1(10)
    
    select * from emp;
```

```
    cf. 다른 방법으로 명시적 커서를 활용이 있음
    
    create or replace procedure p1(k number)
    is
        CURSOR c1                     --select문을 CURSOR로 명시
        IS
        select sal                    --평범한 select문이 온다.
          from emp
         where deptno = k; 
        
        v_sal emp.sal%type;
    begin
        OPEN c1;                      -- Active Set 생성  --한건도 없어도, 여러 건이여도 에러가 아니다.
        
        loop
            FETCH c1 INTO v_sal;      -- current row 가서 가져오기 --c1의 결과에서 한개를 꺼내서 v_sal 변수에 넣는다.
            exit when c1%notfound;    --무한루프기 때문에 조건을 줘서 빠져나가야한다.
            dbms_output.put_line(v_sal);
        end loop;
        
        CLOSE c1;                     --서버 메모리 닫기
    end;
    /
    
    exec p1(10)
    exec p1(30)
```
   
[5] 레코드 여러개 리턴
```
    create or replace procedure p1(k number)
    is 
        TYPE emp_table_type IS TABLE OF emp%rowtype
            INDEX BY PLS_INTEGER;
        t emp_table_type;
    begin
        select * BULK COLLECT INTO t
          from emp
         where deptno = k;
         
         FOR i IN t.first .. t.last LOOP
            dbms_output.put_line(t(i).empno||' '||t(i).ename);
         END LOOP;
    end;
     /
     
     exec p1(10)
     exec p1(30)
```
    
[6] 반복적인 일부의 값만 리턴
```
    -- 프로시져 형태로
    create or replace procedure p1(k number)
    is
        TYPE rt IS RECORD
        (ename emp.ename%type, 
           job emp.job%type, 
           sal emp.sal%type);
           
        TYPE emp_table_type IS TABLE OF rt
            INDEX BY PLS_INTEGER;
            
        t emp_table_type;
    begin
        select ename, job, sal BULK COLLECT INTO t
          from emp
         where deptno = k;

         FOR i IN t.first .. t.last LOOP
            dbms_output.put_line(t(i).ename||' '||t(i).job||' '||t(i).sal);
         END LOOP;
    end;
    /
    
    exec p1(10)
    exec p1(30) 
```

```
    --패키지 형태로 

    create or replace package pack1
    is
        TYPE rt IS RECORD 
        (ename emp.ename%type,
         job   emp.job%type,
         sal   emp.sal%type);
    
        TYPE emp_table_type IS TABLE OF rt
          INDEX BY PLS_INTEGER;
    end;
      /
    
    create or replace procedure p1(k number)
    is
        t pack1.emp_table_type; 
    begin
        select ename, job, sal BULK COLLECT INTO t
          from emp
         where deptno = k;
    
        FOR i IN t.first .. t.last LOOP
          dbms_output.put_line(t(i).ename||' '||t(i).job);
        END LOOP;
    end;
    /
    
    exec p1(10)
    exec p1(30)
```

## 문제.부서와 소속 사원을 다음과 같은 형태로 나타내세요
```

     DEPTNO DNAME         LOC        SAWON
  --------- ------------- ---------- --------------------------------------------
         10 ACCOUNTING    NEW YORK   CLARK, KING, MILLER     
         20 RESEARCH      DALLAS     ADAMS, FORD, JONES, SCOTT, SMITH  
         30 SALES         CHICAGO    ALLEN, BLAKE, JAMES, MARTIN, TURNER, WARD
         40 OPERATIONS    BOSTON


    --해답 @@@@ 수정해야함.
    create or replace function dept_sawon(d number)
        return varchar2
    is
        TYPE emp_ename_tab_type IS TABLE OF emp.ename%type
            INDEX BY PLS_INTEGER;
        
        enames emp_ename_tab_type;  
        
        ret varchar2(100);
    begin
        select ename BULK COLLECT INTO enames
        from emp
        where deptno = d
        order by ename;
        
        ret := enames(1);
        
        for i in 2 .. enames.last loop
            ret := ret||', '||enames(i);
        end loop;
        
        return ret;
    end;
    /
    
    select deptno, dname, loc, dept_sawon(deptno) as sawon
      from dept;
```

## setter & getter 처럼 만들어봅시다.
```
    --ready
    drop table t1 purge;
    
    create table t1
    as
    select * from emp;
```

```
    --setter
    create or replace procedure t1_set_ename(a t1.empno%type, b t1.ename%type)
    is
    begin
        update t1
           set ename = b;
         where empno = a;
    end;
    /

    select * from t1;
    exec t1_set_ename(7369, 'QUEEN')
    select * from t1;
```

```
    --getter
    create or replace function t1_get_ename(a t1.empno%type)
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
    exec dbms_output.put_line(t1_get_ename(7369))
    select * from t1;
```

## setter & getter를 package로 
```
    --패키지 변환
    create or replace package t1_pack
    is
        procedure t1_set_ename(a t1.empno%type, b t1.ename%type);
    
        function t1_get_ename(a t1.empno%type)
            return t1.ename%type;
    end;
    /
    
    create or replace package body t1_pack
    is
        procedure t1_set_ename(a t1.empno%type, b t1.ename%type)
        is
        begin
            update t1
               set ename = b
             where empno = a;
        end;  
    
        function t1_get_ename(a t1.empno%type)
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
```

## 2-11. 변수 유형
```
    * PL/SQL 변수         - Scalar : 문자, 숫자, 날짜, Boolean
                         - Composite
                         - Ref
                         - LOB
      
    * NON-PL/SQL 변수     - Substitution Variable(치환변수)
                         - Bind Variable
                         
    
      cf. Built-in Datatypes
          http://www.mysqltutorial.org/mysql-stored-procedure-tutorial.aspx
```

## 2-26. 
```
    create table t9
    (a number(4),
     b varchar2(10),
     c clob);            --데이터를 128TB 넣을 수 있다. 함부로 쓰면 안된다.
    
    set long 20000
    
    select dbms_metadata.get_ddl('TABLE', T9', user) as ret
    from dual;
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>