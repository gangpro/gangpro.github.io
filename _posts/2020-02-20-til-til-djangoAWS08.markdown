---
layout: post
title: '[DjangoAWS] 장고와 AWS 탄력적 IP, AWS Route 53 DNS'
subtitle: 
categories: til
tags: til django aws
comments: true
date: 2020-02-20 10:30:17 +0900
lastmod: 2020-02-20 10:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



이번 포스트에서는 배포한 웹서비스에 도메인 네임으로 접속할 수 있도록 하는 방법을 알아볼 것이다. DNS는 IP 주소에 도메인 이름을 부여해서 외우기 힘든 IP 주소 대신 알아보기 쉬운 도메인 이름을 통해서 특정 서버에 접속할 수 있도록 해주는 시스템이다. 지금 우리가 AWS 컴퓨터에 올려놓은 웹 사이트에 접속하기 위해서는 `*.ap-northeast-2.compute.amazonaws.com` 와 같은 주소로 접속해야한다. 이것도 DNS를 통해 부여된 도메인 이름이긴 하지만 차라리 IP 주소를 외우는 것이 더 쉬울 것만 같이 복잡하고 길다. 이제 웹 사이트에게 간단하고 알아보기 쉬운 DNS를 새로 부여해보자.<br/>


## 탄력적 IP 설정(고정 IP)
---
### 새 주소 할당

우선, 기존에 사용하는 IP는 고정IP가 아니기 때문에 우선 고정IP 설정을 먼저 하겠다.

- 네트워크 및 보안 - 탄력적 IP - 새주소 할당

<img width="1680" alt="Screen Shot 2019-07-04 at 20 35 50" src="https://user-images.githubusercontent.com/46523571/60663885-6ef23f80-9e9b-11e9-921a-6fdcd35b3597.png">

- ``할당`` 버튼을 클릭

<img width="1680" alt="Screen Shot 2019-07-04 at 20 38 15" src="https://user-images.githubusercontent.com/46523571/60664004-c7294180-9e9b-11e9-992b-f0b0286a23fc.png">

- 아주 빠르게 탄력적 IP가 생성 되었다. ``닫기`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-04 at 20 38 51" src="https://user-images.githubusercontent.com/46523571/60664005-c7294180-9e9b-11e9-8462-e6e329314573.png">

### 주소 연결

- 기존에 생성한 ubuntu에 연결해주기 위해 
- ``작업`` - ``주소 연결`` 클릭

<img width="1680" alt="Screen Shot 2019-07-04 at 20 40 22" src="https://user-images.githubusercontent.com/46523571/60664100-fcce2a80-9e9b-11e9-96e0-5097a88dab52.png">

### 주소연결

- 인스턴스 : 기존에 생성한 ubuntu 인스턴스 연결(대시보드에 생성한 ubuntu 인스턴스 고유 번호를 볼 수 있다.)
- 프라이빗 IP : 위와 같다.
- ``연결`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-04 at 20 42 11" src="https://user-images.githubusercontent.com/46523571/60664215-461e7a00-9e9c-11e9-9c3f-24f5c7c6742e.png">

- 주소 연결 요청 성공!
- ``닫기`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-04 at 20 45 26" src="https://user-images.githubusercontent.com/46523571/60664344-a9a8a780-9e9c-11e9-83c6-b906b7bfb729.png">

### 탄력적 IP 주의 사항

탄력적 IP를 사용한다고 할당 받은 다음에 사용을 안하면 추후 과금이 될 수 있으니 유의하세요.

## AWS Route 53 도메인 등록하기
---
먼저 도메인 이름을 부여하려면 사용가능한 도메인 이름을 확보해야한다.<br/>
도메인 이름은 도메인 이름 호스팅 사이트에서 구매할 수 있으며, `Hosting.kr`, `gabia.com` 등 여러 개의 호스팅 사이트가 있다.<br/>
이 포스트에서는 `AWS Route 53` 에서 도메인을 구매하여 적용해볼 것이다.<br/>

### AWS Route 53 검색

<img width="1680" alt="Screen Shot 2019-07-04 at 20 14 50" src="https://user-images.githubusercontent.com/46523571/60663087-30f41c00-9e99-11e9-8bec-6548c160679b.png">

### 도메인 등록

<img width="1680" alt="Screen Shot 2019-07-04 at 20 15 09" src="https://user-images.githubusercontent.com/46523571/60663088-30f41c00-9e99-11e9-96f5-2fac35821f1c.png">

### 도메인 등록 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-04 at 20 15 42" src="https://user-images.githubusercontent.com/46523571/60663089-30f41c00-9e99-11e9-82be-c700e8d69f55.png">

### 도메인 이름 선택

- 나의 경우 ``kangpro404`` 선택한 후 ``변경`` 버튼을 눌렀더니 사용 가능한 도메인 주소가 나열된다.

<img width="1680" alt="Screen Shot 2019-07-04 at 20 18 48" src="https://user-images.githubusercontent.com/46523571/60663090-30f41c00-9e99-11e9-8830-dc2fd843275f.png">

- ``장바구니 추가`` 버튼을 클릭 후 ``계속`` 버튼을 누른다.

<img width="1680" alt="Screen Shot 2019-07-04 at 20 24 23" src="https://user-images.githubusercontent.com/46523571/60663294-c4c5e800-9e99-11e9-8394-43f1e0175f76.png">

- 도메인에 대한 연락처 세부 정보를 입력 한 후 ``계속`` 버튼을 누른다.

<img width="1680" alt="Screen Shot 2019-07-04 at 20 26 14" src="https://user-images.githubusercontent.com/46523571/60663396-13738200-9e9a-11e9-8538-407b4651a865.png">

### 연락처 세부 정보 확인

- 도메인 자동 갱신 여부 확인 : ``비활성화`` 
- 이용 약관 : ``AWS 도메인 이름 등록 계약을 읽고 동의합니다.`` 체크
- 등록 연락처의 이메일 주소 확인 : 등록한 이메일 들어가면 인증 이메일이 와있을 것이다. 인증하면 상태 아래와 같이 변경 됨.
- 정보 확인 후 ``구매 완료`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-04 at 20 47 26" src="https://user-images.githubusercontent.com/46523571/60664495-115ef280-9e9d-11e9-8e03-e5f4835afe02.png">

크레딧으로 구매하려했는데.. 안된다고 한다. 어쩔 수 없이 신용카드로 결제해야할 듯. ``구매 완료`` 버튼 클릭... 

<img width="543" alt="Screen Shot 2019-07-04 at 20 50 35" src="https://user-images.githubusercontent.com/46523571/60664632-63077d00-9e9d-11e9-9a5b-c6409fe756e3.png">

### 구매 완료

<img width="1680" alt="Screen Shot 2019-07-04 at 20 51 40" src="https://user-images.githubusercontent.com/46523571/60664700-892d1d00-9e9d-11e9-9bb0-cf9b7a8e3eb8.png">

`` 도메인으로 이동`` 버튼 클릭 후 ``도메인 등록 진행 중``을 확인 할 수 있다.<br/>
도메인 등록을 완료하는데 최장 3일이 걸린다고하니 조금 기다려보자.<br/>

<img width="1680" alt="Screen Shot 2019-07-04 at 20 52 26" src="https://user-images.githubusercontent.com/46523571/60664753-a8c44580-9e9d-11e9-855e-c20fc77afc7a.png">

### ubuntu 인스턴스 Route 53 연결

- 호스팅 영역 - 레코드 세트 생성
- 이름 : www (이건 내가 하고 싶어서 설정)
- 값 : ubuntu 인스턴스 탄력적 IP
- ``생성`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-04 at 21 10 01" src="https://user-images.githubusercontent.com/46523571/60665705-3143e580-9ea0-11e9-9ce9-7fa0b9ceedcd.png">

## Nginx 및 Django 설정
---
### Nginx 설정

이제 `kangpro404.com` 으로 접속하면 AWS의 퍼블릭 DNS로 접속하게 된다.<br/>
하지만 Nginx는 `kangpro404.com` 이라는 주소로 들어온 요청을 알아듣지 못한다.<br/>
Nginx가 이 주소로 오는 요청을 받을 수 있도록 ``프로젝트`` - ``.config``- ``nginx``-  `mydjango.conf` 를 수정해주어야한다.<br/>

```
# mydjango.conf

server {
    listen 80;
    server_name *.compute.amazonaws.com *.kangpro404.com;
    charset utf-8;
    client_max_body_size 128M;
.
.
.
```

`server_name` 항목에 `*.kangpro404.com` 를 추가해주면 `kangpro404.com` 도메인 이름으로 오는 요청을 모두 받을 수 있게 된다.<br/>

### Django 설정

이제 Nginx는 `kangpro404.com` 로 오는 요청을 받아 장고에게 전달해줄 수 있게 되었지만, 장고에서 이 요청을 처리하게 하려면 장고에서도 이 도메인 이름을 허가해주어야 한다.<br/>
`settings.py` 를 열고 `ALLOWED_HOSTS` 변수에 아래와 같이 추가하자.<br/>

```
# settings.py

ALLOWED_HOSTS = [
    'localhost',
    '.ap-northeast-2.compute.amazonaws.com',
    '.kangpro404.com',
]
```

이제 장고도 `kangpro404.com` 도메인 이름으로 오는 요청을 처리할 수 있게 되었다.

```
scp -i ~/.ssh/EC2-kangpro.pem -r ~/Documents/Portfolio/Django ubuntu@ec2-54-180-48-48.ap-northeast-2.compute.amazonaws.com:/srv/
```

```
ssh -i ~/.ssh/EC2-kangpro.pem ubuntu@ec2-54-180-48-48.ap-northeast-2.compute.amazonaws.com
```

```
sudo cp -f /srv/Django/.config/nginx/mydjango.conf /etc/nginx/sites-available/mydjango.conf

sudo ln -sf /etc/nginx/sites-available/mydjango.conf /etc/nginx/sites-enabled/mydjango.conf

sudo ln -f /srv/Django/.config/uwsgi/uwsgi.service /etc/systemd/system/uwsgi.service

sudo systemctl daemon-reload

sudo systemctl enable uwsgi

sudo systemctl | grep nginx

sudo systemctl | grep uwsgi

sudo systemctl daemon-reload

sudo systemctl restart nginx uwsgi
```

이제 다시 관리자 페이지로 접속해보자.

```
http://ec2-54-180-48-48.ap-northeast-2.compute.amazonaws.com/admin
http://www.kangpro404.com/admin
```

브라우저에서 `www.kangpro404.com/admin` 로 접속을 해보면 업로드했던 장고 프로젝트가 나타나는 것을 볼 수 있다.

<img width="1680" alt="Screen Shot 2019-07-04 at 22 48 01" src="https://user-images.githubusercontent.com/46523571/60671338-d5805900-9ead-11e9-93b6-86ecf4629991.png">








## 참고
AWS Route 53 : [https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/Welcome.html](https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/Welcome.html)<br/>
나채원님 블로그 : [https://nachwon.github.io/django-deploy-1-aws/](https://nachwon.github.io/django-deploy-1-aws/)<br/>
<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>

## 환경
---
> macOS Mojave 10.14.5, 
> python 3.7.3, 
> django 2.2.3, 
> PyCharm CE 2018.3.7,
> AWS ubuntu 16.04,
> AWS RDS MySQL 5.7.22,
> AWS Route 53 DNS.