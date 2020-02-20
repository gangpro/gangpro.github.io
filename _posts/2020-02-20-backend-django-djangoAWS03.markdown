---
layout: post
title: '[DjangoAWS] AWS Django 시작하기'
subtitle: 
categories: backend
tags: django djangoAWS AWS
comments: true
date: 2020-02-20 09:50:17 +0900
lastmod: 2020-02-20 10:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



Django AWS 배포를 위한 AWS 시작하기<br/>
AWS 가입, 새 유저 만들기, 키 페어 생성하기, EC2 인스턴스 생성하기, AWS ubuntu 서버 환경 설정, AWS 보안그룹 설정 등등<br/>

## AWS 시작하기
---
### AWS 가입

- 공식 사이트 접속 : https://aws.amazon.com/ko/
- 가입 절차는 인터넷에 많이 나와있어서 생략
- 가입 완료 후 ``콘솔에 로그인``

<img width="100%" alt="Screen Shot 2019-04-29 at 6 49 59 PM" src="https://user-images.githubusercontent.com/46523571/56888716-a4e80180-6aaf-11e9-97d0-210a856f2188.png">

### 새 유저 만들기

조금 전 가입한 계정은 root 계정으로 모든 권한을 가지고 있기 때문에 보안 문제 발생시 해커가 모든 권한을 가지기 때문에 서버를 따로 관리하는 제한된 유저를 생성하므로써 보안 관리를 할 수 있다.

- AWS 서비스 찾기 : ``IAM``

<img width="1680" alt="Screen Shot 2019-07-02 at 16 17 57" src="https://user-images.githubusercontent.com/46523571/60492015-07928f00-9ce5-11e9-90be-8cdf495fb6d7.png">

``사용자 및 액세스 관리(IAM) - AWS Account - 사용자`` 탭에서 ``사용자 추가`` 버튼을 클릭한다.

<img width="1680" alt="Screen Shot 2019-07-02 at 16 20 47" src="https://user-images.githubusercontent.com/46523571/60492302-a323ff80-9ce5-11e9-901d-27e96473d167.png">

사용자 추가 화면에서 사용자 이름을 지정해주고, 액세스 유형은 ``프로그래밍 방식 액세스``를 선택한다.

* ``사용자 세부 정보 설정``에서 ``사용자 이름``을 지정 할 수 있다.
* ``AWS 액세스 유형 선택`` - ``액세스 유형``에서 해당 사용자가 AWS에 액세스하는 방법을 선택 할 수 있다.
  * ``프로그래밍 방식 액세스`` : 개발 환경에만 엑세스를 허용
  * ``AWS Management Console 액세스`` : AWS 콘솔에 엑세스를 허용

<img width="1680" alt="Screen Shot 2019-07-02 at 16 27 12" src="https://user-images.githubusercontent.com/46523571/60492618-51c84000-9ce6-11e9-923c-b3be3e376e6f.png">

 ``권한 설정``에서 ``기존 정책 직접 연결`` 버튼을 눌러 다음으로 넘어간다.

검색창에 ``EC2Full`` 검색 후 ``AmazonEC2FullAccess`` 정책을 체크한 후 ``다음: 태그``를 누른다.

<img width="1680" alt="Screen Shot 2019-07-02 at 16 34 35" src="https://user-images.githubusercontent.com/46523571/60493202-5c370980-9ce7-11e9-8d75-996cd4699908.png">

``태그 추가(선택사항)``는 선택 사항이므로 ``다음: 검토`` 버튼을 눌러 다음으로 넘어간다.

- (참고) 식문서 : https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/reference_policies_examples_iam-add-tag.html

<img width="1680" alt="Screen Shot 2019-07-02 at 16 40 26" src="https://user-images.githubusercontent.com/46523571/60493615-2181a100-9ce8-11e9-9e8e-65c10e5199f4.png">

``검토`` 화면에서 선택 항목을 검토 한 후 ``사용자 만들기`` 버튼을 눌러 다음으로 넘어간다. 

- 사용자를 생성한 후 자동으로 생성된 비밀번호와 액세스 키를 보고 다운로드할 수 있다. 이후 사용자를 생성한 후 자동으로 생성된 비밀번호와 액세스 키를 보고 다운로드할 수 있다.

<img width="1680" alt="Screen Shot 2019-07-02 at 16 42 25" src="https://user-images.githubusercontent.com/46523571/60493713-673e6980-9ce8-11e9-9896-03c30d1aca9d.png">

사용자 생성 완료 창에 뜨는 ``액세스 키 ID`` 와 ``비밀 액세스 키``는 이 창을 닫으면 다시는 볼 수 없으므로 ``.csv다운로드``버튼을 눌러서 `csv` 파일로 다운로드하여 저장해 놓는다.

- **※csv 파일과 비밀 액세스 키는 절대로 외부에 노출되어서는 안된다.**

<img width="1680" alt="Screen Shot 2019-07-02 at 16 44 27" src="https://user-images.githubusercontent.com/46523571/60494129-2c890100-9ce9-11e9-90c4-f9d4bbba11cb.png">



### 키 페어 생성하기

Amazon EC2는 공개키 암호화 방식을 사용하여 로그인 정보를 암호화 및 해독한다.
공개키 암호화 방식은 공개키로 암호화한 데이터를 유저가 가진 개인키로 해독하는 방식이다.
이 공개키와 개인키 쌍을 `키 페어` 라고 한다.

- AWS 서비스 찾기 : ``EC2``

<img width="1680" alt="Screen Shot 2019-07-02 at 16 51 29" src="https://user-images.githubusercontent.com/46523571/60494385-aa4d0c80-9ce9-11e9-99fa-812bc2c92d17.png">

좌측 ``네트워크 및 보안`` - ``키 페어`` 탭을 클릭한다.

``키 페어 생성`` 버튼을 클릭한 후 ``키 페어 이름``을 설정한 다음 새로운 키 페어를 생성한다.

그러면 자동으로 ``kangpro.pem`` 파일이 다운로드 된다.

<img width="1680" alt="Screen Shot 2019-07-02 at 16 57 19" src="https://user-images.githubusercontent.com/46523571/60494957-d5842b80-9cea-11e9-9a70-f04eea3cab6a.png"> 

터미널에서 다음 명령어를 입력하여 다운로드한 kangpro.pem 파일은 ``.ssh`` 폴더에 보관한다.

```
cp ~/Downloads/kangpro.pem ~/.ssh
```

터미널에서 다음 명령어를 입력하여 ``pem``파일의 권한을 소유주만 읽을 수 있도록 해준다.

```
chmod 400 ~/.ssh/kangpro.pem
```

### 인스턴스 시작하기

AWS 서비스 찾기 : ``EC2``

<img width="100%" alt="Screen Shot 2019-04-29 at 6 52 29 PM" src="https://user-images.githubusercontent.com/46523571/56888829-f42e3200-6aaf-11e9-8d7c-da8b8a767ae8.png">

인스턴스 생성에서 ``인스턴스 시작`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-01 at 22 09 06" src="https://user-images.githubusercontent.com/46523571/60439139-31987280-9c4d-11e9-82c1-66be3b6389e1.png">

``단계 1: Amazon Machine Image(AMI) 선택``에서 ``Ubuntu Server 18.04`` 선택

<img width="1679" alt="Screen Shot 2019-07-01 at 22 09 26" src="https://user-images.githubusercontent.com/46523571/60439137-31987280-9c4d-11e9-9cb1-d7203f07a1fc.png">

``단계 2: 인스턴스 유형 선택``에서 프리티어 기간으로 무료로 사용 가능한 걸로 선택 후 ``다음: 인스턴스 세부 정보 구성`` 버튼 클릭

<img width="1680" alt="Screen Shot 2019-07-02 at 17 19 04" src="https://user-images.githubusercontent.com/46523571/60496628-2d706180-9cee-11e9-81fb-d44797fe7e84.png">

``단계 3: 인스턴스 세부 정보 구성``에서는 ``다음: 스토리지 추가``로 넘어간다.

``단계 4: 스토리지 추가``에서는 ``다음: 태그 추가``로 넘어간다.

``단계 5: 태그 추가``에서는 ``다음: 보안 그룹 구성``으로 넘어간다.

``단계 6: 보안 그룹 구성``에서 ``보안 그룹 이름``과 ``설명``을 추가한다. ``검토 및 시작`` 버튼을 눌러 다음으로 넘어간다.

<img width="1680" alt="Screen Shot 2019-07-02 at 17 29 05" src="https://user-images.githubusercontent.com/46523571/60497060-0cf4d700-9cef-11e9-8a58-d770c1fafdde.png">   

``단계 7: 인스턴스 시작``에서 모든 정보를 검토 한 후 ``시작하기`` 버튼을 누른다.

기존 키 페어 선택 또는 새 키 페어 생성에서 ``기존 키 페어 선택``을 누른 후 기존에 생성한 키 페어를 선택하고 체크 박스도 클릭 한 후 ``인스턴스 시작`` 버튼을 눌러 인스턴스를 생성한다.

<img width="1680" alt="Screen Shot 2019-07-02 at 17 33 41" src="https://user-images.githubusercontent.com/46523571/60497351-a1f7d000-9cef-11e9-9889-c5252e151b6d.png">

``시작 상태`` 화면이 뜨면서 인스턴스 생성이 완료 된걸 볼 수 있다. 

이제 ``인스턴스 보기`` 버튼을 눌러 다음으로 넘어가자.

<img width="1680" alt="Screen Shot 2019-07-02 at 17 34 57" src="https://user-images.githubusercontent.com/46523571/60497459-d9667c80-9cef-11e9-96ee-f3a57b5559bd.png">

인스턴스 생성 완료!!

<img width="1680" alt="Screen Shot 2019-07-02 at 17 36 48" src="https://user-images.githubusercontent.com/46523571/60497563-09ae1b00-9cf0-11e9-8081-b1ac12fbfc72.png">

### 인스턴스 접속하기

macOS에서 ``터미널`` 실행

<img width="792" alt="Screen Shot 2019-07-01 at 22 33 07" src="https://user-images.githubusercontent.com/46523571/60440701-53dfbf80-9c50-11e9-8d5f-d26c0af5af6d.png">

키 페어 이동 및 권환 설정을 아래와 같이 해준다. (참고 1-3. 키 페어 생성하기에서 했다면 넘어가도 된다. )

- ``cp ~/Downloads/kangpro.pem ~/.ssh``
- 키 페어 권한 설정
  - ``chmod 400 ~/.ssh/kangpro.pem``
- (참고) 본인이 생성한 키 페어 이름을 작성하시면 됩니다. 저의 경우 kangpro.pem 

<img width="950" alt="Screen Shot 2019-07-02 at 17 42 19" src="https://user-images.githubusercontent.com/46523571/60497964-c1432d00-9cf0-11e9-8ead-008fc4e81039.png">

ssh -i ~/.ssh/본인이생성한키페어이름.pem ubuntu@본인인스턴스퍼블릭 DNS(IPv4) 접속한다. 본인 인스턴스 퍼블릭 DNS(IPv4)는 인스턴스 화면에서 확인 할 수 있다.

- ``ssh -i ~/.ssh/kangpro.pem ubuntu@ec2-54-180-145-60.ap-northeast-2.compute.amazonaws.com``

<img width="950" alt="Screen Shot 2019-07-02 at 17 44 40" src="https://user-images.githubusercontent.com/46523571/60498190-1aab5c00-9cf1-11e9-9a9c-3d8c04da041b.png">

```
Last login: Tue Jul  2 15:51:25 on ttys000
➜  ~ cp ~/Downloads/kangpro.pem ~/.ssh
➜  ~ chmod 400 ~/.ssh/kangpro.pem
➜  ~ ssh -i ~/.ssh/kangpro.pem ubuntu@ec2-54-180-145-60.ap-northeast-2.compute.amazonaws.com 
The authenticity of host 'ec2-54-180-145-60.ap-northeast-2.compute.amazonaws.com (54.180.145.60)' can't be established.
ECDSA key fingerprint is SHA256:SiX6aoeX9sbFEv6wveI5VB5AQHWHiRmTfTkq7RfdOQI.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'ec2-54-180-145-60.ap-northeast-2.compute.amazonaws.com,54.180.145.60' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.2 LTS (GNU/Linux 4.15.0-1039-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-172-31-24-0:~$ 
```

### (참고) 인스턴스 삭제

- 만약 과금문제 및 AWS 인스턴스 사용을 안 할 시에는 종료를 통해 삭제 할 수 있다.
  <img width="100%" alt="Screen Shot 2019-04-07 at 2 34 46 AM" src="https://user-images.githubusercontent.com/46523571/56888245-7d446980-6aae-11e9-9137-b15e18d42e26.png">
  <img width="100%" alt="Screen Shot 2019-04-07 at 2 35 40 AM" src="https://user-images.githubusercontent.com/46523571/56888246-7d446980-6aae-11e9-9aeb-e2fdde4946ad.png">

## AWS ubuntu 서버 환경 설정
---
AWS 서버에 처음 접속한 뒤 다음의 기본 설정들을 세팅한다.

### 기본 설정

기본 설정

```
sudo vi /etc/default/locale

LC_CTYPE="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
LANG="en_US.UTF-8"
```

패키지 정보 업데이트

```
sudo apt-get update
```

패키지 의존성 검사 및 업그레이드

```
sudo apt-get dist-upgrade
```

Python 패키지 매니저 설치

```
sudo apt-get install python-pip
```

zsh 설치

```
sudo apt-get install zsh
```

oh my zsh 설치

```
sudo curl -L http://install.ohmyz.sh | sh
```

기본 쉘을 zsh로 변경한 뒤 재접속 (chsh 다음에 유저명을 입력해주어야한다.)

```
sudo chsh ubuntu -s /usr/bin/zsh
```

### pyenv 설치

```
# pyenv 필수 패키지 설치

sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev
```

```
# pyenv git clone
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

```
# z-shell인 경우:
vi ~/.zshrc

파일의 가장 아랫부분에 아래의 내용 추가:
# ~/.zshrc의 pyenv 환경변수 설정

export PATH="/home/ubuntu/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

```
# pyenv install --list 명령어로 설치 가능한 버전 확인
# 나의 경우 파이썬 3.7.3을 설치
pyenv install 3.7.3
```

```
# Pillow를 위한 Python 라이브러리 설치합니다.
sudo apt-get install python-dev python-setuptools
```

### Django 환경 설정

장고 프로젝트는 `root` 디렉토리의 `srv` 폴더에 업로드한다.<br/>
`srv` 폴더의 소유자를 `ubuntu` 로 변경한다.

```
sudo chown -R ubuntu:ubuntu /srv/
```

위와 같이 `srv` 폴더의 소유자와 그룹이 `ubuntu` 로 설정된 것을 확인한다.

### Django 프로젝트 서버에 업로드하기

간단한 장고 프로젝트를 시작하여 AWS 가상 컴퓨팅 환경에 업로드 해본다.

`config` 폴더의 `settings.py` 의 `ALLOWED_HOSTS` 에 다음과 같이 추가하여 접속을 허용해준다.

```
# settings.py

ALLOWED_HOSTS = [
    'localhost',
    '.ap-northeast-2.compute.amazonaws.com',
]
```

### scp를 사용하여 업로드하기

```
scp -i 키페어경로 -r 보낼폴더경로 유저명@퍼블릭DNS:받을폴더경로
```

아래의 명령어를 이용해서 `EC2_Deploy_Project` 폴더를 AWS 서버의 `srv` 폴더 아래로 복사한다.
(아래의 경우는 `EC2_Deploy_Project` 폴더의 상위 폴더에서 실행할 경우임.)

```
# aws가 아닌 로컬에서 진행해야함
scp -i ~/.ssh/EC2-kangpro.pem -r ~/Documents/Portfolio/Django ubuntu@ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com:/srv/
```

### 서버에서 Python 가상환경 설치하기

AWS 서버에 로컬 서버에서 생성했던 pyenv 가상환경 이름과 동일한 이름으로 가상환경을 생성한다.

```
pyenv virtualenv 3.7.3 venv
```

AWS 서버에서 `/srv/EC2_Deploy_Project` 로 이동하면 자동으로 pyenv 가상환경이 `ec2_deploy` 로 설정되어 있는 것을 확인 할 수 있다.<br/>
다음의 명령어를 입력하여 `requirements.txt` 에 기재되어있는 패키지들을 설치해준다.

```
pip install -r requirements.txt
```

만약 pip 버전이 최신버전이 아니라는 에러가 날 경우 아래 명령어를 입력해준 다음 다시 설치한다.

```
pip install --upgrade pip
```

가상환경으로 접속 한 다음 

```
(venv) ➜  Django git:(master) ls 
mydjango  README.md  requirements.txt  venv
(venv) ➜  Django git:(master) cd mydjango 
(venv) ➜  mydjango git:(master) ls
config  db.sqlite3  manage.py
(venv) ➜  mydjango git:(master) python manage.py runserver 0.0.0.0:8000
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

July 03, 2019 - 08:37:23
Django version 2.2.3, using settings 'config.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
^C%                                                                                         (venv) ➜  mydjango git:(master) python manage.py runserver             
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

July 03, 2019 - 08:37:42
Django version 2.2.3, using settings 'config.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
^C%                                                                                         (venv) ➜  mydjango git:(master) ./manage.py runserver 0:8000
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

July 03, 2019 - 08:38:24
Django version 2.2.3, using settings 'config.settings'
Starting development server at http://0:8000/
Quit the server with CONTROL-C.
[03/Jul/2019 08:38:36] "GET / HTTP/1.1" 200 16348
Not Found: /robots.txt
[03/Jul/2019 08:38:36] "GET /robots.txt HTTP/1.1" 404 2014
[03/Jul/2019 08:38:36] "GET /static/admin/css/fonts.css HTTP/1.1" 200 423
[03/Jul/2019 08:38:36] "GET /static/admin/fonts/Roboto-Regular-webfont.woff HTTP/1.1" 200 85876
[03/Jul/2019 08:38:36] "GET /static/admin/fonts/Roboto-Bold-webfont.woff HTTP/1.1" 200 86184
[03/Jul/2019 08:38:36] "GET /static/admin/fonts/Roboto-Light-webfont.woff HTTP/1.1" 200 85692
Not Found: /favicon.ico
[03/Jul/2019 08:38:36] "GET /favicon.ico HTTP/1.1" 404 2017
```

### AWS 보안그룹 설정

- AWS - 네트워크 및 보안 - 보안 그룹
- 처음 인스턴스 생성 했을 때 생긴 보안 그룹을 편집하면 됩니다.
- 인바운드 규칙 편집

<img width="1680" alt="Screen Shot 2019-07-02 at 19 00 31" src="https://user-images.githubusercontent.com/46523571/60504035-cb1e5d80-9cfb-11e9-8873-98408a4e52dc.png">

aws에서 실행한거 드디어 된다.

<img width="1680" alt="Screen Shot 2019-07-03 at 17 38 47" src="https://user-images.githubusercontent.com/46523571/60577009-80631b00-9db9-11e9-8cf9-9299a74dded6.png">












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