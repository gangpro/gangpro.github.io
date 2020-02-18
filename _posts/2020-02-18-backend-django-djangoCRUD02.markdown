---
layout: post
title: '[DjangoCRUD] 장고 CRUD ORM 문법'
subtitle: 
categories: backend
tags: django djangoCRUD
comments: true
date: 2020-02-18 16:40:17 +0900
lastmod: 2020-02-18 16:50:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고 ORM 문법 사용해보기<br/>

## Django 쉘 실행 및 쿼리문 작성법
---
Django 쉘 실행 방법<br/>

```shell
(venv) ➜  DjangoCRUD python manage.py shell
Python 3.7.3 (default, Mar 27 2019, 09:23:15) 
[Clang 10.0.1 (clang-1001.0.46.3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> 

quit()
```
(참고) Python 쉘 실행 방법<br/>

```shell
(venv) ➜  DjangoCRUD python
Python 3.7.3 (default, Mar 27 2019, 09:23:15) 
[Clang 10.0.1 (clang-1001.0.46.3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 

quit()
```

쿼리문 작성해 보기<br/>

```shell
# from boards 앱 models.파이 import 클래스명 
>>> from boards.models import Board



# 빈 쿼리를 보여주지만 이렇게 뜨면 데이터베이스가 연결이 잘 된거임.
#  모델 이름. 오브젝트. 올()
>>> Board.objects.all()
<QuerySet []>
```

## CREATE 세가지 방법(1)
---
인스턴스를 생성 후 값 넣기<br/>

```python
# from boards 앱 models.파이 import 클래스명 
>>> from boards.models import Board

# board 변수 = Board() 클래스 를 통해 하나의 인스턴스를 생성한다.
>>> board = Board()
>>> board
<Board: Board object (None)>
>>> 

# 값 넣기
>>> board.title = 'new Board'
>>> board.content = 'Hello, World'
>>> board
<Board: Board object (None)>
>>> 

# 값 확인
>>> board.title
'new Board'
>>> board.content
'Hello, World'
>>> 

# 데이터베이스에 저장
>>> board.save()
>>> board
<Board: Board object (1)>
>>> 

# id 첫번째에 저장이 잘 된거를 확인 할 수 있다.
>>> board.id
1
>>> 

# 데이터베이스 객체 저장 확인
>>> Board.objects.all()
<QuerySet [<Board: Board object (1)>]>
>>> 
```

## CREATE 세가지 방법(2)
---
인스턴스를 생성 할 때 값을 바로 넣을 수 있다.<br/>

```python
>>> board = Board(title="Second Board", content="Django !!")

>>> board.title
'Second Board'

>>> board.content
'Django !!'

>>> 

# 데이터베이스 저장 및 확인
>>> board.save()
>>> board
<Board: Board object (2)>
>>> 
```

## CREATE 세가지 방법(3)
---
@<br/>
```python
>>> Board.objects.create(title="Third board", content="Happy Hacking")
<Board: Board object (3)>
>>> 
```

세가지 방법 불러오기<br/>

```shell
>>> Board.objects.all()
<QuerySet [<Board: Board object (1)>, <Board: Board object (2)>, <Board: Board object (3)>]>
>>> 


```

@ <br/>

```python
>>> board = Board()
>>> board.title = "New Board"
>>> 


```

@ <br/>

```python
>>> board.full_clean()
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/Users/kang/Documents/PycharmProjects/DjangoCRUD/venv/lib/python3.7/site-packages/django/db/models/base.py", line 1203, in full_clean
    raise ValidationError(errors)
django.core.exceptions.ValidationError: {'content': ['이 필드는 빈 칸으로 둘 수 없습니다.']}
>>> Board.objects.all()
<QuerySet [<Board: Board object (1)>, <Board: Board object (2)>, <Board: Board object (3)>]>
>>> 
  
  
```

## 표현식 변경
---
borad - models.py 표현식을 바꿀 수 있다. 코드 수정<br/>

```python
from django.db import models

# Create your models here.
# 상속
# 상속 받고자하는 다른 class 를 ()괄호 안에 넣어준다.
class Board(models.Model):

    # 클래스 변수 -> DB 의 필드를 나타냄.
    # id 는 기본적으로 처음 테이블 생성시 자동으로 만들어진다. 그래서 아래와 같이 만들어도 되고 안 만들어도 된다.
    # id = models.AutoField(primary_key=True)
    title = models.CharField(max_length=10)  # 길이의 문자열을 제한할 때 쓴다.  # CharField 괄호 안에 max_length=0 가 필수로 있어야 한다.
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)  # auto_now_add : '객체를 하나 생성할 때만 시간을 담겠다' 라는 의미
    updated_at = models.DateTimeField(auto_now=True)  # auto_now : 지금 작업을 할 때


    # 인스턴스 그 자체 형식을 어떻게 출력할지 정하는 class method 다
    def __str__(self):
        return f'{self.id}번째 글 - {self.title} : {self.content}'
```

Django 쉘 실행<br/>

```shell
(venv) ➜  DjangoCRUD python manage.py shell
Python 3.7.3 (default, Mar 27 2019, 09:23:15) 
[Clang 10.0.1 (clang-1001.0.46.3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> 


>>> from boards.models import Board
>>> Board.objects.all()
<QuerySet [<Board: 1번째 글 - new Board : Hello, World>, <Board: 2번째 글 - Second Board : Django !!>, <Board: 3번째 글 - Third board : Happy Hacking>]>
>>> 

```

boards - admin.py 확인해보기<br/>

작성한 모델을 불러와야함<br/>

```python
from django.contrib import admin
from .models import Board


# Register your models here.
admin.site.register(Board)
```

admin 계정 만들기<br/>
- python manage.py createsuperuser
  - username : kang
  - 이메일 주소 : 
  - Password : kang@
  - Password (again) : kang@

```

(venv) ➜  DjangoCRUD python manage.py createsuperuser
사용자 이름 (leave blank to use 'kang'): kang
이메일 주소: 
Password: 
Password (again): 
비밀번호가 사용자 이름와 너무 유사합니다.
비밀번호가 너무 짧습니다. 최소 8 문자를 포함해야 합니다.
Bypass password validation and create user anyway? [y/N]: y
Superuser created successfully.
(venv) ➜  DjangoCRUD 

```

서버 실행<br/>

`python manage.py runserver`<br/>

접속 [http://127.0.0.1:8000/admin/login/?next=/admin/](http://127.0.0.1:8000/admin/login/?next=/admin/) <br/>

로그인<br/>

<img width="523" alt="Screen Shot 2019-06-05 at 15 17 28" src="https://user-images.githubusercontent.com/46523571/58934314-0da85500-87a5-11e9-8bad-18334afba704.png"><br/>

admin 페이지<br/>

<img width="1680" alt="Screen Shot 2019-06-05 at 15 16 01" src="https://user-images.githubusercontent.com/46523571/58934276-ea7da580-87a4-11e9-8783-21856eba8b51.png"><br/>




## READ 읽기
---
Django 쉘 접속<br/>

```shell
python manage.py shell
```

```shell
>>> from django.db import models

# SELECT * FROM boards;
>>> Board.objects.all()
>>> 

# SELECT * FROM boards WHERE title='new Board';
>>> Board.objects.filter(title="new Board")
<QuerySet [<Board: 1번째 글 - new Board : Hello, World>]>
>>> 

# new Board에 추가하기
>>> Board.objects.create(title="new Board", content="Happy Django")
<Board: 4번째 글 - new Board : Happy Django>
>>> 

# new Board에 여러개 가져옴 
>>> Board.objects.filter(title="new Board")
<QuerySet [<Board: 1번째 글 - new Board : Hello, World>, <Board: 4번째 글 - new Board : Happy Django>]>
>>> 

# new Board에 한개만 가져옴
# SELECT * FROM boards WHERE title='new Board' LIMIT 1;
>>> Board.objects.filter(title="new Board").first()

# id로 가져오기
# SELECT * FROM boards WHERE id=1;
>>> Board.objects.filter(id=1)
<QuerySet [<Board: 1번째 글 - new Board : Hello, World>]>
>>> 
이렇게 가져와도 좋지만 더 좋은 방법은 아래와 같다.

# 참고로 get은 id 에서만 사용 가능
# PK만 get으로 가져올 수 있다. get은 값이 중복이거나 일치하는 값이 없으면 오류가 나기 때문이다. 즉 PK에만 사용하자!
>>> Board.objects.get(id=1)
<Board: 1번째 글 - new Board : Hello, World>
>>> 

그러므로 아래는 오류남
>>> Board.objects.get(title="new Board")
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/Users/kang/Documents/PycharmProjects/DjangoCRUD/venv/lib/python3.7/site-packages/django/db/models/manager.py", line 82, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/Users/kang/Documents/PycharmProjects/DjangoCRUD/venv/lib/python3.7/site-packages/django/db/models/query.py", line 412, in get
    (self.model._meta.object_name, num)
boards.models.Board.MultipleObjectsReturned: get() returned more than one Board -- it returned 2!
>>> 

```

정렬<br/>

```shell
# 오름차순 정렬 표현
# SELECT * FROM boards ORDER BY title ASC; >>> Board.objects.order_by('title').all()
>>> 

# 내림차순 정렬 표현
# SELECT * FROM boards ORDER BY title ASC; >>> 
Board.objects.order_by('-title').all()
>>> 

```

QuerySet은 list처럼 index접근 및 list methods 중 일부 사용 가능하나, 실제 list type은 아니다.<br/>

```shell
# 리스트와 같은 형태로 온다.
>>> Board.objects.all()
<QuerySet [<Board: 1번째 글 - new Board : Hello, World>, <Board: 2번째 글 - Second Board : Django !!>, <Board: 3번째 글 - Third board : Happy Hacking>, <Board: 4번째 rd : Happy Django>]>
>>> 

# 인덱스 접근해서 한가지 가져올 수 있다.
>>> board = Board.objects.all()[2]
>>> board
<Board: 3번째 글 - Third board : Happy Hacking>
>>> 

# 인덱스 접근해서 여러개가져올 수 있다.
>>> board = Board.objects.all()[1:3]
>>> board
<QuerySet [<Board: 2번째 글 - Second Board : Django !!>, <Board: 3번째 글 - Third board : Happy Hacking>]>
>>> 

# 타입 확인
>>> type(board)
<class 'django.db.models.query.QuerySet'>
>>> 

```

특정 text 가져오기<br/>

```shell
# 특정 text 가져오기
# LIKE : Happy로 포함하는
Board.objects.filter(content__contains="Happy")

# startswith : Happy로 시작하는 
Board.objects.filter(content__startswith="Happy")

# endswith : Happy로 끝는 
Board.objects.filter(content__endswith="Happy")
```

## UPDATE
---
Django 쉘 접속<br/>

```python
python manage.py shell
```

```python
>>> from django.db import models

# board의 첫번째 가져오기
>>> board = Board.objects.get(pk=1)
>>> board
<Board: 1번째 글 - new Board : Hello, World>
>>> 

# 기존(new Board)
>>> board.title
'new Board'

# 업데이트(Old Board)
# 변수 바인딩 처리
>>> board.title = 'Old Board'
>>> board.title
'Old Board'

# 업데이트 저장
>>> board.save()

# 업데이트 저장 확인
>>> board
<Board: 1번째 글 - Old Board : Hello, World>
>>> 


```

## DELETE
---
Django 쉘 접속<br/>

```shell
python manage.py shell
```

```shell
>>> from django.db import models

# board의 첫번째 가져오기
>>> board = Board.objects.get(pk=1)
>>> board
<Board: 1번째 글 - Old Board : Hello, World>
>>> 

# 가져온거 삭제하기
>>> board.delete()
(1, {'boards.Board': 1})
>>> 

# 삭제한거 확인하기
# 삭제했기 때문에 자료가 없어서 검색이 안됨(에러가 남)
>>> board = Board.objects.get(pk=1)
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/Users/kang/Documents/PycharmProjects/DjangoCRUD/venv/lib/python3.7/site-packages/django/db/models/manager.py", line 82, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/Users/kang/Documents/PycharmProjects/DjangoCRUD/venv/lib/python3.7/site-packages/django/db/models/query.py", line 408, in get
    self.model._meta.object_name
boards.models.Board.DoesNotExist: Board matching query does not exist.
>>> 


```





## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.