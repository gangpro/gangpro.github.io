---
layout: post
title: '[DjangoCRUD] 장고 CRUD 활용하기'
subtitle: 
categories: backend
tags: django djangoCRUD
comments: true
date: 2020-02-18 17:10:17 +0900
lastmod: 2020-02-18 17:20:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고 CRUD 활용하기<br/>

## Create, Read, Update, Delete
---
crud - urls.py 코드 수정<br/>

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('boards/', include('boards.urls')),
    path('admin/', admin.site.urls),
]
```

boards - urls.py 생성 후 코드 수정<br/>

```python
from django.urls import path
from . import views

# 이름은 반드시 urlpatterns 라고 해야 장고가 자동으로 경로를 찾는다!!
urlpatterns = [
    path('', views.index),  # '' 는 boards라고 생각하면 된다.
    path('new/', views.new),  # 사용자 입력 페이지
    path('create/', views.create),  # 데이터 저장 페이지
]
```

boards - views.py 코드 수정<br/>

```python
from django.shortcuts import render

# Create your views here.
def index(request):
    return render(request, 'boards/index.html')

```

boards 안에 templates 폴더 만들기<br/>

boards - templates 폴더 안에 boards 폴더 만들기<br/>

boards - templates - boards 폴더 안에 index.html 파일 생성 후 코드 작성<br/>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>boards - index</title>
</head>
<body>

    <h1>Welcome to Boards page</h1>

</body>
</html>
```

사용자가 입력하는 페이지 : new.html<br/>

boards - templates - boards - new.html 코드 작성<br/>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>boards - new</title>
</head>
<body>

    <h1>NEW</h1>
    <form action="">
        <input type="text", name="title" />
        <textarea name="content" id="" cols="30" rows="10"></textarea>
        <input type="submit">
    </form>

</body>
</html>
```

## UPDATE
---
boards - urls.py 생성 후 코드 수정<br/>

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index),  # www.web.com/boards/
    path('new/', views.new),  # 사용자의 입력을 받아서 게시글을 작성하는 곳
    path('create/', views.create),  # 사용자가 입력한 데이터를 전송받고 실제 DB에 작성 및 사용자 피드백
    path('<int:id>/', views.detail),  # www.web.com/boards/<id>/
    path('<int:id>/delete/', views.delete),
    path('<int:id>/edit/', views.edit),  # 게시글 수정 페이지 렌더링
]
```

boards - views.py 코드 수정<br/>

```python
from django.shortcuts import render, redirect
from .models import Board

# Create your views here
def index(request):
    # Board 의 전체 데이터를 불러온다 - QuerySet
    boards = Board.objects.all()
    context = {'boards': boards}
    return render(request, 'boards/index.html', context)

# 사용자 입력을 받는 페이지 렌더링
def new(request):
    return render(request, 'boards/new.html')

# 데이터를 받아서 실제 DB 에 작성
def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')
    board = Board(title=title, content=content)
    board.save()
    return redirect(f'/boards/{board.id}/')

# 특정 게시글 하나만 가지고 온다.
def detail(request, id):
    # Board 클래스를 사용해서 id 값에 맞는 데이터를 가지고 온다.
    # context 로 넘겨서 detail.html 페이지에서 title 과 content 를
    # 출력해본다.
    board = Board.objects.get(id=id)
    context = {'board': board}
    return render(request, 'boards/detail.html', context)

def delete(request, id):
    board = Board.objects.get(id=id)
    board.delete()
    return redirect('/boards/')

# 게시글 수정 페이지 렌더링
def edit(request):
    return render(request, 'boards/edit.html')

```

사용자가 수정하는 페이지 : edit.html<br/>

boards - templates - boards - edit.html 코드 작성 <br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>EDIT</h1>
    <form action="/boards/create/" method="post">
        {% csrf_token %}
        <label for="title">Title</label><br />
        <input name="title" id="title" type="text" value="{{ board.title }}"/><br />  <!-- value="{{ board.title }} : 기존에 작성한거 보여줌 -->
        <label for="content">Content</label><br />
        <textarea name="content" id="content">{{ board.content }}</textarea><br />
        <input type="submit" />
    </form>
    <a href="/boards/">BACK</a>
{% endblock %}
{% endraw %}
```

수정하고 저장하는 함수 및 패스 등등 : update<br/>

boards - urls.py 코드 수정<br/>

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index),  # www.web.com/boards/
    path('new/', views.new),  # 사용자의 입력을 받아서 게시글을 작성하는 곳
    path('create/', views.create),  # 사용자가 입력한 데이터를 전송받고 실제 DB에 작성 및 사용자 피드백
    path('<int:id>/', views.detail),  # www.web.com/boards/<id>/
    path('<int:id>/delete/', views.delete),
    path('<int:id>/edit/', views.edit),  # 게시글 수정 페이지 렌더링
    path('<int:id>/update/', views.update),  # 사용자가 입력한 수정 데이터를 전송 받고 실제 DB 에 수정 후 저장
]
```

boards - views.py 코드 수정<br/>

```python
from django.shortcuts import render, redirect
from .models import Board


# Create your views here
def index(request):
    # Board 의 전체 데이터를 불러온다 - QuerySet
    boards = Board.objects.all()
    context = {'boards': boards}
    return render(request, 'boards/index.html', context)


# 사용자 입력을 받는 페이지 렌더링
def new(request):
    return render(request, 'boards/new.html')


# 데이터를 받아서 실제 DB 에 작성
def create(request):
    title = request.POST.get('title')   # 사용자가 POST로 보낸 걸 get 가져오기
    content = request.POST.get('content')   # 사용자가 POST로 보낸 걸 get 가져오기
    board = Board(title=title, content=content)
    board.save()
    return redirect(f'/boards/{board.id}/')


# 특정 게시글 하나만 가지고 온다.
def detail(request, id):
    # Board 클래스를 사용해서 id 값에 맞는 데이터를 가지고 온다.
    # context 로 넘겨서 detail.html 페이지에서 title 과 content 를
    # 출력해본다.
    board = Board.objects.get(id=id)
    context = {'board': board}
    return render(request, 'boards/detail.html', context)


def delete(request, id):
    board = Board.objects.get(id=id)
    board.delete()
    return redirect('/boards/')


# 게시글 수정 페이지 렌더링
def edit(request, id):
    # id 값에 맞는 board 데이터 꺼낸 후 edit.html 로 넘기기
    board = Board.objects.get(id=id)
    context = {'board': board}
    return render(request, 'boards/edit.html', context)


def update(request, id):
    title = request.POST.get('title')   # 사용자가 POST 로 보낸 걸 get 가져오기
    content = request.POST.get('content')   # 사용자가 POST 로 보낸 걸 get 가져오기
    # id 값에 맞는 board 데이터를 위에서 주어진 title 과 content 에 맞게 수정한 뒤 저장하는 로직
    # 1. Board 클래스를 통해 id 값에 맞는 데이터를 가져온다.
    board = Board.objects.get(id=id)
    # 2. 해당 데이터의 내용을 주어진 title, content 로 수정한다.
    board.title = title
    board.content = content
    # 3. 저장한다.
    board.save()

    # print(title, content)
    return redirect(f'/boards/{id}/')
```

boards - templates - boards - edit.html 코드 수정 <br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>EDIT</h1>
    <form action="/boards/{{ board.id }}/update/" method="post">
        {% csrf_token %}
        <label for="title">Title</label><br />
        <input name="title" id="title" type="text" value="{{ board.title }}"/><br />  <!-- value="{{ board.title }} : 기존에 작성한거 보여줌 -->
        <label for="content">Content</label><br />
        <textarea name="content" id="content">{{ board.content }}</textarea><br />
        <input type="submit" />
    </form>
    <a href="/boards/">BACK</a>
{% endblock %}
{% endraw %}

```

boards - templates - boards - detail.html 코드 수정 <br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>Detail</h1>
    <h2>{{ board.title }}</h2>
    <p>{{ board.id }}번째 글</p>
    <p>{{ board.created_at }}</p>
    <hr />
    <p>{{ board.content }}</p>
    <hr />
    <a href="/boards/">[뒤로가기]</a>
    <form action="/boards/{{ board.id }}/delete/" method="post">
        {% csrf_token %}
        <input class="btn btn-danger" type="submit" value="삭제하기"/>
    </form>

    <a href="/boards/{{ board.id }}/edit/">[수정하기]</a>

{% endblock %}
{% endraw %}
```

## 게시판 완성!!
---

## TIP
---
ex) /board/<id>를 /board/<id>/detail로 바꾸고자 할때 어떻게 하면 좋을까요?<br/>

boards - urls.py 코드 수정
- 세번째 인자를 줄 수도 있다.
-  추가로 변수 설정

```python
from django.urls import path
from . import views


app_name = 'boards'


urlpatterns = [
    path('', views.index, name='index'),  # www.web.com/boards/
    path('new/', views.new, name='new'),  # 사용자의 입력을 받아서 게시글을 작성하는 곳
    path('create/', views.create, name='create'),  # 사용자가 입력한 데이터를 전송받고 실제 DB에 작성 및 사용자 피드백
    path('<int:id>/', views.detail, name='detail'),  # www.web.com/boards/<id>/
    path('<int:id>/delete/', views.delete, name='delete'),
    path('<int:id>/edit/', views.edit, name='edit'),  # 게시글 수정 페이지 렌더링
    path('<int:id>/update/', views.update, name='update'),  # 사용자가 입력한 수정 데이터를 전송 받고 실제 DB 에 수정 후 저장
]
```

boards - views.py 코드 수정<br/>

```python
(기존) return redirect(f'/boards/{id}/')
(변경) return redirect('boards:detail', id)    # boards:detail 콜론에 띄어쓰기가 있으면 안됨.

from django.shortcuts import render, redirect
from .models import Board


# Create your views here
def index(request):
    # Board 의 전체 데이터를 불러온다 - QuerySet
    boards = Board.objects.all()
    context = {'boards': boards}
    return render(request, 'boards/index.html', context)


# 사용자 입력을 받는 페이지 렌더링
def new(request):
    return render(request, 'boards/new.html')


# 데이터를 받아서 실제 DB 에 작성
def create(request):
    title = request.POST.get('title')   # 사용자가 POST로 보낸 걸 get 가져오기
    content = request.POST.get('content')   # 사용자가 POST로 보낸 걸 get 가져오기
    board = Board(title=title, content=content)
    board.save()
    # return redirect(f'/boards/{board.id}/')
    return redirect('boards:detail', board.id)

# 특정 게시글 하나만 가지고 온다.
def detail(request, id):
    # Board 클래스를 사용해서 id 값에 맞는 데이터를 가지고 온다.
    # context 로 넘겨서 detail.html 페이지에서 title 과 content 를
    # 출력해본다.
    board = Board.objects.get(id=id)
    context = {'board': board}
    return render(request, 'boards/detail.html', context)


def delete(request, id):
    board = Board.objects.get(id=id)
    board.delete()
    # return redirect('/boards/')
    return redirect('boards:index')  # boards:index 콜론에 띄어쓰기가 있으면 안됨.


# 게시글 수정 페이지 렌더링
def edit(request, id):
    # id 값에 맞는 board 데이터 꺼낸 후 edit.html 로 넘기기
    board = Board.objects.get(id=id)
    context = {'board': board}
    return render(request, 'boards/edit.html', context)


def update(request, id):
    title = request.POST.get('title')   # 사용자가 POST 로 보낸 걸 get 가져오기
    content = request.POST.get('content')   # 사용자가 POST 로 보낸 걸 get 가져오기
    # id 값에 맞는 board 데이터를 위에서 주어진 title 과 content 에 맞게 수정한 뒤 저장하는 로직
    # 1. Board 클래스를 통해 id 값에 맞는 데이터를 가져온다.
    board = Board.objects.get(id=id)
    # 2. 해당 데이터의 내용을 주어진 title, content 로 수정한다.
    board.title = title
    board.content = content
    # 3. 저장한다.
    board.save()

    # print(title, content)
    # return redirect(f'/boards/{id}/')
    return redirect('boards:detail', id)  # boards:detail 콜론에 띄어쓰기가 있으면 안됨.


```

boards - templates - boards - index.html 코드 수정 <br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>Welcome to boards</h1>
    <hr />
    {% for board in boards %}
        <p>글 번호 : [{{ board.id }}]ㅤㅤ ㅤㅤ ㅤㅤ 글 제목 : {{ board.title }}ㅤㅤㅤ ㅤ ㅤㅤ 글 내용 : {{ board.content }} </p>
        <!--<a href="/boards/{{ board.id }}/">[상세글 보러가기]</a>-->
        <a href="{% url 'boards:detail' board.id %}">[상세글 보러가기]</a>
        <hr />
    {% endfor %}
{% endraw %}
```

boards - templates - boards - new.html 코드 수정 <br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>NEW</h1>
    <!--<form action="/boards/create/" method="post">-->
    <form action="{% url 'boards:create' %}" method="post">
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

boards - templates - boards - edit.html 코드 수정 <br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>EDIT</h1>
    <!--<form action="/boards/{{ board.id }}/update/" method="post">-->
    <form action="{% url 'boards:update' board.id %}" method="post">
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

boards - templates - boards - detail.html 코드 수정 <br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>EDIT</h1>
    <!--<form action="/boards/{{ board.id }}/update/" method="post">-->
    <form action="{% url 'boards:update' board.id %}" method="post">
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

## ADMIN 계정 만들기
---
터미널 실행 `python manage.py createsuperuser`

```python
(venv) ➜  DjangoCRUD git:(master) python manage.py createsuperuser
사용자 이름: kangpro
이메일 주소: 
Password: kangpro@
Password (again): kangpro@
비밀번호가 사용자 이름와 너무 유사합니다.
Bypass password validation and create user anyway? [y/N]: y
Superuser created successfully.
(venv) ➜  DjangoCRUD git:(master) 
```

boards - admin.py 수정<br/>

```python
from django.contrib import admin
from .models import Board


# Register your models here.
# admin 페이지에 board 추가
admin.site.register(Board)
```

boards - models.py 수정<br/>

```python
from django.db import models


# Create your models here.
# 상속
# 상속 받고자하는 다른 class 를 ()괄호 안에 넣어준다.
class Board(models.Model):

    # 클래스 변수 -> DB 의 필드를 나타냄.
    # id 는 기본적으로 처음 테이블 생성시 자동으로 만들어진다. 그래서 아래와 같이 만들어도 되고 안 만들어도 된다.
    # id = models.AutoField(primary_key=True)
    title = models.CharField(max_length=20)  # 길이의 문자열을 제한할 때 쓴다.  # CharField 괄호 안에 max_length=0 가 필수로 있어야 한다.
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)  # auto_now_add : '객체를 하나 생성할 때만 시간을 담겠다' 라는 의미
    updated_at = models.DateTimeField(auto_now=True)  # auto_now : 지금 작업을 할 때


    # 인스턴스 그 자체 형식을 어떻게 출력할지 정하는 class method 다
    def __str__(self):
        return f'{self.id}번째 글ㅤㅤ ㅤㅤ ㅤㅤ {self.title} : {self.content}'


```





## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.