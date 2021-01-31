---
layout: post
title: '[oracle] 오라클 서버 관리 - 데이터베이스 저장 영역 구조 관리'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 11:12:17 +0900
lastmod: 2019-04-17 11:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - 데이터베이스 저장 영역 구조 관리<br />

# Oracle Storage Structure
> 데이터베이스 저장 영역 구조 관리 : 11gWS1 교재 7장<br>

## Oracle Storage Structure
![Oracle Storage Structure](https://user-images.githubusercontent.com/46523571/55932774-0281ee80-5c66-11e9-8ff1-abc1ec8405df.png)
[Source](http://oracle-sliit.blogspot.com/2008/12/logical-and-physical-database-structure.html)
  - Tablespace : 하나 또는 여러 개의 datafile을 재료로 만들어진 컨테이너
  - Segment    : space occupying object
  - Extent     : continuous block  
  - Block      : (저장 및 I/O의 최소단위)
###
      cf.object와 segment 비교
    
         object  : table, index, undo, view, sequence, procedure, function, ...
         segment : table, index, undo, ...

## 예제로 개념이해하기
      (1)
    
      create table myts
      datafile '/경로/myts01.dbf' size 2G,
               '/경로/myts02.dbf' size 2G;
    
      -> segment 몇 개? 0
      -> extent  몇 개? 2
###
     (2)
    
      create table emp(컬럼 설정, ...)
      tablespace myts;
      
      -> segment 몇 개? 1
      -> extent  몇 개? 3
    
      -> 데이터가 계속 입력되어 emp가 extent 2개를 더 받음
      
      -> segment 몇 개? 1
      -> extent  몇 개? 5
###
     (3)
    
      create table dept컬럼 설정, ...)
      tablespace myts;
      
      -> segment 몇 개? 2
      -> extent  몇 개? 6
    
      -> 데이터가 계속 입력되어 emp가 extent 1개를 더 받음
      
      -> segment 몇 개? 2
      -> extent  몇 개? 7
###
      (4)
    
      Meta data를 쿼리해서 이런 상황을 확인해 봅시다.
    
      [orcl:~]$ export ORACLE_SID=orcl
    
      [orcl:~]$ sqlplus / as sysdba
    
      SQL> set linesize 400
      SQL> col segment_name format a40
    
      SQL> select owner, segment_type, segment_name, extents, tablespace_name
           from dba_segments 
           where owner = 'SH';
    
      SQL> col segment_name format a10
    
      SQL> select segment_name, 
                  extent_id, file_id, block_id, blocks
           from dba_extents
           where owner = 'SH'
           and segment_name = 'TIMES';

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>