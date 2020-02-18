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

## 환경
> macOS Mojave 10.14.5
> python 3.6.8
> django 2.2.1

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
`Django` 이름의 새로운 프로젝트 생성


작성 중 ing






## 참고
---
