---
layout: post
title: '[AWS] 05. SSH 공개키 생성, 복사, 접속 확인'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 05:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

SSH 공개키 생성, 복사, 접속 확인<br />

# SSH 공개키 생성, 복사, 접속 확인
> Install Hadoop on AWS <br>

## SSH 공개키 생성
* 하둡 클러스터의 각종 daemon 프로세스들을 SSH 프로토콜을 이용하므로 name node에서 secondary name node와 data node에 SSH를 통해 접속할 수 있어야 한다.

* SSH 공개키 생성
  -  하둡은 SSH 프로토콜을 이용해 하둡 클러스터 간의 내부 통신을 수행한다. 따라서 name node에서 SSH 공개키를 생성하고, 이 공개키를 전체 서버에 복사해야 한다.
  - ※ ~/.ssh/id_rsa.pub 파일은? 
  - : Contains the protocol version 2 DSA, ECDSA or RSA public key for authentication. The contents of this file should be added to ~/.ssh/authorized_keys on all machines where the user wishes to log in using public key authentication. There is no need to keep the contents of this file secret.
```
    Last login: Tue Apr 23 14:18:02 on ttys001
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-15-164-47-50.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 05:18:10 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory

    [ec2-user@namenode ~]$ cd

    [ec2-user@namenode ~]$ ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/ec2-user/.ssh/id_rsa):      <-엔터
    Enter passphrase (empty for no passphrase):              <-엔터
    Enter same passphrase again:                  <-엔터
    Your identification has been saved in /home/ec2-user/.ssh/id_rsa.
    Your public key has been saved in /home/ec2-user/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:IKc/ovTMXvOXPxNporMypHMpsD0DqO5bnEjENd/QWBQ ec2-user@namenode.localdomain
    The key's randomart image is:
    +---[RSA 2048]----+
    |   o .=E.        |
    |. . o.o.         |
    | o  ..o.         |
    |.    + .         |
    | o  .   S    .   |
    |o = ...   . +    |
    |...O.o=. . + .   |
    |..o=Bo=+o o o    |
    |o+o.=* oo+ ..o   |
    +----[SHA256]-----+

    [ec2-user@namenode ~]$ 
```

* 아래의 cat 명령어 사용시 반드시 꺽쇠기호(>)를 두 개 사용하여야 한다. 그렇지 않으면 EC2 인스턴스에 접속 불가 상태가 벌어진다. 
  - (>>) 의미는 append라는 의미. (>)가 하나이면 기존의 파일 내용이 없어진다.
```
    [ec2-user@namenode ~]$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    [ec2-user@namenode ~]$ 
```

* 아래의 ssh 명령어로 localhost 및 자신의 hostname으로 자동 접속되는지 확인한다.
  - 처음 접속시에는 "Are you sure you want to continue connecting (yes/no)?"라고 물으면 이때 yes를 입력한다. 두번째 접속부터는 이와 같은 질문 없이 자동으로 로그인이 되면 정상인 상태이다.
```
    [ec2-user@namenode ~]$ ssh localhost
    The authenticity of host 'localhost (127.0.0.1)' can't be established.
    ECDSA key fingerprint is SHA256:ROmGYauune7X2iAN5rV1l+pyxs9ZKX03ar7+X8AAlaY.
    ECDSA key fingerprint is MD5:4a:93:a2:59:10:94:b9:46:2a:e7:d4:ae:64:9e:4f:82.
    Are you sure you want to continue connecting (yes/no)? yes          <-yes
    Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
    Last login: Tue Apr 23 05:18:20 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    
    [ec2-user@namenode ~]$ 
```

```
    # 모든 node에 아래와 같이 host 설정하기
    [ec2-user@namenode ~]$ sudo vi /etc/hosts
    
    # vi 편집기 실행 아래 내용 추가
    15.164.47.50   namenode
    13.125.187.86  snamenode
    15.164.13.90   data01
    13.124.188.140 data02
    52.78.13.217   data03
    172.31.20.100  namenode-internal
    172.31.30.31   snamenode-internal
    172.31.30.98   data01-internal
    172.31.28.203  data02-internal
    172.31.17.175  data03-internal
    # vi 편집기 종료
    
    [ec2-user@namenode ~]$ 
```
    
<img width="836" alt="Screen Shot 2019-04-23 at 2 28 07 PM" src="https://user-images.githubusercontent.com/46523571/56556890-2503f800-65d4-11e9-9808-572d475f3003.png">

```
    [ec2-user@namenode ~]$ ssh namenode
    The authenticity of host 'namenode (15.164.47.50)' can't be established.
    ECDSA key fingerprint is SHA256:ROmGYauune7X2iAN5rV1l+pyxs9ZKX03ar7+X8AAlaY.
    ECDSA key fingerprint is MD5:4a:93:a2:59:10:94:b9:46:2a:e7:d4:ae:64:9e:4f:82.
    Are you sure you want to continue connecting (yes/no)? yes      <-yes
    Warning: Permanently added 'namenode,15.164.47.50' (ECDSA) to the list of known hosts.
    Last login: Tue Apr 23 05:21:36 2019 from namenode.localdomain
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    [ec2-user@namenode ~]$ 
```

<br>
<br>
<br>

## SSH 공개키 복사 - namenode
* 리눅스 클라이언트에서 scp 명령어를 사용해서 id_rsa.pub 파일을 namenode에서 다른 4대의 서버 즉, snamenode, data01, data02, data03 서버로 copy 할 수 있다.
* PC에 보관한 hdcluster.pem 파일을 ftp나 scp 명령어 또는 프로그램으로 리눅스 클라이언트로 복사한다.
* 리눅스 클라이언트에서 다음의 scp 명령어를 실행한다. 명령어 라인 끝에 콜론(:)을 빠트리면 안된다.

* GitBash를 이용하여 hdcluster.pem 파일을 namenode(15.164.47.50)에 보내기
```
    student@M14049 MINGW64 ~
    $ scp -i /c/Users/student/Desktop/hdcluster.pem /c/Users/student/Desktop/hdcluster.pem ec2-user@15.164.47.50:
    hdcluster.pem                                 100% 1692   349.7KB/s   00:00
```

## SSH 공개키 복사 - snamenode, data01, data02, data03
* namenode에서 다음의 명령어를 사용하여 id_rsa.pub 파일을 다른 4대의 서버로 copy한다.
```
    [ec2-user@namenode ~]$ scp -i hdcluster.pem .ssh/id_rsa.pub ec2-user@snamenode:.ssh
    id_rsa.pub                                                100%  411   637.4KB/s   00:00    

    [ec2-user@namenode ~]$ scp -i hdcluster.pem .ssh/id_rsa.pub ec2-user@data01:.ssh
    id_rsa.pub                                                100%  411   605.2KB/s   00:00    

    [ec2-user@namenode ~]$ scp -i hdcluster.pem .ssh/id_rsa.pub ec2-user@data02:.ssh
    id_rsa.pub                                                100%  411   630.5KB/s   00:00    

    [ec2-user@namenode ~]$ scp -i hdcluster.pem .ssh/id_rsa.pub ec2-user@data03:.ssh
    id_rsa.pub                                                100%  411   672.7KB/s   00:00    

    [ec2-user@namenode ~]$ 
```

## PuTTY 설정
```
    login as: ec2-user
    Authenticating with public key "imported-openssh-key"
    Last login: Tue Apr 23 06:38:04 2019 from namenode
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    
    # hdcluster.pem 권한 변경
    [ec2-user@namenode ~]$ chmod 400 hdcluster.pem
    
    # ssh 명령어를 사용하여 namenode에 로그인
    [ec2-user@namenode ~]$ ssh -i hdcluster.pem ec2-user@namenode
    Last login: Tue Apr 23 06:40:07 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    [ec2-user@namenode ~]$
    
    # snamenode 접속
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 06:50:54 2019 from namenode
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
       
    # 아래 명령어 실행
    [ec2-user@snamenode ~]$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    [ec2-user@snamenode ~]$ exit
    logout
    Connection to ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com closed.
    
    # data01 접속
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 06:18:17 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
  
    # 아래 명령어 실행
    [ec2-user@data01 ~]$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    [ec2-user@data01 ~]$ exit
    logout
    Connection to ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com closed.
    
    # data02 접속
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 06:18:44 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
      
    # 아래 명령어 실행
    [ec2-user@data02 ~]$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    [ec2-user@data02 ~]$ exit
    logout
    Connection to ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com closed.
    
    # data03 접속  
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-52-78-13-217.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 06:19:25 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
 
    # 아래 명령어 실행
    [ec2-user@data03 ~]$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    [ec2-user@data03 ~]$ 
```

## 다음과 같이 namenode에서 snamenode, data01, data02, data03으로 ssh 접속 확인하기
```
    # namenode에서 ssh snamenode 접속 확인
    
    [ec2-user@namenode ~]$ ssh snamenode
    Last login: Tue Apr 23 06:53:55 2019 from namenode
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    
    [ec2-user@snamenode ~]$ exit
    logout
    Connection to snamenode closed.
```
<img width="835" alt="Screen Shot 2019-04-23 at 4 24 31 PM" src="https://user-images.githubusercontent.com/46523571/56562449-51c00b80-65e4-11e9-8df7-d5826b1e3ca9.png">

```
    # namenode에서 ssh data01 접속 확인
    
    [ec2-user@namenode ~]$ ssh data01
    Last login: Tue Apr 23 06:55:04 2019 from namenode
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    
    [ec2-user@data01 ~]$ exit
    logout
    Connection to data01 closed.
```
<img width="837" alt="Screen Shot 2019-04-23 at 4 23 51 PM" src="https://user-images.githubusercontent.com/46523571/56562386-3228e300-65e4-11e9-9f3a-4c58fb91c77c.png">

```
    # namenode에서 ssh data02 접속 확인
    
    [ec2-user@namenode ~]$ ssh data02
    Last login: Tue Apr 23 07:19:12 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    
    [ec2-user@data02 ~]$ exit
    logout
    Connection to data02 closed.
```
<img width="837" alt="Screen Shot 2019-04-23 at 4 23 09 PM" src="https://user-images.githubusercontent.com/46523571/56562314-19203200-65e4-11e9-8bd6-c3f1c171e1ae.png">

```
    # namenode에서 ssh data03 접속 확인
    
    [ec2-user@namenode ~]$ ssh data03
    Last login: Tue Apr 23 07:19:24 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    [ec2-user@data03 ~]$ 
```
<img width="837" alt="Screen Shot 2019-04-23 at 4 22 26 PM" src="https://user-images.githubusercontent.com/46523571/56562269-0279db00-65e4-11e9-9c9a-f3a903e86c20.png">

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>