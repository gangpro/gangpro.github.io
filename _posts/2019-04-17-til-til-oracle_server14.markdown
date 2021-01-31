---
layout: post
title: '[oracle] 오라클 서버 관리 - undo 데이터 관리'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 14:12:17 +0900
lastmod: 2019-04-17 14:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - undo 데이터 관리<br />

# Managing Undo Data
> undo 데이터 관리 : 11gWS1 교재 10장<br>

## 
* undo advisor를 활용해서 이 서버에 필요한 undo 크기를 파악한 뒤 적절한 크기의 undo 테이블스페이스를 사용하도록 설정하는 것
###
      SQL> create undo tablespace myundo_ts
           datafile '/u01/app/oracle/oradata/orcl/undo43.dbf' size 100m;
    
      SQL> alter system set undo_tablespace = myundo_ts;
    
      SQL> show parameter undo
    
      NAME                                 TYPE        VALUE
      ------------------------------------ ----------- ------------------------------
      undo_management                      string      AUTO
      undo_retention                       integer     900
      undo_tablespace                      string      MYUNDO_TS
    
      SQL> select tablespace_name, contents
           from dba_tablespaces;
    
      TABLESPACE_NAME                CONTENTS
      ------------------------------ ---------
      SYSTEM                         PERMANENT   --> permanent : 모든 유형의 segment 보관 가능
      SYSAUX                         PERMANENT
      USERS                          PERMANENT
      EXAMPLE                        PERMANENT
      MYTS                           PERMANENT
      UNDOTBS1                       UNDO        --> undo      : 오직 undo segment만 보관 가능   
      MYUNDO_TS                      UNDO
      TEMP                           TEMPORARY   --> temporary : 오직 temporary segment만 보관 가능   


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>