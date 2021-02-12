---
layout: post
title: '[AWS] 02. AWS 탄력적 IP(고정 IP) 설정'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 02:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

AWS 탄력적 IP(고정 IP) 설정 <br />


# AWS 탄력적 IP(고정 IP) 설정
> Install Hadoop on AWS <br>

## 탄력적 IP 설정(고정 IP 설정) 및 주소 연결
* 좌측 탭 - 네트워크 및 보안 - 탄력적 IP 선택
* '새 주소 할당' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 10 23 49 AM" src="https://user-images.githubusercontent.com/46523571/56545250-f5dc8f00-65b1-11e9-8e8c-3fb7f0f31d44.png">

* '할당' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 10 24 47 AM" src="https://user-images.githubusercontent.com/46523571/56545282-13115d80-65b2-11e9-9bac-ba2e8b29afb8.png">

* 새 주소 요청 성공
* '닫기' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 10 25 34 AM" src="https://user-images.githubusercontent.com/46523571/56545320-33d9b300-65b2-11e9-8237-456b5979dde1.png">

* 탄력적 IP 선택 - '작업' 버튼 - 주소 연결 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 10 27 44 AM" src="https://user-images.githubusercontent.com/46523571/56545431-89ae5b00-65b2-11e9-8e7d-72092a6b8dfc.png">

* 인스턴스 1개 선택
<img width="100%" alt="Screen Shot 2019-04-23 at 10 32 48 AM" src="https://user-images.githubusercontent.com/46523571/56545652-38eb3200-65b3-11e9-9b61-2cfa787d0277.png">

* 프라이빗 IP 선택
* '연결' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 10 31 58 AM" src="https://user-images.githubusercontent.com/46523571/56545654-3983c880-65b3-11e9-8782-0b16bedd0803.png">

* 주소 연결 요청 성공
<img width="1017" alt="Screen Shot 2019-04-23 at 10 35 05 AM" src="https://user-images.githubusercontent.com/46523571/56545750-7bad0a00-65b3-11e9-9ef2-65d80d58aae8.png">

* 위와 같은 방법으로 나머지 4개 인스턴스도 탄력적 IP(고정 IP)를 설정하고 주소를 연결한다.
<img width="100%" alt="Screen Shot 2019-04-23 at 10 38 09 AM" src="https://user-images.githubusercontent.com/46523571/56545897-f2e29e00-65b3-11e9-8f94-c1b50133d4f2.png">

<br>
<br>
<br>

## 인스턴스 이름 변경
* 좌측 탭 인스턴스 - 인스턴스 클릭 - Name 탭에 마우스를 올리면 연필모양이 있다.
* 연필모양을 클릭하면 인스턴스 이름을 변경 할 수 있음.
<img width="100%" alt="Screen Shot 2019-04-23 at 10 42 34 AM" src="https://user-images.githubusercontent.com/46523571/56546133-97fd7680-65b4-11e9-940b-d0840902386a.png">

## EC2 인스턴스를 통한 가상 머신 구성 완료

<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>