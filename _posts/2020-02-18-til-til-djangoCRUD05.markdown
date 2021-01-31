---
layout: post
title: '[DjangoCRUD] 장고 게시판에 댓글 기능 구현하기'
subtitle: 
categories: til
tags: til django
comments: true
date: 2020-02-18 17:25:17 +0900
lastmod: 2020-02-18 17:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고 게시판에 댓글 기능 구현하기<br/>

## 게시판에 댓글 기능 추가
---
데이터베이스 모델 수정<br/>

boards - models.py 코드 수정<br/>

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


# 상속 받고자하는 다른 class 를 ()괄호 안에 넣어준다.
class Comment(models.Model):
    content = models.TextField()  # 댓글 내용
    created_at = models.DateTimeField(auto_now_add=True)  # auto_now_add : '객체를 하나 생성할 때만 시간을 담겠다' 라는 의미
    updated_at = models.DateTimeField(auto_now=True)  # auto_now : 지금 작업을 할 때
    # 일(게시판) 대 다(댓글들) 관계 이기 때문에 foreign key 설정을 해줘야한다.
    board = models.ForeignKey(Board, on_delete=models.CASCADE)
    # 게시판이 삭제 될 때 댓글들 처리 옵션은 아래와 같다.
    # Board, on_delete=models.CASCADE -> Board 게시판 삭제하면 댓글들도 다 삭제
    # Board, on_delete = models.SET_NULL --> Board 게시판 삭제하면 댓글들 널 처리
    # Board, on_delete = models.Do.NOTHING -> Board 게시판 삭제하면 댓글을은 아무 처리도 하지 마세요.

```

데이터베이스 모델 반영, 터미널 실행  `python manage.py makemigrations`<br/>

```python
(venv) ➜  DjangoCRUD python manage.py makemigrations
Migrations for 'boards':
  boards/migrations/0003_comment.py
    - Create model Comment
(venv) ➜  DjangoCRUD 


```

데이터베이스 모델 적용, 터미널 실행 `python manage.py migrate boards`<br/>

```python
(venv) ➜  DjangoCRUD python manage.py migrate 
Operations to perform:
  Apply all migrations: admin, auth, boards, contenttypes, sessions
Running migrations:
  Applying boards.0003_comment... OK
(venv) ➜  DjangoCRUD 


```

데이터베이스 설계도 확인하기, 터미널 실행 `python manage.py showmigrations`<br/>

```python
(venv) ➜  DjangoCRUD python manage.py showmigrations
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
 [X] 0003_logentry_add_action_flag_choices
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
 [X] 0008_alter_user_username_max_length
 [X] 0009_alter_user_last_name_max_length
 [X] 0010_alter_group_name_max_length
 [X] 0011_update_proxy_permissions
boards
 [X] 0001_initial
 [X] 0002_auto_20190611_0919
 [X] 0003_comment
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
sessions
 [X] 0001_initial
(venv) ➜  DjangoCRUD 

```

Django Extentions를 설치하면 shell_plus 사용이 가능하다. 터미널 실행 `pip install django-extensions`<br/>

```python
(venv) ➜  DjangoCRUD pip install django-extensions
Collecting django-extensions
  Downloading https://files.pythonhosted.org/packages/0b/d8/0a99fb0b4af1f4e8970e6e5f56361b6b1edc92ac64ee36a197f58f546f03/django_extensions-2.1.9-py2.py3-none-any.whl (223kB)
    100% |████████████████████████████████| 225kB 803kB/s 
Collecting six>=1.2 (from django-extensions)
  Downloading https://files.pythonhosted.org/packages/73/fb/00a976f728d0d1fecfe898238ce23f502a721c0ac0ecfedb80e0d88c64e9/six-1.12.0-py2.py3-none-any.whl
Installing collected packages: six, django-extensions
Successfully installed django-extensions-2.1.9 six-1.12.0
You are using pip version 10.0.1, however version 19.1.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
(venv) ➜  DjangoCRUD 

```

crud - settings.py 코드 수정<br/>

```python
# Application definition

INSTALLED_APPS = [
    # Local Apps
    'boards',

    # 3rd party Apps
    'django_extensions',

    # Django Apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

Django에 있는 모델들을 모두 import 된 상태로 사용 가능하게 한다. 터미널 실행 `python manage.py shell_plus`<br/>

```python
(venv) ➜  DjangoCRUD python manage.py shell_plus
# Shell Plus Model Imports
from boards.models import Board, Comment
from django.contrib.admin.models import LogEntry
from django.contrib.auth.models import Group, Permission, User
from django.contrib.contenttypes.models import ContentType
from django.contrib.sessions.models import Session
# Shell Plus Django Imports
from django.core.cache import cache
from django.conf import settings
from django.contrib.auth import get_user_model
from django.db import transaction
from django.db.models import Avg, Case, Count, F, Max, Min, Prefetch, Q, Sum, When, Exists, OuterRef, Subquery
from django.utils import timezone
from django.urls import reverse
Python 3.7.3 (default, Mar 27 2019, 09:23:15) 
[Clang 10.0.1 (clang-1001.0.46.3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> 

```

shell_plus를 사용하면 아래와 같이 import 할 필요 없이 바로 사용이 가능하다.<br/>

```python
>>> boards = Board.objects.all()
>>> boards
<QuerySet [<Board: Board object (2)>, <Board: Board object (3)>, <Board: Board object (4)>, <Board: Board object (16)>]>
>>> comments = Comment.objects.all()
>>> comments
<QuerySet []>
>>> 


```

## 특정 게시글에 댓글 생성하기
---
특정 게시글에 댓글 생성하기<br/>

```python
# 1. 특정 게시글 불러오기 
board = Board.objects.get(pk=16) # 16번글

# 2. 댓글 생성(1)
>>> comment = Comment()  # 인스턴스 생성
>>> comment
<Comment: Comment object (None)>

>>> comment.content = '첫번째 댓글입니다.'  # 인스턴스 변수 할당
>>> comment.content
'첫번째 댓글입니다.'
  
>>> board
<Board: Board object (16)>
>>> comment.board = board  # 16 게시판을 참조해 라는 의미
>>> comment.save()
>>> comment
<Comment: Comment object (1)>
  
>>> comment.id
1
  
>>> comment.pk
1
  
>>> comment.board
<Board: Board object (16)>
  
>>> comment.board_id  # 게시판 fk의 id 값 가져오기
16
  
>>> comment.board.id  # 게시판 id 값 가져오기
16

>>> comment.board.title  # 게시판 title 가져오기
'테스트'

  
# 3. 댓글 생성(2)
>>> board = Board.objects.get(pk=4)  # 4번 게시글 가져오기
>>> board
<Board: Board object (4)>

>>> comment = Comment()  # 인스턴스 생성
>>> comment.content = '두번째 댓글입니다.'  # 인스턴스 변수 할당
>>> comment.content
'두번째 댓글입니다.'
>>> 

>>> comment.board_id = board.id
  or
>>> comment.board_id = 4  # 4 게시판을 참조해 라는 의미
>>> comment.save()
>>> comment
<Comment: Comment object (2)>
>>> 

>>> comment.board
<Board: Board object (4)>
  
  
# 4. 댓글 생성(3) - 인스턴스 생성할 때 값을 바로 넣는 방법
>>> board = Board.objects.get(pk=4)  # 4번 게시글 가져오기
>>> board
<Board: Board object (4)>
  
>>> comment = Comment(board_id=board.id, content='세번째 댓글입니다.')  # 인스턴스 생성과 동시에 댓글 달기

>>> comment.save()
>>> comment
<Comment: <Board(4): Comment(3 - 세번째 댓글입니다.)>>
  

```

모든 댓글 가져오기<br/>

```python
# 모든 댓글 가져오기
>>> comments = Comment.objects.all()
>>> comments
<QuerySet [<Comment: Comment object (1)>, <Comment: Comment object (2)>]>
>>> 

# 댓글들 내용을 볼 수 없기 때문에 
# boards - models.py 코드 수정 

      # 인스턴스 그 자체 형식을 어떻게 출력할지 정하는 class method 다
    def __str__(self):
        return f'<Board({self.board_id}): Comment({self.id} - {self.content})>'

  
# shell_plus 재접속 후
>>> comments = Comment.objects.all()
>>> comments
<QuerySet [<Comment: <Board(16): Comment(1 - 첫번째 댓글입니다.)>>, <Comment: <Board(4): Comment(2 - 두번째 댓글입니다.)>>]>
>>> 
```

게시판의 댓글 확인하는 쿼리문<br/>

```python
# 게시글을 불러서 댓글 확인 하기 그런데 안된다. 이유는 아래와 같다.
>>> board = Board.objects.get(pk=16)
>>> board
<Board: Board object (16)>
>>> board.comment
Traceback (most recent call last):
  File "<console>", line 1, in <module>
AttributeError: 'Board' object has no attribute 'comment'
>>> 

# board가 접근 할 수 있는지 확인 할 수 있다.
>>> dir(board)
['DoesNotExist', 'MultipleObjectsReturned', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__setstate__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_check_column_name_clashes', '_check_constraints', '_check_field_name_clashes', '_check_fields', '_check_id_field', '_check_index_together', '_check_indexes', '_check_local_fields', '_check_long_column_names', '_check_m2m_through_same_relationship', '_check_managers', '_check_model', '_check_model_name_db_lookup_clashes', '_check_ordering', '_check_property_name_related_field_accessor_clashes', '_check_single_primary_key', '_check_swappable', '_check_unique_together', '_do_insert', '_do_update', '_get_FIELD_display', '_get_next_or_previous_by_FIELD', '_get_next_or_previous_in_order', '_get_pk_val', '_get_unique_checks', '_meta', '_perform_date_checks', '_perform_unique_checks', '_save_parents', '_save_table', '_set_pk_val', '_state', 'check', 'clean', 'clean_fields',

'comment_set', 

'content', 'created_at', 'date_error_message', 'delete', 'from_db', 'full_clean', 'get_deferred_fields', 'get_next_by_created_at', 'get_next_by_updated_at', 'get_previous_by_created_at', 'get_previous_by_updated_at', 'id', 'objects', 'pk', 'prepare_database_save', 'refresh_from_db', 'save', 'save_base', 'serializable_value', 'title', 'unique_error_message', 'updated_at', 'validate_unique']

# 그냥 쿼리로는 안된다. 
>>> board.comment_set
<django.db.models.fields.related_descriptors.create_reverse_many_to_one_manager.<locals>.RelatedManager object at 0x10275d438>

# board의 댓글들을 확인 할 수 있다.
>>> board.comment_set.all()
<QuerySet [<Comment: <Board(16): Comment(1 - 첫번째 댓글입니다.)>>]>
>>> 


```

이제 관리자 페이지에서 댓글들을 확인 할 수 있도록 설정 해줘야 한다. boards - admin.py 코드 수정<br/>

```python
from django.contrib import admin
from .models import Board, Comment


# admin 페이지 board 게시판 뷰 설정
class BoardAdmin(admin.ModelAdmin):
    list_display = ['id', 'title', 'content']


# Register your models here.
admin.site.register(Board, BoardAdmin)
admin.site.register(Comment)

```

## 댓글 패스 및 함수 설정
---
Comment의 URL path 설정 및 변수명 조금 더 명시적으로 표기( id -> board_id ). boards - urls.py 코드 수정<br/>

```python
from django.urls import path
from . import views


app_name = 'boards'


urlpatterns = [
    path('', views.index, name='index'),  # www.web.com/boards/
    path('new/', views.new, name='new'),  # 사용자의 입력을 받아서 게시글을 작성하는 곳
    # path('create/', views.create, name='create'),  # 사용자가 입력한 데이터를 전송받고 실제 DB에 작성 및 사용자 피드백
    path('<int:board_id>/', views.detail, name='detail'),  # www.web.com/boards/<id>/
    path('<int:board_id>/delete/', views.delete, name='delete'),
    path('<int:board_id>/edit/', views.edit, name='edit'),  # 게시글 수정 페이지 렌더링
    # path('<int:board_id>/update/', views.update, name='update'),  # 사용자가 입력한 수정 데이터를 전송 받고 실제 DB 에 수정 후 저장


    # Comments
    path('<int:board_id>/comments/', views.comment_create, name='comment_create')

]
```

변수명 조금 더 명시적으로 표시. boards - urls.py 코드 수정<br/>

```python
from django.shortcuts import render, redirect, get_object_or_404
from django.views.decorators.http import require_http_methods, require_GET
from .models import Board


# Create your views here
# @require_GET  # GET 요청만 들어오게 만들 수 있다.
def index(request):
    # Board 의 전체 데이터를 불러온다 - QuerySet
    boards = Board.objects.all()
    context = {'boards': boards}
    return render(request, 'boards/index.html', context)


# RESTful 후
# 사용자 입력을 받는 페이지 렌더링
# @require_http_methods(['GET', 'POST'])
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



# RESTful 후
# 특정 게시글 하나만 가지고 온다.
def detail(request, board_id):
    # Board 클래스를 사용해서 id 값에 맞는 데이터를 가지고 온다.
    # context 로 넘겨서 detail.html 페이지에서 title 과 content 를
    # 출력해본다.
    board = get_object_or_404(Board, id=board_id)
    context = {'board': board}
    return render(request, 'boards/detail.html', context)



# RESTful 처리 후 # require_http_methods 처리 후
@require_http_methods(['POST'])
# 허락할 것만 리스트에 담는다. ['POST', 'GET'] 이렇게 쓸 수 있음.
# POST 만 오게끔 설정했는데 GET 이 오면 url에 HTTP ERROR 405 뜬다.
# 콘솔창에는 Method Not Allowed (GET): /boards/5/delete/
# [11/Jun/2019 15:27:49] "GET /boards/5/delete/ HTTP/1.1" 405 0 뜬다.
def delete(request, board_id):
    # if request.method == 'GET':
    #     # GET 요청으로 들어오면 detail page 로 다시 redirect
    #     return redirect('boards:detail', id)  # boards:detail 콜론에 띄어쓰기가 있으면 안됨.
    # else:
        # POST 요청으로 들어오면 정상 삭제
        board = Board.objects.get(id=board_id)
        board.delete()
        return redirect('boards:index')



# RESTful 후
# 게시글 수정 페이지 렌더링
def edit(request, board_id):
    # 0. Board 클래스를 통해 id 값에 맞는 데이터를 가져온다.
    # board = Board.objects.get(id=id)  # DRY : Don't repeat yourself
    board = get_object_or_404(Board, id=board_id)

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
        return redirect('board:detail', board_id)  # 주의! boards:detail 콜론에 띄어쓰기가 있으면 안됨.

```

댓글 함수 생성. board - views.py 코드 수정<br/>

```python
# RESTful 후
# 특정 게시글 하나만 가지고 온다.
def detail(request, board_id):
    # Board 클래스를 사용해서 id 값에 맞는 데이터를 가지고 온다.
    # context 로 넘겨서 detail.html 페이지에서 title 과 content 를
    # 출력해본다.
    board = get_object_or_404(Board, id=board_id)
    comments = board.comment_set.order_by('-id').all()  # id의 역순으로 댓글 순서 정렬
    context = {'board': board, 'comments': comments}
    return render(request, 'boards/detail.html', context)


def comment_create(request, board_id):
    # 댓글 생성하는 로직
    content = request.POST.get('content')
    comment = Comment()
    comment.board_id = board_id
    comment.content = content
    comment.save()

    return redirect('boards:detail', board_id)
```

boards - templates - boards - detail.html 코드 수정<br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>Detail</h1>
    <h2>제목 : {{ board.title }}</h2>
    <p>{{ board.id }}번째 글</p>
    <p>{{ board.created_at }}</p>
    <hr />
    <p><b>내용 : </b></p>
    <p>{{ board.content }}</p>
    <hr />

    <p><b>댓글 목록</b></p>
    {% for comment in comments %}
        <li>{{ comment.content }}</li>
    {% empty %}
        <p>아직 댓글이 없습니다.</p>
    {% endfor %}
    <hr />


    <form action="{% url 'boards:comment_create' board.id %}" method="post">
        {% csrf_token %}    <!-- 우리 웹 서버에서 어떤 작성이 일어날때는 csrf_token이 필요하다라는 의미 // 보안상 필요 -->
        <input type="text" name="content" size="50%" placeholder="댓글을 입력해 주세요.">
        <input class="btn btn-warning" type="submit" value="댓글달기">
    </form>
    <hr />

    <form action="{% url 'boards:index' %}" method="post">
        {% csrf_token %}    <!-- 우리 웹 서버에서 어떤 작성이 일어날때는 csrf_token이 필요하다라는 의미 // 보안상 필요 -->
        <input class="btn btn-secondary" type="submit" value="뒤로가기"/>
    </form>

    <form action="{% url 'boards:delete' board.id %}" method="post">
        {% csrf_token %}
        <input class="btn btn-danger" type="submit" value="삭제하기"/>
    </form>

    <a href="{% url 'boards:edit' board.id %}">[수정하기]</a>

{% endblock %}
{% endraw %}
```

## 댓글 삭제 기능 추가
---
댓글 삭제 기능 패스 추가. boards - urls.py 코드 수정<br/>

```python
urlpatterns = [
    # Comments
    path('<int:board_id>/comments/', views.comment_create, name='comment_create'),
    path('<int:board_id>/comments/<int:comment_id>/delete', views.comment_delete, name='comment_delete'),

]
```

댓글 삭제 기능 함수 추가. boards - views.py 코드 수정<br/>

```python
from django.shortcuts import render, redirect, get_object_or_404
from django.views.decorators.http import require_http_methods, require_GET, require_POST
from .models import Board, Comment

.....중간 생략 

@require_POST
def comment_delete(request, board_id, comment_id):
    # 댓글 삭제 로직
    comment = Comment.objects.get(pk=comment_id)
    comment.delete()

    return redirect('boards:detail', board_id)
```

boards - templates - boards - detail.html 코드 수정<br/>

```python
{% raw %}
{% extends 'boards/base.html' %}

{% block body %}
    <h1>Detail</h1>
    <h2>제목 : {{ board.title }}</h2>
    <p>{{ board.id }}번째 글</p>
    <p>{{ board.created_at }}</p>
    <hr />
    <p><b>내용 : </b></p>
    <p>{{ board.content }}</p>
    <hr />

    <p><b>댓글 목록</b></p>
    {% for comment in comments %}
        <li>{{ comment.content }}</li>
        <!--<form action="{# url 'boards:comment_delete' board_id=board.id comment_id=comment.id #}"></form> 같은 의미-->
        <form action="{% url 'boards:comment_delete' board.id comment.id %}" method="post">
            {% csrf_token %}
        <input class="btn btn-outline-dark" type="submit" value="삭제">
        </form>
    {% empty %}
        <p>아직 댓글이 없습니다 :(</p>
    {% endfor %}
    <hr />

.....중간 생략 
{% endraw %}
```



## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.