---
layout: post
title: '[AWS] 03. mac 터미널을 통해 AWS 인스턴스 접속'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 03:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

mac 터미널을 통해 AWS 인스턴스 접속<br />

# Mac 터미널을 통해 AWS EC2 리눅스 접속
> Install Hadoop on AWS <br>

> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> AWS : Amazon Web Services with EC2(Amazon Linux AMI)<br>
> JDK : jdk-7u51-linux-x64<br>
> Hadoop : Hadoop 1.2.1<br>

## namenode 인스턴스 접속
* '연결' 버튼 클릭
<img width="1680" alt="Screen Shot 2019-04-23 at 11 03 46 AM" src="https://user-images.githubusercontent.com/46523571/56547162-86699e00-65b7-11e9-84c5-c557a24c9f36.png">

* 인스턴스에 연결 내용 확인
<img width="826" alt="Screen Shot 2019-04-23 at 11 26 31 AM" src="https://user-images.githubusercontent.com/46523571/56548296-b2d2e980-65ba-11e9-9c80-bbe4c15cbf9f.png">

* 터미널 실행
* namenode EC2 접속 
###
    chmod 400 ~/.ssh/mykeypair.pem
    ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-15-164-47-50.ap-northeast-2.compute.amazonaws.com
    
* [인스턴스 접속 참고 URL](https://aws.amazon.com/ko/getting-started/tutorials/launch-a-virtual-machine/)

<img width="939" alt="Screen Shot 2019-04-23 at 11 21 47 AM" src="https://user-images.githubusercontent.com/46523571/56548069-0e50a780-65ba-11e9-8b90-40ab92b26502.png">

<br>
<br>
<br>

## namenode 호스트 이름 변경
* [참고 URL](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/set-hostname.html)
###
    # namanode 접속한 터미널에서 
    $ sudo vi /etc/sysconfig/network

    # vi 편집기 실행 후 i를 눌러 편집 사용
    NETWORKING=yes
    HOSTNAME=namenode.localdomain
    NOZEROCONF=yes
    # :wq!를 눌러 vi 편집기 종료
    
<img width="938" alt="Screen Shot 2019-04-23 at 11 33 16 AM" src="https://user-images.githubusercontent.com/46523571/56548658-9aaf9a00-65bb-11e9-972e-1450fd08b310.png">
<img width="937" alt="Screen Shot 2019-04-23 at 11 31 57 AM" src="https://user-images.githubusercontent.com/46523571/56548595-705ddc80-65bb-11e9-9eb9-b99f416d9a34.png">


    # namanode 접속한 터미널에서 
    $ sudo vi /etc/hosts

    # vi 편집기 실행 후 i를 눌러 편집 사용
    127.0.0.1   namenode.localdomain localhost localhost.localdomain
    ::1         localhost6 localhost6.localdomain6
    # :wq!를 눌러 vi 편집기 종료

<img width="939" alt="Screen Shot 2019-04-23 at 11 37 48 AM" src="https://user-images.githubusercontent.com/46523571/56548900-448f2680-65bc-11e9-97d5-8d10086da32b.png">
<img width="940" alt="Screen Shot 2019-04-23 at 11 38 30 AM" src="https://user-images.githubusercontent.com/46523571/56548916-5670c980-65bc-11e9-8d69-c1890a0f89da.png">


    # 인스턴스를 재부팅 한다.
    $ sudo reboot
<img width="940" alt="Screen Shot 2019-04-23 at 11 40 08 AM" src="https://user-images.githubusercontent.com/46523571/56548998-8fa93980-65bc-11e9-8dcd-951d1b6ac531.png">


    # namnnode 인스턴스에 로그인
<img width="941" alt="Screen Shot 2019-04-23 at 11 41 23 AM" src="https://user-images.githubusercontent.com/46523571/56549066-bc5d5100-65bc-11e9-936f-d353db46cebb.png">


    # 호스트 이름 변경되었는지 확인. 
    $ hostname
<img width="940" alt="Screen Shot 2019-04-23 at 11 42 15 AM" src="https://user-images.githubusercontent.com/46523571/56549103-da2ab600-65bc-11e9-9c2e-670140060065.png">

<br>
<br>
<br>

## 나머지 4개(snamenode, data01, data02, data03)도 설정한다.
###
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com
    Warning: Identity file hdcluster.pem not accessible: No such file or directory.
    The authenticity of host 'ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com (13.125.187.86)' can't be established.
    ECDSA key fingerprint is SHA256:qk/vYI7wHe+B/DXvVvElgXiwh466rD6pHw1W/3DYBTo.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com,13.125.187.86' (ECDSA) to the list of known hosts.
    ec2-user@ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com: Permission denied (publickey).

    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@ip-172-31-30-31 ~]$ hostname
    ip-172-31-30-31
    
    [ec2-user@ip-172-31-30-31 ~]$ sudo vi /etc/sysconfig/network
    [ec2-user@ip-172-31-30-31 ~]$ sudo vi /etc/hosts
    [ec2-user@ip-172-31-30-31 ~]$ sudo reboot
    [ec2-user@ip-172-31-30-31 ~]$ 
    Broadcast message from ec2-user@ip-172-31-30-31
        (/dev/pts/0) at 4:03 ...
    
    The system is going down for reboot NOW!
    Connection to ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com closed by remote host.
    Connection to ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com closed.

    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 04:01:04 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@snamenode ~]$ hostname
    snamenode.localdomain
    
    [ec2-user@snamenode ~]$ exit
    logout
    Connection to ec2-13-125-187-86.ap-northeast-2.compute.amazonaws.com closed.
###
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com
    The authenticity of host 'ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com (15.164.13.90)' can't be established.
    ECDSA key fingerprint is SHA256:cqTcW7anI+bM9V1xrDPyj/M0HQoP2exNn21rxlQul/Y.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com,15.164.13.90' (ECDSA) to the list of known hosts.
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@ip-172-31-30-98 ~]$ sudo vi /etc/sysconfig/network
    [ec2-user@ip-172-31-30-98 ~]$ sudo vi /etc/hosts
    [ec2-user@ip-172-31-30-98 ~]$ sudo reboot
    [ec2-user@ip-172-31-30-98 ~]$ 
    Broadcast message from ec2-user@ip-172-31-30-98
        (/dev/pts/0) at 4:06 ...
    
    The system is going down for reboot NOW!
    Connection to ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com closed by remote host.
    Connection to ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com closed.
    
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 04:05:14 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@data01 ~]$ hostname
    data01.localdomain
    
    [ec2-user@data01 ~]$ exit
    logout
    Connection to ec2-15-164-13-90.ap-northeast-2.compute.amazonaws.com closed.
###
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com
    The authenticity of host 'ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com (13.124.188.140)' can't be established.
    ECDSA key fingerprint is SHA256:kz9D7MjzhpevKpH41qyXP0P9vqFZjapbkL0sfJSwGfQ.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com,13.124.188.140' (ECDSA) to the list of known hosts.
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@ip-172-31-28-203 ~]$ sudo vi /etc/sysconfig/network
    [ec2-user@ip-172-31-28-203 ~]$ sudo vi /etc/hosts
    [ec2-user@ip-172-31-28-203 ~]$ sudo reboot
    [ec2-user@ip-172-31-28-203 ~]$ 
    Broadcast message from ec2-user@ip-172-31-28-203
        (/dev/pts/0) at 4:08 ...
    
    The system is going down for reboot NOW!
    Connection to ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com closed by remote host.
    Connection to ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com closed.
    
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 04:07:35 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@data02 ~]$ hostname
    data02.localdomain
    
    [ec2-user@data02 ~]$ reboot
    reboot: Need to be root
    
    [ec2-user@data02 ~]$ exit
    logout
    Connection to ec2-13-124-188-140.ap-northeast-2.compute.amazonaws.com closed.
###
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-52-78-13-217.ap-northeast-2.compute.amazonaws.com
    The authenticity of host 'ec2-52-78-13-217.ap-northeast-2.compute.amazonaws.com (52.78.13.217)' can't be established.
    ECDSA key fingerprint is SHA256:QW8UD4I2AZkW0c8wIgho6or/+mQqxp8Aq+cR8C4rqIc.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'ec2-52-78-13-217.ap-northeast-2.compute.amazonaws.com,52.78.13.217' (ECDSA) to the list of known hosts.
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@ip-172-31-17-175 ~]$ sudo vi /etc/sysconfig/network
    [ec2-user@ip-172-31-17-175 ~]$ sudo vi /etc/hosts
    [ec2-user@ip-172-31-17-175 ~]$ sudo reboot
    [ec2-user@ip-172-31-17-175 ~]$ 
    Broadcast message from ec2-user@ip-172-31-17-175
        (/dev/pts/0) at 4:10 ...
    
    The system is going down for reboot NOW!
    Connection to ec2-52-78-13-217.ap-northeast-2.compute.amazonaws.com closed by remote host.
    Connection to ec2-52-78-13-217.ap-northeast-2.compute.amazonaws.com closed.
    
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-52-78-13-217.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 04:09:50 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@data03 ~]$ hostname
    data03.localdomain
    
    [ec2-user@data03 ~]$ 

<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>