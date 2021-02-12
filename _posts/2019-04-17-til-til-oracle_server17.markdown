---
layout: post
title: '[oracle] 오라클 서버 관리 - 백업 및 복구 개념, 백업 수행, 복구 수행'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 17:12:17 +0900
lastmod: 2019-04-17 17:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - 백업 및 복구 개념, 백업 수행, 복구 수행<br />

# Backup and Recovery
> 백업 및 복구 개념, 백업 수행, 복구 수행 : 11gWS2 교재 14~16장<br>

## 용어
* Backup
* Restore
* Recovery

<br>
<br>
<br>

## 데이터베이스 모드 변경 : Noarchivelog -> Archivelog
```
    [orcl:~]$ who am i
    oracle   pts/3        Apr 22 15:47 (:0.0)
    
    [orcl:~]$ export ORACLE_SID=prod
    
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
    
    SQL> archive log list
    Database log mode              No Archive Mode
    Automatic archival             Disabled
    Archive destination            /u01/app/oracle/product/11.2.0/dbhome_1/dbs/arch
    Oldest online log sequence     39
    Current log sequence           40
    
    SQL> ! mkdir /u01/app/oracle/oradata/arch_prod1
    
    SQL> ! mkdir /u01/app/oracle/oradata/arch_prod2
    
    SQL> alter system set log_archive_dest_1 = 'location=/u01/app/oracle/oradata/arch_prod1/';
    
    System altered.
    
    SQL> alter system set log_archive_dest_2 = 'location=/u01/app/oracle/oradata/arch_prod2/';
    
    System altered.
    
    SQL> show parameter log_archive
    리스트 쭈욱~ 나옴.

    SQL> shutdown immediate
    Database closed.
    Database dismounted.
    ORACLE instance shut down.
    
    SQL> startup mount
    ORACLE instance started.
    
    Total System Global Area  175775744 bytes
    Fixed Size                  1335248 bytes
    Variable Size             100663344 bytes
    Database Buffers           67108864 bytes
    Redo Buffers                6668288 bytes
    Database mounted.
    
    SQL> alter database archivelog;
    
    Database altered.
    
    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            /u01/app/oracle/oradata/arch_prod2/
    Oldest online log sequence     39
    Next log sequence to archive   40
    Current log sequence           40
    
    SQL> ! ls /u01/app/oracle/oradata/arch_prod*
    /u01/app/oracle/oradata/arch_prod1:
    
    /u01/app/oracle/oradata/arch_prod2:
    
    SQL> alter database open;
    
    Database altered.
    
    SQL> alter system switch logfile;
    
    System altered.
    
    SQL> ! ls /u01/app/oracle/oradata/arch_prod*
    /u01/app/oracle/oradata/arch_prod1:
    1_40_1005054039.dbf
    
    /u01/app/oracle/oradata/arch_prod2:
    1_40_1005054039.dbf
    
    SQL> alter system switch logfile;
    
    System altered.
    
    SQL> alter system switch logfile;
    
    System altered.
    
    SQL> ! ls /u01/app/oracle/oradata/arch_prod*
    /u01/app/oracle/oradata/arch_prod1:
    1_40_1005054039.dbf  1_41_1005054039.dbf  1_42_1005054039.dbf
    
    /u01/app/oracle/oradata/arch_prod2:
    1_40_1005054039.dbf  1_41_1005054039.dbf  1_42_1005054039.dbf
    
    SQL> 
```

## Whole Closed 백업
```
    SQL> shutdown immediate
    Database closed.
    Database dismounted.
    ORACLE instance shut down.
    
    SQL> ! mkdir /u01/app/oracle/oradata/prod_backup
    
    SQL> ! cp /u01/app/oracle/oradata/prod/* /u01/app/oracle/oradata/prod_backup
    
    SQL> 
```

## 복구
* cf. http://www.yes24.com/Product/Goods/5926350
* cf. http://www.yes24.com/Product/Goods/5926357
* 일반 Datafile 훼손 복구 사례
```
    (1) 데이터 활동
    
    [orcl:~]$ export ORACLE_SID=prod
    [prod:~]$ sqlplus / as sysdba
    
    SQL> startup
    ORACLE instance started.
    
    Total System Global Area  175775744 bytes
    Fixed Size                  1335248 bytes
    Variable Size             100663344 bytes
    Database Buffers           67108864 bytes
    Redo Buffers                6668288 bytes
    Database mounted.
    Database opened.
    
    SQL> select name from v$datafile;
    
    NAME
    --------------------------------------------------------------------------------
    /u01/app/oracle/oradata/prod/system01.dbf
    /u01/app/oracle/oradata/prod/sysaux01.dbf
    /u01/app/oracle/oradata/prod/undotbs01.dbf
    /u01/app/oracle/oradata/prod/users01.dbf
    /u01/app/oracle/oradata/prod/users02.dbf
        
    SQL> conn itzy/jyp        <- 07장에서 만든 itzy/jyp
    Connected.
    
    SQL> create table t7      <- 임의 데이터 생성
         (no number,
          name varchar2(30));
    Table created.
    
    SQL> insert into t7       <- 임의 데이터 삽입
         values(1000, 'Very Important!');
    1 row created.
    
    SQL> insert into t7       <- 임의 데이터 삽입
         values(2000, 'Valuable Data');
    1 row created.
    
    SQL> commit;
    Commit complete.
    
    SQL> select * from t7;

        NO NAME
    ------ ------------------------------
      1000 Very Important!
      2000 Valuable Data

    SQL> conn / as sysdba
    Connected.
     
    SQL> alter system switch logfile;
    SQL> /
    SQL> /
    SQL> /
    SQL> /
```

```
    (2) 에러 발생 원인
    SQL> ! rm /u01/app/oracle/oradata/prod/users01.dbf        <-파일 훼손
    
    SQL> startup force
    ORACLE instance started.
    
    Total System Global Area  175775744 bytes
    Fixed Size                  1335248 bytes
    Variable Size             100663344 bytes
    Database Buffers           67108864 bytes
    Redo Buffers                6668288 bytes
    Database mounted.
    ORA-01157: cannot identify/lock data file 4 - see DBWR trace file
    ORA-01110: data file 4: '/u01/app/oracle/oradata/prod/users01.dbf'
    
    SQL> alter database datafile 4 offline;
    Database altered.

    SQL> alter database open;
    Database altered.
    
    SQL> select * from itzy.t7;
    select * from itzy.t7
                       *
    ERROR at line 1:
    ORA-00376: file 4 cannot be read at this time
    ORA-01110: data file 4: '/u01/app/oracle/oradata/prod/users01.dbf'    
```

```
    (3) 복구 = 복원 + Redo 적용
    
    SQL> ! cp /u01/app/oracle/oradata/prod_backup/users01.dbf /u01/app/oracle/oradata/prod      <-prod_backup에 있는 백업 파일을 prod에 데이터 복원
    
    SQL> recover datafile 4;      
    --백업 후 복구를 너무 빨리 해서 시스템에 남아있는걸로 바로 복구 되었다는 의미. 
    --복구시 아래 내용을 참고하자.
    Media recovery complete.
    
    --복구 방법
    Specify log: {<RET>=suggested | filename | AUTO | CANCEL}
    |  <- 엔터키를 치세요. 한개 한개씩 체크
    
    Specify log: {<RET>=suggested | filename | AUTO | CANCEL}
    |auto       <-알아서 다 체크
    
    SQL> alter database datafile 4 online;
    Database altered.

    SQL> select * from itzy.t7;     <--복원 확인
        NO NAME
    ------ ------------------------------
      1000 Very Important!
      2000 Valuable Data
```

* System 혹은 Undo Datafile 훼손 복구 사례
```
    SQL> ! rm /u01/app/oracle/oradata/prod/system01.dbf
    
    SQL> startup force
    ORACLE instance started.
    
    Total System Global Area  175775744 bytes
    Fixed Size                  1335248 bytes
    Variable Size             100663344 bytes
    Database Buffers           67108864 bytes
    Redo Buffers                6668288 bytes
    Database mounted.
    ORA-01157: cannot identify/lock data file 1 - see DBWR trace file
    ORA-01110: data file 1: '/u01/app/oracle/oradata/prod/system01.dbf'

    --data file 1이 망가진건 사람 몸으로 치면 뇌가 망가진 상태임. 
    --data file 1이 망가진건 offline 할 필요 없이 바로 복구 해도 됨.
    --그래도 처음이니 아래처럼 해보기.
    SQL> alter database datafile 1 offline; 
    Database altered.

    SQL> alter database open;
    alter database open
    *
    ERROR at line 1:
    ORA-01147: SYSTEM tablespace file 1 is offline
    ORA-01110: data file 1: '/u01/app/oracle/oradata/prod/system01.dbf'

    SQL> ! cp /u01/app/oracle/oradata/prod_backup/system01.dbf /u01/app/oracle/oradata/prod
    
    SQL> recover datafile 1;
    
    Specify log: {<RET>=suggested | filename | AUTO | CANCEL}
    |auto
    
    SQL> alter database datafile 1 online;
    Database altered.
    
    SQL> alter database open;
    Database altered.
```

* Temporary Datafile 훼손 복구 사례 
```
    SQL> ! rm /u01/app/oracle/oradata/prod/temp01.tmp

    SQL> ! vi + /u01/app/oracle/diag/rdbms/prod/prod/trace/alert_prod.log
    --중간쯤 자동으로 생성되었음을 확인할 수 있음
    Re-creating tempfile /u01/app/oracle/oradata/prod/temp01.tmp
```

* Control file 훼손 복구 사례 
```
    SQL> select name from v$controlfile;
    NAME
    --------------------------------------------------------------------------------
    /u01/app/oracle/oradata/prod/control01.ctl
    /u01/app/oracle/oradata/prod/control02.ctl
    
    SQL> !rm /u01/app/oracle/oradata/prod/control02.ctl

    SQL> startup force
    ORACLE instance started.
    
    Total System Global Area  175775744 bytes
    Fixed Size                  1335248 bytes
    Variable Size             100663344 bytes
    Database Buffers           67108864 bytes
    Redo Buffers                6668288 bytes
    ORA-00205: error in identifying control file, check alert log for more info
    
    SQL> ! vi + /u01/app/oracle/diag/rdbms/prod/prod/trace/alert_prod.log
    ORA-00210: cannot open the specified control file
    ORA-00202: control file: '/u01/app/oracle/oradata/prod/control02.ctl'

    SQL> !cp /u01/app/oracle/oradata/prod/control01.ctl /u01/app/oracle/oradata/prod/control02.ctl

    SQL> startup force
    ORACLE instance started.
    
    Total System Global Area  175775744 bytes
    Fixed Size                  1335248 bytes
    Variable Size             100663344 bytes
    Database Buffers           67108864 bytes
    Redo Buffers                6668288 bytes
    Database mounted.
    Database opened.
    
    SQL> 
```

* Redo log file 훼손 복구 사례 
```
    SQL> col member format a50
    
    SQL> select group#, member from v$logfile;
        GROUP# MEMBER
    ---------- --------------------------------------------------
             1 /u01/app/oracle/oradata/prod/redo01_a.log
             1 /u01/app/oracle/oradata/prod/redo01_b.log
             2 /u01/app/oracle/oradata/prod/redo02_a.log
             2 /u01/app/oracle/oradata/prod/redo02_b.log
    
    SQL> !rm /u01/app/oracle/oradata/prod/redo01_b.log

    SQL> startup force
    ORACLE instance started.
    
    Total System Global Area  175775744 bytes
    Fixed Size                  1335248 bytes
    Variable Size             100663344 bytes
    Database Buffers           67108864 bytes
    Redo Buffers                6668288 bytes
    Database mounted.
    Database opened.
    
    SQL> ! vi + /u01/app/oracle/diag/rdbms/prod/prod/trace/alert_prod.log
    ORA-00313: open failed for members of log group 1 of thread 1  <- 멤버가 하나 손상되었음을 확인
    
    SQL> !cp /u01/app/oracle/oradata/prod/redo01_a.log /u01/app/oracle/oradata/prod/redo01_b.log
    
    SQL> shutdown immediate
    Database closed.
    Database dismounted.
    ORACLE instance shut down.
    
    SQL> 
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>