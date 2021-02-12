---
layout: post
title: '[oracle] 오라클 서버 관리 - 데이터베이스 인스턴스 관리'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 07:12:17 +0900
lastmod: 2019-04-17 07:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - 데이터베이스 인스턴스 관리<br />

# Start & End Oracle Instance
> 데이터베이스 인스턴스 관리 : 11gWS1 교재 4장<br>

## 인스턴스 관리를 위한 기본 지식
```
    1. Management Framework : Linstenser, Database Control, Instance
    2. 파라미터 및 파라미터 파일
      - 파라미터 -> 
      - 파라미터 파일 -> 
    3. Startup 및 Shutdown
      - Startup 옵션 : startup (nomount | mount | open) (restrict)
      - Shutdown 옵션 : shutdown (normal | transacitional | immediate | abort)
    4. Diagnostic Tools (진단도구)
      - Diagnostic File -> alert_sid.log : background_dump_dest 파라미터가 가리키는 위치
                        -> BGP 생성 file : background_dump_dest 파라미터가 가리키는 위치
                        -> Server Process 생성 파일 : user_dump_dest 파라미터가 가리키는 위치

      - Meta Data 확인용 뷰 -> Static Data Rictionary View : user_***, all_***, dba_***
                          -> Dynamic Performance View    : v$***
        cf. https://docs.oracle.com/cd/E11882_01/server.112/e40540/intro.htm#CNCPT958
```

## DBA 접속 방법
* SQL 상태에서 conn / as sysdba
* [orcl:~] 상태에서 sqlplus / as sysdba

## 실습
```
    Last login: Mon Apr  8 09:20:41 2019
    [orcl:~]$ echo $ORACLE_SID
    orcl
    
    [orcl:~]$ who am i
    oracle   pts/2        Apr  8 09:41 (000.000.000.000)
    
    [orcl:~]$ export ORACLE_SID=prod
    [prod:~]$ echo $ORACLE_SID
    prod
    
    [prod:~]$ sqlplus / as sysdba
    
    SQL*Plus: Release 11.2.0.1.0 Production on Mon Apr 8 10:22:39 2019
    
    Copyright (c) 1982, 2009, Oracle.  All rights reserved.
    
    Connected to an idle instance.
    
    SQL> startup              <- prod 서버가 아직 없어서 에러
    ORA-01078: failure in processing system parameters
    LRM-00109: could not open parameter file '/u01/app/oracle/product/11.2.0/dbhome_                                               1/dbs/initprod.ora'
    
    SQL> exit
    Disconnected
    
    [prod:~]$ export ORACLE_SID=orcl
    [orcl:~]$ echo $ORACLE_SID
    orcl
    
    [orcl:~]$ sqlplus / as sysdba
    
    SQL*Plus: Release 11.2.0.1.0 Production on Mon Apr 8 10:23:19 2019
    
    Copyright (c) 1982, 2009, Oracle.  All rights reserved.
    
    Connected to an idle instance.
    
    SQL> startup force      <- shutdown abort + startup
   
    
    ORACLE instance started.
    
    Total System Global Area 1489829888 bytes
    Fixed Size                  1336624 bytes
    Variable Size             872418000 bytes
    Database Buffers          603979776 bytes
    Redo Buffers               12095488 bytes
    Database mounted.
    Database opened.
    
    SQL> !ps -ef|grep orcl
    oracle    5421     1  0 10:23 ?        00:00:00 ora_pmon_orcl
    oracle    5423     1  2 10:23 ?        00:00:00 ora_vktm_orcl
    oracle    5427     1  0 10:23 ?        00:00:00 ora_gen0_orcl
    oracle    5429     1  0 10:23 ?        00:00:00 ora_diag_orcl
    oracle    5431     1  0 10:23 ?        00:00:00 ora_dbrm_orcl
    oracle    5433     1  0 10:23 ?        00:00:00 ora_psp0_orcl
    oracle    5435     1  0 10:23 ?        00:00:00 ora_dia0_orcl
    oracle    5437     1  5 10:23 ?        00:00:00 ora_mman_orcl
    oracle    5439     1  0 10:23 ?        00:00:00 ora_dbw0_orcl
    oracle    5441     1  0 10:23 ?        00:00:00 ora_lgwr_orcl
    oracle    5443     1  0 10:23 ?        00:00:00 ora_ckpt_orcl
    oracle    5445     1  0 10:23 ?        00:00:00 ora_smon_orcl
    oracle    5447     1  0 10:23 ?        00:00:00 ora_reco_orcl
    oracle    5449     1  3 10:23 ?        00:00:00 ora_mmon_orcl
    oracle    5451     1  0 10:23 ?        00:00:00 ora_mmnl_orcl
    oracle    5453     1  0 10:23 ?        00:00:00 ora_d000_orcl
    oracle    5455     1  0 10:23 ?        00:00:00 ora_s000_orcl
    oracle    5493  5395  9 10:23 ?        00:00:01 oracleorcl (DESCRIPTION=(LOCAL=YES)(ADDRESS=(PROTOCOL=beq)))
    oracle    5495     1  0 10:23 ?        00:00:00 ora_p000_orcl
    oracle    5497     1  0 10:23 ?        00:00:00 ora_p001_orcl
    oracle    5499     1  0 10:23 ?        00:00:00 ora_qmnc_orcl
    oracle    5505     1 14 10:23 ?        00:00:00 ora_m002_orcl
    oracle    5511     1  0 10:23 ?        00:00:00 ora_cjq0_orcl
    oracle    5512  5395  0 10:23 pts/2    00:00:00 /bin/bash -c ps -ef|grep orcl
    oracle    5514  5512  0 10:23 pts/2    00:00:00 grep orcl
    
    SQL> startup force nomount
    ORACLE instance started.
    
    Total System Global Area 1489829888 bytes
    Fixed Size                  1336624 bytes
    Variable Size             872418000 bytes
    Database Buffers          603979776 bytes
    Redo Buffers               12095488 bytes
    
    SQL> select instance_name, status from v$instance;
    
    INSTANCE_NAME    STATUS
    ---------------- ------------
    orcl             STARTED
    
    SQL> startup force mount
    ORACLE instance started.
    
    Total System Global Area 1489829888 bytes
    Fixed Size                  1336624 bytes
    Variable Size             872418000 bytes
    Database Buffers          603979776 bytes
    Redo Buffers               12095488 bytes
    Database mounted.
    
    SQL> select instance_name, status from v$instance;
    
    INSTANCE_NAME    STATUS
    ---------------- ------------
    orcl             MOUNTED
    
    SQL> startup force open
    ORACLE instance started.
    
    Total System Global Area 1489829888 bytes
    Fixed Size                  1336624 bytes
    Variable Size             872418000 bytes
    Database Buffers          603979776 bytes
    Redo Buffers               12095488 bytes
    Database mounted.
    Database opened.
    
    SQL> select instance_name, status from v$instance;
    
    INSTANCE_NAME    STATUS
    ---------------- ------------
    orcl             OPEN
    
    SQL> startup force nomount
    ORACLE instance started.
    
    Total System Global Area 1489829888 bytes
    Fixed Size                  1336624 bytes
    Variable Size             872418000 bytes
    Database Buffers          603979776 bytes
    Redo Buffers               12095488 bytes
    
    SQL> alter database mount;
    
    Database altered.
    
    SQL> alter database open;
    
    Database altered.
    
    SQL> startup force restrict
    ORACLE instance started.
    
    Total System Global Area 1489829888 bytes
    Fixed Size                  1336624 bytes
    Variable Size             872418000 bytes
    Database Buffers          603979776 bytes
    Redo Buffers               12095488 bytes
    Database mounted.
    Database opened.
    
    SQL> select instance_name, logins
           from v$instance;  2
    
    INSTANCE_NAME    LOGINS
    ---------------- ----------
    orcl             RESTRICTED     <- restricted session 권한이 있어야 접속이 가능한 상태
    
    SQL> alter system disable restricted session;
    
    System altered.
    
    SQL> select instance_name, logins
           from v$instance;  2
    
    INSTANCE_NAME    LOGINS
    ---------------- ----------
    orcl             ALLOWED    <- create session 권한이 있어야 접속이 가능한 상태
    
    SQL> shutdown immediate
    Database closed.
    Database dismounted.
    ORACLE instance shut down.
    
    SQL> exit
    Disconnected from Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - Production
    With the Partitioning, OLAP, Data Mining and Real Application Testing options
    
    [orcl:~]$ sqlplus ora_user/hong
    
    SQL*Plus: Release 11.2.0.1.0 Production on Mon Apr 8 10:27:37 2019
    
    Copyright (c) 1982, 2009, Oracle.  All rights reserved.
    
    ERROR:
    ORA-01034: ORACLE not available     <- 셧다운되어 있으므로 DBA에게 서버를 시작시켜달라고 요청해야 함
    ORA-27101: shared memory realm does not exist
    Linux Error: 2: No such file or directory
    Process ID: 0
    Session ID: 0 Serial number: 0
        
    SP2-0157: unable to CONNECT to ORACLE after 3 attempts, exiting SQL*Plus
```

## 오라클 데이터베이스 구조 확인
```
    [orcl:~]$ sqlplus / as sysdba
    
    SQL*Plus: Release 11.2.0.1.0 Production on Mon Apr 8 10:29:28 2019
    
    Copyright (c) 1982, 2009, Oracle.  All rights reserved.
    
    Connected to an idle instance.  <- 인스턴스 셧다운 상태임을 표시함
    
    SQL> startup
    ORACLE instance started.
    
    Total System Global Area 1489829888 bytes
    Fixed Size                  1336624 bytes
    Variable Size             872418000 bytes
    Database Buffers          603979776 bytes
    Redo Buffers               12095488 bytes
    Database mounted.
    Database opened.

               ~ 쿼리 결과를 잘 해석하려면 문서를 확인해야 함 : https://docs.oracle.com/cd/E11882_01/server.112/e40402/dynviews_part.htm#i403961

    SQL> select *      from v$database;
    SQL> select name   from v$datafile;
    SQL> select member from v$logfile;
    SQL> select name   from v$controlfile;

    SQL> select * from v$instance;
    SQL> select * from v$sga;
    SQL> select paddr, name      <- paddr 컬럼이 00인 프로세스는 시작되지 않은 것임
         from v$bgprocess
         order by paddr;
```

## 테이블 스페이스 생성
```
    SQL> select * from v$tablespace;
    
           TS# NAME                           INC BIG FLA ENC
    ---------- ------------------------------ --- --- --- ---
             0 SYSTEM                         YES NO  YES
             1 SYSAUX                         YES NO  YES
             2 UNDOTBS1                       YES NO  YES
             4 USERS                          YES NO  YES
             3 TEMP                           NO  NO  YES
             6 EXAMPLE                        YES NO  YES
             7 MYTS                           YES NO  YES
    
    7 rows selected.
    
    SQL> !vi tbs.sql
         
         vi 편집기 실행
         set linesize 100

         col tablespace_name format a30
         col file_name format a60
 
         select tablespace_name, file_name 
         from dba_data_files;

         clear col
  
         set linesize 400
         vi 편집기 종료
    
    SQL> @tbs
    
    TABLESPACE_NAME                FILE_NAME
    ------------------------------ ------------------------------------------------------------
    USERS                          /u01/app/oracle/oradata/orcl/users01.dbf
    UNDOTBS1                       /u01/app/oracle/oradata/orcl/undotbs01.dbf
    SYSAUX                         /u01/app/oracle/oradata/orcl/sysaux01.dbf
    SYSTEM                         /u01/app/oracle/oradata/orcl/system01.dbf
    EXAMPLE                        /u01/app/oracle/oradata/orcl/example01.dbf
    MYTS                           /u01/app/oracle/oradata/orcl/myts.dbf
    
    6 rows selected.
    
    SQL> 
```

## 사용자 관리
```
    --생성
    SQL> create tablespace app_ts
         datafile '/u01/app/oracle/oradata/orcl/app_ts01.dbf' size 100m,
                  '/u01/app/oracle/oradata/orcl/app_ts02.dbf' size 100m autoextend on next 10m maxsize 2G;
    
         Tablespace created.
    
    
    SQL> alter tablespace app_ts
         add datafile '/u01/app/oracle/oradata/orcl/app_ts03.dbf' size 100m;
    
         Tablespace altered.
    
    SQL> alter database '/u01/app/oracle/oradata/orcl/app_ts01.dbf' autoextend on next 10m maxsize 20G;
    
         Database altered.

    SQL> @tbs
    
         TABLESPACE_NAME                FILE_NAME
         ------------------------------ ------------------------------------------------------------
         USERS                          /u01/app/oracle/oradata/orcl/users01.dbf
         UNDOTBS1                       /u01/app/oracle/oradata/orcl/undotbs01.dbf
         SYSAUX                         /u01/app/oracle/oradata/orcl/sysaux01.dbf
         SYSTEM                         /u01/app/oracle/oradata/orcl/system01.dbf
         EXAMPLE                        /u01/app/oracle/oradata/orcl/example01.dbf
         MYTS                           /u01/app/oracle/oradata/orcl/myts.dbf
         APP_TS                         /u01/app/oracle/oradata/orcl/app_ts01.dbf
         APP_TS                         /u01/app/oracle/oradata/orcl/app_ts02.dbf
         APP_TS                         /u01/app/oracle/oradata/orcl/app_ts03.dbf
        
         9 rows selected.
```

```
    --삭제
    SQL> drop tablespace app_ts
         including contents and datafiles;

         Tablespace dropped.
    
    SQL> @tbs
    
         TABLESPACE_NAME                FILE_NAME
         ------------------------------ ------------------------------------------------------------
         USERS                          /u01/app/oracle/oradata/orcl/users01.dbf
         UNDOTBS1                       /u01/app/oracle/oradata/orcl/undotbs01.dbf
         SYSAUX                         /u01/app/oracle/oradata/orcl/sysaux01.dbf
         SYSTEM                         /u01/app/oracle/oradata/orcl/system01.dbf
         EXAMPLE                        /u01/app/oracle/oradata/orcl/example01.dbf
         MYTS                           /u01/app/oracle/oradata/orcl/myts.dbf
    
         6 rows selected.
```
    
## 사용자 관리
```
    SQL> show parameter name
    
        NAME_COL_PLUS_SHOW_PARAM                                                         TYPE
        -------------------------------------------------------------------------------- -----------
        VALUE_COL_PLUS_SHOW_PARAM
        ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
        db_file_name_convert                                                             string
        
        db_name                                                                          string
        orcl
        db_unique_name                                                                   string
        orcl
        global_names                                                                     boolean
        FALSE
        instance_name                                                                    string
        
        NAME_COL_PLUS_SHOW_PARAM                                                         TYPE
        -------------------------------------------------------------------------------- -----------
        VALUE_COL_PLUS_SHOW_PARAM
        ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
        orcl
        lock_name_space                                                                  string
        
        log_file_name_convert                                                            string
        
        service_names                                                                    string
        orcl
```

```
    SQL> select username, account_status
      2  from dba_users
      3  order by username;
    
        USERNAME                       ACCOUNT_STATUS
        ------------------------------ --------------------------------
        ANONYMOUS                      EXPIRED & LOCKED
        APEX_030200                    EXPIRED & LOCKED
        APEX_PUBLIC_USER               EXPIRED & LOCKED
        APPQOSSYS                      EXPIRED & LOCKED
        BI                             EXPIRED & LOCKED
        CTXSYS                         EXPIRED & LOCKED
        DBSNMP                         OPEN
        DIP                            EXPIRED & LOCKED
        EXFSYS                         EXPIRED & LOCKED
        FLOWS_FILES                    EXPIRED & LOCKED
        HR                             EXPIRED & LOCKED
        
        USERNAME                       ACCOUNT_STATUS
        ------------------------------ --------------------------------
        IX                             EXPIRED & LOCKED
        MDDATA                         EXPIRED & LOCKED
        MDSYS                          EXPIRED & LOCKED
        MGMT_VIEW                      OPEN
        OE                             EXPIRED & LOCKED
        OLAPSYS                        EXPIRED & LOCKED
        ORACLE_OCM                     EXPIRED & LOCKED
        ORA_USER                       OPEN
        ORDDATA                        EXPIRED & LOCKED
        ORDPLUGINS                     EXPIRED & LOCKED
        ORDSYS                         EXPIRED & LOCKED
        
        USERNAME                       ACCOUNT_STATUS
        ------------------------------ --------------------------------
        OUTLN                          EXPIRED & LOCKED
        OWBSYS                         EXPIRED & LOCKED
        OWBSYS_AUDIT                   EXPIRED & LOCKED
        PM                             EXPIRED & LOCKED
        SCOTT                          EXPIRED & LOCKED
        SH                             EXPIRED & LOCKED
        SI_INFORMTN_SCHEMA             EXPIRED & LOCKED
        SPATIAL_CSW_ADMIN_USR          EXPIRED & LOCKED
        SPATIAL_WFS_ADMIN_USR          EXPIRED & LOCKED
        SYS                            OPEN
        SYSMAN                         OPEN
        
        USERNAME                       ACCOUNT_STATUS
        ------------------------------ --------------------------------
        SYSTEM                         OPEN
        WMSYS                          EXPIRED & LOCKED
        XDB                            EXPIRED & LOCKED
        XS$NULL                        EXPIRED & LOCKED
        
        37 rows selected.
```

```
    SQL> alter user hr
      2  identified by hr
      3  account unlock;
    
        User altered.
```

```
    SQL> select distinct privilege          <-관리 가능한 시스템 권한 @@@
      2  from dba_sys_privs
      3  order by 1;
    
        PRIVILEGE
        ----------------------------------------
        ADMINISTER ANY SQL TUNING SET
        ADMINISTER DATABASE TRIGGER
        ADMINISTER RESOURCE MANAGER
        ADMINISTER SQL MANAGEMENT OBJECT
        ADMINISTER SQL TUNING SET
        ADVISOR
        ALTER ANY ASSEMBLY
        ALTER ANY CLUSTER
        ALTER ANY CUBE
        ALTER ANY CUBE DIMENSION
        ALTER ANY DIMENSION
        
        PRIVILEGE
        ----------------------------------------
        ALTER ANY EDITION
        ALTER ANY EVALUATION CONTEXT
        ALTER ANY INDEX
        ALTER ANY INDEXTYPE
        ALTER ANY LIBRARY
        ALTER ANY MATERIALIZED VIEW
        ALTER ANY MINING MODEL
        ALTER ANY OPERATOR
        ALTER ANY OUTLINE
        ALTER ANY PROCEDURE
        ALTER ANY ROLE
        
        PRIVILEGE
        ----------------------------------------
        ALTER ANY RULE
        ALTER ANY RULE SET
        ALTER ANY SEQUENCE
        ALTER ANY SQL PROFILE
        ALTER ANY TABLE
        ALTER ANY TRIGGER
        ALTER ANY TYPE
        ALTER DATABASE
        ALTER PROFILE
        ALTER RESOURCE COST
        ALTER ROLLBACK SEGMENT
        
        PRIVILEGE
        ----------------------------------------
        ALTER SESSION
        ALTER SYSTEM
        ALTER TABLESPACE
        ALTER USER
        ANALYZE ANY
        ANALYZE ANY DICTIONARY
        AUDIT ANY
        AUDIT SYSTEM
        BACKUP ANY TABLE
        BECOME USER
        CHANGE NOTIFICATION
        
        PRIVILEGE
        ----------------------------------------
        COMMENT ANY MINING MODEL
        COMMENT ANY TABLE
        CREATE ANY ASSEMBLY
        CREATE ANY CLUSTER
        CREATE ANY CONTEXT
        CREATE ANY CUBE
        CREATE ANY CUBE BUILD PROCESS
        CREATE ANY CUBE DIMENSION
        CREATE ANY DIMENSION
        CREATE ANY DIRECTORY
        CREATE ANY EDITION
        
        PRIVILEGE
        ----------------------------------------
        CREATE ANY EVALUATION CONTEXT
        CREATE ANY INDEX
        CREATE ANY INDEXTYPE
        CREATE ANY JOB
        CREATE ANY LIBRARY
        CREATE ANY MATERIALIZED VIEW
        CREATE ANY MEASURE FOLDER
        CREATE ANY MINING MODEL
        CREATE ANY OPERATOR
        CREATE ANY OUTLINE
        CREATE ANY PROCEDURE
        
        PRIVILEGE
        ----------------------------------------
        CREATE ANY RULE
        CREATE ANY RULE SET
        CREATE ANY SEQUENCE
        CREATE ANY SQL PROFILE
        CREATE ANY SYNONYM
        CREATE ANY TABLE
        CREATE ANY TRIGGER
        CREATE ANY TYPE
        CREATE ANY VIEW
        CREATE ASSEMBLY
        CREATE CLUSTER
        
        PRIVILEGE
        ----------------------------------------
        CREATE CUBE
        CREATE CUBE BUILD PROCESS
        CREATE CUBE DIMENSION
        CREATE DATABASE LINK
        CREATE DIMENSION
        CREATE EVALUATION CONTEXT
        CREATE EXTERNAL JOB
        CREATE INDEXTYPE
        CREATE JOB
        CREATE LIBRARY
        CREATE MATERIALIZED VIEW
        
        PRIVILEGE
        ----------------------------------------
        CREATE MEASURE FOLDER
        CREATE MINING MODEL
        CREATE OPERATOR
        CREATE PROCEDURE
        CREATE PROFILE
        CREATE PUBLIC DATABASE LINK
        CREATE PUBLIC SYNONYM
        CREATE ROLE
        CREATE ROLLBACK SEGMENT
        CREATE RULE
        CREATE RULE SET
        
        PRIVILEGE
        ----------------------------------------
        CREATE SEQUENCE
        CREATE SESSION
        CREATE SYNONYM
        CREATE TABLE
        CREATE TABLESPACE
        CREATE TRIGGER
        CREATE TYPE
        CREATE USER
        CREATE VIEW
        DEBUG ANY PROCEDURE
        DEBUG CONNECT SESSION
        
        PRIVILEGE
        ----------------------------------------
        DELETE ANY CUBE DIMENSION
        DELETE ANY MEASURE FOLDER
        DELETE ANY TABLE
        DEQUEUE ANY QUEUE
        DROP ANY ASSEMBLY
        DROP ANY CLUSTER
        DROP ANY CONTEXT
        DROP ANY CUBE
        DROP ANY CUBE BUILD PROCESS
        DROP ANY CUBE DIMENSION
        DROP ANY DIMENSION
        
        PRIVILEGE
        ----------------------------------------
        DROP ANY DIRECTORY
        DROP ANY EDITION
        DROP ANY EVALUATION CONTEXT
        DROP ANY INDEX
        DROP ANY INDEXTYPE
        DROP ANY LIBRARY
        DROP ANY MATERIALIZED VIEW
        DROP ANY MEASURE FOLDER
        DROP ANY MINING MODEL
        DROP ANY OPERATOR
        DROP ANY OUTLINE
        
        PRIVILEGE
        ----------------------------------------
        DROP ANY PROCEDURE
        DROP ANY ROLE
        DROP ANY RULE
        DROP ANY RULE SET
        DROP ANY SEQUENCE
        DROP ANY SQL PROFILE
        DROP ANY SYNONYM
        DROP ANY TABLE
        DROP ANY TRIGGER
        DROP ANY TYPE
        DROP ANY VIEW
        
        PRIVILEGE
        ----------------------------------------
        DROP PROFILE
        DROP PUBLIC DATABASE LINK
        DROP PUBLIC SYNONYM
        DROP ROLLBACK SEGMENT
        DROP TABLESPACE
        DROP USER
        ENQUEUE ANY QUEUE
        EXECUTE ANY ASSEMBLY
        EXECUTE ANY CLASS
        EXECUTE ANY EVALUATION CONTEXT
        EXECUTE ANY INDEXTYPE
        
        PRIVILEGE
        ----------------------------------------
        EXECUTE ANY LIBRARY
        EXECUTE ANY OPERATOR
        EXECUTE ANY PROCEDURE
        EXECUTE ANY PROGRAM
        EXECUTE ANY RULE
        EXECUTE ANY RULE SET
        EXECUTE ANY TYPE
        EXECUTE ASSEMBLY
        EXPORT FULL DATABASE
        FLASHBACK ANY TABLE
        FLASHBACK ARCHIVE ADMINISTER
        
        PRIVILEGE
        ----------------------------------------
        FORCE ANY TRANSACTION
        FORCE TRANSACTION
        GLOBAL QUERY REWRITE
        GRANT ANY OBJECT PRIVILEGE
        GRANT ANY PRIVILEGE
        GRANT ANY ROLE
        IMPORT FULL DATABASE
        INSERT ANY CUBE DIMENSION
        INSERT ANY MEASURE FOLDER
        INSERT ANY TABLE
        LOCK ANY TABLE
        
        PRIVILEGE
        ----------------------------------------
        MANAGE ANY FILE GROUP
        MANAGE ANY QUEUE
        MANAGE FILE GROUP
        MANAGE SCHEDULER
        MANAGE TABLESPACE
        MERGE ANY VIEW
        ON COMMIT REFRESH
        QUERY REWRITE
        READ ANY FILE GROUP
        RESTRICTED SESSION
        RESUMABLE
        
        PRIVILEGE
        ----------------------------------------
        SELECT ANY CUBE
        SELECT ANY CUBE DIMENSION
        SELECT ANY DICTIONARY
        SELECT ANY MINING MODEL
        SELECT ANY SEQUENCE
        SELECT ANY TABLE
        SELECT ANY TRANSACTION
        UNDER ANY TABLE
        UNDER ANY TYPE
        UNDER ANY VIEW
        UNLIMITED TABLESPACE
        
        PRIVILEGE
        ----------------------------------------
        UPDATE ANY CUBE
        UPDATE ANY CUBE BUILD PROCESS
        UPDATE ANY CUBE DIMENSION
        UPDATE ANY TABLE
        
        202 rows selected.
```

```
    SQL> create user bts
         identified by army
         default tablespace example
         temporary tablespace temp
         quota 10m on users
         quota unlimited on example;
    
        User created.
        
    SQL> grant create session, create table to bts;
        
        Grant succeeded.
        
    SQL> conn bts/army
    
        Connected.
        
    SQL> create table t1 (no number);
        
        Table created.
        
    SQL> create table t2 (no number) tablespace users;
        
        Table created.
        
    SQL> select table_name, tablespace_name
           from user_tables;
        
        TABLE_NAME                     TABLESPACE_NAME
        ------------------------------ ------------------------------
        T2                             USERS
        T1                             EXAMPLE
        
    SQL> alter table t1 move tablespace users;
        
        Table altered.
        
    SQL> select table_name, tablespace_name
           from user_tables;
        
        TABLE_NAME                     TABLESPACE_NAME
        ------------------------------ ------------------------------
        T1                             USERS
        T2                             USERS
        
    SQL>
```
    
## DBCA 방식이 아닌 수동으로 create database 명령으로 데이터베이스 생성
* https://docs.oracle.com/cd/E11882_01/server.112/e25494/create.htm#ADMIN11073
```
    0.디렉토리 및 파라미터 파일 생성 
    
       [orcl:~]$ vi + $ORACLE_HOME/sqlplus/admin/glogin.sql 
            
            vi 편집기 실행
            define _editor=vi    <-- 마지막줄에 추가하기
            vi 편집기 종료
    
       [orcl:~]$ vi /etc/oratab

            orcl:/u01/app/oracle/product/11.2.0/dbhome_1:N
            prod:/u01/app/oracle/product/11.2.0/dbhome_1:N
    
       [orcl:~]$ rm -rf $ORACLE_BASE/oradata/prod       <-prod 폴더 삭제
       [orcl:~]$ mkdir -p $ORACLE_BASE/oradata/prod     <-prod 디렉도리 생성
       [orcl:~]$ ls -lR $ORACLE_BASE/oradata
    
            /u01/app/oracle/oradata:
            total 8
            drwxr-x--- 2 oracle oinstall 4096 Apr  8 11:13 orcl
            drwxr-xr-x 2 oracle oinstall 4096 Apr  8 13:09 prod
            
            /u01/app/oracle/oradata/orcl:
            total 1713480
            -rw-r----- 1 oracle oinstall   9748480 Apr  8 13:13 control01.ctl
            -rw-r----- 1 oracle oinstall   9748480 Apr  8 13:13 control02.ctl
            -rw-r----- 1 oracle oinstall 104865792 Apr  8 12:05 example01.dbf
            -rw-r----- 1 oracle oinstall 104865792 Apr  8 12:05 myts.dbf
            -rw-r----- 1 oracle oinstall  52429312 Apr  8 12:00 redo01.log
            -rw-r----- 1 oracle oinstall  52429312 Apr  8 13:13 redo02.log
            -rw-r----- 1 oracle oinstall  52429312 Apr  8 10:29 redo03.log
            -rw-r----- 1 oracle oinstall 534781952 Apr  8 13:13 sysaux01.dbf
            -rw-r----- 1 oracle oinstall 713039872 Apr  8 13:10 system01.dbf
            -rw-r----- 1 oracle oinstall  30416896 Apr  8 10:26 temp01.dbf
            -rw-r----- 1 oracle oinstall 110108672 Apr  8 13:13 undotbs01.dbf
            -rw-r----- 1 oracle oinstall   5251072 Apr  8 12:05 users01.dbf
            
            /u01/app/oracle/oradata/prod:
            total 0
    
       [orcl:~]$ export ORACLE_SID=prod     <-내가 작업할 데이터베이스를 prod로 변경
    
       [prod:~]$ vi $ORACLE_HOME/dbs/initprod.ora   <-prod란 이름의 정해진 경로에 정해진 이름으로 파일 생성. 이를 파라미터 파일 생성이라고 한다.
     
            vi 편집기 실행
            db_name       = prod
            instance_name = prod
            compatible    = 11.2.0
            processes     = 100     <-오라클 서버에 붙을 수 있는 갯수
            
            undo_management = auto
            undo_tablespace = undotbs01
        
            db_cache_size    = 64m
            shared_pool_size = 72m
            db_block_size    = 4096
        
            control_files = ('$ORACLE_BASE/oradata/prod/control01.ctl',
                             '$ORACLE_BASE/oradata/prod/control02.ctl')
        
            remote_login_passwordfile = exclusive
            vi 편집기 종료
```

```
    1.Software 시작
    
       [prod:~]$ sqlplus / as sysdba
    
       SQL> startup nomount            
            ORACLE instance started.
            
            Total System Global Area  175775744 bytes
            Fixed Size                  1335248 bytes
            Variable Size             100663344 bytes
            Database Buffers           67108864 bytes
            Redo Buffers                6668288 bytes

       SQL> select instance_name, status from v$instance;
            INSTANCE_NAME                    STATUS
            -------------------------------- ------------------------
            prod                             STARTED
        
       SQL> !ps -ef|grep smon
            oracle   10484     1  0 18:17 ?        00:00:00 asm_smon_+ASM
            oracle    4556     1  0 15:53 ?        00:00:00 ora_smon_prod
            oracle    8148     1  0 14:52 ?        00:00:01 ora_smon_orcl
```

```
    2.Create database 명령 실행 
    
       SQL> create database prod        --시간 소요 됨
            logfile group 1 ('$ORACLE_BASE/oradata/prod/redo01_a.log', 
                             '$ORACLE_BASE/oradata/prod/redo01_b.log') size 20m,
                    group 2 ('$ORACLE_BASE/oradata/prod/redo02_a.log', 
                             '$ORACLE_BASE/oradata/prod/redo02_b.log') size 20m
            datafile '$ORACLE_BASE/oradata/prod/system01.dbf' size 200m autoextend on next 20m maxsize unlimited 
            sysaux datafile '$ORACLE_BASE/oradata/prod/sysaux01.dbf' size 200m autoextend on next 20m maxsize unlimited 
            undo tablespace undotbs01 datafile '$ORACLE_BASE/oradata/prod/undotbs01.dbf' size 100m autoextend on next 20m maxsize 2G 
            default temporary tablespace temp tempfile '$ORACLE_BASE/oradata/prod/temp01.tmp' size 20m autoextend on next 20m maxsize 2G;
       
            Database created.
        
       SQL> !ls -l $ORACLE_BASE/oradata/prod
       SQL> select instance_name, status from v$instance;   --인스턴스 상태 확인 
             INSTANCE_NAME                    STATUS
             -------------------------------- ------------------------
             prod                             OPEN
```

```
    3.필수 Script 수행
    
       SQL> alter user sys identified by oracle;        -- 기본 암호 : change_on_install -> 새로운 패스워드
            User altered.
       SQL> alter user system identified by oracle;     -- 기본 암호 : manager -> 새로운 패스워드
       SQL> ed after_db_create.sql       --반드시 돌려야하는 스크립트 생성
            
            vi 편집기 실행
            conn sys/oracle as sysdba
            @?/rdbms/admin/catalog.sql
            @?/rdbms/admin/catproc.sql
        
            conn system/oracle
            @?/sqlplus/admin/pupbld.sql
            vi 편집기 종료
    
       SQL> @ after_db_create.sql       --시간이 다소 소요 됨
       
            데이터베이스 생성하는 단계로 엄청난 명령어가 실행된다. 
            중간중간에 가끔 에러?같은거는 drop이 실행됐는데 drop할게 없어서 뜨는 명령어니 무시해도 된다.
            SQL> -- End of pupbld.sql  --이게 뜨면 데이터베이스 생성 완료
            
            DB 생성 완료!!!!! 축하합니다.
            
       SQL> exit
```

```
    Test
       [prod:~]$ ps -ef|grep smon
    
            oracle   24145     1  0 18:06 ?        00:00:00 ora_smon_prod   <-수동으로 만든거
            oracle   22122     1  0 17:52 ?        00:00:00 ora_smon_orcl   <-DBCA 만든거
        
       [prod:~]$ export ORACLE_SID=orcl
       [prod:~]$ sqlplus / as sysdba
       
       SQL> startup force
            ORACLE instance started.
            
            Total System Global Area 1489829888 bytes
            Fixed Size                  1336624 bytes
            Variable Size             872418000 bytes
            Database Buffers          603979776 bytes
            Redo Buffers               12095488 bytes
            Database mounted.
            Database opened.

       SQL> select instance_name from v$instance;
            INSTANCE_NAME
            ----------------
            orcl

       SQL> exit
    
       [ocrl:~]$ export ORACLE_SID=prod         --sql을 나갔다가 실행하면 초기값을 ocrl로 줬기 때문에 prod로 바꿔줘야한다.
       [prod:~]$ sqlplus / as sysdba
       
       SQL> startup force
            ORACLE instance started.
            
            Total System Global Area  175775744 bytes
            Fixed Size                  1335248 bytes
            Variable Size             100663344 bytes
            Database Buffers           67108864 bytes
            Redo Buffers                6668288 bytes
            Database mounted.
            Database opened.
            
       SQL> select instance_name from v$instance;
            INSTANCE_NAME
            ----------------
            prod

       SQL> exit
    
      [prod:~]$ vi /etc/oratab      --DBCA에서 DB삭제시 수동으로 만든 prod도 뜰 수 있게하는 작업
            vi 편집기 실행
            orcl:/u01/app/oracle/product/11.2.0/dbhome_1:N  --기존에 만들어져 있음 
            prod:/u01/app/oracle/product/11.2.0/dbhome_1:N  --추가하기
            vi 편집기 종료
```

```
      [prod:~]$ export ORACLE_SID=prod      --prod로 접속
      [prod:~]$ sqlplus / as sysdba         --sqlplus 접속
      
      SQL> select name from v$datafile;
        
            NAME
            --------------------------------------------------------------------------------
            /u01/app/oracle/oradata/prod/system01.dbf
            /u01/app/oracle/oradata/prod/sysaux01.dbf
            /u01/app/oracle/oradata/prod/undotbs01.dbf

      SQL> --prod 데이터베이스에 새로운 user 생성
            create tablespace users01
            datafile '/u01/app/oracle/oradata/prod/users01.dbf' size 10m;
            
            Tablespace created.
            
            create tablespace users02
            datafile '/u01/app/oracle/oradata/prod/users02.dbf' size 10m;
            
            Tablespace created.
            
      SQL> create user itzy
           identified by jyp
           default tablespace users01
           temporary tablespace temp
           quota 1m on users01
           quota 1m on users02;
           
           User created.
           
      SQL> grant connect, resource
           to itzy;
           
           Grant succeeded.
       
      SQL> exit
```

## 파라미터 파일 실습
```
    --가상머신 터미널에서 실행 
    
    [orcl:~]$ . oraenv                  <-가상환경 변경 명령문
        ORACLE_SID = [orcl] ? prod      <-orcl에서 prod로 변경
        The Oracle base for ORACLE_HOME=/u01/app/oracle/product/11.2.0/dbhome_1 is /u01/app/oracle

    [prod:~]$ sqlplus / as sysdba       <-sysdba로 접속
    
    SQL*Plus: Release 11.2.0.1.0 Production on Tue Apr 9 14:10:44 2019
    
    Copyright (c) 1982, 2009, Oracle.  All rights reserved.
    
    
    Connected to:
    Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - Production
    With the Partitioning, OLAP, Data Mining and Real Application Testing options
    
    SQL> startup force                  <-인스턴스 상태 변경
        ORACLE instance started.
        
        Total System Global Area  175775744 bytes
        Fixed Size                  1335248 bytes
        Variable Size             100663344 bytes
        Database Buffers           67108864 bytes
        Redo Buffers                6668288 bytes
        Database mounted.
        Database opened.
    
    SQL> show parameter spfile
    
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        spfile                               string
        
    SQL> create spfile from pfile;      <-pfile로 spfile 만듬
        
        File created.
        
    SQL> !ls $ORACLE_HOME/dbs/*prod.ora
        /u01/app/oracle/product/11.2.0/dbhome_1/dbs/initprod.ora
        /u01/app/oracle/product/11.2.0/dbhome_1/dbs/spfileprod.ora
        
    SQL> startup force
        ORACLE instance started.
        
        Total System Global Area  175775744 bytes
        Fixed Size                  1335248 bytes
        Variable Size             100663344 bytes
        Database Buffers           67108864 bytes
        Redo Buffers                6668288 bytes
        Database mounted.
        Database opened.
    SQL> show parameter spfile
        
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        spfile                               string      /u01/app/oracle/product/11.2.0
                                                         /dbhome_1/dbs/spfileprod.ora

    SQL> !ls $ORACLE_HOME/dbs/*.ora
    /u01/app/oracle/product/11.2.0/dbhome_1/dbs/init.ora
    /u01/app/oracle/product/11.2.0/dbhome_1/dbs/initprod.ora
    /u01/app/oracle/product/11.2.0/dbhome_1/dbs/spfileorcl.ora
    /u01/app/oracle/product/11.2.0/dbhome_1/dbs/spfileprod.ora
    
    SQL> startup force pfile='$ORACLE_HOME/dbs/initprod.ora'
    SQL> show parameter spfile
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        spfile                               string
```     

## Diagnostic Tools 실습
```
    SQL> show parameter dump_dest
    
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        background_dump_dest                 string      /u01/app/oracle/diag/rdbms/pro
                                                         d/prod/trace
        core_dump_dest                       string      /u01/app/oracle/diag/rdbms/pro
                                                         d/prod/cdump
        user_dump_dest                       string      /u01/app/oracle/diag/rdbms/pro
                                                         d/prod/trace
    SQL> exit
        
        Disconnected from Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - Production
        With the Partitioning, OLAP, Data Mining and Real Application Testing options

    [prod:~]$ cd /u01/app/oracle/diag/rdbms/prod/prod/trace
    
    [prod:trace]$ ls alert*
        alert_prod.log
    
    [prod:trace]$ vi + alert_prod.log
        --vi 편집기 안에 내용
        Thread 1 advanced to log sequence 39 (thread open)
        Thread 1 opened at log sequence 39
          Current log# 1 seq# 39 mem# 0: /u01/app/oracle/oradata/prod/redo01_a.log
          Current log# 1 seq# 39 mem# 1: /u01/app/oracle/oradata/prod/redo01_b.log
        Successful open of redo thread 1
        MTTR advisory is disabled because FAST_START_MTTR_TARGET is not set
        SMON: enabling cache recovery
        Successfully onlined Undo Tablespace 2.
        Verifying file header compatibility for 11g tablespace encryption..
        Verifying 11g file header compatibility for tablespace encryption completed
        SMON: enabling tx recovery
        Database Characterset is US7ASCII
        No Resource Manager plan active
        replication_dependency_tracking turned off (no async multimaster replication found)
        Starting background process QMNC
        Tue Apr 09 14:22:08 2019
        QMNC started with pid=20, OS id=10830
        Completed: ALTER DATABASE OPEN
        Tue Apr 09 14:27:08 2019
        Starting background process SMCO
        Tue Apr 09 14:27:08 2019
        SMCO started with pid=17, OS id=10878
        "alert_prod.log" 879L, 40091C 
        --vi 편집기 종료

    [prod:trace]$ ls
        alert_prod.log       prod_ora_10585.trc   prod_p000_10627.trm
        prod_ckpt_8096.trc   prod_ora_10585.trm   prod_p000_10694.trc
        prod_ckpt_8096.trm   prod_ora_10586.trc   prod_p000_10694.trm
        prod_dbrm_10599.trc  prod_ora_10586.trm   prod_p000_10826.trc
        prod_dbrm_10599.trm  prod_ora_10625.trc   prod_p000_10826.trm
        prod_dbrm_10666.trc  prod_ora_10625.trm   prod_p000_8112.trc
        prod_dbrm_10666.trm  prod_ora_10653.trc   prod_p000_8112.trm
        prod_dbrm_10798.trc  prod_ora_10653.trm   prod_p001_10629.trc
        prod_dbrm_10798.trm  prod_ora_10692.trc   prod_p001_10629.trm
        prod_dbrm_8084.trc   prod_ora_10692.trm   prod_p001_10696.trc
        prod_dbrm_8084.trm   prod_ora_10785.trc   prod_p001_10696.trm
        prod_j000_10082.trc  prod_ora_10785.trm   prod_p001_10828.trc
        prod_j000_10082.trm  prod_ora_10824.trc   prod_p001_10828.trm
        prod_lgwr_7665.trc   prod_ora_10824.trm   prod_p001_8114.trc
        prod_lgwr_7665.trm   prod_ora_7615.trc    prod_p001_8114.trm
        prod_lgwr_8094.trc   prod_ora_7615.trm    prod_smon_8098.trc
        prod_lgwr_8094.trm   prod_ora_7676.trc    prod_smon_8098.trm
        prod_mman_10605.trc  prod_ora_7676.trm    prod_vktm_10591.trc
        prod_mman_10605.trm  prod_ora_7826.trc    prod_vktm_10591.trm
        prod_mman_10672.trc  prod_ora_7826.trm    prod_vktm_10658.trc
        prod_mman_10672.trm  prod_ora_8070.trc    prod_vktm_10658.trm
        prod_mman_10804.trc  prod_ora_8070.trm    prod_vktm_10790.trc
        prod_mman_10804.trm  prod_ora_8071.trc    prod_vktm_10790.trm
        prod_mman_7661.trc   prod_ora_8071.trm    prod_vktm_7647.trc
        prod_mman_7661.trm   prod_ora_8105.trc    prod_vktm_7647.trm
        prod_mman_8090.trc   prod_ora_8105.trm    prod_vktm_8076.trc
        prod_mman_8090.trm   prod_ora_8110.trc    prod_vktm_8076.trm
        prod_mmon_7673.trc   prod_ora_8110.trm    prod_w000_9914.trc
        prod_mmon_7673.trm   prod_p000_10627.trc  prod_w000_9914.trm    
    
    [prod:trace]$ ls *ora*      <-서버 프로세서가 남긴 것들
        prod_ora_10585.trc  prod_ora_10653.trm  prod_ora_7615.trc  prod_ora_8070.trm
        prod_ora_10585.trm  prod_ora_10692.trc  prod_ora_7615.trm  prod_ora_8071.trc
        prod_ora_10586.trc  prod_ora_10692.trm  prod_ora_7676.trc  prod_ora_8071.trm
        prod_ora_10586.trm  prod_ora_10785.trc  prod_ora_7676.trm  prod_ora_8105.trc
        prod_ora_10625.trc  prod_ora_10785.trm  prod_ora_7826.trc  prod_ora_8105.trm
        prod_ora_10625.trm  prod_ora_10824.trc  prod_ora_7826.trm  prod_ora_8110.trc
        prod_ora_10653.trc  prod_ora_10824.trm  prod_ora_8070.trc  prod_ora_8110.trm

    [prod:trace]$
```

## Meta data 확인용 view
```
    [prod:trace]$ cd        <-홈으로 복귀
    [prod:~]$ pwd
        /home/oracle
    
    [prod:~]$ sqlplus / as sysdba 
      
    SQL> startup force nomount;
        ORACLE instance started.
        
        Total System Global Area  175775744 bytes
        Fixed Size                  1335248 bytes
        Variable Size             100663344 bytes
        Database Buffers           67108864 bytes
        Redo Buffers                6668288 bytes
```

```
    * Dynamic Performance View는 데이터의 출처가 parameter file, instance, control file임(마운트가 되어야 파일 읽을 수 있다.)
    * Static Data Dictionary View는 데이터의 출처가 datafile임(open이 되어야 파일 읽을 수 있다.)
        
    SQL> select * from v$instance;  --성공
    SQL> select * from v$datafile;  --에러
    SQL> select * from dba_users;   --에러
    
    SQL> alter database mount;    
    SQL> select * from v$instance;  --성공
    SQL> select * from v$datafile;  --에러
    SQL> select * from dba_users;   --에러            
            
    SQL> alter database open;    
    SQL> select * from v$instance;  --성공
    SQL> select * from v$datafile;  --에러
    SQL> select * from dba_users;   --에러   
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>