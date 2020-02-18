---
layout: post
title: '[Django] 장고 스타일 활용하기'
subtitle: 
categories: backend
tags: django basic
comments: true
date: 2020-02-18 15:45:17 +0900
lastmod: 2020-02-18 15:45:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고에서 스타일(Style) 활용하기<br/>

## 구조 만들기
---
Utilities - templates - utilities - base.html 생성 및 코드 작성<br/>

다른 페이지가 base.html 파일을 상속을 받는다.<br/>

```html
{% raw %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Utilities Base</title>
</head>
<body>
    <header>
        <h1>Utilities Base</h1>
    </header>

    <section>  <!--index.html 부분이라고 생각하면 된다. -->
        {% block body %}
        {% endblock %}
    </section>

    <footer>
        <p>Copyright @ 2019</p>
    </footer>
</body>
</html>
{% endraw %}
```

Utilities - templates - utilities - index.html 코드 수정 <br/>

기존<br/>

```html
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
변경<br/>

```html
{% raw %}
{% extends 'utilities/base.html' %} 
<!--Utilities - templates 안에서 파일을 찾는다. +'경로('utilities/base.html')' -->

{% block body %}
    <h2>Welcome to Utilities Page ;)</h2>
{% endblock %}

{% endraw %}
```

서버를 실행 후 [http://127.0.0.1:8000/utilities/index/](http://127.0.0.1:8000/utilities/index/) 접속하면 index.html을 상속 받은 base.html을 볼 수 있다.<br/>

특이한건 F12 키 누른 후 개발자모드로 보면 index.html 정보가 없이 header와 footer만 볼 수 있다.<br/>

Utilities - static - stylesheets - base.css 파일 생성 및 코드 작성<br/>

```css
h2 {
    color: yellow;
}
```

base.html 코드 수정<br/>

```html
{% raw %}
{% load static %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Utilities Base</title>

    <!--CSS 파일 불러오는 주소 static 적고 '경로' 표시 -->
    <link rel="stylesheet" href="{% static 'stylesheets/base.css' %}" type="text/css">

</head>
<body>
    <header>
        <h1>Utilities Base</h1>
    </header>

    <section>  <!--index.html 부분이라고 생각하면 된다. -->
        {% block body %}
        {% endblock %}
    </section>

    <footer>
        <p>Copyright @ 2019</p>
    </footer>
</body>
</html>
{% endraw %}
```

index.html에서 작성한 스타일을 base.html이 상속을 받아서 코드 한줄로 표현 할 수 있다.<br/>


## 서버 실행
---
서버 실행 후 아래와 같이 결과가 나온다.<br/>






## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.