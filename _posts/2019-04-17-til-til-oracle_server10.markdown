---
layout: post
title: '[oracle] 오라클 서버 관리 - Oracle 네트워크 환경 구성 위한 기본 지식'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 10:12:17 +0900
lastmod: 2019-04-17 10:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - Oracle 네트워크 환경 구성 위한 기본 지식<br />

# Configure Oracle Network Environment
> Oracle 네트워크 환경 구성 위한 기본 지식 : 11gWS1 교재 6장<br>

## 서버측 설정
* 리스너 설정 : n개

## 클라이언트측 설정
* easy connect : sqlplus user/pw@ip:port/service_name
* local naming : sqlplus user/pw@network_alias         -> tnsnames.ora 파일 설정이 필요함
* 기타
 
## 서버측 설정 실습 : connect-time failover 및 connect-time load balance
###    
    --리스너 상태 확인 
    [orcl:~]$ ps -ef|grep lsnr      <-현재 리스너 상태 확인
    oracle   11705     1  0 08:27 ?        00:00:00 /u01/app/oracle/product/11.2.0/dbhome_1/bin/tnslsnr LISTENER -inherit
    oracle   12504 12467  0 10:21 pts/3    00:00:00 grep lsnr
    [orcl:~]$
###
    --현재 실행 중인 리스터 정지
    [orcl:~]$ lsnrctl stop LISTENER     <-LISTNER 리스너 정지
    
    LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 11-APR-2019 10:23:27
    
    Copyright (c) 1991, 2009, Oracle.  All rights reserved.
    
    Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=edydr1p0.us.oracle.com)(PORT=1521)))
    The command completed successfully
    [orcl:~]$
###
    --기존에 있던 리스너 삭제 후 수동으로 리스너 추가    
    [orcl:~]$ vi $ORACLE_HOME/network/admin/listener.ora

    vi 편집기 실행 후 아래 복사 붙여넣기(리스너 수동 세팅 방법임.)
    LISTENER =
      (DESCRIPTION =
        (ADDRESS = (PROTOCOL = TCP)(HOST = edydr1p0.us.oracle.com)(PORT = 1521))
      )
    
    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (GLOBAL_DBNAME = orcl)
          (ORACLE_HOME = /u01/app/oracle/product/11.2.0/dbhome_1)
          (SID_NAME = orcl)
        )
      )
    
    ADR_BASE_LISTENER = /u01/app/oracle
    
    L2 =
      (DESCRIPTION =
        (ADDRESS = (PROTOCOL = TCP)(HOST = edydr1p0.us.oracle.com)(PORT = 1621))
      )
    
    SID_LIST_L2 =
      (SID_LIST =
        (SID_DESC =
          (GLOBAL_DBNAME = orcl)
          (ORACLE_HOME = /u01/app/oracle/product/11.2.0/dbhome_1)
          (SID_NAME = orcl)
        )
      )
    
    ADR_BASE_L2 = /u01/app/oracle
    vi 편집기 종료
###    
      --리스너 다시 시작해보기
      [orcl:~]$ lsnrctl start LISTENER        
            LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 11-APR-2019 10:34:59
            
            Copyright (c) 1991, 2009, Oracle.  All rights reserved.
            
            Starting /u01/app/oracle/product/11.2.0/dbhome_1/bin/tnslsnr: please wait...
            
            TNSLSNR for Linux: Version 11.2.0.1.0 - Production
            System parameter file is /u01/app/oracle/product/11.2.0/dbhome_1/network/admin/listener.ora
            Log messages written to /u01/app/oracle/diag/tnslsnr/edydr1p0/listener/alert/log.xml
            Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=edydr1p0.us.oracle.com)(PORT=1521)))
            
            Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
            STATUS of the LISTENER
            ------------------------
            Alias                     LISTENER
            Version                   TNSLSNR for Linux: Version 11.2.0.1.0 - Production
            Start Date                11-APR-2019 10:34:59
            Uptime                    0 days 0 hr. 0 min. 0 sec
            Trace Level               off
            Security                  ON: Local OS Authentication
            SNMP                      OFF
            Listener Parameter File   /u01/app/oracle/product/11.2.0/dbhome_1/network/admin/listener.ora
            Listener Log File         /u01/app/oracle/diag/tnslsnr/edydr1p0/listener/alert/log.xml
            Listening Endpoints Summary...
              (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=edydr1p0.us.oracle.com)(PORT=1521)))
            Services Summary...
            Service "orcl" has 1 instance(s).
              Instance "orcl", status UNKNOWN, has 1 handler(s) for this service...
            The command completed successfully
            [orcl:~]$  
      
      [orcl:~]$ lsnrctl start L2
            LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 11-APR-2019 10:37:06
            
            Copyright (c) 1991, 2009, Oracle.  All rights reserved.
            
            Starting /u01/app/oracle/product/11.2.0/dbhome_1/bin/tnslsnr: please wait...
            
            TNSLSNR for Linux: Version 11.2.0.1.0 - Production
            System parameter file is /u01/app/oracle/product/11.2.0/dbhome_1/network/admin/listener.ora
            Log messages written to /u01/app/oracle/diag/tnslsnr/edydr1p0/l2/alert/log.xml
            Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=edydr1p0.us.oracle.com)(PORT=1621)))
            
            Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=edydr1p0.us.oracle.com)(PORT=1621)))
            STATUS of the LISTENER
            ------------------------
            Alias                     L2
            Version                   TNSLSNR for Linux: Version 11.2.0.1.0 - Production
            Start Date                11-APR-2019 10:37:06
            Uptime                    0 days 0 hr. 0 min. 0 sec
            Trace Level               off
            Security                  ON: Local OS Authentication
            SNMP                      OFF
            Listener Parameter File   /u01/app/oracle/product/11.2.0/dbhome_1/network/admin/listener.ora
            Listener Log File         /u01/app/oracle/diag/tnslsnr/edydr1p0/l2/alert/log.xml
            Listening Endpoints Summary...
              (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=edydr1p0.us.oracle.com)(PORT=1621)))
            Services Summary...
            Service "orcl" has 1 instance(s).
              Instance "orcl", status UNKNOWN, has 1 handler(s) for this service...
            The command completed successfully
            [orcl:~]$

      [orcl:~]$ export ORACLE_SID=orcl      <-
      [orcl:~]$ sqlplus / as sysdba         <-시스템계정 로그인 해보기
      SQL> startup force
            ORACLE instance started.
            
            Total System Global Area 1489829888 bytes
            Fixed Size                  1336624 bytes
            Variable Size             872418000 bytes
            Database Buffers          603979776 bytes
            Redo Buffers               12095488 bytes
            Database mounted.
            Database opened.
      SQL>

## 클라이언트 설정(Windows에서) 실습 : connect-time failover 및 connect-time load balance
* Oracle XE가 설치된 경우
###
      C:\Users\student> sqlplus ora_user/hong@192.168.56.101:1521/orcl      <-별도의 설정파일이 없는 easy connect 방식
      SQL> exit
    
      C:\Users\student> sqlplus ora_user/hong@192.168.56.101:1621/orcl
      SQL> exit
    
      C:\Users\student> notepad c:\oraclexe\app\oracle\product\11.2.0\server\network\admin\tnsnames.ora
    
      vi 편집기 시작 - 기존에 내용에서 아래 내용 추가하기
        _orcl =
          (DESCRIPTION =
            (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.56.101)(PORT = 1521))    <-1521포트가 죽어있다면 1621로 가게 된다.
            (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.56.101)(PORT = 1621))      즉, 둘중 한개가 문제가 생기면 다른게 작동. 오 좋다.
            (CONNECT_DATA =
              (SERVER = DEDICATED)
              (SERVICE_NAME = orcl)
            )
          )
      vi 편집기 종료
      
      C:\Users\student> sqlplus ora_user/hong@_orcl

* Oracle Instant Client 이용하는 경우
###

    C:\Users\student> notepad ora_user.bat

         set PATH=C:\Users\student\instantclient_12_1;%path%
         set TNS_ADMIN=C:\Users\student\instantclient_12_1

         sqlplus ora_user/hong@amu



    C:\Users\student> notepad C:\Users\student\instantclient_12_1\tnsnames.ora
        
        --노트패드 실행 후 들여쓰기 없이 써야 됩니다.
        amu =
          (DESCRIPTION =
            (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.56.101)(PORT = 1521))
            (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.56.101)(PORT = 1621))
            (CONNECT_DATA =
              (SERVER = DEDICATED)
              (SERVICE_NAME = orcl)
            )
          )
        
    C:\Users\student> ora_user
    
    SQL> exit


## Database Link 활용하기
      -> 새로운 명령 프롬프트 시작
    
      C:\Users\student> sqlplus ace29/me@70.12.114.184:1521/xe
    
      SQL> DROP DATABASE LINK mylink;
    
      SQL> CREATE DATABASE LINK mylink
           CONNECT TO ora_user
           IDENTIFIED BY hong
           USING '_orcl';
    
      SQL> show user
      USER is "ACE29"
    
      SQL> select * from kor_loan_status;           -- 에러
      SQL> select * from kor_loan_status@mylink;    -- 성공
    
      SQL> create synonym emp_orcl
           for employees@mylink;  
    
      SQL> col DEPARTMENT_NAME format a15
    
      SQL> select d.department_id, d.department_name, e.employee_id
           from emp_orcl e, departments d           <-e: ace 안에 VM 안의 orcl 데이터 , d:ace 안 데이터
           where e.department_id = d.department_id
           order by 1;

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>