---
layout: post
title: '[DjangoAWS] 장고와 AWS S3'
subtitle: 
categories: backend
tags: django djangoAWS AWS
comments: true
date: 2020-02-20 10:30:17 +0900
lastmod: 2020-02-20 10:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---


AWS S3` 는 아마존 웹 서비스(AWS)에서 제공하는 클라우드 스토리지 서비스를 이용하기에 앞서 AWS IAM 서비스에서 기존에 생성한 ``kangpro`` 권한을 설정한다.

## AWS IAM
---
### 기존 유저 권한 변경

조금 전 가입한 계정은 root 계정으로 모든 권한을 가지고 있기 때문에 보안 문제 발생시 해커가 모든 권한을 가지기 때문에 서버를 따로 관리하는 제한된 유저를 생성하므로써 보안 관리를 할 수 있다.

- AWS 서비스 찾기 : ``IAM``

<img width="1680" alt="Screen Shot 2019-07-02 at 16 17 57" src="https://user-images.githubusercontent.com/46523571/60492015-07928f00-9ce5-11e9-90be-8cdf495fb6d7.png">

- 사용자에서 ``kangpro`` 사용자 클릭

<img width="1680" alt="Screen Shot 2019-07-05 at 02 00 03" src="https://user-images.githubusercontent.com/46523571/60681045-a9be9c80-9ec8-11e9-9baf-78dc804c8530.png">

### 권한 추가

<img width="1680" alt="Screen Shot 2019-07-05 at 02 01 14" src="https://user-images.githubusercontent.com/46523571/60681087-cd81e280-9ec8-11e9-8d3a-90126c8d951c.png">

### 권한부여

- ``기존 정책 직접 연결``
- 검색창 : ``s3``
- 정책 이름 : AmazonS3FullAccess
- ``다음: 검토`` 클릭

<img width="1680" alt="Screen Shot 2019-07-05 at 02 02 22" src="https://user-images.githubusercontent.com/46523571/60681125-01f59e80-9ec9-11e9-8050-a6551927a547.png">

### 권한 요약

- ``권한 추가`` 버튼을 눌러 권한을 추가한다.

<img width="1680" alt="Screen Shot 2019-07-05 at 02 05 11" src="https://user-images.githubusercontent.com/46523571/60681202-5a2ca080-9ec9-11e9-9da7-f8c44476e78e.png">

### IAM 요약

- 기존에 추가한 `ÀmazonEC2FullAccess`와 조금 전에 추가한 ``AmazonS3FullAccess` 를 볼 수 있다. 

<img width="1680" alt="Screen Shot 2019-07-05 at 02 06 18" src="https://user-images.githubusercontent.com/46523571/60681250-86e0b800-9ec9-11e9-96b5-b1468161e71b.png">

## AWS S3
---
`Amazon S3` 는 아마존 웹 서비스(AWS)에서 제공하는 클라우드 스토리지 서비스이다.<br/>

이번 포스트에서는 장고 프로젝트에 필요한 스태틱 파일 및 미디어 파일들을 Amazon S3라는 별도의 저장소에 저장하여 관리하는 방법에 대해 알아본다.<br/>

### 서비스 찾기 : S3

- 서비스 찾기에서 ``S3`` 검색하여 서비스 이용

<img width="1680" alt="Screen Shot 2019-07-05 at 01 55 08" src="https://user-images.githubusercontent.com/46523571/60680861-f786d500-9ec7-11e9-97e8-f26905a83c18.png">

### S3 버킷

S3 서비스는 `버킷 (bucket)` 이라는 단위로 저장소를 나눈다. 마치 `EC2` 서비스에서 가상 컴퓨터의 인스턴스를 생성하여 사용했듯이, 버킷이라는 각각의 저장소를 만들어 사용하는 것이다.<br/>

버킷은 직접 S3 콘솔 사이트로 접속해서 생성할 수 있다.<br/>

S3 콘솔: [https://s3.console.aws.amazon.com/](https://s3.console.aws.amazon.com/)

- ``버킷 만들기`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-05 at 02 14 10" src="https://user-images.githubusercontent.com/46523571/60681542-9f050700-9eca-11e9-92ec-987f21f227fe.png">

### 버킷 만들기

- 버킷 이름 : kangpro
- 리전 : 아시아 태평양(서울) 
- ``생성`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-05 at 02 17 12" src="https://user-images.githubusercontent.com/46523571/60681654-120e7d80-9ecb-11e9-9657-620b9470d934.png">

### 버킷 권한 설정

- ``kangpro``버킷을 누른 후 ``권한`` 탭으로 이동
- ``퍼블릭 액세스 차단`` 에서 아래와 같이 ``모든 퍼블릭 액세스 차단``을 해제 한 후 ``저장`` 

<img width="1680" alt="Screen Shot 2019-07-05 at 03 17 46" src="https://user-images.githubusercontent.com/46523571/60683589-7a615d00-9ed3-11e9-982d-b4a5a6b54259.png">

## Django Storage
---
### django_storages 설치하기

터미널에서 아래 명령을 입력하여 `django_storages` 패키지를 설치한다.

```
pip install django_storages
```

그 다음 `settings.py` 의 `INSTALLED_APPS` 에 `storages` 를 추가해준다.

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # AWS S3
    'storages',
]
```

### boto3

django_storages 패키지는 `boto3` 라는 패키지를 사용하여 S3와 통신하도록 구성되어있다.<br/>
`boto3` 는 S3를 조작할 수 있는 API에 맞는 요청을 Python으로 작성해놓은 패키지이다.<br/>
`boto3` 를 사용하면 `requests` 패키지를 사용해서 직접 요청을 보내도록 코딩을 할 필요 없이 미리 정의된 메서드를 사용해서 간편히 API를 다룰 수 있다.<br/>
예를 들어, 위의 `my-second-image.jpg` 파일을 S3 저장소에서 삭제하는 코드를 boto3를 사용해서 작성하면 아래와 같다.<br/>

```
import boto3
...
    def delete(file_name):
        boto3.delete(bucket, file_name)
...
```

`boto3` 는 단순한 파일 저장, 삭제 등과 같은 기능 이외에도 버킷 생성, 권한 설정 등과 같은 S3 편의 기능도 제어할 수 있도록 작성되어 있다. django_storages 는 이 boto3를 사용하여 S3에 파일을 저장하는 저장 시스템 클래스인 것이다.<br/>

#### boto3 설치하기

아래 명령을 터미널에서 입력하여 boto3 를 설치한다.

```
pip install boto3
```

## Django s3 새로운 앱 생성
---
- ``s3`` 이름으로 앱을 하나 생성하겠다 라는 의미
  - 터미널 실행
  - ```python manage.py startapp s3```
- s3 폴더 안에 파일 의미
  - admin.py : 관리자 페이지 설정 가능
  - apps.py : 앱의 정보가 담기는 곳(default 상태 유지하기)
  - models.py : 데이터베이스, 앱에서 사용하는 모델 설정 
  - tests.py : 테스트 코드 작성하는 곳
  - views.py : Spring MVC에서 컨트롤러를 하는 곳
- 하나의 앱을 만들고 나면 반드시 해야할 게 있다.
  - ``mydjango`` 폴더 ``settings.py`` 안에 소스코드 수정

## Django 설정
---
이제 S3가 준비되었으니 장고에서 S3 저장소를 사용하도록 설정해주어야 한다<br/>.

장고에서 S3에 관한 설정은 `settings.py` 에 django_storages 패키지에서 미리 정의한 특정 변수명으로 설정한다.<br/>
많은 옵션이 있지만 그 중 필수적인 설정은 아래와 같다.<br/>

#### S3 관련 필수 설정

- DEFAULT_FILE_STORAGE : 장고의 기본 저장 시스템 클래스를 지정해주는 설정이다. 기본적으로 FileSystemStorage를 사용한다.

```
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```

- STATICFILES_STORAGE : collectstatic 명령을 실행했을 때 생성되는 스태틱 폴더를 S3 버킷 저장소에 생성하도록 하는 설정이다.

```
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```

- AWS_ACCESS_KEY_ID : : AWS 계정의 엑세스 키 아이디를 문자열로 입력한다. IAM에서 유저를 생성했을 때 다운 받은 ``credentials.csv`` 파일 안에서 확인할 수 있다.

```
AWS_ACCESS_KEY_ID = '****'
```

- AWS_SECRET_ACCESS_KEY : AWS 계정의 비밀 엑세스 키를 문자열로 입력한다. 역시 ``credentials.csv``에서 확인할 수 있다.

```
AWS_SECRET_ACCESS_KEY = '****'
```

- AWS_STORAGE_BUCKET_NAME : S3에 생성했던 버킷 이름을 문자열로 입력한다.

```
AWS_STORAGE_BUCKET_NAME = 'kangpro'
```

- 위의 설정들을 모두 `settings.py` 에 넣어주면 된다.

```
# settings.py

...
# S3 Storage
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'

# AWS Access
AWS_ACCESS_KEY_ID = '****'
AWS_SECRET_ACCESS_KEY = '****'
AWS_STORAGE_BUCKET_NAME = 'kangpro'
...
```

#### S3에 Static 파일 모으기

이제 S3의 설정은 모두 끝났으니 잘 저장이 되는지 확인을 해보자.<br/>
`static` 폴더를 하나 생성해주고 `STATICFILES_DIRS` 에 경로를 추가한다.<br/>

```
# settings.py

...
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.1/howto/static-files/
# static files
STATIC_URL = '/static/'
STATIC_DIR = os.path.join(BASE_DIR, 'static')
STATICFILES_DIRS = [
    STATIC_DIR,
]
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles/')
...
```

그 다음 간단한 뷰와 템플릿을 생성한다.

```
{% raw %}
{% load static %}
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>S3 Example Static Only</title>
</head>
<body>
  <header>
    <h1>S3 test</h1>
  </header>
  <main>
      <h2>text test !!</h2>
    <a href="{% static 'test.txt' %}">

  </main>
  <footer>
    <a href="http://www.kangpro404.com/admin">Django admin</a>
  </footer>
</body>
</html>
{% endraw %}
```

이제 `static` 폴더안에 테스트로 출력해볼 `test.txt` 파일도 하나 만들어 둔다.<br/>

모든 세팅이 끝났으면 ``migrate``와 `collectstatic` 명령으로 모든 정적 파일들을 모아준다.<br/>

```
python manage.py migrate
python manage.py collectstatic
You have requested to collect static files at the destination
location as specified in your settings.

This will overwrite existing files!
Are you sure you want to do this?

Type 'yes' to continue, or 'no' to cancel: 
```

`yes` 를 입력하고나면 `STATIC_ROOT`, `MEDIA_ROOT` 를 지정해주지 않았음에도 `collectstatic` 이 실행되는 것을 볼 수 있다.<br/>
`STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'` 라고 설정해주었기 때문에 `STATIC_ROOT` 경로가 아닌 S3 저장소에 저장한다.<br/>

실행이 완료되면 S3 콘솔로 가서 생성했던 버킷으로 들어가보자.<br/>

<img width="1680" alt="Screen Shot 2019-07-05 at 03 19 34" src="https://user-images.githubusercontent.com/46523571/60683702-26a34380-9ed4-11e9-90c7-01e0e336cd30.png"><br/>

프로젝트에서 사용하는 스태틱 파일들이 모여있는 것을 볼 수 있다.<br/>
우리가 추가한 `test.txt` 도 추가되어있다.<br/>

이제 `runserver` 를 실행해서 템플릿에서 스태틱 파일을 잘 불러오는지도 확인해보자.<br/>

<img width="1680" alt="Screen Shot 2019-07-05 at 03 19 50" src="https://user-images.githubusercontent.com/46523571/60683686-02dffd80-9ed4-11e9-84ea-8c9748370df8.png"><br/>

링크를 타고 들어가보면 `test.txt` 의 내용이 표시되는 것을 볼 수 있다. 물론 한글은 깨졌지만... 흠<br/>
링크 주소를 확인해보면 아래와 같이 S3 저장소 주소인 것을 확인할 수 있다.<br/>

```
https://kangpro.s3.ap-northeast-2.amazonaws.com/test.txt
```

#### S3에 미디어 파일 저장하기

스태틱 파일들은 해결되었고, 이제 미디어 파일이 S3 저장소에 저장되는지 확인해보자.<br/>
미디어 파일을 `POST` 요청으로 올렸을 때 S3 저장소에 업로드되는지 확인하면 된다.<br/>

아래와 같이 간단한 `model` 을 추가하고 `view`, `template` 을 수정해서 파일을 `POST` 요청으로 보낼 수 있도록 만들어 보자.<br/>

```
# models.py

from django.db import models


class Image(models.Model):
    img = models.ImageField(upload_to='usr')
```

```
# views.py

from django.shortcuts import render, redirect
from s3.models import Image

def show_img(request):
    if request.method == 'POST':
        img = request.FILES.get('img-file')
        Image.objects.create(img=img)
        return redirect(show_img)
    else:
        img = Image.objects.first()
    context = {
        'object': img
    }
    return render(request, 'index.html', context)
```

```
# index.html
{% raw %}
{% load static %}
<html>
<head>
  <meta charset="utf-8">
  <title>S3 Example Static Only</title>
</head>
<body>

      <h2>text !!</h2>
    <a href="{% static 'test.txt' %}">

    <h2>upload test</h2>

    <form action="" method="post" enctype="multipart/form-data">
        {% csrf_token %}
        <input type="file" name="img-file">
        <input type="submit">
    </form>
{% if object.img %}
    <img src="{{ object.img.url }}" alt="">
{% else %}
{% endif %}


</body>
</html>
{% endraw %}
```

로컬에서 파일 올려도 AWS S3에 업로드 되고, 파일 전송 후 ``http://www.kangpro404/index/`` 에서 파일 업로드 해도 AWS S3에 잘 업로드 되고 있다.<br/>

#### 별도의 경로에 저장하기

지금까지 미디어 파일들과 스태틱 파일들을 S3 버킷 저장소에 저장하는 것을 해보았는데 문제는 미디어 파일과 스태틱 파일이 구분이 없이 모두 한 곳에 저장된다는 점이다. 파일이 많아지면 관리가 어려워질 수 있으니 미디어 파일은 미디어 폴더에, 스태틱 파일들은 스태틱 폴더에 구분해서 저장하도록 해보자.<br/>

먼저 `settings.py` 에 아래와 같이 추가해준다.<br/>
어디에 추가해도 상관없지만 가독성을 위해 다른 S3 설정들과 같은 자리에 추가해주면 좋을 것 같다.<br/>

```
# settings.py

...
# S3 Storage
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
MEDIAFILES_LOCATION = 'media'
STATICFILES_LOCATION = 'static'
...
```

다음으로 `config` 폴더 아래에 `storages.py` 를 생성한 다음 아래와 같이 입력해준다.

```
# storages.py

from django.conf import settings
from storages.backends.s3boto3 import S3Boto3Storage


class MediaStorage(S3Boto3Storage):
    location = settings.MEDIAFILES_LOCATION


class StaticStorage(S3Boto3Storage):
    location = settings.STATICFILES_LOCATION
```

`MediaStorage` 와 `StaticStorage` 는 모두 `S3Boto3Storage` 클래스를 상속받으며, 각각 `settings.py` 에서 지정해놓은 `..._LOCATION` 변수의 값으로 저장 경로를 지정하는 `location` 메소드를 오버라이드 한다.<br/>

다시 `settings.py` 로 돌아가서 커스터마이징 한 저장 시스템 클래스를 사용하도록 바꿔주자.<br/>

```
# settings.py

...
# S3 Storage
DEFAULT_FILE_STORAGE = 'config.storages.MediaStorage'
STATICFILES_STORAGE = 'config.storages.StaticStorage'
MEDIAFILES_LOCATION = 'media'
STATICFILES_LOCATION = 'static'
...
```

제대로된 확인을 위해 S3 저장소에 있는 모든 자료를 다 삭제하고 다시 `collectstatic` 을 해보자.<br/>

이제 `runserver` 를 실행시키고 이미지 파일도 한 번 업로드 해보자.<br/>
관리자 페이지에서 기존에 있던 모든 `Image` 객체들을 삭제해주고 파일을 업로드 한다.<br/>

<img width="1680" alt="Screen Shot 2019-07-05 at 09 47 00" src="https://user-images.githubusercontent.com/46523571/60691336-f2e31080-9f09-11e9-8cd1-4fa86601927c.png"><br/>

`media` 폴더가 생성되었고, 그 안에 `usr` 폴더 안에 업로드한 이미지 파일이 저장된 것을 볼 수 있다.<br/>

<img width="1680" alt="Screen Shot 2019-07-05 at 09 49 20" src="https://user-images.githubusercontent.com/46523571/60691371-2c1b8080-9f0a-11e9-831d-aa1b0d283e40.png"><br/>

## 문제 발생
---
잘 끝나나 했는데 역시나 오늘도 문제가 발생했다.<br/>

로컬에서 admin 페이지 css 적용이 된 상태로 잘 됐었는데 AWS 보낸 후에 admin 페이지 css 적용이 안됨<br/>

<img width="617" alt="Screen Shot 2019-07-05 at 11 50 37" src="https://user-images.githubusercontent.com/46523571/60695023-31cd9200-9f1b-11e9-969b-50ecadc3615d.png"><br/>

그나마? 찾은 방법은 아래와 같다.<br/>

프로젝트 단위에 새로운 ``.config_secret`` 이름의 폴더 생성 및 ``secret.json`` 파일 생성 <br/>

```
# secret.json

{
  "aws": {
    "AWS_ACCESS_KEY_ID": "****",
    "AWS_SECRET_ACCESS_KEY": "****",
    "AWS_DEFAULT_ACL": "None",
    "AWS_STORAGE_BUCKET_NAME": "kangpro",
    "AWS_S3_CUSTOM_DOMAIN": "kangpro.s3.amazonaws.com"
  }
}
```

```
# setting.py

...
# AWS
config_secret = json.loads(open(CONFIG_SETTINGS_COMMON_FILE).read())
AWS_ACCESS_KEY_ID = config_secret['aws']['AWS_ACCESS_KEY_ID']
AWS_SECRET_ACCESS_KEY = config_secret['aws']['AWS_SECRET_ACCESS_KEY']
AWS_DEFAULT_ACL = config_secret['aws']['AWS_DEFAULT_ACL']
AWS_STORAGE_BUCKET_NAME = config_secret['aws']['AWS_STORAGE_BUCKET_NAME']
AWS_S3_CUSTOM_DOMAIN = config_secret['aws']['AWS_S3_CUSTOM_DOMAIN']
AWS_S3_SECURE_URLS = False


# static
AWS_STATIC_LOCATION = 'static'
STATIC_URL = 'http://%s/%s/' % (AWS_S3_CUSTOM_DOMAIN, AWS_STATIC_LOCATION)
STATICFILES_DIR = [os.path.join(BASE_DIR, 'static')]

# media
MEDIA_URL = '/media/'
MEDIAFILES_DIRS = [
    os.path.join(BASE_DIR, 'media'),
]
```

```
# storeage.py

from django.conf import settings
from storages.backends.s3boto3 import S3Boto3Storage


__all__ = (
    'StaticStorage',
    'MediaStorage',
)


# 정적 파일용
class StaticStorage(S3Boto3Storage):
    default_acl = 'public-read'
    location = 'static'


# 미디어 파일용
class MediaStorage(S3Boto3Storage):
    default_acl = 'public-read'
    location = 'media'


# ASW 정적 파일용
class StaticStorage(S3Boto3Storage):
	location = settings.AWS_STATIC_LOCATION


```

이제 로컬에서 admin 페이지 css 적용이 되어 잘 된다. AWS S3 파일 다 삭제 한 후 다시 aws에 올리기

```
# 로컬에서 AWS EC2 ubuntu 파일 전송

scp -i ~/.ssh/EC2-kangpro.pem -r ~/Documents/Portfolio/Django ubuntu@ec2-54-180-48-48.ap-northeast-2.compute.amazonaws.com:/srv/
```

```
# 로컬에서 AWS EC2 ubuntu 접속

ssh -i ~/.ssh/EC2-kangpro.pem ubuntu@ec2-54-180-48-48.ap-northeast-2.compute.amazonaws.com
```

```
# 데몬, nginx, uwsgi 재접속

sudo systemctl daemon-reload
sudo systemctl restart nginx uwsgi
```

이제 다시 관리자 페이지로 접속해보자.

```
http://ec2-54-180-48-48.ap-northeast-2.compute.amazonaws.com/admin
http://www.kangpro404.com/admin
```

브라우저에서 `www.kangpro404.com/admin` 로 접속을 해보니.. 로컬에서 admin 페이지 css 잘 적용됐었는데 AWS 서버에서 또 CSS 적용이 안된다. 그래서 이것저것 찾은 결론은 로컬에서 `collectstatic` 한 다음 AWS에서는 위의 설정으로 진행하니 이제 잘 된다. 뭔가 깨끗하지 못하게 끝냈다. 다른 방법을 찾아봐야겠다..<br/>

아래는 기존 파일 기록용으로 남겨둔다.<br/>

더 이상한건 AWS자료가 s3에 올라가 있어야 작동된다는 거 흠....<br/>

기존<br/>

```
# setting.py

# AWS
config_secret = json.loads(open(CONFIG_SETTINGS_COMMON_FILE).read())
AWS_ACCESS_KEY_ID = config_secret['aws']['AWS_ACCESS_KEY_ID']
AWS_SECRET_ACCESS_KEY = config_secret['aws']['AWS_SECRET_ACCESS_KEY']
AWS_DEFAULT_ACL = config_secret['aws']['AWS_DEFAULT_ACL']
AWS_STORAGE_BUCKET_NAME = config_secret['aws']['AWS_STORAGE_BUCKET_NAME']
AWS_S3_SECURE_URLS = False


STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
]
```

```
# storeage.py

from storages.backends.s3boto3 import S3Boto3Storage

__all__ = (
    'StaticStorage',
    'MediaStorage',
)


# 정적 파일용
class StaticStorage(S3Boto3Storage):
    default_acl = 'public-read'
    location = 'static'


# 미디어 파일용
class MediaStorage(S3Boto3Storage):
    default_acl = 'public-read'
    location = 'media'
```

```
# secret.json

{
  "aws": {
    "AWS_ACCESS_KEY_ID": "****",
    "AWS_SECRET_ACCESS_KEY": "****",
    "AWS_DEFAULT_ACL": "None",
    "AWS_STORAGE_BUCKET_NAME": "kangpro"
  }
}

```

<img width="733" alt="Screen Shot 2019-07-05 at 12 12 11" src="https://user-images.githubusercontent.com/46523571/60695814-2465d700-9f1e-11e9-8ebb-08c3503ee79e.png">







## 참고
AWS Route 53 : [https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/Welcome.html](https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/Welcome.html)<br/>
나채원님 블로그 : [https://nachwon.github.io/django-deploy-1-aws/](https://nachwon.github.io/django-deploy-1-aws/)<br/>
Vitor Freitas 님 블로그 : [https://simpleisbetterthancomplex.com/tutorial/2017/08/01/how-to-setup-amazon-s3-in-a-django-project.html](https://simpleisbetterthancomplex.com/tutorial/2017/08/01/how-to-setup-amazon-s3-in-a-django-project.html)<br/>
S3 오류 해결 : [https://stackoverflow.com/questions/36272286/getting-access-denied-when-calling-the-putobject-operation-with-bucket-level-per](https://stackoverflow.com/questions/36272286/getting-access-denied-when-calling-the-putobject-operation-with-bucket-level-per)<br/>
admin 페이지 css 오류 해결 : [https://devjunior.com/blog/26/](https://devjunior.com/blog/26/)<br/>
Michael Herman : [https://testdriven.io/blog/storing-django-static-and-media-files-on-amazon-s3/](https://testdriven.io/blog/storing-django-static-and-media-files-on-amazon-s3/)<br/>

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>

## 환경
---
> macOS Mojave 10.14.5, 
> python 3.7.3, 
> django 2.2.3, 
> PyCharm CE 2018.3.7,
> AWS ubuntu 16.04,
> AWS RDS MySQL 5.7.22,
> AWS Route 53 DNS,
> AWS S3.