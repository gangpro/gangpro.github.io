---
layout: post
title: '[AWS] 01. AWS EC2 인스턴스 생성'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 01:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

AWS EC2 인스턴스 생성<br />

# AWS EC2 인스턴스 생성
> Install Hadoop on AWS <br>

> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> AWS : Amazon Web Services with EC2(Amazon Linux AMI)<br>
> JDK : jdk-7u51-linux-x64<br>
> Hadoop : Hadoop 1.2.1<br>

## AWS 콘솔 접속 및 AWS EC2 인스턴스 생성
* https://aws.amazon.com/ko/
<img width="100%" alt="Screen Shot 2019-04-23 at 9 20 09 AM" src="https://user-images.githubusercontent.com/46523571/56542547-c3c72f00-65a9-11e9-96de-151cc77c02ac.png">

* EC2 서비스 이용
<img width="100%" alt="Screen Shot 2019-04-23 at 9 19 42 AM" src="https://user-images.githubusercontent.com/46523571/56542589-e0fbfd80-65a9-11e9-84d2-d1d5a4ce8ae5.png">

* EC2 인스턴스 생성
  - 리전 : 아시아 태평양 (서울)
  - Amazon EC2 사용하여 Amazon EC2 인스턴스 가상 서버 시작
<img width="100%" alt="Screen Shot 2019-04-23 at 9 27 45 AM" src="https://user-images.githubusercontent.com/46523571/56542683-28828980-65aa-11e9-9f4d-ea344d43ef4b.png">

<br>
<br>
<br>

## 단계 1 : Amazon Machine Image(AMI) 선택
* AMI는 인스턴스를 시작하는 데 필요한 소프트웨어 구성(운영 체제, 애플리케이션 서버, 애플리케이션)이 포함된 템플릿입니다. AWS, 사용자 커뮤니티 또는 AWS Marketplace에서 제공하는 AMI를 선택하거나, 자체 AMI 중 하나를 선택할 수도 있습니다.
* 사용할 인스턴스 : Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type
  - Amazon Linux AMI는 EBS 기반의 AWS 지원 이미지입니다. 기본 이미지에는 AWS 명령줄 도구, Python, Ruby, Perl 및 Java가 있습니다. 리포지토리에는 Docker, PHP, MySQL, PostgreSQL 및 기타 패키지가 포함됩니다.
<img width="100%" alt="Screen Shot 2019-04-23 at 9 32 08 AM" src="https://user-images.githubusercontent.com/46523571/56542885-c413fa00-65aa-11e9-8aab-d080578d58d6.png">

<br>
<br>
<br>

## 단계 2 : 인스턴스 유형 선택
* Amazon EC2는 각 사용 사례에 맞게 최적화된 다양한 인스턴스 유형을 제공합니다.
* t2.micro (프리 티어 사용 가능) 선택
* '다음 : 인스턴스 세부 정보 구성' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 9 34 04 AM" src="https://user-images.githubusercontent.com/46523571/56543039-37b60700-65ab-11e9-999f-dd1f66759571.png">

<br>
<br>
<br>

## 단계 3: 인스턴스 세부 정보 구성
* Hadoop 설치를 위해 인스턴스 개수 : 5개
  - Name Node 1대, Secondary Name Node 1대, Data Node 3대로 구성
* 고정 IP를 사용하기 위해 퍼블릭 IP 자동 할당은 비활성화로 체크
* '다음 : 스토리지 추가' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 9 38 42 AM" src="https://user-images.githubusercontent.com/46523571/56543261-e8240b00-65ab-11e9-90ce-a0b774ab3e54.png">

<br>
<br>
<br>

## 단계 4: 스토리지 추가
* '다음 : 태그 추가' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 9 43 12 AM" src="https://user-images.githubusercontent.com/46523571/56543387-494bde80-65ac-11e9-9cad-d468cf96edc3.png">

<br>
<br>
<br>

## 단계 5: 태그 추가
* '태그 추가' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 9 44 31 AM" src="https://user-images.githubusercontent.com/46523571/56543509-a051b380-65ac-11e9-9b9e-27decd0d078b.png">

* 키 : NAME, 값 : hdcluster 추가
* '다음 : 보안 그룹 구성' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 9 46 36 AM" src="https://user-images.githubusercontent.com/46523571/56543561-c24b3600-65ac-11e9-95d9-02eb84f9333a.png">

<br>
<br>
<br>

## 단계 6: 보안 그룹 구성
* '검토 및 시작' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 9 48 37 AM" src="https://user-images.githubusercontent.com/46523571/56543662-0e967600-65ad-11e9-9a4b-38d3fd5ed93d.png">

<br>
<br>
<br>

## 단계 7: 인스턴스 시작 검토
* '시작하기' 버튼 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 10 02 53 AM" src="https://user-images.githubusercontent.com/46523571/56544298-04757700-65af-11e9-8aee-b8e92c0c635e.png">

<br>
<br>
<br>

## 기존 키페어 선택 또는 새 키 페어 생성
* 새 키 페어 생성
* 키 페어 이름 : hdcluster
* '키 페어 다운로드' 클릭
<img width="702" alt="Screen Shot 2019-04-23 at 10 05 53 AM" src="https://user-images.githubusercontent.com/46523571/56544480-967d7f80-65af-11e9-8f6e-4684d1d65aa5.png">

* 터미널 실행 후 다운로드한 hdcluster.pem 파일을 .ssh 폴더에 이동
###
    mv ~/Downloads/hdcluster.pem ~/.ssh/hdcluster.pem
* [키 페어 관련 참고 URL](https://aws.amazon.com/ko/getting-started/tutorials/launch-a-virtual-machine/)
<img width="839" alt="Screen Shot 2019-04-23 at 10 14 28 AM" src="https://user-images.githubusercontent.com/46523571/56544841-9cc02b80-65b0-11e9-9645-c1219412a8a5.png">

* Mac - Users - 계정 - 'Shift + Command + .(숨김 파일 및 폴더 보기)' - .ssh - hdcluster.pem 파일 확인
<img width="841" alt="Screen Shot 2019-04-23 at 10 16 22 AM" src="https://user-images.githubusercontent.com/46523571/56544940-e01a9a00-65b0-11e9-9b76-443dfac83874.png">

<br>
<br>
<br>

## 시작 상태
* '인스턴스 보기' 클릭
<img width="100%" alt="Screen Shot 2019-04-23 at 10 19 06 AM" src="https://user-images.githubusercontent.com/46523571/56545051-469fb800-65b1-11e9-9d30-ff917a1ce256.png">

<br>
<br>
<br>

## 인스턴스 생성 완료
<img width="100%" alt="Screen Shot 2019-04-23 at 10 22 56 AM" src="https://user-images.githubusercontent.com/46523571/56545204-c7f74a80-65b1-11e9-8bb6-3fb3d4901776.png">

<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>