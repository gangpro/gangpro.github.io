---
layout: post
title: '[Django] 장고 스타일 활용하기'
subtitle: 
categories: backend
tags: django basic
comments: true
date: 2020-02-18 14:40:17 +0900
lastmod: 2020-02-18 14:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



> 장고에서 스타일(Style) 적용하기<br/>

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

```html
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

pages 폴더 안에 새로운 `static` 폴더 생성 <br/>

pages - static 폴더 안에 새로운 `stylesheets` 폴더 생성 <br/>

pages - static - stylesheets 폴더 안에 새로운 `style.css` 파일 생성 후 코드 작성 <br/>

```css
/* 경로 : pages/static/stylesheets/style.css */

h1 {
    color: blue;
}
```

static_example.html 코드 수정<br/>

```html
<!--{% load static %} 커밋 중 에러로 사용시 주석 해제 해야함.-->
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

</body>
</html>
```

## 서버 실행
---
서버 실행 후 아래와 같이 결과가 나온다.<br/>

<img width="378" alt="Screen Shot 2019-06-04 at 15 35 11" src="https://user-images.githubusercontent.com/46523571/58856869-c22b7380-86de-11e9-87d9-1248237a04d5.png"><br/>





## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.