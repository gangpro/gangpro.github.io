---
layout: post
title: '[DjangoBasic] ASCII art API 활용하기'
subtitle: 
categories: til
tags: til django
comments: true
date: 2020-02-18 11:35:17 +0900
lastmod: 2020-02-18 14:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



영어단어를 입력하면 워드아트 느낌?으로 표현할 수 있게 하는 사이트 API 활용해보기<br/>
`http://artii.herokuapp.com/`<br/>

## 구조 만들기
---
urls.py에 패스 추가하기<br/>

```python
from django.contrib import admin
from django.urls import path
from pages import views

urlpatterns = [
    path('artii/', views.artii),    # 워드아트? http://artii.herokuapp.com/
path('artiiResult/', views.artiiResult),
]
```

views.py에 함수 정의하기<br/>

```python
def artii_result(request):
    # 
    return render(request, 'artii_result.html')
```

pages - templates 폴더 안에 artii.html 파일 생성 후 코드 작성<br/>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>artii</title>
</head>
<body>

    <h1>환영합니다 :D</h1>

    <form action="/artiiResult/">
        영단어를 입력해주세요:
        <input type="text" name="word">
        <input type="submit">
    </form>

</body>
</html>
```

pages - templates 폴더 안에 artiiResult.html 파일 생성 후 코드 작성<br/>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>artiiResult</title>
</head>
<body>

    <h1></h1>

    <pre>{{ artiiResult }}</pre>  <!--html에 입력한 내용을 전부다 보여주는 tag-->

</body>
</html>
```

## 폰트 가져오기
---
폰트 리스트 : [http://artii.herokuapp.com/fonts_list](http://artii.herokuapp.com/fonts_list)<br/>

views.py에 함수 정의 추가하기<br/>

```python
from django.shortcuts import render
import requests
import random

def artii(request):
    return render(request, 'artii.html')

def artiiResult(request):
    # 1. form 태그로 날린 데이터를 받는다.
    word = request.GET.get('word')

    # 2. ARTII api 로 요청을 보내 응답결과를 text 로 fonts 에 저장한다. (fonts 를 받는다.)
    api_url = 'http://artii.herokuapp.com'
    fonts = requests.get(f'{api_url}/fonts_list').text
    # print(type(fonts))
    # print(fonts)

    # 3. fonts(str) 를 font_list(list) 으로 바꾼다.
    font_list = fonts.split('\n')
    # print(font_list)

    # 4. random 으로 font 1개를 선택해서 font 라는 변수에 저장한다.
    font = random.choice(font_list)

    # 5. 1번에서 받은 word 와 font 로 다시 요청을 보내서 응답결과를 artiiResult 라는 변수에 저장한다.
    # 사이트 양식 참고 : http://artii.herokuapp.com/make?text=ASCII+art&font=trek
    result = requests.get(f'{api_url}/make?text={word}&font={font}').text

    # 6. artiiResult 에 담긴 문자열을 artiiResult.html 로 넘겨준다.
    context = {
        'result': result,
        'font': font,
        'word': word,
    }

    return render(request, 'artiiResult.html', context)
```

artiiResult.html 파일 수정<br/>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>artiiResult</title>
</head>
<body>

    <h1></h1>
    <p> font: {{ font }}</p>
    <p> word: {{ word }}</p>
    <pre>{{ result }}</pre>  <!--html에 입력한 내용을 전부다 보여주는 tag-->

</body>
</html>
```

## 서버 실행
---
서버 실행 후 사용자가 영어단어를 입력하면 아래와 같이 결과가 나온다.<br/>

<img width="553" alt="Screen Shot 2019-06-04 at 15 17 30" src="https://user-images.githubusercontent.com/46523571/58855803-eafe3980-86db-11e9-9e18-8289d0855a6b.png"><br/>

<img width="570" alt="Screen Shot 2019-06-04 at 15 16 18" src="https://user-images.githubusercontent.com/46523571/58855747-c6a25d00-86db-11e9-8f0d-28aa7ee89f53.png"><br/>





## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.