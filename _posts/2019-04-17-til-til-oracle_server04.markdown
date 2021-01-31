---
layout: post
title: '[oracle] 오라클 서버 관리 - Oracle Database 11g 설치'
subtitle: 
categories: til
tags: til oracle server
comments: true
date: 2019-04-17 04:12:17 +0900
lastmod: 2019-04-17 04:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 서버 관리 - Oracle Database 11g 설치<br />

# Install Oracle Database 11g on Linux on VirtualBox on macOS
> Oracle 소프트웨어 설치 : 11gWS1 교재 3장<br>

## 가상환경 접속
* Username : 사용자명 입력
* Password : 비밀번호 입력
<img width="721" alt="Screen Shot 2019-04-05 at 3 07 44 PM" src="https://user-images.githubusercontent.com/46523571/55633697-e3173b80-57f7-11e9-8c31-d95dcbbf0b61.png">
<img width="719" alt="Screen Shot 2019-04-05 at 3 07 55 PM" src="https://user-images.githubusercontent.com/46523571/55633699-e3173b80-57f7-11e9-9953-7491430888cf.png">

## 오라클 데이터베이스 11g 설치(오라클 DBMS EE 설치)
    1. 터미널 실행
    2. [orcl:~]$cd /stage/11.2.0/database/Disk1/
    3. [orcl:Disk1]$ ./runInstaller

<img width="718" alt="Screen Shot 2019-04-05 at 3 10 02 PM" src="https://user-images.githubusercontent.com/46523571/55633700-e3afd200-57f7-11e9-94b7-4b5c8fd2e35d.png">

## Configure Security Updates
* Next - 오류 팝업창 - Yes - Next
<img width="497" alt="Screen Shot 2019-04-05 at 3 23 44 PM" src="https://user-images.githubusercontent.com/46523571/55633704-e3afd200-57f7-11e9-9044-04774b904f40.png">

## Installation Option
* Install database software only 옵션 체크 - Next 
<img width="500" alt="Screen Shot 2019-04-05 at 3 25 10 PM" src="https://user-images.githubusercontent.com/46523571/55633705-e4486880-57f7-11e9-8a96-980134d1195f.png">

## Grid Options
* Single instance database installation 옵션 체크 - Next
<img width="498" alt="Screen Shot 2019-04-05 at 3 26 01 PM" src="https://user-images.githubusercontent.com/46523571/55633706-e4486880-57f7-11e9-88f8-584151e197d6.png">

## Product Languages
* 왼쪽의 이용가능한 언어를 모두 오른쪽으로 이동 - Next
<img width="500" alt="Screen Shot 2019-04-05 at 3 26 41 PM" src="https://user-images.githubusercontent.com/46523571/55633707-e4486880-57f7-11e9-89b7-ae254af76822.png">

## Database Edition
* Enterprise Edition 옵션 체크 - Next
<img width="498" alt="Screen Shot 2019-04-05 at 3 27 15 PM" src="https://user-images.githubusercontent.com/46523571/55633709-e4e0ff00-57f7-11e9-8d05-bbaf00ec9958.png">

## Installation Location
* Next
<img width="498" alt="Screen Shot 2019-04-05 at 3 29 17 PM" src="https://user-images.githubusercontent.com/46523571/55633711-e4e0ff00-57f7-11e9-96fc-d220c946aac4.png">

## Create Inventory
* Next
<img width="497" alt="Screen Shot 2019-04-05 at 3 29 29 PM" src="https://user-images.githubusercontent.com/46523571/55633712-e4e0ff00-57f7-11e9-88dc-ed685b881786.png">

## Operating System Groups
* Next
<img width="496" alt="Screen Shot 2019-04-05 at 3 29 49 PM" src="https://user-images.githubusercontent.com/46523571/55633713-e5799580-57f7-11e9-9821-787c8ede323a.png">

## Summary
* Finish
<img width="497" alt="Screen Shot 2019-04-05 at 3 30 20 PM" src="https://user-images.githubusercontent.com/46523571/55633714-e5799580-57f7-11e9-8c18-4449f38eae5f.png">

## Install Product 1
<img width="497" alt="Screen Shot 2019-04-05 at 3 31 53 PM" src="https://user-images.githubusercontent.com/46523571/55633715-e5799580-57f7-11e9-9f4b-6c2ebf3f2db7.png">

## Install Product 2 - 정말 중요한 단계!!!
* 정말 중요한 단계!! 여기서 팝업창 OK를 눌렀다면.. 리눅스 가져오기부터 다시 새로 해야합니다..
* OK버튼을 누르기 전에 해야할 일이 있음. 아래 계속.

## Execute Configuration scripts 관련
* 터미널 실행
###
    [orcl:~]$ su -
    Password: 비밀번호 입력
    [root@edydrlp0 ~]#
    
    [root@edydrlp0 ~]# /u01/app/oraInventory/orainsRoot.sh
    
    [root@edydrlp0 ~]# /u01/app/oracle/product/11.2.0/dbhome_1/root.sh
    
    위 2개 코드 적용 완료 후 설치화면 OK 버튼 클릭

<img width="720" alt="Screen Shot 2019-04-05 at 3 50 53 PM" src="https://user-images.githubusercontent.com/46523571/55633717-e6122c00-57f7-11e9-8fe0-94b9bb9ce3a7.png">
<img width="719" alt="Screen Shot 2019-04-05 at 3 51 31 PM" src="https://user-images.githubusercontent.com/46523571/55633719-e6122c00-57f7-11e9-90e2-1e465f2e0d5b.png">
<img width="723" alt="Screen Shot 2019-04-05 at 3 52 09 PM" src="https://user-images.githubusercontent.com/46523571/55633720-e6122c00-57f7-11e9-911f-715c264e6b51.png">
<img width="721" alt="Screen Shot 2019-04-05 at 3 52 50 PM" src="https://user-images.githubusercontent.com/46523571/55633721-e6aac280-57f7-11e9-967b-d67fa6ef4c72.png">
    

## Finish
* close
<img width="499" alt="Screen Shot 2019-04-05 at 3 53 21 PM" src="https://user-images.githubusercontent.com/46523571/55633723-e6aac280-57f7-11e9-9d4a-e7f066631405.png">


<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>