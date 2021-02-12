---
layout: post
title: '[AWS] 04. AWS EC2 인스턴스에 Oracle jdk 다운로드 및 설치'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 04:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

AWS EC2 인스턴스에 Oracle jdk 다운로드 및 설치<br />

# AWS EC2 인스턴스에 Oracle jdk 다운로드 및 설치
> Install Hadoop on AWS <br>

## Oracle jdk 다운로드 및 설치
* namenode 접속
```
    @KANG ~ $ssh -i ~/.ssh/hdcluster.pem ec2-user@ec2-15-164-47-50.ap-northeast-2.compute.amazonaws.com
    Last login: Tue Apr 23 02:41:14 2019 from 222.107.238.24
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
    22 package(s) needed for security, out of 29 available
    Run "sudo yum update" to apply all updates.
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    
    [ec2-user@namenode ~]$ hostname
    namenode.localdomain
```

* Oracle jdk 다운로드
```
    [ec2-user@namenode ~]$ wget --no-check-certificate https://mirror.its.sfu.ca/mirror/CentOS-Third-Party/RCG/common/x86_64/jdk-7u51-linux-x64.rpm
    --2019-04-23 04:18:49--  https://mirror.its.sfu.ca/mirror/CentOS-Third-Party/RCG/common/x86_64/jdk-7u51-linux-x64.rpm
    Resolving mirror.its.sfu.ca (mirror.its.sfu.ca)... 142.58.101.156
    Connecting to mirror.its.sfu.ca (mirror.its.sfu.ca)|142.58.101.156|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 122639592 (117M) [application/x-rpm]
    Saving to: 'jdk-7u51-linux-x64.rpm'
    
    jdk-7u51-linux-x64.rpm    100%[====================================>] 116.96M   784KB/s    in 2m 36s  
    
    2019-04-23 04:21:27 (767 KB/s) - 'jdk-7u51-linux-x64.rpm' saved [122639592/122639592]
```

* namenode에 jdk 설치
```
    [ec2-user@namenode ~]$ sudo rpm -i jdk-7u51-linux-x64.rpm
    Unpacking JAR files...
        rt.jar...
        jsse.jar...
        charsets.jar...
        tools.jar...
        localedata.jar...
        jfxrt.jar...
```

* namenode에 심볼릭 링크 생성 [참고 URL](https://skyoo2003.github.io/post/2017/03/17/what-is-alternatives-command)
```
    [ec2-user@namenode ~]$ sudo alternatives --install /usr/bin/java java /usr/java/jdk1.7.0_51/bin/java 20000      <-심볼릭 링크 생성
    
    [ec2-user@namenode ~]$ ls -al /usr/bin/java                 <-심볼릭 체크
    lrwxrwxrwx 1 root root 22 Apr 23 04:35 /usr/bin/java -> /etc/alternatives/java
    
    [ec2-user@namenode ~]$ ls -al /etc/alternatives/java        <-심볼릭 체크
    lrwxrwxrwx 1 root root 30 Apr 23 04:35 /etc/alternatives/java -> /usr/java/jdk1.7.0_51/bin/java
    
    [ec2-user@namenode ~]$ sudo alternatives --config java
    
    There are 2 programs which provide 'java'.
    
      Selection    Command
    -----------------------------------------------
       1           /usr/lib/jvm/jre-1.7.0-openjdk.x86_64/bin/java
    *+ 2           /usr/java/jdk1.7.0_51/bin/java
    
    Enter to keep the current selection[+], or type selection number:       <-엔터
    
    [ec2-user@namenode ~]$ 
```

* namenode의 .bash_profile 수정
```
    [ec2-user@namenode ~]$ vi .bash_profile

    # vi 편집기에 아래 내용 추가하기
    export JAVA_HOME=/usr/java/jdk1.7.0_51 
    export PATH=$JAVA_HOME/bin:$PATH
    # vi 편집기 종료
```
<img width="550" alt="Screen Shot 2019-04-23 at 1 45 09 PM" src="https://user-images.githubusercontent.com/46523571/56554856-0a2e8500-65ce-11e9-9f9f-50806f3a562c.png">

* namenode의 .bash_profile 적용
```
    [ec2-user@namenode ~]$ . .bash_profile
    [ec2-user@namenode ~]
```

* namenode의 java 버전 및 설치 확인
```
    [ec2-user@namenode ~]$ java -version
    java version "1.7.0_51"
    Java(TM) SE Runtime Environment (build 1.7.0_51-b13)
    Java HotSpot(TM) 64-Bit Server VM (build 24.51-b03, mixed mode)
    
    [ec2-user@namenode ~]$ javac -version
    javac 1.7.0_51
    
    [ec2-user@namenode ~]$ jps
    
    23567 Jps
    [ec2-user@namenode ~]$ 
```

## 나머지 4개도 jdk 다운로드, jdk 설치, 심볼릭 링크 생성, .bash_profile 수정, .bash_profile 적용, java 버전 확인하기

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>