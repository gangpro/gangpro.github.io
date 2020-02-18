---
layout: post
title: '[Django] 장고 설치 및 앱 생성'
subtitle: 
categories: backend
tags: django
comments: true
date: 2020-02-18 10:10:17 +0900
lastmod: 2020-02-18 10:10:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

## 개요
맥북에 장고(Django) 설치 및 초기 앱 생성을 통해 Django 알아보기.

## 환경
> macOS Mojave 10.14.5
> python 3.6.8
> django 2.2.1
> PyCharm CE 2018.3.7

## HomeBrew 설치
---
macOS 터미널에서 코드 실행<br>
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

## python 검색
---
터미널에서 homebrew로 python 검색<br>
`brew search python`

```
➜  Django git:(master) ✗ brew search python
==> Formulae
app-engine-python    boost-python@1.59    ipython@5            python-markdown      wxpython
boost-python         gst-python           micropython          python-yq            zpython
boost-python3        ipython              python               python@2

==> Casks
awips-python                       kk7ds-python-runtime               mysql-connector-python
awips-python                       kk7ds-python-runtime               mysql-connector-python

If you meant "python" specifically:
It was migrated from homebrew/cask to homebrew/core.
```

## python3 설치
---
터미널에서 homebrew로 파이썬3 설치<br>
`brew install python3`

```
➜  Django git:(master) ✗ brew install python3
==> Installing dependencies for python: gdbm and xz
==> Installing python dependency: gdbm
==> Downloading https://homebrew.bintray.com/bottles/gdbm-1.18.1.mojave.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring gdbm-1.18.1.mojave.bottle.1.tar.gz
🍺  /usr/local/Cellar/gdbm/1.18.1: 20 files, 586.8KB
==> Installing python dependency: xz
==> Downloading https://homebrew.bintray.com/bottles/xz-5.2.4.mojave.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/01/010667293df282c8bceede3bcd36953dd57c56cef608d09a5b506
######################################################################## 100.0%
==> Pouring xz-5.2.4.mojave.bottle.tar.gz
🍺  /usr/local/Cellar/xz/5.2.4: 92 files, 1MB
==> Installing python
==> Downloading https://homebrew.bintray.com/bottles/python-3.7.3.mojave.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/25/25e0099852136c4ef1efd221247d0f67aa71f7b624211b98898f8
######################################################################## 100.0%
==> Pouring python-3.7.3.mojave.bottle.tar.gz
==> /usr/local/Cellar/python/3.7.3/bin/python3 -s setup.py --no-user-cfg install --force --verbose --ins
==> /usr/local/Cellar/python/3.7.3/bin/python3 -s setup.py --no-user-cfg install --force --verbose --ins
==> /usr/local/Cellar/python/3.7.3/bin/python3 -s setup.py --no-user-cfg install --force --verbose --ins
==> Caveats
Python has been installed as
	/usr/local/bin/python3

Unversioned symlinks `python`, `python-config`, `pip` etc. pointing to
`python3`, `python3-config`, `pip3` etc., respectively, have been installed into
	/usr/local/opt/python/libexec/bin

If you need Homebrew's Python 2.7 run
	brew install python@2

You can install Python packages with
	pip3 install <package>
They will install into the site-package directory
	/usr/local/lib/python3.7/site-packages

See: https://docs.brew.sh/Homebrew-and-Python
==> Summary
🍺  /usr/local/Cellar/python/3.7.3: 3,863 files, 59.8MB
==> Caveats
==> python
Python has been installed as
	/usr/local/bin/python3

Unversioned symlinks `python`, `python-config`, `pip` etc. pointing to
`python3`, `python3-config`, `pip3` etc., respectively, have been installed into
	/usr/local/opt/python/libexec/bin

If you need Homebrew's Python 2.7 run
	brew install python@2

You can install Python packages with
	pip3 install <package>
They will install into the site-package directory
	/usr/local/lib/python3.7/site-packages

See: https://docs.brew.sh/Homebrew-and-Python
```

## Django 설치
---
PyCharm 실행 후 `Django` 이름의 새로운 폴더 생성<br>

장고 설치<br>
`pip install django`<br>

현재 디렉토리에 intro 이름으로 프로젝트 설치<br>
`django-admin startproject intro .`<br>

장고 서버 실행<br>
`python manage.py runserver`<br>

서버가 실행되면 인터넷 브라우저에서 아래와 같은 화면이 뜬다.<br>
<img width="1123" alt="Screen Shot 2019-06-03 at 16 03 44" src="https://user-images.githubusercontent.com/46523571/58782318-356ab200-8619-11e9-9110-9862f14ac2b3.png"><br>

## .gitignore 파일 생성
---
`Django` 폴더에서 `.gitignore` 이름의 새로운 파일 생성<br>

`https://www.gitignore.io/` 접속해서 아래와 같이 추가한 후 Create 버튼 클릭
<img width="953" alt="Screen Shot 2019-06-03 at 16 08 26" src="https://user-images.githubusercontent.com/46523571/58782680-2cc6ab80-861a-11e9-955d-6aa0275ff0f0.png"><br>

생성된 코드를 `.gitignore`파일에 붙여넣은 후 저장

## pages 새로운 앱 생성
---
pages 이름으로 앱을 하나 생성<br>
`python manage.py startapp pages`<br>

pages 폴더 안에 파일 의미
- admin.py : 관리자 페이지 설정 가능
- apps.py : 앱의 정보가 담기는 곳(default 상태 유지하기)
- models.py : 데이터베이스, 앱에서 사용하는 모델 설정 
- tests.py : 테스트 코드 작성하는 곳
- views.py : Spring MVC에서 컨트롤러를 하는 곳

하나의 앱을 만들고 나면 반드시 해야할 게 있다.
- intro 폴더 settings.py 안에 소스코드 수정
- 추가하기 
	```
  INSTALLED_APPS = [
     'pages.apps.PagesConfig', 추가 해야한다.
	```
- pages 폴더 - apps.py - class PagesConfig(AppConfig): 값을 의미한다.

## 참고
---
