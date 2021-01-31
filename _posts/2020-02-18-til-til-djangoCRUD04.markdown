---
layout: post
title: '[DjangoCRUD] 장고 RESTful 웹 서비스 구성하기'
subtitle: 
categories: til
tags: til django
comments: true
date: 2020-02-18 17:20:17 +0900
lastmod: 2020-02-18 17:25:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고 RESTful 웹 서비스 구성하기<br/>

## NEW 수정
---
boards -views.py 코드 수정<br/>

```python
# 사용자 입력을 받는 페이지 렌더링
def new(request):
    # GET 사용자가 작성할 수 있는 게시글을 달라라는 행위
    # POST 사용자가 게시글을 작성하겠다라는 행위
    print(request.method)  # new.html 에 가면 GET 인지 POST 인지 확인 할 수 있음. GET 임

    # GET
    if request.method == 'GET':
        return render(request, 'boards/new.html')
    # POST
    else:
        print(request.method)  # new.html 에 가서 새로 글을 작성하면 GET 인지 POST 인지 확인 할 수 있음. POST 임
        title = request.POST.get('title')  # 사용자가 POST 로 보낸 걸 get 가져오기
        content = request.POST.get('content')  # 사용자가 POST 로 보낸 걸 get 가져오기
        board = Board(title=title, content=content)
        board.save()
        return redirect('boards:detail', board.id)

# 위에 new 함수에서 RESTful 처리 했기 때문에 create 함수는 삭제 처리
# # 데이터를 받아서 실제 DB 에 작성
# def create(request):
#     title = request.POST.get('title')   # 사용자가 POST 로 보낸 걸 get 가져오기
#     content = request.POST.get('content')   # 사용자가 POST 로 보낸 걸 get 가져오기
#     board = Board(title=title, content=content)
#     board.save()
#     # return redirect(f'/boards/{board.id}/')
#     return redirect('boards:detail', board.id)
```

- boards - templates - boards 폴더 안에 new.html 코드 수정<br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>NEW</h1>
    <!--<form action="/boards/create/" method="post">-->
    <!--<form action="{# url 'boards:create' #}" method="post">-->
    <form action="{% url 'boards:new' %}" method="post"> <!-- RESTful 처리 -->
        {% csrf_token %}
        <label for="title">Title</label><br />
        <input name="title" id="title" type="text" /><br />
        <label for="content">Content</label><br />
        <textarea name="content" id="content"></textarea><br />
        <input type="submit" />
    </form>
    <!--<a href="/boards/">BACK</a>-->
    <a href="{% url 'boards:index' %}">BACK</a>
{% endblock %}
{% endraw %}
```

boards - urls.py 코드 수정<br/>

```python
from django.urls import path
from . import views


app_name = 'boards'


urlpatterns = [
    path('', views.index, name='index'),  # www.web.com/boards/
    path('new/', views.new, name='new'),  # 사용자의 입력을 받아서 게시글을 작성하는 곳
    # path('create/', views.create, name='create'),  # 사용자가 입력한 데이터를 전송받고 실제 DB에 작성 및 사용자 피드백
    path('<int:id>/', views.detail, name='detail'),  # www.web.com/boards/<id>/
    path('<int:id>/delete/', views.delete, name='delete'),
    path('<int:id>/edit/', views.edit, name='edit'),  # 게시글 수정 페이지 렌더링
    path('<int:id>/update/', views.update, name='update'),  # 사용자가 입력한 수정 데이터를 전송 받고 실제 DB 에 수정 후 저장
]
```

## EDIT 추가 수정
---
boards -views.py 코드 수정<br/>

```python
# RESTful 후
# 게시글 수정 페이지 렌더링
def edit(request, id):
    # 0. Board 클래스를 통해 id 값에 맞는 데이터를 가져온다.
    board = Board.objects.get(id=id)  # DRY : Don't repeat yourself
    # 1. 사용자의 요청이 GET 인지 POST 인지 확인한다.
    if request.method == 'GET':
        # 2. GET 요청이면 사용자에게 수정할 페이지를 보여준다.
        context = {'board': board}
        return render(request, 'boards/edit.html', context)
    else:
        # 3. POST 요청이면 사용자가 보낸 데이터를 받아서 수정한 뒤 detail page 로 redirect 한다.
        title = request.POST.get('title')  # 사용자가 POST 로 보낸 걸 get 가져오기
        content = request.POST.get('content')  # 사용자가 POST 로 보낸 걸 get 가져오기
        # id 값에 맞는 board 데이터를 위에서 주어진 title 과 content 에 맞게 수정한 뒤 저장하는 로직
        # 해당 데이터의 내용을 주어진 title, content 로 수정한다.
        board.title = title
        board.content = content
        # 저장한다.
        board.save()
        return redirect('board:detail', id)  # 주의! boards:detail 콜론에 띄어쓰기가 있으면 안됨.

# RESTful 전
# def edit(request, id):
#     board = Board.objects.get(id=id)
#     context = {'board': board}
#     return render(request, 'boards/edit.html', context)

# RESTful 전
# def update(request, id):
#     title = request.POST.get('title')   # 사용자가 POST 로 보낸 걸 get 가져오기
#     content = request.POST.get('content')   # 사용자가 POST 로 보낸 걸 get 가져오기
#     # id 값에 맞는 board 데이터를 위에서 주어진 title 과 content 에 맞게 수정한 뒤 저장하는 로직
#     # 1. Board 클래스를 통해 id 값에 맞는 데이터를 가져온다.
#     board = Board.objects.get(id=id)
#     # 2. 해당 데이터의 내용을 주어진 title, content 로 수정한다.
#     board.title = title
#     board.content = content
#     # 3. 저장한다.
#     board.save()
#
#     # print(title, content)
#     # return redirect(f'/boards/{id}/')
#     return redirect('boards:detail', id)  # boards:detail 콜론에 띄어쓰기가 있으면 안됨.

```

boards - templates - boards 폴더 안에 edit.html 코드 수정<br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>EDIT</h1>
    <!--<form action="/boards/{{ board.id }}/update/" method="post">-->
    <!--<form action="{# url 'boards:update' board.id #}" method="post">-->
    <form action="{% url 'boards:edit' board.id %}" method="post">  <!-- RESTful 처리 -->
        {% csrf_token %}
        <label for="title">Title</label><br />
        <input name="title" id="title" type="text" value="{{ board.title }}"/><br />  <!-- value="{{ board.title }} : 기존에 작성한거 보여줌 -->
        <label for="content">Content</label><br />
        <textarea name="content" id="content">{{ board.content }}</textarea><br />
        <input class="btn btn-success" type="submit" value="제출하기" />
    </form>

    <!--<a href="/boards/">[뒤로가기]</a>-->
    <!--<form action="/boards/" method="post">-->
    <form action="{% url 'boards:index' %}" method="post">
        {% csrf_token %}    <!-- 우리 웹 서버에서 어떤 작성이 일어날때는 csrf_token이 필요하다라는 의미 // 보안상 필요 -->
        <input class="btn btn-secondary" type="submit" value="뒤로가기"/>
    </form>
{% endblock %}
{% endraw %}
```

boards - urls.py 코드 수정<br/>

```python
from django.urls import path
from . import views


app_name = 'boards'


urlpatterns = [
    path('', views.index, name='index'),  # www.web.com/boards/
    path('new/', views.new, name='new'),  # 사용자의 입력을 받아서 게시글을 작성하는 곳
    # path('create/', views.create, name='create'),  # 사용자가 입력한 데이터를 전송받고 실제 DB에 작성 및 사용자 피드백
    path('<int:id>/', views.detail, name='detail'),  # www.web.com/boards/<id>/
    path('<int:id>/delete/', views.delete, name='delete'),
    path('<int:id>/edit/', views.edit, name='edit'),  # 게시글 수정 페이지 렌더링
    # path('<int:id>/update/', views.update, name='update'),  # 사용자가 입력한 수정 데이터를 전송 받고 실제 DB 에 수정 후 저장
]
```

## DELETE 추가 수정
---
boards - views.py 코드 수정(방법 1)<br/>
- [http://127.0.0.1:8000/boards/5/delete/](http://127.0.0.1:8000/boards/5/delete/) 처리하면 글이 삭제되는 현상이 있음 그러므로 REST 처리를 해서 GET이 아닌 POST 처리하게끔 만든다. 즉 url에 위와 같이 입력해도 삭제가 되지 않는다.<br/>

```python
# RESTful 전
def delete(request, id):
    board = Board.objects.get(id=id)
    board.delete()
    # return redirect('/boards/')
    return redirect('boards:index')  # boards:index 콜론에 띄어쓰기가 있으면 안됨.
  

  
# RESTful 후
def delete(request, id):
    if request.method == 'GET':
        # GET 요청으로 들어오면 detail page 로 다시 redirect
        return redirect('boards:detail', id)  # boards:detail 콜론에 띄어쓰기가 있으면 안됨.
    else:
        # POST 요청으로 들어오면 정상 삭제
        board = Board.objects.get(id=id)
        board.delete()
        return redirect('boards:index')
```

boards -views.py 코드 수정(방법 2)<br/>
- POST 만 오게끔 설정 할 수 있다. 그럼 기존 if문을 안써도 된다.

```python
from django.views.decorators.http import require_http_methods


# RESTful 처리 후 # require_http_methods 처리 후
@require_http_methods(['POST'])
# 허락할 것만 리스트에 담는다. ['POST', 'GET'] 이렇게 쓸 수 있음.
# POST 만 오게끔 설정했는데 GET 이 오면 url에 HTTP ERROR 405 뜬다.
# 콘솔창에는 Method Not Allowed (GET): /boards/5/delete/
# [11/Jun/2019 15:27:49] "GET /boards/5/delete/ HTTP/1.1" 405 0 뜬다.
def delete(request, id):
    # if request.method == 'GET':
    #     # GET 요청으로 들어오면 detail page 로 다시 redirect
    #     return redirect('boards:detail', id)  # boards:detail 콜론에 띄어쓰기가 있으면 안됨.
    # else:
        # POST 요청으로 들어오면 정상 삭제
        board = Board.objects.get(id=id)
        board.delete()
        return redirect('boards:index')
```

## 상태 코드(Status Code) 수정
---
boards - views.py 코드 수정
  - 사용자가 홈페이지에 없는 페이지를 입력했을 때 500번 에러가 뜬다. 이럴때 404 에러 뜨게 해야한다. 예를 들면 현재 1번 게시글이 삭제된 상태인데 1번 글을 조회했을 경우. [http://127.0.0.1:8000/boards/1/](http://127.0.0.1:8000/boards/1/)
  - 사용자에게 제대로 된? 상태 코드를 보여주는게 좋다.

```python
from django.shortcuts import render, redirect, get_object_or_404


# RESTful 후
# 특정 게시글 하나만 가지고 온다.
def detail(request, id):
    # Board 클래스를 사용해서 id 값에 맞는 데이터를 가지고 온다.
    # context 로 넘겨서 detail.html 페이지에서 title 과 content 를
    # 출력해본다.
    board = get_object_or_404(Board, id=id)
    context = {'board': board}
    return render(request, 'boards/detail.html', context)

```

## html에서 주석 처리 주의 사항
---
문제 <br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>NEW</h1>
    <!--<form action="/boards/create/" method="post">-->
    <!--<form action="{% url 'boards:create' %}" method="post">-->
    <form action="{% url 'boards:new' %}" method="post"> <!-- RESTful 처리 -->
        {% csrf_token %}
        <label for="title">Title</label><br />
        <input name="title" id="title" type="text" /><br />
        <label for="content">Content</label><br />
        <textarea name="content" id="content"></textarea><br />
        <input type="submit" />
    </form>
    <!--<a href="/boards/">BACK</a>-->
    <a href="{% url 'boards:index' %}">BACK</a>
{% endblock %}
{% endraw %}















정답 : <!--<form action="{# url 'boards:create' #}" method="post">-->

```




## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.