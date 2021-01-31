---
layout: post
title: '[eGovFrame] macOS에 전자정부 표준프레임워크 개발환경구축'
subtitle: 
categories: env
tags: env egovframework spring
comments: true
date: 2020-03-01 10:00:17 +0900
lastmod: 2020-03-04 18:10:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

macOS에 전자정부 표준프레임워크 개발환경구축<br />

> 개발프레임워크는 정보시스템 개발을 위해 필요한 기능 및 아키텍처를 미리 만들어 제공함으로써 효율적인 어플리케이션 구축을 지원합니다. “전자정부 표준프레임워크”는 공공사업에 적용되는 개발프레임워크의 표준 정립으로 응용 SW 표준화, 품질 및 재 사용성 향상을 목표로 합니다. 이를 통해“전자정부 서비스의 품질향상” 및 “정보화 투자 효율성 향상”을 달성하고, 대ㆍ중소기업이 동일한 개발기반 위에서 공정 경쟁이 가능하게 됩니다.
>
> ※ 표준프레임워크는 기존 다양한 플랫폼(.NET, php 등) 환경을 대체하기 위한 표준은 아니며, java 기반의 정보시스템 구축에 활용하실 수 있는 개발·운영 표준 환경을 제공하기 위한 것입니다. [출처](https://www.egovframe.go.kr/EgovIntro.jsp?menu=1&submenu=1)



## 전자정부프레임워크(eGovFrame) 환경구축
---
### 1. 전자정부프레임워크 개발자 개발환경 구성 가이드 참고

참고 : [https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev3.8:install_guide](https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev3.8:install_guide)



### 2. Eclipse IDE Mars 2 Packages Release 4.5.2 설치

Eclipse IDE for Java EE Developers - Mac Cocoa 64-bit 다운로드<br />

참고 : [https://www.eclipse.org/downloads/packages/release/Mars/2](https://www.eclipse.org/downloads/packages/release/Mars/2)



###  3. Maven 설치

터미널 - ``brew install maven`` 



### 4. Spring IDE Core 3.7.3 설치

- Eclipse - Help - Install New Software
- Install 화면
  Work with : ``http://dist.springsource.com/release/TOOLS/update/e4.4/`` -> ``Core / Spring IDE`` 체크 - Next
- Install Detail 화면
  Spring IDE Core(Required) 클릭 - Next 
- Review Licenses 화면
  I accept the terms of the license agreements 동의 버튼 클릭 - Finish
- 설치 완료 - 재부팅



### 5. UML2 Extension 5.1.2 설치

- Eclipse - Help - Install New Software
- Install 화면
  Work with : ``http://download.eclipse.org/releases/mars/`` -> ``Modeling`` 체크 -> ``UML2 Extender SDK`` Next
- Install Detail 화면
  UML2 Extender SDK 클릭 - Next 
- Review Licenses 화면
  I accept the terms of the license agreements 동의 버튼 클릭 - Finish
- 설치 완료 - 재부팅



### 6. Subbersive SVN Team Provider 3.0.4 설치

- Eclipse - Help - Install New Software
- Install 화면
  Work with : ``http://download.eclipse.org/releases/mars/`` -> ``Collaboration`` 체크 -> ``Subbersive SVN Team Provider(3.0.4)`` Next
- Install Detail 화면
  Subbersive SVN Team Provider 클릭 - Next 
- Review Licenses 화면
  I accept the terms of the license agreements 동의 버튼 클릭 - Finish
- 설치 완료 - 재부팅



### 7. Subversive SVN Connector 6.0.1 설치

- Eclipse - Help - Install New Software
- Install 화면
  Work with : ``http://community.polarion.com/projects/subversive/download/eclipse/5.0/mars-site/`` -> ``Subversive SVN Connectors`` -> ``Subversive SVN Connectors(5.0.3)`` 와 ``SVNKit 1.8.12 Implementation(5.0.3)`` -> Next
- Install Detail 화면
  Subversive SVN Connector와 SVNKit 1.8.12 Implementations 클릭 - Next 
- Review Licenses 화면
  I accept the terms of the license agreements 동의 버튼 클릭 - Finish
- 설치 완료 - 재부팅



### 8. eGovFrame 3.6.0 설치

- Eclipse - Help - Install New Software
- Install 화면
  Work with : ``http://maven.egovframe.kr:8080/update_3.6/`` -> ``eGovFrame`` -> Next
- Install Detail 화면
  eGovFrame 모두 클릭 - Next 
- Review Licenses 화면
  I accept the terms of the license agreements 동의 버튼 클릭 - Finish
- 설치 완료 - 재부팅



### 9. eGovFrame 테스트

- Eclipse - File - New - Project - eGovFrame - Web Project - Next
- Create eGovFrame Web Project
  - Project name : example
  - Dynamic Web Module version : 3.1
  - Maven Setting - Group Id : example
  - Maven Setting - Artiface Id : example
  - Maven Setting - Version : 1.0.0
  - Next
- Generate Example 
  - Generate Example 체크
  - Finish
- Tomcat 설정 및 서버 구동



### 10. 테스트 완료

<img width="746" alt="Screen Shot 2019-11-30 at 21 44 41" src="https://user-images.githubusercontent.com/46523571/69900751-b03f3a80-13ba-11ea-948b-c56f52d02960.png">



## 참고
[https://printhelloworld.tistory.com/182](https://printhelloworld.tistory.com/182)<br/>
[http://blog.naver.com/PostView.nhn?blogId=nodry&logNo=221001460883](http://blog.naver.com/PostView.nhn?blogId=nodry&logNo=221001460883)<br/>

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>



## 환경
---
> macOS Catalina 10.15.1,
> Java JDK 1.8.0_101,
> Eclipse Mars 2 Release 4.5.2,
> Spring IDE Core 3.7.3,
> UML2 Extension 5.1.2,
> Subbersive SVN Team Provider 3.0.4,
> Subversive SVN Connector 6.0.1,
> eGovFrame 3.6.0.