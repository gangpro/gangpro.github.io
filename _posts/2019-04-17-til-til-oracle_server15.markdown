---
layout: post
title: '[oracle] 오라클 서버 관리 - 오라클 데이터베이스 감사 구현 관리'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 15:12:17 +0900
lastmod: 2019-04-17 15:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - 오라클 데이터베이스 감사 구현 관리<br />

# Implementing Oracle Database Auditing
> 오라클 데이터베이스 감사 구현 관리 : 11gWS1 교재 11장<br>

## 정당한 권한이 있는 유저가 엉뚱한 짓 하는 것을 확인하는 것
```
     - Mandatory auditing          : alert_SID.log, ?/rdbms/audit 
     - Standard database auditing  : audit_trail 파라미터 및 audit 명령
     - Value-based auditing        : 트리거
     - Fine-grained auditing (FGA) : dbms_fga 패키지
     - DBA auditing                : sys 감사
```

## Standard database auditing
```
      [orcl:~]$ export ORACLE_SID=prod
      [prod:~]$  sqlplus / as sysdba
    
      SQL> show parameter audit_trail
    
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        audit_trail                          string      NONE
    
      SQL> alter system set audit_trail=db scope=spfile;
      SQL> startup force
    
      SQL> audit table;                           --> Statement auditing
      SQL> audit select any table;                --> System-privilege auditing
      SQL> audit update on ora_user.employees;    --> Object-privilege auditing
    
      SQL> conn system/oracle
    
      SQL> create table t2012 (no number);
      SQL> select * from ora_user.employees;
      SQL> update ora_user.employees set salary = salary+0 where rownum = 1;
      SQL> commit;
```
     
* Query로 확인하세요.
```
        select * from DBA_AUDIT_OBJECT;
        select * from dba_audit_trail;
    
      SQL> conn / as sysdba
    
      SQL> noaudit table; 
      SQL> noaudit select any table;  
      SQL> noaudit update on ora_user.employees 
    
      SQL> alter system set audit_trail=none scope=spfile;
      SQL> startup force
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>