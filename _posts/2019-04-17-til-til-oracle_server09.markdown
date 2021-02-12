---
layout: post
title: '[oracle] 오라클 서버 관리 - ASM 인스턴스 관리를 위한 기본 지식'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 09:12:17 +0900
lastmod: 2019-04-17 09:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - ASM 인스턴스 관리를 위한 기본 지식<br />

# Manage AWS Instance
> ASM 인스턴스 관리를 위한 기본 지식 : 11gWS1 교재 5장<br>

## Oracle DBMS Installation
  - Single Instance, Multiple Instance
  - GI(ASM + Oracle Restart) + Single Instance
  - GI(ASM + Oracle Clusterware) + Multiple Instance

## 5-4. ASM 인스턴스 구조

## 5-9. DB 인스턴스와 ASM 연동
```
  create tablespace 이름
  datafile '+디스크그룹이름';
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>