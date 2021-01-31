---
layout: post
title: '[DjangoAWS] Django AWS 배포해보기'
subtitle: 
categories: til
tags: til django aws
comments: true
date: 2020-02-20 09:20:17 +0900
lastmod: 2020-02-20 09:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



AWS EC2(ubuntu), uWSGI, Nginx, RDS(MySQL), Route53, S3 활용한 Django 웹 배포<br/>
(기능) Django RESTful Framework 게시판 기능, 회원가입 기능, 팔로우 기능, 좋아요 기능 등
<br/>

## Django 구조
---
```
Django
├── .config
│   └── nginx               # 성능에 중점을 둔 차세대 웹 서버 소프트웨어
│       └── mydjango.conf
│   └── uwsgi               # 장고와 웹서버를 연결해주는 역할을 하는 Python 프레임워크
│       └── mydjango.ini
│       └── uwsgi.service
├── .config_secret          # AWS secret 정보
│   └── secret.json
├── mydjango                # 프로젝트 폴더
│   └── config              # 기본 폴더
│       └── __init__.py     # Python으로 하여금 이 디렉토리를 패키지처럼 다루라고 알려주는 단순한 빈 파일
│       └── settings.py     # 현재 Django 프로젝트의 환경 및 구성을 저장
│       └── storages.py     # static & media 파일 저장
│       └── urls.py         # 현재 Django project 의 URL 선언을 저장
│       └── wsgi.py         # 현재 프로젝트를 서비스하기 위한 WSGI 호환 웹 서버의 진입점
│   └── manage.py           # Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인의 유틸리티
├── .git                    # git 관리
├── .gitignore              # git 버전 관리에 제외할 파일 목록
└── requirements.txt        # 패키지 세팅사항 저장
```

## Apps 구조
---
```
│   └── accounts        # accounts app
│       └── migrations
│       └── templates
│           └── accounts
│           		└── change_password.html
│           		└── login.html
│           		└── signup.html
│           		└── update.html
│       └── __init__.py
│       └── admin.py
│       └── apps.py
│       └── forms.py
│       └── models.py
│       └── tests.py
│       └── urls.py
│       └── views.py
```

```
│   └── boards      # boards app
│       └── migrations
│       └── templates
│           └── boards
│           		└── _profile.html
│           		└── base.html
│           		└── detail.html
│           		└── form.html
│           		└── index.html
│       └── __init__.py
│       └── admin.py
│       └── apps.py
│       └── forms.py
│       └── models.py
│       └── tests.py
│       └── urls.py
│       └── views.py
```





## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.