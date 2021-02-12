---
layout: post
title: '[oracle] 오라클 서버 관리 - DBCA를 사용하여 오라클 데이터베이스 생성'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 06:12:17 +0900
lastmod: 2019-04-17 06:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - DBCA를 사용하여 오라클 데이터베이스 생성<br />

# Create Database with DBCA
> DBCA를 사용하여 오라클 데이터베이스 생성 : 11gWS1 교재 3장<br>

## DBCA로 데이터베이스 생성 
* 가상환경에서 터미널 실행
```
    [orcl:Disk1]$ dbca
```
<img width="100%" alt="Screen Shot 2019-04-05 at 4 22 06 PM" src="https://user-images.githubusercontent.com/46523571/55670218-bd079f00-58bc-11e9-95f9-a927ab3483ad.png">

<br>
<br>
<br>

## Database Configuration Assistant : Welcome
* 아래와 같은 화면이 나오면 Next
<img width="100%" alt="Screen Shot 2019-04-05 at 4 22 45 PM" src="https://user-images.githubusercontent.com/46523571/55628329-62524280-57eb-11e9-866a-1749ed7b6bbf.png">

<br>
<br>
<br>

## Step 1 of 13 : Operations
* 수행할 작업 선택하기
* Create a Database 옵션 선택 - Next

<img width="100%" alt="Screen Shot 2019-04-05 at 4 23 16 PM" src="https://user-images.githubusercontent.com/46523571/55628393-93cb0e00-57eb-11e9-8f6c-1533c1a6a1b1.png">

<br>
<br>
<br>

## Step 2 of 13 : Database Templates
* 데이터베이스 템플리트
<img width="100%" alt="Screen Shot 2019-04-05 at 4 23 35 PM" src="https://user-images.githubusercontent.com/46523571/55628394-93cb0e00-57eb-11e9-8c64-ba768a14b559.png">

<br>
<br>
<br>

## Step 3 of 13 : Database Identification
* 데이터베이스 ID
<img width="100%" alt="Screen Shot 2019-04-05 at 4 24 30 PM" src="https://user-images.githubusercontent.com/46523571/55628396-93cb0e00-57eb-11e9-9caa-dcd28c136a08.png">

<br>
<br>
<br>

## Step 4 of 13 : Management Options
* 관리 옵션 
<img width="100%" alt="Screen Shot 2019-04-05 at 4 24 58 PM" src="https://user-images.githubusercontent.com/46523571/55628397-9463a480-57eb-11e9-9435-14dcc3e82625.png">

<br>
<br>
<br>

## Step 5 of 13 : Database Credentials
* 데이터베이스 인증서
* 비밀번호는 영문으로 소문자, 대문자, 숫자, 특수기호를 섞어서 사용해야 합니다.
<img width="100%" alt="Screen Shot 2019-04-05 at 4 25 49 PM" src="https://user-images.githubusercontent.com/46523571/55628398-9463a480-57eb-11e9-9587-f4efccea55e5.png">

<br>
<br>
<br>

## Step 6 of 13 : Database File Locations
* 데이터베이스 파일 경로
<img width="100%" alt="Screen Shot 2019-04-05 at 4 28 58 PM" src="https://user-images.githubusercontent.com/46523571/55628399-9463a480-57eb-11e9-9a1b-a46c2c6430f9.png">

<br>
<br>
<br>

## Step 7 of 13 : Recovery Configuration
* 복구 구성
<img width="100%" alt="Screen Shot 2019-04-05 at 4 29 52 PM" src="https://user-images.githubusercontent.com/46523571/55628400-94fc3b00-57eb-11e9-9868-b42fa82e0dd9.png">

<br>
<br>
<br>

## Step 8 of 13 : Database Content
* 데이터베이스 내
<img width="100%" alt="Screen Shot 2019-04-05 at 4 30 17 PM" src="https://user-images.githubusercontent.com/46523571/55628401-94fc3b00-57eb-11e9-9fa4-16eccdae7746.png">

<br>
<br>
<br>

## Step 9 of 13 : Initialization Parameters
* 초기화 매개변수
* 설정 수정!!
  - 문자집합 탭 선택
  - 문자 집합 목록에서 선택 
  - UFT8 - 유니코드 3.0 UTF-8 범용 문자 집합, CESU-8 호환 선택
<img width="100%" alt="Screen Shot 2019-04-05 at 4 31 03 PM" src="https://user-images.githubusercontent.com/46523571/55628402-94fc3b00-57eb-11e9-8bd6-28268ef2e7e9.png">

<br>
<br>
<br>

## Step 10 of 13 : Database Storage
* 데이터베이스 저장 영역
<img width="100%" alt="Screen Shot 2019-04-05 at 4 31 27 PM" src="https://user-images.githubusercontent.com/46523571/55628403-94fc3b00-57eb-11e9-95be-42633934a396.png">

<br>
<br>
<br>

## Step 11 of 13 : Confirmation
* 데이터베이스 생성 요약 
<img width="100%" alt="Screen Shot 2019-04-05 at 4 32 38 PM" src="https://user-images.githubusercontent.com/46523571/55628405-9594d180-57eb-11e9-960f-d9fe11688963.png">

<br>
<br>
<br>

## Step 12 of 13 : Creation Options 
* 데이터베이스 생성 시작
<img width="100%" alt="Screen Shot 2019-04-05 at 4 32 53 PM" src="https://user-images.githubusercontent.com/46523571/55628407-9594d180-57eb-11e9-891b-b060869f5463.png">

<br>
<br>
<br>

## Step 13 of 13 : Database Configuration Assiatant
* 데이터베이스 생성 완료
<img width="100%" alt="Screen Shot 2019-04-05 at 4 35 52 PM" src="https://user-images.githubusercontent.com/46523571/55628408-962d6800-57eb-11e9-9433-e0170348bed9.png">

<br>
<br>
<br>

## 인터넷 접속
* Or you can add an exception 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-05 at 5 14 16 PM" src="https://user-images.githubusercontent.com/46523571/55628409-962d6800-57eb-11e9-9a00-595822aa4bbe.png">

<br>
<br>
<br>

* Add Exception 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-05 at 5 14 30 PM" src="https://user-images.githubusercontent.com/46523571/55628410-962d6800-57eb-11e9-9995-3e34f50a99c3.png">

<br>
<br>
<br>

* Get Certificate 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-05 at 5 14 59 PM" src="https://user-images.githubusercontent.com/46523571/55628411-96c5fe80-57eb-11e9-8039-5992789435b1.png">

<br>
<br>
<br>

* Username : 사용자명 입력
* Password : 비밀번호 입력
* Connect As : SYSDBA 선택
<img width="100%" alt="Screen Shot 2019-04-05 at 5 15 49 PM" src="https://user-images.githubusercontent.com/46523571/55628413-96c5fe80-57eb-11e9-93dc-ecf84a42c166.png">

<br>
<br>
<br>

## 가상환경에서 DBA 접속 완료
<img width="100%" alt="Screen Shot 2019-04-05 at 5 19 54 PM" src="https://user-images.githubusercontent.com/46523571/55628414-975e9500-57eb-11e9-972a-7c91c357d93a.png">

<br>
<br>
<br>

## 맥에서 가상환경 DBA 접속 방법
* VirtualBox Network - Port Forwarding에 아이피 등록 후 맥에서 가상환경 DBA 접속 가능
```
    VirtualBox - Preferences - Newtwork - Adds New NAT network 버튼 클릭
    Network Name : NatNetwork
    Network CIDR : 10.0.2.0/24
    Network Options : Supports DHCP 체크
```
<img width="100%" alt="55634103-b7488580-57f8-11e9-8be0-f8ee863ad38f" src="https://user-images.githubusercontent.com/46523571/55670551-ef1b0000-58c0-11e9-88dd-ff3728c41503.png">

<br>
<br>
<br>

* Port Forwarding
```
    Name : OracleDB
    Protocol : TDP
    Host IP : 70.12.240.242         <-맥 IP
    Host Port : 1158
    Guest IP : 192.168.56.102       <-DBA IP
    Guest Port : 1158
```

<img width="100%" alt="Screen Shot 2019-04-05 at 7 09 42 PM" src="https://user-images.githubusercontent.com/46523571/55670601-7b2d2780-58c1-11e9-82fe-d75e1a66014c.png">

<br>
<br>
<br>

* 맥 터미널에서 접속 확인
```
    @KANG ~ $ssh oracle@192.168.56.102          <-DBA IP
    oracle@192.168.56.102's password: 
    Last login: Fri Apr  5 19:14:03 2019 from 192.168.56.1
    [orcl:~]$ 
```

<img width="100%" alt="Screen Shot 2019-04-05 at 6 56 16 PM" src="https://user-images.githubusercontent.com/46523571/55671018-39eb4680-58c6-11e9-8f46-acbdb176ed60.png">

* 맥 크롬에서 접속이 안되는 문제 발생.... 문제 해결 중
![Screen Shot 2019-04-05 at 7 09 42 PM (2)](https://user-images.githubusercontent.com/46523571/55671055-bbdb6f80-58c6-11e9-9120-819c4e47a2c5.png)

<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>