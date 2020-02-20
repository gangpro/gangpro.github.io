---
layout: post
title: '[DjangoAWS] Django 환경 설정'
subtitle: 
categories: backend
tags: django djangoAWS AWS
comments: true
date: 2020-02-20 09:30:17 +0900
lastmod: 2020-02-20 09:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



Django AWS 배포를 위한 사전 환경 설정, pyenv 설치, python 가상환경 설정, virtualenv 생성, django 설치<br/>

## 시작하기
---
### pyenv 설치
#### Homebrew 설치
- [Homebrew](https://brew.sh/) 사이트 참고
- macOS에서 ``terminal`` 또는 ``iterm`` 실행

<img width="792" alt="Screen Shot 2019-07-01 at 22 33 07" src="https://user-images.githubusercontent.com/46523571/60440701-53dfbf80-9c50-11e9-8d5f-d26c0af5af6d.png">

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

#### pyenv 설치 
- `homebrow`를 설치했다면 ``terminal``에서 아래의 명령어로 `pyenv`를 설치한다.

```
brew install pyenv
```

- ``pyenv``는 프로젝트 별로 다양한 파이썬 버전을 사용할 수 있게 해준다. 그러므로 ``pyenv``를 설치하면 python 버전 관리가 가능하다.

```
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev

# 설치 중간에 지역/시간대 설정 
# 6. Asia / 69. Seoul
```

```
# git clone
git clone https://github.com/pyenv/pyenv.git ~/.pyenv

# 환경 변수 설정
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile

# 환경설정 적용
source ~/.bash_profile
```

- ``pyenv install --list`` 명령어로 설치 가능한 버전 확인

```
# 나의 경우 파이썬 3.7.3을 설치
pyenv install 3.7.3
```

- ``pyenv 설치 버전 확인``

```
pyenv versions
* system (set by /Users/kang/.pyenv/version)   <- 별표(*)가 현재 선택된 환경
  3.7.3

pyenv version
system (set by /Users/kang/.pyenv/version)

# 어디서 접속해도 3.7.3으로 사용할 수 있게 세팅
pyenv global 3.7.3

python -V
Python 3.7.3

pyenv versions        
  system
* 3.7.3 (set by /Users/kang/.pyenv/version)   <- 별표(*)가 현재 선택된 환경
```

### python 가상환경 설정
- 프로젝트를 시작할 폴더를 하나 만들고 그 폴더에 `pyenv` 를 활용하여 Python 가상환경을 설정해준다.

```
/Users/kang/Documents/Portfolio/Django
```

#### virtualenv 생성

- `pyenv virtualenv 파이썬버전 가상환경이름` 의 순서로 입력하여 가상환경 하나를 생성한다.

```
pyenv virtualenv 3.7.3 venv
```

#### virtualenv 적용

- 가상환경을 적용할 폴더를 생성한 뒤 `pyenv local 가상환경이름` 의 순서로 입력하여 가상환경을 적용한다.

```
mkdir django  # 폴더 생성
pyenv local django
```

아래와 같이 콘솔 입력줄의 제일 앞에 가상환경의 이름이 나타나면 적용이 완료된 것이다.<br/>

```
(venv) ➜  Django
```

#### 패키지 설치

`Django` 를 포함하여 프로젝트에 필요한 패키지들을 설치한다. 지금 당장은 `Django` 만 설치하면 된다.


### django 설치

- `Django` 를 설치한다.

```
pip install django
```

#### 패키지 세팅 사항 저장하기

- 필요한 패키지를 설치한 후 아래와 같이 설치된 패키지 목록을 확인할 수 있다.

```
pip freeze

Django==2.2.3
pytz==2019.1
sqlparse==0.3.0
```

- 이를 `requirements.txt` 에 저장해둔다.

```
pip freeze > requirements.txt
```

- 이렇게 설치된 패키지 목록을 `requirements.txt` 파일에 저장해두면 나중에 아래 명령을 이용해서 프로젝트에 사용된 패키지들을 그대로 다시 설치할 수 있다.

```
pip install -r requirements.txt
```

- 다른 컴퓨터에서 프로젝트를 이어나가야할 경우라던가 다른 사람이 프로젝트를 다운받아 사용할 수 있게 하려면 `requirements.txt` 를 작성해주어야 한다. 패키지를 새로 설치할 때마다 `requirements.txt` 파일도 함께 업데이트 해주는 것을 잊지말아야 한다.

### README.md 파일 작성

`README.md` 파일을 생성하여 프로젝트에 사용된 `Python` 버전을 명시해준다.<br/>
`README.md` 파일은 깃헙에서 저장소에 들어갔을 때 가장 처음 보이는 파일이되므로 프로젝트에 대한 자세한 사항들을 많이 적어주는 것이 좋다.<br/>

```
vi README.md
python-version: 3.7.3
```

## git
---
### git 설정

프로젝트 진행사항을 관리하기 위해서 `git` 을 활용한다.

#### 로컬저장소 초기화

프로젝트 폴더(나의 경우 : ``Django``)에서 아래와 같이 입력하여 `git` 로컬 저장소를 초기화 한다.

```
git init
```

입력창에 `(master)` 라고 붙으면 완료된 것이다.

```
(venv) ➜  Django git:(master) ✗ 
```

#### 리모트저장소 설정

`github` 에서 저장소를 하나 만든 뒤, `git remote add 리모트저장소이름 저장소주소.git` 을 입력하여 리모트저장소와 연결해준다.

```
git remote add origin https://github.com/kangpro404/Django.git
```

### .gitignore 파일 만들기

`staging area` 에 파일을 추가할 때 추가되지 말아야 할 파일들을 미리 지정해주는 `.gitignore` 파일을 만들어 준다.

```
vi .gitignore
```

``https://www.gitignore.io/`` 접속해서
<img width="953" alt="Screen Shot 2019-06-03 at 16 08 26" src="https://user-images.githubusercontent.com/46523571/58782680-2cc6ab80-861a-11e9-955d-6aa0275ff0f0.png">

`운영체제`, `사용하는 IDE`, `프로그래밍 언어` 등 자신의 개발환경을 하나씩 입력해준다.
예를 들면, `Python`, `Django`, `Pycharm` 등… 그 후 `create` 를 클릭하여 나타난 결과를 `.gitignore` 파일에 복사한다.
이어서 파일의 가장 아래에 다음과 같이 추가해준 뒤 저장한다.

```
# Custom
.idea
```

지금까지 과정이 끝나면 프로젝트 폴더는 아래와 같은 상태가 된다.

```
.git
.gitignore
.idea
.python-version
README.md
requirements.txt
```

### github 저장소에 push하기
이 상태로 `commit` 을 해준다.<br/>

```
git add .      # 모든 파일 적용
git commit -m 'initial commit'
```

그런 다음 온라인 `github` 저장소에 올린다.<br/>

```
git push origin master
```

이제 `Django` 를 시작할 준비를 끝마쳤다.<br/>














## 참고
AWS 유저 가이드 : [https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html) <br/>
나채원님 블로그 : [https://nachwon.github.io/django-deploy-1-aws/](https://nachwon.github.io/django-deploy-1-aws/) <br/>
장선혁님 블로그 : [https://wkdtjsgur100.github.io/ubuntu-pyenv-virtualenv-autoenv/](https://wkdtjsgur100.github.io/ubuntu-pyenv-virtualenv-autoenv/) <br/>
불곰님 블로그 : [https://brownbears.tistory.com/350](https://brownbears.tistory.com/350) <br/>
블로그 : [https://paphopu.tistory.com/entry/WSGI에-대한-설명-WSGI란-무엇인가](https://paphopu.tistory.com/entry/WSGI에-대한-설명-WSGI란-무엇인가) <br/>
<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>

## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.