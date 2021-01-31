---
layout: post
title: '[oracle] 오라클 서버 관리 - 유저 보안 관리'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 12:12:17 +0900
lastmod: 2019-04-17 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - 유저 보안 관리<br />

# Administetering User Security
> 유저 보안 관리 : 11gWS1 교재 8장<br>

## 알고 있는 내용
###
      - create user 명령 : 사용자명, 암호, 공간 할당, ...
    
      - grant 권한         to 사용자 : 시스템 권한 관리
        grant 권한 on 객체 to 사용자 : 객체 권한 관리

## 더 배울 내용
* Role : 권한 관리의 편의를 위해 활용
###    
        [orcl:~]$ export ORACLE_SID=orcl
        [orcl:~]$ sqlplus / as sysdba
    
        SQL>
    
        create user u1 identified by u1;
        create user u2 identified by u2;
        create user u3 identified by u3;
    
        create user p1 identified by p1;
        create user p2 identified by p2;
    
        create user a1 identified by a1;
        create user a2 identified by a2;
    
        SQL>
    
        create role user_role;
        create role dev_role;
        create role adm_role;
    
        SQL>
    
        grant create session, create table       to user_role;
        grant select on ora_user.kor_loan_status to user_role;
    
        grant user_role                         to dev_role;
        grant create view, create database link to dev_role;
        grant all on ora_user.kor_loan_status   to dev_role;
    
        grant dev_role         to adm_role;
        grant select any table to adm_role;
       
        SQL>
    
        grant user_role to u1, u2, u3;
        grant dev_role  to p1, p2;
        grant adm_role  to a1, a2;
    
        SQL>
    
        create user u4 identified by u4;
        grant user_role to u4;
     
        SQL> 
    
        grant create procedure
        to dev_role;
###
* Profile : 자원 사용 제한 및 암호 관리
###
        https://oracle-base.com/articles/misc/basic-security-measures-for-oracle#password-aging-expiration-and-history
    
        SQL> 
    
        select username, profile
        from dba_users
        where username like 'U%';
    
        CREATE PROFILE user_prof LIMIT
          FAILED_LOGIN_ATTEMPTS  3    -- Account locked after 3 failed logins.
          PASSWORD_LOCK_TIME     5    -- Number of days account is locked for. UNLIMITED required explicit unlock by DBA.
          PASSWORD_LIFE_TIME    30    -- Password expires after 90 days.
          PASSWORD_GRACE_TIME    3    -- Grace period for password expiration.
          PASSWORD_REUSE_TIME  120    -- Number of days until a specific password can be reused. UNLIMITED means never.
          PASSWORD_REUSE_MAX    10    -- The number of changes required before a password can be reused. UNLIMITED means never.
          sessions_per_user      3    -- 개
          cpu_per_session      100    -- 초 
          /
    
        ALTER USER u1 PROFILE user_prof;
        ALTER USER u4 PROFILE user_prof;


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>