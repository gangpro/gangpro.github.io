---
layout: post
title: '[oracle] 오라클 서버 관리 - intro'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 00:12:17 +0900
lastmod: 2019-04-17 00:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - <br />

# 오라클 서버 관리
> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave, version 10.14.3<br>
> VirtualBox : Version 6.0.4<br>
> Enterprise Linux, Oracle 11g 

## 목차(11gWS1 교재(D50102KR20_sg1.pdf))
1. VirtureBox 설치
2. VirtureBox에 Enterprise Linux 설치
3. Network 세팅
4. Oracle Database 11g 설치
5. 리스너 추가
6. DBCA로 데이터베이스 생성
7. 오라클 서버 시작과 종료
8. 명령문으로 데이터베이스 생성(수동설치)
9. ASM 인스턴스 관리
10. Oracle 네트워크 환경 구성
11. 데이터베이스 저장 영역 구조 관리
12. 유저 보안 관리
13. 데이터 동시성 관리
14. 언두 데이터 관리
15. 오라클 데이터베이스 감사(audit) 구현
16. 데이터베이스 유지관리, 성능 관리
17. 백업 및 복구 개념, 백업 수행, 복구 수행


## (참고) 기초 개념
* 오라클 (데이터베이스) 서버 = 데이터베이스(Database) + 인스턴스(Instance)
  - 1 : 1 = Single   Instance 또는 Stand Alone
  - 1 : N = Multiple Instance 또는 Real Application Cluster
  - N : 1 = Multitenant Database 

  - Databse = = Datafile + Redo log file + Control file
  - Instance = Memory(SGA) + Process(BGP)
    - SGA(System Global Area)  = Shared Pool + Databse Buffer Cache + Redo Log Buffer + 기타
    - BGP(Backaground Process) = PMON, SMON, DBWn, LGWR, CKPT, ARCn
```
        ~ 인스턴스 상태 변경 : startup -> nomount -> mount -> open
                                   |          |        |
                               parameter   control   datafile
                               file        file      redo log file

        - Startup 옵션 : startup (nomount | mount | open) (restrict)
        - Shutdown 옵션 : shutdown (normal | transacitional | immediate | abort)
```

* Backup(복사), Restore(복원), Recovery(복구)


## (참고) 오라클 데이터베이스 서버 관리자 
* Tasks of a Database Administrator
  - https://docs.oracle.com/cd/E11882_01/server.112/e25494/dba.htm#ADMIN11020

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>