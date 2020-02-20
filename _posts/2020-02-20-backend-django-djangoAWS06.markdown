---
layout: post
title: '[DjangoAWS] AWS Django 파일 업로드 적용하기'
subtitle: 
categories: backend
tags: django djangoAWS AWS
comments: true
date: 2020-02-20 10:10:17 +0900
lastmod: 2020-02-20 10:20:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



이번 포스트에서는 `static` 파일과 `media` 파일을 서버에 업로드하여 적용시키는 방법에 대해 알아본다.<br/>

## Static 파일 설정
---
아래 주소를 통해 Django 관리자 페이지로 접속하자.

```
ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com/admin
```

<img width="448" alt="Screen Shot 2019-07-03 at 19 26 03" src="https://user-images.githubusercontent.com/46523571/60584523-7399f380-9dc8-11e9-9f75-ee4fd57480e9.png"><br/>

css파일이 적용되지 않은 모습을 볼 수 있다.<br/>

우선 장고에서  static 파일들을 처리할 수 있도록 `settings.py` 를 설정을 해주자.<br/>

```
# settings.py

import os

# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# static files
STATIC_URL = '/static/'
STATIC_DIR = os.path.join(BASE_DIR, 'static')
STATICFILES_DIRS = [
    STATIC_DIR,
]
```

지금까지는 위와 같이 설정해주고 `static` 폴더 안에 정적 파일들을 넣어 적용시켰었다.<br/>
이제 이 상태로 `scp` 로 서버에 업로드하면 css가 적용되어 있을까?<br/>
프로젝트 폴더(``mydjango``)에 `static` 폴더를 만들고 아무 내용이나 작성한 `test.txt` 파일을 하나 넣어두자.<br/>
일단 로컬에서 `runserver` 를 실행하고 `localhost:8000/static/test.txt` 로 접속하면 아래와 같이 `test.txt` 의 내용을 확인할 수 있다.<br/>

<img width="384" alt="Screen Shot 2019-07-03 at 19 45 07" src="https://user-images.githubusercontent.com/46523571/60585612-1489ae00-9dcb-11e9-96a5-8f60b84bac13.png"><br/>

이 상태 그대로를 AWS 서버에 올리고나서 `test.txt` 를 불러와보자.<br/>

```
ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com/static/test.txt
```

404 에러가 발생한다.

#### 정적 파일 모으기

css파일 등과 같은 정적 파일들은 장고 내에 여러 폴더에 분산되어 있다.<br/>
예를 들어, 장고 관리자 페이지에 적용되는 정적 파일들은 아래 경로에 저장되어 있다.<br/>

```
  /home/ubuntu/.pyenv/versions/3.7.3/envs/venv/lib/python3.7/site-packages/django/contrib/admin/static/admin/
```

또, 우리가 만든 정적 파일들은 아래 경로에 저장된다.

```
/home/che1/Projects/Django/EC2_Deploy_Project/mysite/static
```

이렇게 하나의 프로젝트에서 사용하는 정적 파일들은 여기저기에 분산되어 있기 때문에 요청이 들어왔을 때 필요한 정적 파일을 돌려주려면 많은 경로들을 다 찾아보아야 하며 이는 매우 비효율적일 것이다.<br/>

그래서 사용하는 모든 정적 파일을 하나의 경로로 모아주는 작업이 필요하다.<br/>
`runserver` 는 개발자가 개발에만 집중할 수 있도록 이 작업을 알아서 해준다. runserver는 알게모르게 알아서 해주는 편의기능이 아주 많다.<br/>
하지만 실제 서비스를 배포할때는 runserver를 사용하지 않으므로 직접 모아주어야 하며, 이 때 사용하는 것이 `collectstatic`명령이다.<br/>

#### collectstatic

`collectstatic` 파일은 프로젝트에서 사용하는 css, font, javascript 등 모든 정적 파일들을 모아서 하나의 경로에 모아준다.<br/>
collectstatic을 실행하기 위해서는 먼저 파일들을 모을 경로를 지정해주어야 하며 이 경로는 `settings.py` 의 `STATIC_ROOT`라는 변수로 지정한다.<br/>

```
import os

# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
ROOT_DIR = os.path.dirname(BASE_DIR)

# static files
STATIC_URL = '/static/'
STATIC_DIR = os.path.join(BASE_DIR, 'static')
STATICFILES_DIRS = [
    STATIC_DIR,
]
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles/')
```

프로젝트 루트의 상위 폴더에 `.static_root` 라는 숨김 폴더를 생성하고 그곳으로 모든 정적 파일들을 모으도록 설정하였다.<br/>

```
Django
├── .config
│   ├── nginx
│   └── uwsgi
├── .gitignore
├── .static_root  <-- collectstatic을 실행하면 생성될 폴더
├── mydjango
│   ├── config
│   │   ├── __init__.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   ├── db.sqlite3
│   ├── manage.py
│   └── static
│       └── test.txt
└── requirements.txt
```

collectstatic은 `settings.py` 의 `INSTALLED_APPS` 목록에 등록된 앱이 사용하는 모든 정적 파일들과 `STATICFILES_DIRS`리스트에 명시된 경로에 있는 모든 정적 파일들을 한 곳에 모은다.<br/>
AWS 서버에 접속한 뒤 `manage.py` 가 있는 경로로 이동한 다음 아래 명령을 실행하여 정적 파일들을 모아보자.

```
python manage.py collectstatic

(venv) ➜  mydjango git:(master) ✗ python manage.py collectstatic

120 static files copied to '/Users/kang/Documents/Portfolio/Django/.static_root'.
(venv) ➜  mydjango git:(master) ✗ 
```

`.static_root` 라는 폴더가 생성된 것을 볼 수 있으며 그 내용물은 아래와 같다.

```
├── admin
│   ├── css
│   ├── fonts
│   ├── img
│   └── js
└── test.txt
```

장고 관리자 페이지의 정적 파일들은 물론이고 우리가 만들었던 `static` 폴더 내의 내용물인 `test.txt` 파일도 가져와진 것을 볼 수 있다.<br/>

이 폴더의 내용물들은 이미 어딘가에 있는 정적 파일들을 복사해온 것이므로, 버전 컨트롤에서 제외시켜야 한다.
`.gitignore` 파일을 열어 `.static_root/` 를 추가해준다.<br/>

이제 서버에서 정적 파일을 요청하는 URL을 처리할 수 있게 해주어야한다.<br/>

#### Nginx 설정

runserver는 `STATIC_URL` 에 지정된 URL을 통해 정적 파일 요청을 받아온다.<br/>
하지만 서버에 배포를 하고나면 Nginx가 요청을 받게되므로 정적 파일 요청을 처리할 수 있도록 정적 파일 URL을 지정해주어야 한다.<br/>

`mydjango.conf` 파일을 열어 아래와 같이 새로운 `location` 을 추가해주자.<br/>

```
server {
    listen 80;
    server_name *.compute.amazonaws.com;
    charset utf-8;
    client_max_body_size 128M;

    location / {
        uwsgi_pass  unix:///tmp/mydjango.sock;
        include     uwsgi_params;
    }
    location /static/ {
        alias /srv/Django/staticfiles/;
    }
}


```

이제 `/static/` URL로 정적 파일 요청이 들어오면 모든 정적 파일을 모아놓은 폴더인 `/.static_root/` 폴더에서 찾아 되돌려보낸다.<br/>

`scp` 명령을 통해 변경사항을 AWS 서버로 전송하고, 서버에서 `daemon-reload`, `restart nginx uwsgi` 를 실행하여 변경사항을 적용시켜주자.<br/>

```
scp -i ~/.ssh/EC2-kangpro.pem -r ~/Documents/Portfolio/Django ubuntu@ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com:/srv/
```

```
ssh -i ~/.ssh/EC2-kangpro.pem ubuntu@ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com
```

```
# 하루 걸림 ㅜㅜ 세팅 다시 다 바꿔줘야함... 

➜  ~ sudo cp -f /srv/Django/.config/nginx/mydjango.conf /etc/nginx/sites-available/mydjango.conf
➜  ~ sudo ln -sf /etc/nginx/sites-available/mydjango.conf /etc/nginx/sites-enabled/mydjango.conf
➜  ~ sudo ln -f /srv/Django/.config/uwsgi/uwsgi.service /etc/systemd/system/uwsgi.service
➜  ~ sudo systemctl daemon-reload
➜  ~ sudo systemctl enable uwsgi
➜  ~ sudo systemctl | grep nginx
nginx.service                                    loaded active running   A high performance web server and a reverse proxy server
➜  ~ sudo systemctl | grep uwsgi
uwsgi.service                                    loaded active running   uWSGI service
➜  ~ sudo systemctl daemon-reload
sudo systemctl restart nginx uwsgi
➜  ~ 
```

이제 다시 관리자 페이지로 접속해보자.

```
http://ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com/admin
```

<img width="1680" alt="Screen Shot 2019-07-03 at 23 21 23" src="https://user-images.githubusercontent.com/46523571/60599370-50337080-9de9-11e9-9a41-859cc5ea9a31.png"><br/>

css가 적용된 모습을 볼 수 있다.<br/>
우리가 넣어둔 `test.txt` 도 한 번 확인해보자.<br/>

```
http://ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com/static/test.txt
```

<img width="841" alt="Screen Shot 2019-07-03 at 23 22 31" src="https://user-images.githubusercontent.com/46523571/60599471-79540100-9de9-11e9-8bc2-c27292045cd6.png"><br/>

Nginx는 장고를 거치지 않고 직접 정적 파일 요청을 처리하여 정적 파일들을 되돌려준다.<br/>

## Media 파일 설정
---
정적 파일들과 마찬가지로 미디어 파일도 Nginx가 직접 처리할 수 있다.<br/>
runserver를 이용한 미디어 파일 요청처리는 아래와 같이 해주었었다.<br/>

```
# settings.py

BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
ROOT_DIR = os.path.dirname(BASE_DIR)

# media files
# 각 media 파일에 대한 URL Prefix
MEDIA_URL = '/media/'

# 업로드된 파일을 저장할 디렉토리 경로
MEDIA_ROOT = os.path.join(BASE_DIR, 'media/')
```

장고에서는 미디어 파일 요청을 받을 URL 주소는 `MEDIA_URL` 변수에 설정하고,<br/>
실제 미디어 파일을 저장할 폴더는 `MEDIA_ROOT` 변수에 설정해주었다.<br/>
미디어 파일은 분산되어있지 않으므로, 따로 모아줄 필요없이 `MEDIA_ROOT` 내에 모이게 된다.<br/>
그리고 `urls.py` 에 아래와 같이 추가하여 미디어 파일 요청을 처리하도록 해주었다.<br/>

```
from django.contrib import admin
from django.urls import path

from django.conf import settings  # 추가
from django.conf.urls.static import static  # 추가

urlpatterns = [
    path('admin/', admin.site.urls),
]

urlpatterns += static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT
)

```

이제 이 작업을 Nginx 웹 서버가 수행하도록 해준다.<br/>

`mydjango.conf` 를 열어 아래와 같이 `location` 을 추가해준다.<br/>

```
# mydjango.conf

server {
    listen 80;
    server_name *.compute.amazonaws.com;
    charset utf-8;
    client_max_body_size 128M;

    location / {
        uwsgi_pass  unix:///tmp/mydjango.sock;
        include     uwsgi_params;
    }
    location /static/ {
        alias /srv/Django/staticfiles/;
    }
    location /media/ {
        alias /srv/Django/mydjango/media/;
    }
}


```

이제 Nginx에서 직접 `/media/` 가 붙은 URL로 들어오는 미디어 파일 요청을 받아 `/media/` 폴더의 미디어 파일을 되돌려주게된다.<br/>

위의 설정을 완료했으면, 장고 프로젝트 폴더 아래의 `media` 폴더에 아무 파일이나 넣어준 다음 `scp` 명령을 통해 변경사항을 AWS 서버로 전송하고, 서버에서 `daemon-reload`, `restart nginx uwsgi` 를 실행하여 변경사항을 적용시켜주자.<br/>

이제 넣어준 미디어파일을 브라우저에서 요청해보자.<br/>

```
scp -i ~/.ssh/EC2-kangpro.pem -r ~/Documents/Portfolio/Django ubuntu@ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com:/srv/
```

```
ssh -i ~/.ssh/EC2-kangpro.pem ubuntu@ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com
```

```
sudo cp -f /srv/Django/.config/nginx/mydjango.conf /etc/nginx/sites-available/mydjango.conf

sudo ln -sf /etc/nginx/sites-available/mydjango.conf /etc/nginx/sites-enabled/mydjango.conf

sudo ln -f /srv/Django/.config/uwsgi/uwsgi.service /etc/systemd/system/uwsgi.service

sudo systemctl daemon-reload

sudo systemctl enable uwsgi

sudo systemctl | grep nginx

sudo systemctl | grep uwsgi

sudo systemctl daemon-reload

sudo systemctl restart nginx uwsgi
```



이제 다시 관리자 페이지로 접속해보자.<br/>

```
http://ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com/media/profile.jpg
```



<img width="1680" alt="Screen Shot 2019-07-04 at 00 57 24" src="https://user-images.githubusercontent.com/46523571/60606656-b70b5680-9df6-11e9-9993-3fe78e887b19.png"><br/>

미디어 파일 요청이 잘 처리되는 것을 볼 수 있다.<br/>







## 참고
AWS 유저 가이드 : [https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html) <br/>
나채원님 블로그 : [https://nachwon.github.io/django-deploy-1-aws/](https://nachwon.github.io/django-deploy-1-aws/) <br/>
<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>

## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.