---
layout: post
title: '[Django] 장고 폼 태그 사용해서 사용자 정보 받기'
subtitle: 
categories: backend
tags: django basic
comments: true
date: 2020-02-18 11:35:17 +0900
lastmod: 2020-02-18 11:35:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고(Django) 기능 중 폼(form) 태그(tag) 알아보기.

## 구조 알아보기
---
urls.py에 패스 추가하기<br/>
```python
from django.contrib import admin
from django.urls import path
from pages import views

urlpatterns = [
    path('throw/', views.throw),  # 사용자가 input으로 보내는 곳
    path('catch/', views.catch),  # 받을 곳

    path('isitbirthday/', views.isitbirthday),
    path('template_language/', views.template_language),
]
```

views.py에 throw&catch 함수 정의하기<br/>
```python
def throw(request):
    return render(request, 'throw.html')


# throw.html에서 보낸 값이 catch(request)에 저장된다.
def catch(request):
    return render(request, 'catch.html')
```

pages - templates 폴더 안에 throw.html 파일 생성 후 코드 작성<br/>
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>throw</title>
</head>
<body>

    <h1>Throw Page</h1>
    <form action="/catch/">
        던질거: <input type="text" name="message">
        <input type="submit">
    </form>

</body>
</html>
```

pages - templates 폴더 안에 catch.html 파일 생성 후 코드 작성<br/>
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>catch</title>
</head>
<body>

    <h1>Catch Page</h1>

</body>
</html>
```

서버 실행 버 실행 후 throw.html에서 input tag에 값을 넣으면 자동으로 catch.html로 이동하면서<br/>
`catch/?message=메시지테스트`<br/>

그럼 일단 오케이!<br/>

## 구조 조금 더 알아보기
---
urls.py에 패스 추가하기<br/>
```python
from django.contrib import admin
from django.urls import path
from pages import views

urlpatterns = [
    path('throw/', views.throw),  # 사용자가 input으로 보내는 곳
    path('catch/', views.catch),  # 받을 곳
    ]
```

views.py에 throw&catch 함수 정의하기<br/>
```python
def throw(request):
    return render(request, 'throw.html')

# throw.html에서 보낸 값이 catch(request)에 저장된다.
def catch(request):
    message = request.GET.get('message')
    message2 = request.GET.get('message2')
    context = {
        'message': message,
        'message2': message2,
    }
    return render(request, 'catch.html', context)
```

pages - templates 폴더 안에 throw.html 파일 생성 후 코드 작성<br/>
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>throw</title>
</head>
<body>

    <h1>Throw Page</h1>
    <form action="/catch/">
        던질거: <input type="text" name="message">
        하나더: <input type="text" name="message2">
        <input type="submit">
    </form>

</body>
</html>
```

pages - templates 폴더 안에 catch.html 파일 생성 후 코드 작성<br/>
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>catch</title>
</head>
<body>

    <h1>Catch Page</h1>
    <p>첫번째 메세지: {{ message }}</p>
    <p>두번째 메세지: {{ message2 }}</p>

</body>
</html>
```

서버 실행 후 throw.html 페이지 input tag에 값을 넣으면 catch.html로 이동하면서 아래와 같이 나온다.<br/>
<img width="592" alt="Screen Shot 2019-06-04 at 13 49 28" src="https://user-images.githubusercontent.com/46523571/58851981-a66ca100-86cf-11e9-9b01-b7e01eaf6d53.png"><br/>





## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.