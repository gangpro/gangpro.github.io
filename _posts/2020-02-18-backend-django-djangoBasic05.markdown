---
layout: post
title: '[Django] 새로운 Utilities 앱 생성 및 응용하기'
subtitle: 
categories: backend
tags: django basic
comments: true
date: 2020-02-18 15:46:17 +0900
lastmod: 2020-02-18 16:05:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고에서 새로운 Utilities 앱 생성 및 응용<br/>

## 구조 만들기
---
Utilities 새로운 앱 생성<br/>

터미널 실행<br/>

`python manage.py startapp utilities` <br/>

Utilities란 이름으로 앱을 하나 생성하겠다 라는 의미<br/>

하나의 앱을 만들고 나면 반드시 해야할 게 있다.<br/>

```python
# intro 폴더 settings.py 안에 소스코드 수정

INSTALLED_APPS = [
    # Local apps
    'pages.apps.PagesConfig',
    'utilities.apps.UtilitiesConfig', # 추가

    # Django apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

추가 설정<br/>
intro 폴더 settings.py 안에 소스코드 수정<br/>
`LANGUAGE_CODE = 'ko-kr'`<br/>
`TIME_ZONE = 'Asia/Seoul'`<br/>

## intro - urls.py 및 pages - urls.py 코드 수정
---
(기존) intro 폴더 안에 urls.py 파일<br/>

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('static_example/', views.static_example),

    path('artii/', views.artii),    # 워드아트? http://artii.herokuapp.com/
    path('artiiResult/', views.artiiResult),

    path('throw/', views.throw),  # 사용자가 input으로 보내는 곳
    path('catch/', views.catch),  # 받을 곳

    path('isitbirthday/', views.isitbirthday),
    path('template_language/', views.template_language),
    path('introduce/<str:name>/<int:age>/', views.introduce),
    path('greeting/<str:name>/', views.greeting),  # django 에서 url 입력시 끝에 /로 끝나야 한다.
    path('greeting/IU/', views.greetingIU),  # django 에서 url 입력시 끝에 /로 끝나야 한다.
    path('dinner/', views.dinner),
    path('index/', views.index),  # index 란 곳으로 들어오면 views 의 index 를 실행하겠다 선언
    path('admin/', admin.site.urls),
]
```

(변경) intro 폴더 안에 urls.py 파일 수정<br/>

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

(추가) pages 폴더 안에 urls.py 파일 추가 후 코드 작성
- 기존의 intro 폴더 안에 urls.py에 있는 path를 가져온거다. 

```python
from django.urls import path
from . import views

urlpatterns = [
    path('static_example/', views.static_example),

    path('artii/', views.artii),    # 워드아트? http://artii.herokuapp.com/
    path('artiiResult/', views.artiiResult),

    path('throw/', views.throw),  # 사용자가 input으로 보내는 곳
    path('catch/', views.catch),  # 받을 곳

    path('isitbirthday/', views.isitbirthday),
    path('template_language/', views.template_language),
    path('introduce/<str:name>/<int:age>/', views.introduce),
    path('greeting/<str:name>/', views.greeting),  # django 에서 url 입력시 끝에 /로 끝나야 한다.
    path('greeting/IU/', views.greetingIU),  # django 에서 url 입력시 끝에 /로 끝나야 한다.
    path('dinner/', views.dinner),
    path('index/', views.index),  # index 란 곳으로 들어오면 views 의 index 를 실행하겠다 선언
]
```

(추가변경) intro 폴더 안에 urls.py 파일 수정<br/>

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('pages/', include('pages.urls')),  # pages로 들어오면 include('경로') 경로로 가~ 라는 의미
    path('admin/', admin.site.urls),
]
```



## 경로 변경해서 접속하기 확인
---
기존에는 `127.0.0.1:8000/파일.html ` 이렇게 갈 수 있었는데 intro의 urls을 page urls로 변경 했기 때문에 `127.0.0.1:8000/pages/파일.html` 해야된다.





* (추가변경) intro 폴더 안에 urls.py 파일 수정

```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('pages/', include('pages.urls')),  # pages로 들어오면 include('경로') 경로로 가~ 라는 의미
    path('utilities/', include('utilities.urls')),
    path('admin/', admin.site.urls),
]
```







* utilities 폴더 안에 templates 폴더 추가

* utilities 폴더 안에 urls.py 파일 추가 후 코드 작성

```
from django.urls import path
from . import views

urlpatterns = [
    path('index/', views.index),
]
```



* utilities 폴더 안에 views.py 파일 수정

```
from django.shortcuts import render


# Create your views here.
def index(request):
    return render(request, 'index.html')
```

* utilities - templates 폴더 안에 index.html 파일 생성 후 코드 작성

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>

    <h1>Welcom to index of Utilities</h1>

</body>
</html>
```



* 서버 실행 후 ```http://127.0.0.1:8000/utilities/index/``` 실행하면 오류가 발생한다. utilities-templates 폴더 안에 index.html을 불러와야 하는데 pages-templates  폴더에 index.html을 잘못 불러온다.  

  * 이는 intro - setting 에 순서에 따라 실행 순서가 실행되므로 디장고가 잘못 불러온 것이다. 

    ```
    INSTALLED_APPS = [
        # 아래 코드 순서에 따라 실행 순서가 정해진다. pages 다음 utilities 이렇게~
    
        # Local apps
        'pages.apps.PagesConfig',
        'utilities.apps.UtilitiesConfig',
    ```

  * 그러므로 아래와 같이 추가 및 파일 이동을 한다.

    * pages - templates -pages 폴더를 새로 만들어서 기존 html 파일을 옮긴다.
    * utilities - templates -utilities 폴더를 새로 만들어서 기존 html 파일을 옮긴다.

* 추가로 views.py의 경로를 폴더경로를 표시해야한다. ex) index.html -> pages/index.html, utilities/index.html 이렇게~







## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.