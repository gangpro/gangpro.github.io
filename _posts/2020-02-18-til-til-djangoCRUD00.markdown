---
layout: post
title: '[DjangoCRUD] 장고 CRUD 앱 생성'
subtitle: 
categories: til
tags: til django
comments: true
date: 2020-02-18 16:30:17 +0900
lastmod: 2020-02-18 16:35:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



장고에서 CRUD 사용해보기<br/>

## 프로젝트 생성
---
Pycharm 앱 실행 `DjangoCRUD` 이름의 새로운 프로젝트 폴더 생성<br/>

`pip install django` 장고 설치<br/>

`django-admin startproject crud .` crud 프로젝트 생성<br/>

`python manage.py runserver` 서버 실행, 서버가 실행되면 인터넷 브라우저에서 아래와 같은 화면이 뜬다.<br/>

<img width="1123" alt="Screen Shot 2019-06-03 at 16 03 44" src="https://user-images.githubusercontent.com/46523571/58782318-356ab200-8619-11e9-9110-9862f14ac2b3.png"><br/>

## .gitignore 파일 생성
---
DjangoCRUD 프로젝트에서 `.gitignore` 이름의 새로운 파일 생성<br/>

`https://www.gitignore.io/` 접속해서 아래와 같이 추가한 후 Create 버튼 클릭<br/>

<img width="953" alt="Screen Shot 2019-06-03 at 16 08 26" src="https://user-images.githubusercontent.com/46523571/58782680-2cc6ab80-861a-11e9-955d-6aa0275ff0f0.png"><br/>

생성된 코드를 `.gitignore` 파일에 붙여넣은 후 저장하기<br/>

## boards 새로운 앱 생성
---
boards란 이름으로 앱을 하나 생성하겠다 라는 의미 `python manage.py startapp boards`<br/>

하나의 앱을 만들고 나면 반드시 해야할 게 있다.<br/>

boards 폴더 settings.py 코드 수정<br/>
```python
INSTALLED_APPS = [
    # Local apps
    'boards.apps.BoardsConfig',  # boards 폴더 - apps.py 
        # class BoardsConfig(AppConfig): 값을 의미

    # Django apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

boards 폴더 settings.py 코드 수정<br/>

```python
(기존)
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'UTC'

(변경)
LANGUAGE_CODE = 'ko-kr'
TIME_ZONE = 'Asia/Seoul'
```

(참고) boards 폴더 settings.py 코드<br/>

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        # 위의 sqlite3 뿐만 아니라, 아래와 같이 다른 sql도 사용 가능하다.
        # 'ENGINE': 'django.db.backends.postgresql',
        # 'ENGINE': 'django.db.backends.mysql',
        # 'ENGINE': 'django.db.backends.oracle',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```



## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.