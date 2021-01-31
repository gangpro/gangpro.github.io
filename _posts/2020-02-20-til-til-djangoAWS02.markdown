---
layout: post
title: '[DjangoAWS] Django 장고 프로젝트 생성'
subtitle: 
categories: til
tags: til django aws
comments: true
date: 2020-02-20 09:40:17 +0900
lastmod: 2020-02-20 09:50:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



Django AWS 배포를 위한 장고 프로젝트 생성<br/>

## Django 프로젝트 시작
---
콘솔에서 프로젝트폴더로 이동한 뒤 `django-admin startproject 프로젝트명` 을 입력하여 새 Django 프로젝트를 시작한다.

```
django-admin startproject mydjango
```

```
Django
├── mydjango  # mydjango라는 이름의 폴더가 생성되었다.
│   ├── manage.py
│   └── myproject
│       ├── __init__.py
│       ├── settings.py
│       ├── urls.py
│       └── wsgi.py
│── .gitignore
├── README.md
└── requirements.txt
```

`myproject` 라는 폴더가 생성되었다.

### 폴더 이름 Refactor

폴더 구조를 보면 `myproject` 폴더 안에 또 `myproject` 라는 폴더가 있는 것을 볼 수 있다. 헷갈리니까 일단 하위 `myproject`폴더를 `config` 로 바꾸어주자. 이 때, `Pycharm` 의 `refactor` 기능을 활용하여 내부 파일들이 바뀐 폴더이름을 제대로 참조할 수 있도록 하는 것이 중요하다. `djangogirls_assignment` 폴더를 `Pycharm` 으로 열고 아래와 같이 설정하자.

<img width="1680" alt="Screen Shot 2019-07-03 at 15 52 11" src="https://user-images.githubusercontent.com/46523571/60570144-e052c500-9dab-11e9-9af5-d2cb2fc9ba00.png">

이름변경하려는 폴더를 우클릭한 뒤, `Refactor` > `Rename` 의 순서로 클릭한다.

<img width="1680" alt="Screen Shot 2019-07-03 at 15 53 03" src="https://user-images.githubusercontent.com/46523571/60570145-e0eb5b80-9dab-11e9-9463-6413c641435b.png">

`Rename` 창에서 **두 개의 체크박스를 모두 체크**한 뒤 `config` 를 입력하고 `Refactor` 버튼을 클릭한다.
`Search for references`: 이 폴더명이 참조된 곳을 찾아 모두 바꿔준다.
`Search for comments and strings`: 이 폴더명이 포함된 코멘트나 문자열까지 찾아 바꿔준다.

<img width="1680" alt="Screen Shot 2019-07-03 at 15 53 44" src="https://user-images.githubusercontent.com/46523571/60570073-b3061700-9dab-11e9-8a08-63d97d9c5d08.png">

`Refactor` 버튼을 누르면 아래 쪽에 `Preview` 창이 하나 뜨고, 어떤 항목이 영향을 받는지 모두 보여준다.
사항들을 확인하고 `Do Refactor` 버튼을 누르면 변경사항이 최종적으로 적용된다.

### Sources Root 설정

현재 `myproject` 폴더 내의 모듈들이 참조하고 있는 이 프로젝트의 `Root directory` 는 가장 최상위 폴더인 `djangogirls_assignment` 이다.
이 경우 나중에 내부 모듈들이 패키지를 참조할 때 문제가 될 수 있다. 이를 예방하기 위해 `Root directory` 를 `Django Applicaion` 폴더 즉, `myproject` 폴더와 일치시켜주어야 한다.

<img width="1680" alt="Screen Shot 2019-07-03 at 15 54 51" src="https://user-images.githubusercontent.com/46523571/60570086-b8fbf800-9dab-11e9-8e21-9ca26dd91592.png">

`myproject` 폴더를 우클릭한 뒤, `Mark Directory as` > `Sources Root` 를 클릭한다.

<img width="415" alt="Screen Shot 2019-07-03 at 15 55 35" src="https://user-images.githubusercontent.com/46523571/60570029-9ff34700-9dab-11e9-8044-4ea93faa0a4d.png">

`myproject` 폴더가 파란색으로 변한 것을 확인할 수 있다.
이제 내부 모듈들이 다른 패키지를 참조할 때 이 폴더를 기준으로 경로를 출발하게 된다.
항상 `Django Applicaion` 폴더를 `Root directory` 로 만들어 주는 것이 좋다.

이제 `myproject` 폴더의 내부는 아래와 같다. 하나씩 간단히 살펴보도록 하자.

```
mydjango
├── manage.py
└── config
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```

**myproject**: `myproject` 폴더는 `Django Application` 이 된다. `Django` 로 만들어진 웹 어플리케이션이다 뭐 그런 정도로 이해하면 될 것 같다. 이 후에 나올 `Django 앱` 과는 다른 것이다.
**manage.py**: 웹사이트의 관리를 도와주는 역할을 하는 스크립트 파일이다. 이 파일의 기능으로 여러가지 개발작업을 수행할 수 있다.
**config**: 웹사이트의 핵심 설정 정보들을 가지고 있는 하위폴더이다. 여러가지 설정과 페이지, 기능 간의 연결을 수행하는 파일들이 들어있다. `__init__.py` 가 있는 걸 보면 알 수 있듯이 Python 패키지이다.
**__init__.py**: 이 파일 때문에 Python은 `myproject` 폴더를 패키지로 인식한다.
**settings.py**: 웹사이트의 여러가지 설정이 들어있는 파일이다.
**urls.py**: 웹사이트의 페이지들을 연결해주는 패턴 목록이 포함된 파일이다.
**wsgi.py**: 뭔지 모르겠다 나중에 알게되면 추가함. 일단 이 튜토리얼에서는 사용하지 않는다.

```
(venv) ➜  mydjango git:(master) ✗ python manage.py runserver
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

July 03, 2019 - 06:57:46
Django version 2.2.3, using settings 'config.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

<img width="1680" alt="Screen Shot 2019-07-03 at 15 58 25" src="https://user-images.githubusercontent.com/46523571/60569989-8d790d80-9dab-11e9-9800-b515a00e0558.png">

개발용 서버가 진짜로 열려있다! 위와 같이 ``The install worked successfully!!` 라는 메세지가 뜨는 것을 확인하면 된다!
이제부터 추가되는 다른 기능들은 이 개발용 서버를 들락날락하면서 확인하면 된다.
개발용 서버는 변경사항이 있을 때마다 **자동으로 리로드하여 적용**시켜 보여준다. 따라서 서버를 껏다 켤 필요가 없다.
단, **새로운 파일이 추가되었을 경우에는 서버를 다시 실행해주어야 적용된다.**














## 참고
AWS 유저 가이드 : [https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html) <br/>
나채원님 블로그 : [https://nachwon.github.io/django-deploy-1-aws/](https://nachwon.github.io/django-deploy-1-aws/) <br/>
장선혁님 블로그 : [https://wkdtjsgur100.github.io/ubuntu-pyenv-virtualenv-autoenv/](https://wkdtjsgur100.github.io/ubuntu-pyenv-virtualenv-autoenv/) <br/>
불곰님 블로그 : [https://brownbears.tistory.com/350](https://brownbears.tistory.com/350) <br/>
블로그 : [https://paphopu.tistory.com/entry/WSGI에-대한-설명-WSGI란-무엇인가](https://paphopu.tistory.com/entry/WSGI에-대한-설명-WSGI란-무엇인가) <br/>
<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>

## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.