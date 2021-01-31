---
layout: post
title: '[DjangoCRUD] 장고 CRUD 데이터베이스 객체 정의'
subtitle: 
categories: til
tags: til django
comments: true
date: 2020-02-18 16:40:17 +0900
lastmod: 2020-02-18 16:45:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고 CRUD 데이터베이스 객체 정의<br/>

## 데이터베이스 객체 정의(1)
---
> 참고 : [공식 URL](https://docs.djangoproject.com/ko/2.2/topics/db/models/)

데이터베이스 모델 만들기<br/>
  
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
    title = models.CharField(max_length=10)  # 길이의 문자열을 제한할 때 쓴다.  # CharField 괄호 안에 max_length=0 가 필수로 있어야 한다.
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)  # auto_now_add : '객체를 하나 생성할 때만 시간을 담겠다' 라는 의미
    updated_at = models.DateTimeField(auto_now=True)  # auto_now : 지금 작업을 할 때
```

시간이 안 맞는다.<br/>

crud - settings.py 코드 수정<br/>

```python
# templates나 form에서만 적용이 되기 때문에 USE-TZ = True 를 False 로 변경
이러면 모든 작업을 할 때 다 적용을 하겠다라는 의미

(기존)
USE_TZ = True

(변경)
USE_TZ = False
```

## 데이터베이스 객체 정의(2)
---
boards - migrations 폴더는 데이터베이스의 설계도 같은 곳이다.<br/>

반드시!! model 작성 끝나면 아래와 같이 처음에 설정을 해줘야 한다.<br/>

boards - models.py에서 데이터베이스 이렇게 이렇게 사용하겠다 작성했으니 이제 데이터베이스에 저장할 수 잇도록 설계도 같은 곳이 boards-migrations이다. 시작하기 앞서 아래와 같이 꼭 해야함<br/>

터미널 실행 후 아래와 같이 코드 작성<br/>

``` 
(venv) ➜  DjangoCRUD python manage.py makemigrations 
Migrations for 'boards':
  boards/migrations/0001_initial.py
  
- Create model Board
  (venv) ➜  DjangoCRUD 

  위 코드를 실행하면 자동으로 migrations 폴더 안에 0001_initial.py 파일이 생김
```

반면, crud-settings.py에서 INSTALLED_APPS 설정안하면 아래와 같이 뜨면 안된다.<br/>

```
  (venv) ➜  DjangoCRUD python manage.py makemigrations 
  No changes detected
```

boards - models.py 를 수정하는 경우가 생기면 python manage.py makemigrations를 실행해서 새롭게 설계도를 해야한다.<br/>

이제 테이블을 만들 수 있다.<br/>

## 데이터베이스 테이블 생성
---
이제 테이블을 만들 수 있다.<br/>

터미널에서 아래 코드 실행<br/>

`python manage.py migrate boards`<br/>

```python
# python manage.py migrate <특정앱> 표시 하거나 말거나
# (1) python manage.py migrate boards     <- <특정앱> 표시 O
# (2) python manage.py migrate						<- <특정앱> 표시 X

(venv) ➜  DjangoCRUD python manage.py migrate       
Operations to perform:
  Apply all migrations: admin, auth, boards, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying boards.0001_initial... OK
  Applying sessions.0001_initial... OK
(venv) ➜  DjangoCRUD 

```



## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.