---
layout: post
title: '[oracle] 오라클 PL/SQL - 명시적 커서 사용'
subtitle: 
categories: til
tags: til oracle plsql
comments: true
date: 2019-04-16 07:12:17 +0900
lastmod: 2019-04-16 07:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 PL/SQL - 명시적 커서 사용<br />

# 7장. 명시적 커서 사용 
> 명시적 커서

## 7-15. 
    
      create or replace procedure p1(w number)
      is
        cursor c1
        is
        select empno, ename, sal, deptno
        from emp
        where deptno = w
        order by sal desc;
    
        r c1%rowtype;
      begin
        open c1;
    
        loop
          fetch c1 into r;
          exit when c1%notfound or c1%rowcount > 3;
          p.p(r.empno||' '||r.ename);
        end loop;
    
        close c1;    
      end;
      /
    
      exec p1(10)
      exec p1(30)
###
      --위에 예제를 좀 줄여서
      create or replace procedure p1(w number)
      is
        cursor c1
        is
        select empno, ename, sal, deptno
        from emp
        where deptno = w
        order by sal desc;
    
        r c1%rowtype;
      begin
        for r in c1 loop
            p.p(r.empno||' '||r.ename);
        end loop;   
      end;
      /
    
      exec p1(10)
      exec p1(30)
###
      --위에 예제를 좀 줄여서
      create or replace procedure p1(w number)
      is
      begin 
        for r in (select empno, ename, sal, deptno
                  from emp
                  where deptno = w
                  order by sal desc) loop
          p.p(r.empno||' '||r.ename);
        end loop;
      end;
      /
    
      exec p1(10)
      exec p1(30)

## 문제. 다음과 같은 결과를 만드세요.
      ------------------------
      10 ACCOUNTING NEW YORK
      ------------------------
      7782 CLARK 2450
      7839 KING 5000
      7934 MILLER 1300
      ------------------------
      20 RESEARCH DALLAS
      ------------------------
      7369 SMITH 800
      7566 JONES 2975
      7788 SCOTT 3000
      7876 ADAMS 1100
      7902 FORD 3000
      ------------------------
      30 SALES CHICAGO
      ------------------------
      7499 ALLEN 1600
      7521 WARD 1250
      7654 MARTIN 1250
      7698 BLAKE 2850
      7844 TURNER 1500
      7900 JAMES 950
      ------------------------
      40 OPERATIONS BOSTON
      ------------------------
      ------------------------
###          
      create or replace procedure p1(w number)
      is
      begin 
        for d in (select * from dept) loop
        end loop;
      end;
      /
    
      create or replace procedure p1(w number)
      is
      begin 
        for d in (select * from dept) loop
          for e in (select empno, ename, sal from emp) loop
          end loop;
        end loop;
      end;
      /
    
      create or replace procedure p1
      is
      begin 
        p.p('------------------------');
        for d in (select * from dept) loop
          p.p(d.deptno||' '||d.dname||' '||d.loc);
          p.p('------------------------');
          for e in (select empno, ename, sal
                    from emp 
                    where deptno = d.deptno) loop
            p.p(e.empno||' '||e.ename||' '||e.sal); 
          end loop;
          p.p('------------------------');
        end loop;
      end;
      /
    
## 7-26. 
      drop table t1 purge;
      drop table t2 purge;
    
      create table t1 as select * from dept;
      create table t2 as select * from emp;
    
      create or replace procedure p1(w varchar2)
      is
        cursor c1
        is
        select e.empno, e.ename, e.sal, e.deptno
        from t2 e, t1 d
        where e.deptno = d.deptno
        and d.loc = w
        for update of e.sal wait 5;
    
        r c1%rowtype;
      begin
        open c1;
    
        loop
          fetch c1 into r;
          exit when c1%notfound;
          update t2
          set sal = r.sal * 1.1
          where empno = r.empno;
        end loop;
    
        close c1;
      end;
      /

          
      select * from t2;
    
      exec p1('DALLAS')
    
###
    
      create or replace procedure p1(w varchar2)
      is
        cursor c1
        is
        select e.empno, e.ename, e.sal, e.deptno
        from t2 e, t1 d
        where e.deptno = d.deptno
        and d.loc = w
        for update of e.sal wait 5;
      begin
        for r in c1 loop
          update t2
          set sal = r.sal * 1.1
          where empno = r.empno;
        end loop;
      end;
      /
    
      select * from t2;
    
      exec p1('DALLAS')
    
###
    
      create or replace procedure p1(w varchar2)
      is
        cursor c1
        is
        select e.rowid as rid, e.empno, e.ename, e.sal, e.deptno
        from t2 e, t1 d
        where e.deptno = d.deptno
        and d.loc = w
        for update of e.sal wait 5;
      begin
        for r in c1 loop
          update t2
          set sal = r.sal * 1.1
          where rowid = r.rid
        end loop;
      end;
      /
    
      select * from t2;
    
      exec p1('DALLAS')
    
###
    
      create or replace procedure p1(w varchar2)
      is
        cursor c1
        is
        select e.empno, e.ename, e.sal, e.deptno
        from t2 e, t1 d
        where e.deptno = d.deptno
        and d.loc = w
        for update of e.sal wait 5;
      begin
        for r in c1 loop
          update t2
          set sal = r.sal * 1.1
          where current of c1;
        end loop;
      end;
      /
    
      select * from t2;
    
      exec p1('DALLAS')
    
      select * from t2;

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>