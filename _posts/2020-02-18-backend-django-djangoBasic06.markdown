---
layout: post
title: '[Django] 장고 이미지 추가하기'
subtitle: 
categories: backend
tags: django basic
comments: true
date: 2020-02-18 16:10:17 +0900
lastmod: 2020-02-18 16:10:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고에서 이미지 추가하기<br/>

## 구조 만들기
---
urls.py에 패스 추가하기<br/>

```python
from django.contrib import admin
from django.urls import path
from pages import views

urlpatterns = [
    path('static_example/', views.views.static_example),
]
```

views.py에 함수 정의하기<br/>

```python
def static_example(request):
    return render(request, 'static_example.html')
```

pages - static 폴더 안에 static_example.html 파일 생성 후 코드 작성<br/>

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>static_example</title>
</head>
<body>

</body>
</html>
```

pages 폴더 안에 새로운 `static` 폴더 생성<br/>

pages - static 폴더 안에 새로운 `images` 폴더 생성<br/>

pages - static - images 폴더 안에 원하는 이미지 추가하기<br/>

static_example.html 코드 수정<br/>

```html
{% raw %}
{% load static %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>static_example</title>

    <!--CSS 파일 불러오는 주소 static 적고 '경로' 표시 -->
    <link rel="stylesheet" href="{% static 'stylesheets/style.css' %}" type="text/css">

</head>
<body>

    <h1>Static Example</h1>
    <img src="{% static 'images/webimage.jpg' %}" alt="심슨 한국 방문">

</body>
</html>
{% endraw %}
```

## 서버 실행
---
서버 실행 후 아래와 같이 결과가 나온다.<br/>

<img width="853" alt="Screen Shot 2019-06-04 at 16 10 17" src="https://user-images.githubusercontent.com/46523571/58858782-4f70c700-86e3-11e9-94b2-922368f9274b.png"><br/>





## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.