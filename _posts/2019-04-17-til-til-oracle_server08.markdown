---
layout: post
title: '[oracle] 오라클 서버 관리 - create database SQL 명령으로 데이터베이스 생성'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 08:12:17 +0900
lastmod: 2019-04-17 08:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - DBCA 방식이 아닌 수동으로 create database SQL 명령으로 데이터베이스 생성<br />

# Create Database SQL Statement

## 06_DBCA 방식이 아닌 수동으로 create database SQL 명령으로 데이터베이스 생성
* https://docs.oracle.com/cd/E11882_01/server.112/e25494/create.htm#ADMIN11073

## 0.디렉토리 및 파라미터 파일 생성 
```
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
            
## 1.Software 시작
```
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

## 2.Create database 명령 실행 
```
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

## 3.필수 Script 수행
```
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

## Test
```
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

## 심심풀이
```
    * 시스템 날짜 확인 
    SQL> select sysdate from dual;
    SYSDATE
    ---------
    08-APR-19
    
    * 내 생일을 기준으로 산 날 카운트
    SQL> select sysdate - to_date('09-DEC-86') from dual;
    SYSDATE-TO_DATE('09-DEC-86')
    ----------------------------
                       11808.459
    
    * 
    SQL> select (sysdate - to_date('09-DEC-86'))/7 from dual;
    (SYSDATE-TO_DATE('09-DEC-86'))/7
    --------------------------------
                          1686.92305
    
    * 
    SQL> select months_between(sysdate , to_date('09-DEC-86')) from dual;
    MONTHS_BETWEEN(SYSDATE,TO_DATE('09-DEC-86'))
    --------------------------------------------
                                      387.982642
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>