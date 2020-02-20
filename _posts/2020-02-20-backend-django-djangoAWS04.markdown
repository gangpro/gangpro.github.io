---
layout: post
title: '[DjangoAWS] 장고와 웹 서버 연결을 위한 WSGI'
subtitle: 
categories: backend
tags: django djangoAWS AWS
comments: true
date: 2020-02-20 10:00:17 +0900
lastmod: 2020-02-20 10:10:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



Django AWS 배포를 위한 장고와 웹 서버 연결해주는 역할을 하는 Python 프레임워크 WSGI 사용<br/>

## WSGI
---
WSGI는 장고와 웹서버를 연결해주는 역할을 하는 Python 프레임워크이다. 프로토콜 개념으로도 이해할 수 있다.
웹서버가 직접적으로 Python으로 된 장고와 통신할 수 없기 때문에 그 사이에서 `WSGI Server(middleware)` 가 실행되어 웹서버와 장고를 연결해준다. 웹서버가 전달받은 사용자의 요청을 WSGI Server에서 처리하여 Django로 넘겨주고, 다시 Django가 넘겨준 응답을 WSGI Server가 받아서 웹서버에 전달한다.

```
사용자 <-> 웹서버 <-> WSGI Server <-> Django
```

WSGI Server에는 여러 가지 종류가 있는데, 그 중 기능이 강력하고 확장성이 뛰어난 `uWSGI` 를 사용할 것이다.

#### uWSGI 설치

먼저 `ssh` 를 통해 AWS 서버에 접속한 다음, 배포에 사용할 새로운 유저를 `deploy` 라는 이름으로 생성해준다.<br/>
보안을 위해 각 기능 별 유저를 설정해주는 것이 좋다.<br/>

```
sudo adduser deploy

(venv) ➜  ~ sudo adduser deploy
Adding user `deploy' ...
Adding new group `deploy' (1001) ...
Adding new user `deploy' (1001) with group `deploy' ...
Creating home directory `/home/deploy' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: kangpro@404
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for deploy
Enter the new value, or press ENTER for the default
	Full Name []: kangpro
	Room Number []: 404
	Work Phone []: 404
	Home Phone []: 404
	Other []: 404
Is the information correct? [Y/n] Y
(venv) ➜  ~ 
```

그런 다음, uWSGI를 설치할 별도의 Python 가상환경을 생성해준다.

```
pyenv virtualenv 3.6.2 uwsgi-env

➜  ~ pyenv virtualenv 3.7.3 uwsgi-env
Looking in links: /tmp/tmpxi0t8sqe
Requirement already satisfied: setuptools in /home/ubuntu/.pyenv/versions/3.7.3/envs/uwsgi-env/lib/python3.7/site-packages (40.8.0)
Requirement already satisfied: pip in /home/ubuntu/.pyenv/versions/3.7.3/envs/uwsgi-env/lib/python3.7/site-packages (19.0.3)
➜  ~ pyenv versions
* system (set by PYENV_VERSION environment variable)
  3.7.3
  3.7.3/envs/uwsgi-env
  3.7.3/envs/venv
  uwsgi-env
  venv
➜  ~ 
```

이 가상환경을 지금 현재의 가상 컴퓨터 셸에만 일시적으로 적용하도록 설정해준다.<br/>
서버 전체에서 하나의 uwsgi를 사용하게 하기 위함이다.<br/>

```
pyenv shell uwsgi-env

(uwsgi-env) ➜  ~  
```

이제 가상환경에 `uwsgi` 를 설치한다.

```
pip install uwsgi
```

#### uWSGI로 서버 열어보기

uWSGI를 실행하려면 `pyenv shell uwsgi-env` 를 입력해 uwsgi-env를 적용한 다음, 아래와 같이 입력한다.

```
uwsgi \
--http :[포트번호] \
--home [virtualenv 경로] \
--chdir [장고프로젝트폴더 경로] \
-w [wsgi 모듈명].wsgi
```

현재 진행중인 프로젝트의 경우를 대입해보면 다음과 같다.

- **포트번호**: 8000
- **virtualenv 경로**: /home/ubuntu/.pyenv/versions/venv
- **장고프로젝트폴더 경로**: /srv/Django/mydjango (manage.py가 있는 경로를 지정해주면 된다.)
- **wsgi 모듈명**: config.wsgi

```
/home/ubuntu/.pyenv/versions/uwsgi-env/bin/uwsgi \
--http :8000 \
--home /home/ubuntu/.pyenv/versions/venv \
--chdir /srv/Django/mydjango \
-w config.wsgi
```

위 명령을 실행하면 아래와 같이 뜨고, uWSGI가 요청을 받을 수 있는 상태가 된다.

```
(uwsgi-env) ➜  ~ /home/ubuntu/.pyenv/versions/uwsgi-env/bin/uwsgi \
--http :8000 \
--home /home/ubuntu/.pyenv/versions/venv \
--chdir /srv/Django/mydjango \
-w config.wsgi
*** Starting uWSGI 2.0.18 (64bit) on [Wed Jul  3 09:18:13 2019] ***
compiled with version: 5.4.0 20160609 on 03 July 2019 09:11:20
os: Linux-4.4.0-1083-aws #93-Ubuntu SMP Wed May 8 16:08:41 UTC 2019
nodename: ip-172-31-18-177
machine: x86_64
clock source: unix
detected number of CPU cores: 1
current working directory: /home/ubuntu
detected binary path: /home/ubuntu/.pyenv/versions/3.7.3/envs/uwsgi-env/bin/uwsgi
!!! no internal routing support, rebuild with pcre support !!!
chdir() to /srv/Django/mydjango
*** WARNING: you are running uWSGI without its master process manager ***
your processes number limit is 3900
your memory page size is 4096 bytes
detected max file descriptor number: 1024
lock engine: pthread robust mutexes
thunder lock: disabled (you can enable it with --thunder-lock)
uWSGI http bound on :8000 fd 4
spawned uWSGI http 1 (pid: 18237)
uwsgi socket 0 bound to TCP address 127.0.0.1:38210 (port auto-assigned) fd 3
Python version: 3.7.3 (default, Jul  3 2019, 08:11:05)  [GCC 5.4.0 20160609]
PEP 405 virtualenv detected: /home/ubuntu/.pyenv/versions/venv
Set PythonHome to /home/ubuntu/.pyenv/versions/venv
*** Python threads support is disabled. You can enable it with --enable-threads ***
Python main interpreter initialized at 0x13d2d00
your server socket listen backlog is limited to 100 connections
your mercy for graceful operations on workers is 60 seconds
mapped 72920 bytes (71 KB) for 1 cores
*** Operational MODE: single process ***
WSGI app 0 (mountpoint='') ready in 0 seconds on interpreter 0x13d2d00 pid: 18236 (default app)
*** uWSGI is running in multiple interpreter mode ***
spawned uWSGI worker 1 (and the only) (pid: 18236, cores: 1)
```

이제 브라우저에서 퍼블릭 DNS의 8000번 포트에 접속해보자.

```
http://13.209.42.132:8000/
```

<img width="1680" alt="Screen Shot 2019-07-03 at 18 22 00" src="https://user-images.githubusercontent.com/46523571/60580122-7b08cf00-9dbf-11e9-97de-f21564c4768d.png">

runserver를 실행하지 않았는데도 접속이 가능한 것을 볼 수 있다. WSGI를 통해 사용자의 요청을 받을 수 있게 되었기 때문이다.<br/>
터미널창을 확인해보면 GET 요청이 들어와있는것을 확인할 수 있다.<br/>

```
*** uWSGI is running in multiple interpreter mode ***
spawned uWSGI worker 1 (and the only) (pid: 18236, cores: 1)
[pid: 18236|app: 0|req: 1/1] 210.104.224.247 () {38 vars in 753 bytes} [Wed Jul  3 09:18:19 2019] GET / => generated 16348 bytes in 5 msecs (HTTP/1.1 200) 3 headers in 96 bytes (1 switches on core 0)
Not Found: /static/admin/css/fonts.css
[pid: 18236|app: 0|req: 2/2] 210.104.224.247 () {38 vars in 777 bytes} [Wed Jul  3 09:18:19 2019] GET /static/admin/css/fonts.css => generated 2062 bytes in 4 msecs (HTTP/1.1 404) 3 headers in 102 bytes (1 switches on core 0)
```

#### ini 파일로 uWSGI 실행하기

매번 uWSGI를 실행할 때마다 위의 복잡한 명령을 입력하기 번거로우므로, 미리 옵션을 파일로 만들어 저장해놓고 실행할 수 있다.

로컬에서 장고 프로젝트 폴더에 `.config` 라는 폴더를 하나 새로 생성하고 그 안에 다시 `uwsgi` 폴더를 생성한다.
`uwsgi` 폴더 안에 `mydjango.ini` 파일을 만들어 준다.

```
Django
├── .config
│   └── uwsgi
│       └── mydjango.ini
├── .git
├── .gitignore
├── mydjango
└── requirements.txt
```

mysite.ini를 열고 다음과 같이 입력한다.

```
[uwsgi]
chdir = /srv/Django/mydjango
module = config.wsgi:application
home = /home/ubuntu/.pyenv/versions/venv

uid = deploy
gid = deploy

http = :8000

enable-threads = true
master = true
vacuum = true
pidfile = /tmp/mydjango.pid
logto = /var/log/uwsgi/mydjango/@(exec://date +%%Y-%%m-%%d).log
log-reopen = true
```

- **chdir**: 장고 프로젝트 폴더의 경로(manage.py가 있는 폴더)
- **module**: 장고 프로젝트 내의 `wsgi.py` 파일의 경로
- **home**: 장고 프로젝트가 실행되는 Python 가상환경 경로
- **uid, gid**: uWSGI를 실행할 사용자 및 사용자그룹 지정
- **http**: http를 통해서 요청을 받을 수 있도록 하며, 요청을 받을 포트 번호를 설정한다.
- **enable-threads**: 스레드 사용 여부
- **master**: 마스터 프로세스 사용 여부
- **vaccum**: 실행시 자동 생성되는 파일 또는 소켓들을 종료될 때 삭제하는 옵션
- **pidfile**: `pidfile` 이 생성될 폴더의 경로를 설정한다. pidfile은 Linux에서 실행되는 프로세스의 `id` 값을 담고있는 파일이다.
- **logto**: 로그 파일을 작성할 위치 설정. 보통 로그는 `/var/log/` 폴더 아래에 생성한다.
- **log-reopen**: uWSGI를 재시작할때 로그를 다시 열어주는 옵션

이제 프로젝트 폴더를 scp를 통해 서버에 전송한 다음 ssh를 통해 서버에 접속한다.

```
scp -i ~/.ssh/EC2-kangpro.pem -r ~/Documents/Portfolio/Django ubuntu@ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com:/srv/
```

```
ssh -i ~/.ssh/EC2-kangpro.pem ubuntu@ec2-13-209-42-132.ap-northeast-2.compute.amazonaws.com
```

uWSGI를 실행하기 전에 `mydjango.ini` 파일에 설정해주었던 `logto` 옵션의 디렉토리를 직접 생성해주어야 한다.

```
sudo mkdir -p /var/log/uwsgi/mydjango
```

그 다음 아래의 명령을 실행해 `ini` 파일로 uWSGI를 실행한다. `sudo` 권한으로 실행해야 하기 때문에, uwsgi-env 가상환경 폴더 안에 있는 uwsgi를 직접 실행해주어야 한다.

```
sudo /home/ubuntu/.pyenv/versions/uwsgi-env/bin/uwsgi -i /srv/Django/.config/uwsgi/mydjango.ini 
```

이제 브라우저에서 퍼블릭 도메인의 8000번 포트로 접속해보면 접속이 되는 것을 볼 수 있다.<br/>

<img width="1680" alt="Screen Shot 2019-07-03 at 18 38 10" src="https://user-images.githubusercontent.com/46523571/60581403-ba381f80-9dc1-11e9-9b47-a3f9ba695986.png"><br/>

uWSGI 실행 로그를 확인하려면 아까 생성해주었던 로그 폴더 안의 `.log` 파일을 열어보면 된다.<br/>

```
sudo cat /var/log/uwsgi/mydjango/로그파일이름.log

➜  ~ sudo cat /var/log/uwsgi/mydjango/2019-07-03.log 
*** Starting uWSGI 2.0.18 (64bit) on [Wed Jul  3 09:37:14 2019] ***
compiled with version: 5.4.0 20160609 on 03 July 2019 09:11:20
os: Linux-4.4.0-1083-aws #93-Ubuntu SMP Wed May 8 16:08:41 UTC 2019
nodename: ip-172-31-18-177
machine: x86_64
clock source: unix
detected number of CPU cores: 1
current working directory: /home/ubuntu
writing pidfile to /tmp/mydjango.pid
detected binary path: /home/ubuntu/.pyenv/versions/3.7.3/envs/uwsgi-env/bin/uwsgi
!!! no internal routing support, rebuild with pcre support !!!
setgid() to 1001
setuid() to 1001
chdir() to /srv/Django/mydjango
your processes number limit is 3900
your memory page size is 4096 bytes
detected max file descriptor number: 1024
lock engine: pthread robust mutexes
thunder lock: disabled (you can enable it with --thunder-lock)
uWSGI http bound on :8000 fd 4
uwsgi socket 0 bound to TCP address 127.0.0.1:42378 (port auto-assigned) fd 3
Python version: 3.7.3 (default, Jul  3 2019, 08:11:05)  [GCC 5.4.0 20160609]
PEP 405 virtualenv detected: /home/ubuntu/.pyenv/versions/venv
Set PythonHome to /home/ubuntu/.pyenv/versions/venv
Python main interpreter initialized at 0x2199740
python threads support enabled
your server socket listen backlog is limited to 100 connections
your mercy for graceful operations on workers is 60 seconds
mapped 145840 bytes (142 KB) for 1 cores
*** Operational MODE: single process ***
WSGI app 0 (mountpoint='') ready in 1 seconds on interpreter 0x2199740 pid: 18870 (default app)
spawned uWSGI master process (pid: 18870)
spawned uWSGI worker 1 (pid: 18873, cores: 1)
spawned uWSGI http 1 (pid: 18874)
subprocess 18871 exited with code 0
[pid: 18873|app: 0|req: 1/1] 210.104.224.247 () {36 vars in 722 bytes} [Wed Jul  3 09:37:41 2019] GET / => generated 16348 bytes in 9 msecs (HTTP/1.1 200) 3 headers in 96 bytes (1 switches on core 0)
Not Found: /static/admin/css/fonts.css
[pid: 18873|app: 0|req: 2/2] 210.104.224.247 () {36 vars in 722 bytes} [Wed Jul  3 09:37:42 2019] GET /static/admin/css/fonts.css => generated 2062 bytes in 6 msecs (HTTP/1.1 404) 3 headers in 102 bytes (1 switches on core 0)
SIGINT/SIGQUIT received...killing workers...
gateway "uWSGI http 1" has been buried (pid: 18874)
worker 1 buried after 1 seconds
goodbye to uWSGI.
```

이전과 같이 GET 요청을 받았다는 로그를 확인할 수 있다.







## 참고
AWS 유저 가이드 : [https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html) <br/>
나채원님 블로그 : [https://nachwon.github.io/django-deploy-1-aws/](https://nachwon.github.io/django-deploy-1-aws/) <br/>
블로그 : [https://paphopu.tistory.com/entry/WSGI에-대한-설명-WSGI란-무엇인가](https://paphopu.tistory.com/entry/WSGI에-대한-설명-WSGI란-무엇인가) <br/>
<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>

## 환경
---
> macOS Mojave 10.14.5, 
> python 3.6.8, 
> django 2.2.1, 
> PyCharm CE 2018.3.7.