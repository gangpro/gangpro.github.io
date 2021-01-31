---
layout: post
title: '[DjangoBasic] 장고 설치 및 앱 생성'
subtitle: 
categories: til
tags: til django
comments: true
date: 2020-02-18 10:10:17 +0900
lastmod: 2020-02-18 11:35:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



맥북에 장고(Django) 설치 및 초기 앱 생성을 통해 Django 알아보기.

## HomeBrew 설치
---
macOS 터미널에서 코드 실행<br/>
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

## python 검색
---
터미널에서 homebrew로 python 검색<br/>
`brew search python`

## python3 설치
---
터미널에서 homebrew로 파이썬3 설치<br/>
`brew install python3`

## Django 설치
---
PyCharm 실행 후 `Django` 이름의 새로운 폴더 생성<br/>

장고 설치<br/>
`pip install django`<br/>

현재 디렉토리에 intro 이름으로 프로젝트 설치<br/>
`django-admin startproject intro .`<br/>

장고 서버 실행<br/>
`python manage.py runserver`<br/>

서버가 실행되면 인터넷 브라우저에서 아래와 같은 화면이 뜬다.<br/>
<img width="1123" alt="Screen Shot 2019-06-03 at 16 03 44" src="https://user-images.githubusercontent.com/46523571/58782318-356ab200-8619-11e9-9110-9862f14ac2b3.png"><br/>

## .gitignore 파일 생성
---
`Django` 폴더에서 `.gitignore` 이름의 새로운 파일 생성<br/>

`https://www.gitignore.io/` 접속해서 아래와 같이 추가한 후 Create 버튼 클릭
<img width="953" alt="Screen Shot 2019-06-03 at 16 08 26" src="https://user-images.githubusercontent.com/46523571/58782680-2cc6ab80-861a-11e9-955d-6aa0275ff0f0.png"><br/>

생성된 코드를 `.gitignore`파일에 붙여넣은 후 저장

## pages 새로운 앱 생성
---
pages 이름으로 앱을 하나 생성<br>
`python manage.py startapp pages`<br/>

pages 폴더 안에 파일 의미
- admin.py : 관리자 페이지 설정 가능
- apps.py : 앱의 정보가 담기는 곳(default 상태 유지하기)
- models.py : 데이터베이스, 앱에서 사용하는 모델 설정 
- tests.py : 테스트 코드 작성하는 곳
- views.py : Spring MVC에서 컨트롤러를 하는 곳

하나의 앱을 만들고 나면 반드시 해야할 게 있다.
- intro 폴더 settings.py 안에 소스코드 수정
- 추가하기 
	```python
  INSTALLED_APPS = [
     'pages.apps.PagesConfig',
  # pages 폴더 - apps.py - class PagesConfig(AppConfig): 값을 의미한다.
	```

추가로 언어와 시간 설정을 위해 intro 폴더 settings.py 안에 소스코드 수정
- `LANGUAGE_CODE = 'ko-kr'`
- `TIME_ZONE = 'Asia/Seoul'`

## intro - urls.py 코드 수정
---
```python
from django.contrib import admin
from django.urls import path
from pages import views

urlpatterns = [
	path('index/', views.index),
	path('admin/', admin.site.urls),
	# index란 곳으로 들어오면 views의 index를 실행하겠다 선언
]
```

## pages - views.py 함수 코드 추가
---
```python
from django.shortcuts import render
    
# Create your views here.
def index(request):
	return render(request, 'index.html')
```

## pages 폴더 - templates 폴더 - index.html 파일 생성
---
pages 폴더 안에 새로운 templates 폴더 생성<br/>

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>index</title>
</head>
<body>

	<h1>Hello World :)</h1>

</body>
</html>
```

## index.html 웹으로 실행
---
서버 시작<br/>
- `python manage.py runserver`<br/>
- `http://127.0.0.1:8000/` 접속하면 서버는 실행되지만 Page not found가 뜸.
- 이유는 우리는 index.html 파일만 만들었기 때문
- `http://127.0.0.1:8000/index/` 이렇게 입력하면 `Hello World`가 뜬다 :)
- <img width="484" alt="Screen Shot 2019-06-03 at 17 09 05" src="https://user-images.githubusercontent.com/46523571/58786361-571c6700-8622-11e9-9c6e-ce307f6d0513.png">





## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.