---
layout: post
title: '[DjangoAWS] 장고와 AWS 데이터베이스 RDS 사용'
subtitle: 
categories: backend
tags: django djangoAWS AWS
comments: true
date: 2020-02-20 10:20:17 +0900
lastmod: 2020-02-20 10:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---



이번 포스트에서는 프로젝트가 사용할 데이터베이스를 별개의 서버에 두고 운영하는 방법을 알아본다.
각자의 기능이 최적으로 운영되도록 하기 위해서는 기능별로 별도의 서버를 두고 관리하는 것이 효율적이다.
아마존에서 제공하는 `RDS` 는 데이터베이스 운영에 최적화된 환경에서 효율적으로 데이터베이스를 관리하기 위해 별도로 운영하는 서버이다.<br/>

## Relational Database Server(RDS) 시작하기
---
아마존 웹 서버 콘솔 페이지로 접속한 다음, 메인화면의 검색창에 `RDS` 를 검색한다.
- AWS : https://aws.amazon.com/ko/

<img width="1680" alt="Screen Shot 2019-07-04 at 17 51 54" src="https://user-images.githubusercontent.com/46523571/60653186-81ad4a00-9e84-11e9-9e7d-edc68fa5c0c7.png"><br/>

### 데이터베이스 생성

왼쪽 메뉴에서 `Dashboard` 를 클릭한 다음 `데이터베이스 생성` 버튼을 눌러 데이터베이스를 생성한다.<br/>

<img width="1680" alt="Screen Shot 2019-07-04 at 17 54 47" src="https://user-images.githubusercontent.com/46523571/60653359-d9e44c00-9e84-11e9-86e9-2a7d5719eaaa.png"><br/>

데이터베이스 엔진을 선택하는 화면에서 원하는 엔진을 선택한다. 이번 포스트에서는 `MySQL` 을 사용한다.<br/>

<img width="1680" alt="Screen Shot 2019-07-04 at 17 56 19" src="https://user-images.githubusercontent.com/46523571/60653467-1021cb80-9e85-11e9-81e6-74be0848aa68.png"><br/>

엔진 옵션
- 엔진유형 : ``MySQL`` 
- 에디션 : MySQL Community
- 버전 :  ``MySQL 5.7.22``을 선택한다. (본인이 원하는 걸로 하면 됩니다.)
- 템플릿 : ``프리 티어``

과금이 조금 될 듯 하다.. <br/>

<img width="1680" alt="Screen Shot 2019-07-04 at 18 56 14" src="https://user-images.githubusercontent.com/46523571/60658085-73176080-9e8d-11e9-8d81-eb7bc8d9280a.png"><br/>

설정
- ``DB 인스턴스 식별자`` : DjangoDB
- ``Master username``: kangpro
- ``마스터 암호`` : ****
- ``암호 확인`` : ****

<img width="1680" alt="Screen Shot 2019-07-04 at 18 57 22" src="https://user-images.githubusercontent.com/46523571/60658150-95a97980-9e8d-11e9-927e-6fb1aaa24404.png"><br/>

``연결``- `추가 연결 구성` 
- 퍼블렉 액세스 기능 : ``예`` 
- `VPC 보안 그룹` : `새로 생성` 
- 새 VPC 보안 그룹 이름 : ``RDS-kangpro``
<br/>

<img width="1680" alt="Screen Shot 2019-07-04 at 19 00 06" src="https://user-images.githubusercontent.com/46523571/60658384-ff298800-9e8d-11e9-92a0-1204328460b0.png"><br/>

추가 구성
- 데이터베이스 옵션
  - 초기 데이터베이스 이름 : ``DjangoDB``
<br/>
<img width="1680" alt="Screen Shot 2019-07-04 at 19 02 09" src="https://user-images.githubusercontent.com/46523571/60658509-4879d780-9e8e-11e9-8c73-01c621eaa209.png"><br/>

그 외 설정은 다 기본 값으로 한다.<br/>

이제 ``데이터베이스 생성``을 눌러 데이터베이스를 생성한다.<br/>

<img width="1680" alt="Screen Shot 2019-07-04 at 19 03 48" src="https://user-images.githubusercontent.com/46523571/60658621-8840bf00-9e8e-11e9-8da8-e5965027cf3b.png"><br/>

인스턴스 생성을 완료한 다음 `인스턴스` 화면으로 이동해서 어느 정도 기다리고 나면 아래와 같이 `사용 가능` 상태가 되어 데이터베이스 서버를 사용할 수 있게 된다.<br/>

<img width="1680" alt="Screen Shot 2019-07-04 at 18 12 23" src="https://user-images.githubusercontent.com/46523571/60654625-495b3b00-9e87-11e9-96ad-a68273c85f86.png"><br/>

이제 `데이터베이스` 메뉴에서 `보안 그룹 규칙`과 `3306` 번 포트와 ``앤드포인트``가 등록되어 있는 것을 볼 수 있다.<br/>

<img width="1680" alt="Screen Shot 2019-07-04 at 18 13 24" src="https://user-images.githubusercontent.com/46523571/60654730-832c4180-9e87-11e9-9393-3ac42743a05f.png"><br/>

## Django에 연결하기
---
이제 생성한 DB 인스턴스를 장고 프로젝트에 연결시켜줄 차례이다.

#### settings.py 설정

`settings.py` 를 열어 `DATABASES` 변수를 아래와 같이 수정해주자.<br/>

```
# settings.py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'DjangoDB',
        'USER': 'kangpro',
        'PASSWORD': '****',
        'HOST': 'djangodb.cncutrxvq54m.ap-northeast-2.rds.amazonaws.com',
        'PORT': '3306',
    }
}

```

각각의 항목에 들어가야할 값은 `RDS` 인스턴스 화면에서 인스턴스를 체크한뒤 `세부 정보` 아이콘을 클릭하여 나타나는 창에서 확인할 수 있다.
- `ENGINE`: sqlite3를 ``mysql``로 바꿔준다.
- `HOST`: `엔드포인트` 항목을 복사-붙여넣기 한다.
- `PORT`: `포트` 항목을 입력한다.
- `NAME`: `DB 이름` 항목을 입력한다.
- `USER`: `사용자 이름` 항목을 입력한다.
- `PASSWORD`: DB 인스턴스를 생성할 때 설정했던 비밀번호를 입력한다.

``mysqlclient`는데이터베이스와 장고를 연결해주는 역할을 한다.<br/>

```
pip install mysqlclient
```

이제 터미널에서 `manage.py` 가 있는 경로로 이동한 다음 `showmigrations` 명령을 실행한다.<br/>

<img width="484" alt="Screen Shot 2019-07-04 at 19 36 22" src="https://user-images.githubusercontent.com/46523571/60660609-128b2200-9e93-11e9-883d-74bb84fb23f4.png"><br/>

마이그레이션 목록이 나타나면 데이터베이스에 연결이 잘 된 것이다.<br/>

#### mysql 접속 해보기

터미널에서 아래와 같이 입력하여 생성한 데이터베이스에 접속해보자.<br/>

``mysql -h <endpoint> -P 3306 -u <username> -P``

```
mysql -h djangodb.cncutrxvq54m.ap-northeast-2.rds.amazonaws.com -P 3306 -u kangpro -p
```

비밀번호를 입력하고 나면 아래와 같이 mysql에 접속하게 된다.<br/>

<img width="954" alt="Screen Shot 2019-07-04 at 19 52 50" src="https://user-images.githubusercontent.com/46523571/60661652-63037f00-9e95-11e9-99fa-94c3297aa4f1.png"><br/>

이제 데이터베이스를 별도의 서버에서 운영할 수 있게 되었다.<br/>

#### RDS 보안그룹 인바운드 설정

인바운드에 EC2 가상컴퓨터의 보안 그룹을 추가시켜주면 된다.<br/>
보안 그룹 편집에서 `소스` 에 EC2 보안 그룹 이름을 검색한 다음 추가한다.<br/>

<img width="1679" alt="Screen Shot 2019-07-04 at 19 48 02" src="https://user-images.githubusercontent.com/46523571/60661321-a6a9b900-9e94-11e9-8169-dc0189114196.png"><br/>

이렇게 하면 AWS 가상 컴퓨터에서 데이터베이스에 접근을 할 수 있게 되어서 띄워놓은 웹서비스에서 데이터베이스가 필요한 작업을 수행할 수 있게 된다.<br/>


## 하루 걸려 찾은 에러ㅠㅠ 

갑자기 AWS 에러 서버 오류가 발생함... 잘 되던게 왜!!... <br/>

결과적으로 로컬에서 pip install mysqlclient 설치가 잘 됐었고 그걸 서버에 올리고 체크를 안함.<br/>

서버 에러뜨는게 당연했고 문제를 찾고 aws에서 pip install mysqlclient를 설치하려 했으나 안됐음.<br/>

그래서 이것저것 설치하다가 찾아서 겨우 해결함 ㅠㅠ<br/>

```
sudo apt-get install libmysqlclient-dev
pip install mysqlclient
```

```
(venv) ➜  mydjango git:(master) sudo apt-get install libmysqlclient-dev
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libmysqlclient20
The following NEW packages will be installed:
  libmysqlclient-dev libmysqlclient20
0 upgraded, 2 newly installed, 0 to remove and 6 not upgraded.
Need to get 1,968 kB of archives.
After this operation, 11.4 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu xenial-updates/main amd64 libmysqlclient20 amd64 5.7.26-0ubuntu0.16.04.1 [812 kB]
Get:2 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu xenial-updates/main amd64 libmysqlclient-dev amd64 5.7.26-0ubuntu0.16.04.1 [1,156 kB]
Fetched 1,968 kB in 0s (31.0 MB/s)
Selecting previously unselected package libmysqlclient20:amd64.
(Reading database ... 87851 files and directories currently installed.)
Preparing to unpack .../libmysqlclient20_5.7.26-0ubuntu0.16.04.1_amd64.deb ...
Unpacking libmysqlclient20:amd64 (5.7.26-0ubuntu0.16.04.1) ...
Selecting previously unselected package libmysqlclient-dev.
Preparing to unpack .../libmysqlclient-dev_5.7.26-0ubuntu0.16.04.1_amd64.deb ...
Unpacking libmysqlclient-dev (5.7.26-0ubuntu0.16.04.1) ...
Processing triggers for libc-bin (2.23-0ubuntu11) ...
Processing triggers for man-db (2.7.5-1) ...
Setting up libmysqlclient20:amd64 (5.7.26-0ubuntu0.16.04.1) ...
Setting up libmysqlclient-dev (5.7.26-0ubuntu0.16.04.1) ...
Processing triggers for libc-bin (2.23-0ubuntu11) ...
(venv) ➜  mydjango git:(master) pip install mysqlclient
Collecting mysqlclient
  Using cached https://files.pythonhosted.org/packages/f4/f1/3bb6f64ca7a429729413e6556b7ba5976df06019a5245a43d36032f1061e/mysqlclient-1.4.2.post1.tar.gz
Installing collected packages: mysqlclient
  Running setup.py install for mysqlclient ... done
Successfully installed mysqlclient-1.4.2.post1
(venv) ➜  mydjango git:(master)
```

```
python manage.py showmigrations
```

```
(venv) ➜  mydjango git:(master) python manage.py showmigrations
admin
 [ ] 0001_initial
 [ ] 0002_logentry_remove_auto_add
 [ ] 0003_logentry_add_action_flag_choices
auth
 [ ] 0001_initial
 [ ] 0002_alter_permission_name_max_length
 [ ] 0003_alter_user_email_max_length
 [ ] 0004_alter_user_username_opts
 [ ] 0005_alter_user_last_login_null
 [ ] 0006_require_contenttypes_0002
 [ ] 0007_alter_validators_add_error_messages
 [ ] 0008_alter_user_username_max_length
 [ ] 0009_alter_user_last_name_max_length
 [ ] 0010_alter_group_name_max_length
 [ ] 0011_update_proxy_permissions
contenttypes
 [ ] 0001_initial
 [ ] 0002_remove_content_type_name
sessions
 [ ] 0001_initial
```

```
mydjango git:(master) mysql -h djangodb.cncutrxvq54m.ap-northeast-2.rds.amazonaws.com -P 3306 -u kangpro -p
```

```
(venv) ➜  mydjango git:(master) mysql -h djangodb.cncutrxvq54m.ap-northeast-2.rds.amazonaws.com -P 3306 -u kangpro -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 56
Server version: 5.7.22-log Source distribution

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```






## 참고
AWS RDS : [https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create-deploy-python-rds.html](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create-deploy-python-rds.html)<br/>
AWS RDS MySQL : [https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_MySQL.html](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_MySQL.html)<br/>
AWS RDS MySQL : [https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html)<br/>
AWS pip install mysqlclient : [https://stackoverflow.com/questions/46902357/error-loading-mysqldb-module-did-you-install-mysqlclient-or-mysql-python](https://stackoverflow.com/questions/46902357/error-loading-mysqldb-module-did-you-install-mysqlclient-or-mysql-python)<br/>
나채원님 블로그 : [https://nachwon.github.io/django-deploy-1-aws/](https://nachwon.github.io/django-deploy-1-aws/)<br/>
<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>

## 환경
---
> macOS Mojave 10.14.5, 
> python 3.7.3, 
> django 2.2.3, 
> PyCharm CE 2018.3.7,
> AWS ubuntu 16.04,
> AWS RDS MySQL 5.7.22.