---
layout: post
title: '[hadoop] macOS에 하둡(hadoop) 개발환경구축'
subtitle: 
categories: env
tags: til env hadoop
comments: true
date: 2019-04-23 00:12:17 +0900
lastmod: 2019-04-23 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

macOS에 하둡(hadoop) 개발환경구축<br />

## 목차
1. VirtureBox 설치
2. VirtureBox에 Enterprise Linux 설치
3. Network 세팅
4. Java JDK 설치
5. Setup Hadoop 설치
6. Mapreduce
7. Hadoop Streaming

<br>
<br>
<br>

## 참고
* Hadoop ≒ Distributed Proramming Framework 
###
    - HDFS      : 분산저장 : NameNode, DataNode       -> hdfs_comic.pdf
    - MapReduce : 분산처리 : JobTracker, TaskTracker  -> MapReduce Design Patterns.pdf

    >> http://terms.naver.com/entry.nhn?docId=1691556&cid=42171&categoryId=42183
    >> Cloudera, Hortonworks, MapR
       -> https://kimws.wordpress.com/2012/02/01/빅데이터-솔루션-업체-관계와-하둡기술기반의-스타/

    >> How To Learn
       -> https://learn.mapr.com/
          https://mapr.com/developer-portal/mapr-tutorials/
       -> https://ko.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/
          https://ko.hortonworks.com/tutorials/
       -> https://www.cloudera.com/about/training.html
          https://www.edureka.co/blog/cloudera-hadoop-tutorial/

    >> 참고 교재
       -> http://www.yes24.com/Product/Goods/47699009 : 실무 프로젝트로 배우는 빅데이터 기술
       -> http://www.yes24.com/Product/Goods/44307209 : 하둡과 스파크를 활용한 실용 데이터 과학 

<br>
<br>
<br>

## 참고.
* 하둡 에코시스템(Hadoop Ecosystem)
* 하둡은 분산 프로그래밍 프레임워크, 하둡 에코시스템은 하둡을 이루고 있는 다양한 서브 프로젝트들의 모임.
###
    하둡 코어 프로젝트
      - 분산 데이터 저장 HDFS
      - 분산 데이터 처리 MapReduce
        
    하둡 서브 프로젝트
      - 워크플로우 관리 Oozie, Ambari
      - 실시간 SQL 질의 Impala, Tajo
      - 직렬화 Avro
      - 데이터 분석 Pig, Hive
      - 데이터 마이닝 Mahout
      - 메타데이터 관리 HCatalog
      - 비정형 데이터 수집 Chuckwa, Flume, Scribe
      - 정형 데이터 수집 Sqoop, hiho
      - 분산 코디네이터 Zookeeper
      - 분산 데이터베이스 Hbase, Cassandra

<br>
<br>
<br>

# [1] Install VirtualBox on macOS
> 개발환경<br> 
> OS : macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> 가상환경 : Enterprise Linux<br>
> JDK : jdk1.8.0_211<br>

## [1-1] 다운로드
* Oracle VirtualBox 사이트 접속 
  - https://www.virtualbox.org/
<img width="1680" alt="55596728-03f27900-5785-11e9-89a6-2df89c49b08f" src="https://user-images.githubusercontent.com/46523571/55633018-7ea7ac80-57f6-11e9-87e4-e58d75a37ec3.png">

* VirtualBox 6.0.4 platform packages 
  - OS X hosts 다운로드
  - https://www.virtualbox.org/wiki/Downloads
<img width="1680" alt="55596750-1a003980-5785-11e9-80a2-8e0d67b48a69" src="https://user-images.githubusercontent.com/46523571/55633019-7ea7ac80-57f6-11e9-9852-81e2ce938bc5.png">

<br>
<br>
<br>

## [1-2] 설치
<img width="672" alt="55596808-854a0b80-5785-11e9-8a07-0f10b2e7877f" src="https://user-images.githubusercontent.com/46523571/55633020-7f404300-57f6-11e9-946a-a3221c6a3937.png">
<img width="617" alt="55596827-97c44500-5785-11e9-8ba7-7e62d06535a8" src="https://user-images.githubusercontent.com/46523571/55633021-7f404300-57f6-11e9-992a-041e0bafb8b6.png">
<img width="619" alt="55596839-a4489d80-5785-11e9-9439-b79ab4ac28cb" src="https://user-images.githubusercontent.com/46523571/55633022-7f404300-57f6-11e9-8347-cd03fa86fe02.png">
<img width="618" alt="55596849-b0345f80-5785-11e9-9a5a-fb60104c17ca" src="https://user-images.githubusercontent.com/46523571/55633023-7fd8d980-57f6-11e9-9fe2-53956099b696.png">
<img width="622" alt="55596858-bf1b1200-5785-11e9-8ab4-15e47d0b7938" src="https://user-images.githubusercontent.com/46523571/55633025-7fd8d980-57f6-11e9-9b18-f4fb5a10c213.png">

<br>
<br>
<br>

# [2] Import Linux on VirtualBox on macOS
## [2-1] 가상환경 가져오기
* Enterprise Linux 5 가져오기
<img width="1676" alt="Screen Shot 2019-04-05 at 10 03 53 AM" src="https://user-images.githubusercontent.com/46523571/55634704-fb885580-57f9-11e9-93c4-49647e802bb6.png">
<img width="905" alt="55596892-f25da100-5785-11e9-8283-fc064cd7074b" src="https://user-images.githubusercontent.com/46523571/55634695-fa572880-57f9-11e9-942f-871780678190.png">
<img width="904" alt="Screen Shot 2019-04-05 at 9 59 20 AM" src="https://user-images.githubusercontent.com/46523571/55634697-faefbf00-57f9-11e9-85f9-5ceb522aa5ab.png">
<img width="908" alt="Screen Shot 2019-04-05 at 9 59 29 AM" src="https://user-images.githubusercontent.com/46523571/55634698-faefbf00-57f9-11e9-983d-96f8ca09e20a.png">
<img width="906" alt="Screen Shot 2019-04-05 at 10 00 25 AM" src="https://user-images.githubusercontent.com/46523571/55634699-faefbf00-57f9-11e9-9a10-0a5837801c4e.png">

* (선택사항) 가상환경 RAM 2GB -> 8GB로 변경
<img width="910" alt="Screen Shot 2019-04-05 at 10 00 57 AM" src="https://user-images.githubusercontent.com/46523571/55634701-faefbf00-57f9-11e9-9565-703ef3da41e3.png">

* import 버튼 클릭
<img width="906" alt="Screen Shot 2019-04-05 at 10 01 28 AM" src="https://user-images.githubusercontent.com/46523571/55634702-fb885580-57f9-11e9-9f27-4d1e5be408b5.png">

* 좌측에 새로운 가상환경 생성 완료
<img width="1680" alt="Screen Shot 2019-04-05 at 10 04 30 AM" src="https://user-images.githubusercontent.com/46523571/55634705-fb885580-57f9-11e9-976c-482f79cb0e64.png">

<br>
<br>
<br>

# [3] Setting Network
## [3-1] 네트워크 세팅
* 가상환경 시작시 네트워크 오류 발생
<img width="442" alt="Screen Shot 2019-04-05 at 10 08 01 AM" src="https://user-images.githubusercontent.com/46523571/55634096-b6afef00-57f8-11e9-8a59-a2dc55b8b78c.png">

* Tools - 3줄짜리 버튼 - network 클릭
<img width="838" alt="Screen Shot 2019-04-05 at 11 00 56 AM" src="https://user-images.githubusercontent.com/46523571/55634097-b6afef00-57f8-11e9-8224-c11279e3ad3a.png">

* HOST 설정 Adapter 설정
  - IPv4 Address : 192.168.56.1
  - IPv4 Network Mask : 255.255.255.0
<img width="841" alt="Screen Shot 2019-04-05 at 11 01 09 AM" src="https://user-images.githubusercontent.com/46523571/55634098-b7488580-57f8-11e9-8900-82cbe6c1960c.png">

* HOST 설정 DHCP Server 설정
  - Server Address : 192.168.56.100
  - Server Mask : 255.255.255.0
  - Lower Address Bound : 192.168.56.101
  - Upper Address Bound : 192.168.56.254
<img width="838" alt="Screen Shot 2019-04-05 at 11 01 17 AM" src="https://user-images.githubusercontent.com/46523571/55634100-b7488580-57f8-11e9-828f-6dce4531a8c8.png">

* VirtualBox - Preferences - Newtwork - Adds New NAT network 버튼 클릭
  - Network Name : NatNetwork
  - Network CIDR : 10.0.2.0/24
  - Network Options : Supports DHCP 체크  
<img width="839" alt="Screen Shot 2019-04-05 at 11 01 45 AM" src="https://user-images.githubusercontent.com/46523571/55634101-b7488580-57f8-11e9-9730-88a612dd18ae.png">
<img width="838" alt="Screen Shot 2019-04-05 at 11 02 00 AM" src="https://user-images.githubusercontent.com/46523571/55634103-b7488580-57f8-11e9-8be0-f8ee863ad38f.png">

* 가상환경 구동 성공
<img width="1213" alt="Screen Shot 2019-04-05 at 11 38 21 AM" src="https://user-images.githubusercontent.com/46523571/55634104-b7e11c00-57f8-11e9-84f3-db5ee6e7ec6a.png">

<br>
<br>
<br>

# [4] Install Java JDK on Linux on VirtualBox on macOS
## [4-1] 가상환경 접속
* Username : 사용자명 입력
* Password : 비밀번호 입력
<img width="721" alt="Screen Shot 2019-04-05 at 3 07 44 PM" src="https://user-images.githubusercontent.com/46523571/55633697-e3173b80-57f7-11e9-8c31-d95dcbbf0b61.png">
<img width="719" alt="Screen Shot 2019-04-05 at 3 07 55 PM" src="https://user-images.githubusercontent.com/46523571/55633699-e3173b80-57f7-11e9-9953-7491430888cf.png">

## [4-2] jdk 확인
      1. 32bit/64bit 확인
      [orcl:~]$ getconf LONG_BIT
      32
      
      2. 사용자 확인
      [orcl:~]$ whoami
      oracle
      
      3. 자바 버전 확인
      [orcl:~]$ java -version
    
      java version "1.6.0_18"
      Java(TM) SE Runtime Environment (build 1.6.0_18-b07)
      Java HotSpot(TM) Server VM (build 16.0-b13, mixed mode)
      
      4. 설치된 자바 위치 확인
      [orcl:~]$ which java
      /usr/java/jdk1.6.0_18/bin/java

<img width="573" alt="Screen Shot 2019-04-21 at 10 08 50 PM" src="https://user-images.githubusercontent.com/46523571/56470536-173e5d80-6482-11e9-81b2-54550ce554aa.png">

## [4-3] 최신버전 JDK 다운로드 및 설치
      http://www.oracle.com/technetwork/java/javase/downloads/index.html 에서 최신 jdk를 다운로드하세요.
      >> jdk1.8.0_211 다운로드하는게 실습에 에러가 적다!
         jdk-*-linux-i586.tar.gz
    
      [orcl:~]$ su -
      Password: oracle   <- 보이지 않습니다.
    
      [root@edydr1p0 ~]# ls /home/oracle/Desktop
    
      [root@edydr1p0 ~]# mv /home/oracle/Desktop/jdk* /usr/java
      
      [root@edydr1p0 ~]# cd /usr/java
    
      [root@edydr1p0 java]# ls -l
    
      [root@edydr1p0 java]# chmod 755 jdk-*-linux-i586.tar.gz
    
      [root@edydr1p0 java]# ls -l
    
      [root@edydr1p0 java]# tar xvfz jdk-*-linux-i586.tar.gz
    
      [root@edydr1p0 java]# exit

## [4-4] 자바 환경 변수 설정      
      [orcl:~]$ cd
      [orcl:~]$ whoami
      oracle
    
      [orcl:~]$ vi .bash_profile
    
      # JAVA_HOME 환경 변수의 값을 적절하게 바꾸세요.
    
        export JAVA_HOME=/usr/java/jdk1.8.0_211
    
      [orcl:~]$ . .bash_profile
      [orcl:~]$ java -version 
    
      java version "1.8.0_211"
      Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
      Java HotSpot(TM) Server VM (build 25.121-b13, mixed mode)

<img width="574" alt="Screen Shot 2019-04-21 at 10 06 45 PM" src="https://user-images.githubusercontent.com/46523571/56470495-c890c380-6481-11e9-81db-2a0d10d03050.png">
<img width="574" alt="Screen Shot 2019-04-21 at 10 09 00 PM" src="https://user-images.githubusercontent.com/46523571/56470535-173e5d80-6482-11e9-8e08-d552f4c4792f.png">

## [4-5] 자바 연습
      [orcl:~]$ vi HelloWorldApp.java
    
        class HelloWorldApp {
            public static void main(String[] args) {
                System.out.println("Hello World!"); // Display the string.
            }
        }
    
      [orcl:~]$ javac HelloWorldApp.java
     
      [orcl:~]$ java HelloWorldApp
      Hello World!
     
      [orcl:~]$ v
<img width="304" alt="Screen Shot 2019-04-19 at 3 21 55 PM" src="https://user-images.githubusercontent.com/46523571/56410155-13aaab00-62b7-11e9-9c30-b94c60f12e2c.png">

<br>
<br>
<br>


## [5-1] 하둡 관리 및 하둡 프로그래밍용 리눅스 유저 생성
* 가상환경 리눅스에서 oracle 유저로 로그인후 root 유저로 전환
###    
      [orcl:~]$ su -
      Password: oracle    <- 보이지 않습니다.

      --hadoop 그룹 생성
      [root@edydr1p0 ~]# groupadd hadoop
      
      --hadoop 유저 생성
      [root@edydr1p0 ~]# useradd hadoop -g hadoop -G vboxsf

      --hadoop 유저 암호 설정  
      [root@edydr1p0 ~]# passwd hadoop
      Changing password for user hadoop.
      New UNIX password: hadoop          <- 보이지 않습니다.
      BAD PASSWORD: it is based on a dictionary word    <-너무 쉬운 비밀번호라 표시된다. 그래도 그냥 아래처럼 hadoop으로 설정
      Retype new UNIX password: hadoop   <- 보이지 않습니다.
      passwd: all authentication tokens updated successfully.

      --하둡 관련 파일 설치용 디렉토리 생성 
      [root@edydr1p0 ~]# mkdir /opt/hadoop

      --디렉토리 소유자 및 그룹 설정
      [root@edydr1p0 ~]# chown hadoop:hadoop /opt/hadoop

* hadoop 유저로 로그인
###
      [root@edydr1p0 ~]# su - hadoop    <- root 유저에서 su를 실행하면 암호를 물어보지 않습니다.
    
      [hadoop@edydr1p0 ~]$ pwd
      /home/hadoop
    
      [hadoop@edydr1p0 ~]$ vi .bash_profile
      export JAVA_HOME=/usr/java/jdk1.8.0_211
      export PATH=$JAVA_HOME/bin:$PATH
      
<img width="826" alt="Screen Shot 2019-04-19 at 3 29 04 PM" src="https://user-images.githubusercontent.com/46523571/56411895-49529280-62bd-11e9-9445-8dd8d87fc0a9.png">

      [hadoop@edydr1p0 ~]$ . .bash_profile
      
      [hadoop@edydr1p0 ~]$ echo $JAVA_HOME
      /usr/java/jdk1.8.0_211

      --하둡 (수업) 관련 파일을 저장할 디렉토리 생성
      [hadoop@edydr1p0 ~]$ mkdir download

<br>
<br>
<br>

## [5-2] Hadoop Setup : Single Node Setup 
* SSH 설정
  - 하둡은 SSH 프로토콜을 이용해 하둡 클러스터 간의 내부 통신을 수행합니다. 네임노드에서 SSH 공개키를 설정하고, 이 공개키를 전체 서버에 복사해야 합니다.
###    
      [hadoop@edydr1p0 ~]$ whoami
      hadoop
    
      [hadoop@edydr1p0 ~]$ cd
      
      [hadoop@edydr1p0 ~]$ ssh-keygen -t rsa
      Generating public/private rsa key pair.
      Enter file in which to save the key (/home/hadoop/.ssh/id_rsa):   <-- 그냥 Enter
      Created directory '/home/hadoop/.ssh'.
      Enter passphrase (empty for no passphrase):   <-- 그냥 Enter
      Enter same passphrase again:    <-- 그냥 Enter
      Your identification has been saved in /home/hadoop/.ssh/id_rsa.
      Your public key has been saved in /home/hadoop/.ssh/id_rsa.pub.
      The key fingerprint is:  <-- 그냥 Enter
      93:11:2f:ce:70:9a:01:39:98:25:c7:9b:d6:e3:7f:f2 hadoop@edydr1p0.us.oracle.com
    
      [hadoop@edydr1p0 ~]$ ls -lR .ssh
      .ssh:
      total 8
      -rw------- 1 hadoop hadoop 1679 Apr 19 15:29 id_rsa
      -rw-r--r-- 1 hadoop hadoop  411 Apr 19 15:29 id_rsa.pub
    
      [hadoop@edydr1p0 ~]$ cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
      
      [hadoop@edydr1p0 ~]$ ssh localhost
      The authenticity of host 'localhost (127.0.0.1)' can't be established.
      RSA key fingerprint is a3:43:5b:0b:8c:76:75:bc:37:b6:29:7f:df:29:ab:f0.
      Are you sure you want to continue connecting (yes/no)? yes      <-- yes
      Warning: Permanently added 'localhost' (RSA) to the list of known hosts.
    
      [hadoop@edydr1p0 ~]$ 

<br>
<br>
<br>

## [5-2-1] Hadoop 다운로드 및 설치 
* http://mirror.apache-kr.org/hadoop/common에서 1.2.X stable 버전을 사용합니다.
###
      --설치할 경로로 이동
      [hadoop@edydr1p0 ~]$ cd /opt/hadoop
      
      --hadoop 다운로드
      [hadoop@edydr1p0 hadoop]$ wget https://archive.apache.org/dist/hadoop/core/hadoop-1.2.1/hadoop-1.2.1.tar.gz
      --2019-04-19 15:33:03--  https://archive.apache.org/dist/hadoop/core/hadoop-1.2.1/hadoop-1.2.1.tar.gz
      Resolving archive.apache.org... 163.172.17.199
      Connecting to archive.apache.org|163.172.17.199|:443... connected.
      HTTP request sent, awaiting response... 200 OK
      Length: 63851630 (61M) [application/x-gzip]
      Saving to: `hadoop-1.2.1.tar.gz'
    
      100%[======================================>] 63,851,630  4.70M/s   in 16s     
    
      2019-04-19 15:33:20 (3.90 MB/s) - `hadoop-1.2.1.tar.gz' saved [63851630/63851630]
      
      --hadoop 압축 해제
      [hadoop@edydr1p0 hadoop]$ tar xvfz hadoop-1.2.1.tar.gz
      엄청 길게 실행 됨+_+
      
      --심볼릭 링크 생성
      [hadoop@edydr1p0 hadoop]$ ln -s hadoop-1.2.1 hadoop
    
      [hadoop@edydr1p0 hadoop]$ cd 
    
      [hadoop@edydr1p0 ~]$ vi + .bash_profile
      가장 아래쪽에 추가하세요.
      export HADOOP_HOME=/opt/hadoop/hadoop
      export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
<img width="827" alt="Screen Shot 2019-04-19 at 3 35 32 PM" src="https://user-images.githubusercontent.com/46523571/56411960-8c146a80-62bd-11e9-942e-22d45121a480.png">
    
      [hadoop@edydr1p0 ~]$ . .bash_profile
    
      [hadoop@edydr1p0 ~]$ env|grep HOME
      HADOOP_HOME=/opt/hadoop/hadoop
      JAVA_HOME=/usr/java/jdk1.8.0_211
      HOME=/home/hadoop
    
      [hadoop@edydr1p0 ~]$ pwd
      /home/hadoop

<br>
<br>
<br>

## [5-2-2] Hadoop 환경설정 파일 수정 : 6개
      [hadoop@edydr1p0 ~]$ cd $HADOOP_HOME/conf
      
      [hadoop@edydr1p0 conf]$ pwd
      /opt/hadoop/hadoop/conf

      [hadoop@edydr1p0 conf]$ ls
      capacity-scheduler.xml      hadoop-policy.xml      slaves
      configuration.xsl           hdfs-site.xml          ssl-client.xml.example
      core-site.xml               log4j.properties       ssl-server.xml.example
      fair-scheduler.xml          mapred-queue-acls.xml  taskcontroller.cfg
      hadoop-env.sh               mapred-site.xml        task-log4j.properties
      hadoop-metrics2.properties  masters

      [hadoop@edydr1p0 conf]$ ls *.sh
      hadoop-env.sh

      [hadoop@edydr1p0 conf]$ ls *.xml
      capacity-scheduler.xml  hadoop-policy.xml      mapred-site.xml
      core-site.xml           hdfs-site.xml
      fair-scheduler.xml      mapred-queue-acls.xml
###
      [hadoop@edydr1p0 conf]$ vi hadoop-env.sh    -- 위에서 여덟번째 줄 부근에 적절하게 입력하세요. 
      export JAVA_HOME=/usr/java/jdk1.8.0_211
      export HADOOP_HOME=/opt/hadoop/hadoop
      export HADOOP_HOME_WARN_SUPPRESS=1 
<img width="825" alt="Screen Shot 2019-04-19 at 3 39 32 PM" src="https://user-images.githubusercontent.com/46523571/56411990-abab9300-62bd-11e9-86df-2144fbe99ab9.png">

###
      [hadoop@edydr1p0 conf]$ more masters        -- SecondaryNameNode 설정
      localhost

###    
      [hadoop@edydr1p0 conf]$ more slaves         -- DataNode, Task Tracker 설정
      localhost

###    
      [hadoop@edydr1p0 conf]$ vi core-site.xml    -- NameNode의 호스트명과 포트 설정
      -- STS 플러그인으로 접속하려면 localhost 대신 ip address 사용할 것

      <configuration>
          <property>
              <name>fs.default.name</name>
              <value>hdfs://192.168.56.102:9000</value>     
          </property>
          <property>
              <name>hadoop.tmp.dir</name>
              <value>/opt/hadoop/hadoop-tmp-dir/</value>
          </property>
      </configuration>
<img width="825" alt="Screen Shot 2019-04-19 at 3 41 57 PM" src="https://user-images.githubusercontent.com/46523571/56412051-ea414d80-62bd-11e9-9246-54389036b4c0.png">
    
###
      [hadoop@edydr1p0 conf]$ vi hdfs-site.xml         -- HDFS에서 사용할 환경정보 설정
    
      <configuration>
          <property>
              <name>dfs.replication</name>
              <value>1</value>
          </property>
          <property>
              <name>dfs.http.address</name>
              <value>192.168.56.102:50070</value>
          </property>
          <property>
              <name>dfs.secondary.http.address</name>
              <value>192.168.56.102:50090</value>
          </property>
          <property>
              <name>dfs.name.dir</name>
              <value>/opt/hadoop/hadoop-tmp-dir/dfs/name</value>
          </property>
          <property>
              <name>dfs.name.edits.dir</name>
              <value>${dfs.name.dir}</value>
          </property>
          <property>
              <name>dfs.data.dir</name>
              <value>/opt/hadoop/hadoop-tmp-dir/dfs/data</value>
          </property>
          <property>
              <name>dfs.permissions</name>
              <value>false</value>
          </property>
      </configuration>
<img width="826" alt="Screen Shot 2019-04-19 at 3 42 46 PM" src="https://user-images.githubusercontent.com/46523571/56412084-0349fe80-62be-11e9-9932-30d54e6c8fd8.png">

###
      --위에서 dfs.permissions를 false로 해서 STS 플러그인에서 파일 업로드를 할 수 있게 한다.
     [hadoop@edydr1p0 conf]$ vi mapred-site.xml       -- JobTracker의 호스트명과 포트 설정
    
      <configuration>
           <property>
               <name>mapred.job.tracker</name>
               <value>192.168.56.102:9001</value>
           </property>
      </configuration>

<br>
<br>
<br>

## [5-2-3] Hadoop 실행
      [hadoop@edydr1p0 conf]$ cd
    
      [hadoop@edydr1p0 ~]$ which hadoop
      /opt/hadoop/hadoop/bin/hadoop
    
      [hadoop@edydr1p0 ~]$ more $HADOOP_HOME/bin/hadoop
      more 100%로 될때까지 엔터 누르기~!
    
      [hadoop@edydr1p0 ~]$ hadoop namenode -format   
    19/04/19 15:48:02 INFO namenode.NameNode: STARTUP_MSG: 
    /************************************************************
    STARTUP_MSG: Starting NameNode
    STARTUP_MSG:   host = edydr1p0.us.oracle.com/127.0.0.1
    STARTUP_MSG:   args = [-format]
    STARTUP_MSG:   version = 1.2.1
    STARTUP_MSG:   build = https://svn.apache.org/repos/asf/hadoop/common/branches/branch-1.2 -r 1503152; compiled by 'mattf' on Mon Jul 22 15:23:09 PDT 2013
    STARTUP_MSG:   java = 1.8.0_211
    ************************************************************/
    19/04/19 15:48:02 INFO util.GSet: Computing capacity for map BlocksMap
    19/04/19 15:48:02 INFO util.GSet: VM type       = 32-bit
    19/04/19 15:48:02 INFO util.GSet: 2.0% max memory = 932184064
    19/04/19 15:48:02 INFO util.GSet: capacity      = 2^22 = 4194304 entries
    19/04/19 15:48:02 INFO util.GSet: recommended=4194304, actual=4194304
    19/04/19 15:48:02 INFO namenode.FSNamesystem: fsOwner=hadoop
    19/04/19 15:48:02 INFO namenode.FSNamesystem: supergroup=supergroup
    19/04/19 15:48:02 INFO namenode.FSNamesystem: isPermissionEnabled=false
    19/04/19 15:48:02 INFO namenode.FSNamesystem: dfs.block.invalidate.limit=100
    19/04/19 15:48:02 INFO namenode.FSNamesystem: isAccessTokenEnabled=false accessKeyUpdateInterval=0 min(s), accessTokenLifetime=0 min(s)
    19/04/19 15:48:02 INFO namenode.FSEditLog: dfs.namenode.edits.toleration.length = 0
    19/04/19 15:48:02 INFO namenode.NameNode: Caching file names occuring more than 10 times 
    19/04/19 15:48:03 INFO common.Storage: Image file /opt/hadoop/hadoop-tmp-dir/dfs/name/current/fsimage of size 112 bytes saved in 0 seconds.
    19/04/19 15:48:03 INFO namenode.FSEditLog: closing edit log: position=4, editlog=/opt/hadoop/hadoop-tmp-dir/dfs/name/current/edits
    19/04/19 15:48:03 INFO namenode.FSEditLog: close success: truncate to 4, editlog=/opt/hadoop/hadoop-tmp-dir/dfs/name/current/edits
    19/04/19 15:48:03 INFO common.Storage: Storage directory /opt/hadoop/hadoop-tmp-dir/dfs/name has been successfully formatted.
    19/04/19 15:48:03 INFO namenode.NameNode: SHUTDOWN_MSG: 
    /************************************************************
    SHUTDOWN_MSG: Shutting down NameNode at edydr1p0.us.oracle.com/127.0.0.1
    ************************************************************/
    
      [hadoop@edydr1p0 ~]$ more $HADOOP_HOME/bin/start-all.sh 
      more 100%로 될때까지 엔터 누르기~!



    
      [hadoop@edydr1p0 ~]$ start-all.sh       // HDFS & 맵리듀스 모두 실행
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out


      [hadoop@edydr1p0 ~]$ jps
    16450 NameNode
    17058 Jps
    16725 SecondaryNameNode
    16967 TaskTracker
    16814 JobTracker
    16574 DataNode

      [hadoop@edydr1p0 ~]$ 

<br>
<br>
<br>

## [5-2-4] jps는 Java Virtual Machine Process Status Tool의 약자이며,
* 시스템에서 실행 중인 자바 프로세스를 출력합니다. 
###    
      [hadoop@edydr1p0 ~]$ jps            
    
      8981 NameNode
      9377 JobTracker
      9616 Jps
      9119 DataNode
      9267 SecondaryNameNode
      9511 TaskTracker
       ==> 만약 jps 명령의 결과가 적절하지 않다면 다음과 같이 삭제하고 압축 해제하는 부분부터 다시 하세요.

       [hadoop@edydr1p0 ~]$ cd /opt/hadoop

       [hadoop@edydr1p0 hadoop]$ ls
       hadoop        hadoop-1.2.1.tar.gz
       hadoop-1.2.1  hadoop-tmp-dir

       [hadoop@edydr1p0 hadoop]$ rm -rf hadoop-tmp-dir
       [hadoop@edydr1p0 hadoop]$ rm -rf hadoop-1.2.1 
       [hadoop@edydr1p0 hadoop]$ rm -rf hadoop

     * http://192.168.56.101:50070  : NameNode
       http://192.168.56.101:50030  : JobTracker

      [hadoop@edydr1p0 ~]$ stop-all.sh        // HDFS & 맵리듀스 모두 종료
    stopping jobtracker
    localhost: stopping tasktracker
    stopping namenode
    localhost: stopping datanode
    localhost: stopping secondarynamenode
###    
      [hadoop@edydr1p0 ~]$ start-dfs.sh       // HDFS만 실행
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out

      [hadoop@edydr1p0 ~]$ jps                // NameNode 및 DataNode는 HDFS의 구성요소
    17539 NameNode
    17669 DataNode
    17845 SecondaryNameNode
    17900 Jps
    
      [hadoop@edydr1p0 ~]$ stop-dfs.sh        // HDFS만 종료
    stopping namenode
    localhost: stopping datanode
    localhost: stopping secondarynamenode
###    
      [hadoop@edydr1p0 ~]$ start-mapred.sh    // Map Reduce만 실행
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out
    
      [hadoop@edydr1p0 ~]$ jps                // JobTracker 및 TaskTracker는 맵리듀스의 구성요소
    18160 JobTracker
    18313 TaskTracker
    18383 Jps
    
      [hadoop@edydr1p0 ~]$ stop-mapred.sh     // Map Reduce만 종료
    stopping jobtracker
    localhost: stopping tasktracker
###    
      [hadoop@edydr1p0 ~]$ start-all.sh 
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out

<br>
<br>
<br>

## [5-3] Hadoop programming 예제
* WordCount 소스 분석 : http://me2.do/5Z4aYa6k
###
      [hadoop@edydr1p0 ~]$ cd
    
      [hadoop@edydr1p0 ~]$ hadoop fs -mkdir input
    
      [hadoop@edydr1p0 ~]$ hadoop fs -ls
      Found 1 items
      drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:09 /user/hadoop/input
  
      [hadoop@edydr1p0 ~]$ hadoop fs -du
      Found 1 items
      0           hdfs://192.168.56.101:9000/user/hadoop/input

      [hadoop@edydr1p0 ~]$ hadoop fs -put $HADOOP_HOME/README.txt input
    
      [hadoop@edydr1p0 ~]$ hadoop fs -ls
      Found 1 items
      drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:10 /user/hadoop/input
     
      [hadoop@edydr1p0 ~]$ hadoop fs -lsr
      drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:10 /user/hadoop/input
      -rw-r--r--   1 hadoop supergroup       1366 2019-04-19 17:10 /user/hadoop/input/README.txt

      [hadoop@edydr1p0 ~]$ hadoop fs -cat /user/hadoop/input/README.txt
    For the latest information about Hadoop, please visit our website at:
    
       http://hadoop.apache.org/core/
    
    and our wiki, at:
    
       http://wiki.apache.org/hadoop/
    
    This distribution includes cryptographic software.  The country in 
    which you currently reside may have restrictions on the import, 
    possession, use, and/or re-export to another country, of 
    encryption software.  BEFORE using any encryption software, please 
    check your country's laws, regulations and policies concerning the
    import, possession, or use, and re-export of encryption software, to 
    see if this is permitted.  See <http://www.wassenaar.org/> for more
    information.
    
    The U.S. Government Department of Commerce, Bureau of Industry and
    Security (BIS), has classified this software as Export Commodity 
    Control Number (ECCN) 5D002.C.1, which includes information security
    software using or performing cryptographic functions with asymmetric
    algorithms.  The form and manner of this Apache Software Foundation
    distribution makes it eligible for export under the License Exception
    ENC Technology Software Unrestricted (TSU) exception (see the BIS 
    Export Administration Regulations, Section 740.13) for both object 
    code and source code.
    
    The following provides more details on the included cryptographic
    software:
      Hadoop Core uses the SSL libraries from the Jetty project written 
    by mortbay.org.
    
      * Usage: hadoop jar <jar> [mainClass] args...
      * Usage: hadoop CLASSNAME args...
    
      [hadoop@edydr1p0 ~]$ ls $HADOOP_HOME/*.jar
    /opt/hadoop/hadoop/hadoop-ant-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-client-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-core-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-examples-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-minicluster-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-test-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-tools-1.2.1.jar

      [hadoop@edydr1p0 ~]$ jar -tf $HADOOP_HOME/hadoop-examples*    // jar 파일 내용 확인
    META-INF/
    META-INF/MANIFEST.MF
    org/
    org/apache/
    org/apache/hadoop/
    org/apache/hadoop/examples/
    org/apache/hadoop/examples/dancing/
    org/apache/hadoop/examples/terasort/
    org/apache/hadoop/examples/AggregateWordCount$WordCountPlugInClass.class
    org/apache/hadoop/examples/AggregateWordCount.class
    org/apache/hadoop/examples/AggregateWordHistogram$AggregateWordHistogramPlugin.class
    org/apache/hadoop/examples/AggregateWordHistogram.class
    org/apache/hadoop/examples/DBCountPageView$AccessRecord.class
    org/apache/hadoop/examples/DBCountPageView$PageviewMapper.class
    org/apache/hadoop/examples/DBCountPageView$PageviewRecord.class
    org/apache/hadoop/examples/DBCountPageView$PageviewReducer.class
    org/apache/hadoop/examples/DBCountPageView.class
    org/apache/hadoop/examples/ExampleDriver.class
    org/apache/hadoop/examples/Grep.class
    org/apache/hadoop/examples/Join.class
    org/apache/hadoop/examples/MultiFileWordCount$MapClass.class
    org/apache/hadoop/examples/MultiFileWordCount$MultiFileLineRecordReader.class
    org/apache/hadoop/examples/MultiFileWordCount$MyInputFormat.class
    org/apache/hadoop/examples/MultiFileWordCount$WordOffset.class
    org/apache/hadoop/examples/MultiFileWordCount.class
    org/apache/hadoop/examples/PiEstimator$HaltonSequence.class
    org/apache/hadoop/examples/PiEstimator$PiMapper.class
    org/apache/hadoop/examples/PiEstimator$PiReducer.class
    org/apache/hadoop/examples/PiEstimator.class
    org/apache/hadoop/examples/RandomTextWriter$Counters.class
    org/apache/hadoop/examples/RandomTextWriter$Map.class
    org/apache/hadoop/examples/RandomTextWriter.class
    org/apache/hadoop/examples/RandomWriter$Counters.class
    org/apache/hadoop/examples/RandomWriter$Map.class
    org/apache/hadoop/examples/RandomWriter$RandomInputFormat$RandomRecordReader.class
    org/apache/hadoop/examples/RandomWriter$RandomInputFormat.class
    org/apache/hadoop/examples/RandomWriter.class
    org/apache/hadoop/examples/SecondarySort$FirstGroupingComparator.class
    org/apache/hadoop/examples/SecondarySort$FirstPartitioner.class
    org/apache/hadoop/examples/SecondarySort$IntPair$Comparator.class
    org/apache/hadoop/examples/SecondarySort$IntPair.class
    org/apache/hadoop/examples/SecondarySort$MapClass.class
    org/apache/hadoop/examples/SecondarySort$Reduce.class
    org/apache/hadoop/examples/SecondarySort.class
    org/apache/hadoop/examples/SleepJob$EmptySplit.class
    org/apache/hadoop/examples/SleepJob$SleepInputFormat$1.class
    org/apache/hadoop/examples/SleepJob$SleepInputFormat.class
    org/apache/hadoop/examples/SleepJob.class
    org/apache/hadoop/examples/Sort.class
    org/apache/hadoop/examples/WordCount$IntSumReducer.class
    org/apache/hadoop/examples/WordCount$TokenizerMapper.class
    org/apache/hadoop/examples/WordCount.class
    org/apache/hadoop/examples/dancing/DancingLinks$ColumnHeader.class
    org/apache/hadoop/examples/dancing/DancingLinks$Node.class
    org/apache/hadoop/examples/dancing/DancingLinks$SolutionAcceptor.class
    org/apache/hadoop/examples/dancing/DancingLinks.class
    org/apache/hadoop/examples/dancing/DistributedPentomino$PentMap$SolutionCatcher.class
    org/apache/hadoop/examples/dancing/DistributedPentomino$PentMap.class
    org/apache/hadoop/examples/dancing/DistributedPentomino.class
    org/apache/hadoop/examples/dancing/OneSidedPentomino.class
    org/apache/hadoop/examples/dancing/Pentomino$ColumnName.class
    org/apache/hadoop/examples/dancing/Pentomino$Piece.class
    org/apache/hadoop/examples/dancing/Pentomino$Point.class
    org/apache/hadoop/examples/dancing/Pentomino$SolutionCategory.class
    org/apache/hadoop/examples/dancing/Pentomino$SolutionPrinter.class
    org/apache/hadoop/examples/dancing/Pentomino.class
    org/apache/hadoop/examples/dancing/Sudoku$CellConstraint.class
    org/apache/hadoop/examples/dancing/Sudoku$ColumnConstraint.class
    org/apache/hadoop/examples/dancing/Sudoku$ColumnName.class
    org/apache/hadoop/examples/dancing/Sudoku$RowConstraint.class
    org/apache/hadoop/examples/dancing/Sudoku$SolutionPrinter.class
    org/apache/hadoop/examples/dancing/Sudoku$SquareConstraint.class
    org/apache/hadoop/examples/dancing/Sudoku.class
    org/apache/hadoop/examples/terasort/TeraGen$RandomGenerator.class
    org/apache/hadoop/examples/terasort/TeraGen$RangeInputFormat$RangeInputSplit.class
    org/apache/hadoop/examples/terasort/TeraGen$RangeInputFormat$RangeRecordReader.class
    org/apache/hadoop/examples/terasort/TeraGen$RangeInputFormat.class
    org/apache/hadoop/examples/terasort/TeraGen$SortGenMapper.class
    org/apache/hadoop/examples/terasort/TeraGen.class
    org/apache/hadoop/examples/terasort/TeraInputFormat$TeraRecordReader.class
    org/apache/hadoop/examples/terasort/TeraInputFormat$TextSampler.class
    org/apache/hadoop/examples/terasort/TeraInputFormat.class
    org/apache/hadoop/examples/terasort/TeraOutputFormat$TeraRecordWriter.class
    org/apache/hadoop/examples/terasort/TeraOutputFormat.class
    org/apache/hadoop/examples/terasort/TeraSort$TotalOrderPartitioner$InnerTrieNode.class
    org/apache/hadoop/examples/terasort/TeraSort$TotalOrderPartitioner$LeafTrieNode.class
    org/apache/hadoop/examples/terasort/TeraSort$TotalOrderPartitioner$TrieNode.class
    org/apache/hadoop/examples/terasort/TeraSort$TotalOrderPartitioner.class
    org/apache/hadoop/examples/terasort/TeraSort.class
    org/apache/hadoop/examples/terasort/TeraValidate$ValidateMapper.class
    org/apache/hadoop/examples/terasort/TeraValidate$ValidateReducer.class
    org/apache/hadoop/examples/terasort/TeraValidate.class

      [hadoop@edydr1p0 ~]$ hadoop jar $HADOOP_HOME/hadoop-examples*.jar wordcount input/README.txt output1
    19/04/19 17:13:29 INFO input.FileInputFormat: Total input paths to process : 1
    19/04/19 17:13:29 INFO util.NativeCodeLoader: Loaded the native-hadoop library
    19/04/19 17:13:29 WARN snappy.LoadSnappy: Snappy native library not loaded
    19/04/19 17:13:30 INFO mapred.JobClient: Running job: job_201904191603_0001
    19/04/19 17:13:31 INFO mapred.JobClient:  map 0% reduce 0%
    19/04/19 17:13:37 INFO mapred.JobClient:  map 100% reduce 0%
    19/04/19 17:13:46 INFO mapred.JobClient:  map 100% reduce 33%
    19/04/19 17:13:48 INFO mapred.JobClient:  map 100% reduce 100%
    19/04/19 17:13:49 INFO mapred.JobClient: Job complete: job_201904191603_0001
    19/04/19 17:13:49 INFO mapred.JobClient: Counters: 29
    19/04/19 17:13:49 INFO mapred.JobClient:   Map-Reduce Framework
    19/04/19 17:13:49 INFO mapred.JobClient:     Spilled Records=262
    19/04/19 17:13:49 INFO mapred.JobClient:     Map output materialized bytes=1836
    19/04/19 17:13:49 INFO mapred.JobClient:     Reduce input records=131
    19/04/19 17:13:49 INFO mapred.JobClient:     Virtual memory (bytes) snapshot=1068228608
    19/04/19 17:13:49 INFO mapred.JobClient:     Map input records=31
    19/04/19 17:13:49 INFO mapred.JobClient:     SPLIT_RAW_BYTES=120
    19/04/19 17:13:49 INFO mapred.JobClient:     Map output bytes=2055
    19/04/19 17:13:49 INFO mapred.JobClient:     Reduce shuffle bytes=1836
    19/04/19 17:13:49 INFO mapred.JobClient:     Physical memory (bytes) snapshot=234520576
    19/04/19 17:13:49 INFO mapred.JobClient:     Reduce input groups=131
    19/04/19 17:13:49 INFO mapred.JobClient:     Combine output records=131
    19/04/19 17:13:49 INFO mapred.JobClient:     Reduce output records=131
    19/04/19 17:13:49 INFO mapred.JobClient:     Map output records=179
    19/04/19 17:13:49 INFO mapred.JobClient:     Combine input records=179
    19/04/19 17:13:49 INFO mapred.JobClient:     CPU time spent (ms)=1670
    19/04/19 17:13:49 INFO mapred.JobClient:     Total committed heap usage (bytes)=212598784
    19/04/19 17:13:49 INFO mapred.JobClient:   File Input Format Counters 
    19/04/19 17:13:49 INFO mapred.JobClient:     Bytes Read=1366
    19/04/19 17:13:49 INFO mapred.JobClient:   FileSystemCounters
    19/04/19 17:13:49 INFO mapred.JobClient:     HDFS_BYTES_READ=1486
    19/04/19 17:13:49 INFO mapred.JobClient:     FILE_BYTES_WRITTEN=119863
    19/04/19 17:13:49 INFO mapred.JobClient:     FILE_BYTES_READ=1836
    19/04/19 17:13:49 INFO mapred.JobClient:     HDFS_BYTES_WRITTEN=1306
    19/04/19 17:13:49 INFO mapred.JobClient:   Job Counters 
    19/04/19 17:13:49 INFO mapred.JobClient:     Launched map tasks=1
    19/04/19 17:13:49 INFO mapred.JobClient:     Launched reduce tasks=1
    19/04/19 17:13:49 INFO mapred.JobClient:     SLOTS_MILLIS_REDUCES=9512
    19/04/19 17:13:49 INFO mapred.JobClient:     Total time spent by all reduces waiting after reserving slots (ms)=0
    19/04/19 17:13:49 INFO mapred.JobClient:     SLOTS_MILLIS_MAPS=6037
    19/04/19 17:13:49 INFO mapred.JobClient:     Total time spent by all maps waiting after reserving slots (ms)=0
    19/04/19 17:13:49 INFO mapred.JobClient:     Data-local map tasks=1
    19/04/19 17:13:49 INFO mapred.JobClient:   File Output Format Counters 
    19/04/19 17:13:49 INFO mapred.JobClient:     Bytes Written=1306
    
      [hadoop@edydr1p0 ~]$ hadoop fs -lsr output1
    -rw-r--r--   1 hadoop supergroup          0 2019-04-19 17:13 /user/hadoop/output1/_SUCCESS
    drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:13 /user/hadoop/output1/_logs
    drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:13 /user/hadoop/output1/_logs/history
    -rw-r--r--   1 hadoop supergroup      13970 2019-04-19 17:13 /user/hadoop/output1/_logs/history/job_201904191603_0001_1555661610610_hadoop_word+count
    -rw-r--r--   1 hadoop supergroup      50400 2019-04-19 17:13 /user/hadoop/output1/_logs/history/job_201904191603_0001_conf.xml
    -rw-r--r--   1 hadoop supergroup       1306 2019-04-19 17:13 /user/hadoop/output1/part-r-00000

      [hadoop@edydr1p0 ~]$ hadoop fs -cat output1/part-r-00000
    (BIS),  1
    (ECCN)  1
    (TSU)   1
    (see    1
    5D002.C.1,      1
    740.13) 1
    <http://www.wassenaar.org/>     1
    Administration  1
    Apache  1
    BEFORE  1
    BIS     1
    Bureau  1
    Commerce,       1
    Commodity       1
    Control 1
    Core    1
    Department      1
    ENC     1
    Exception       1
    Export  2
    For     1
    Foundation      1
    Government      1
    Hadoop  1
    Hadoop, 1
    Industry        1
    Jetty   1
    License 1
    Number  1
    Regulations,    1
    SSL     1
    Section 1
    Security        1
    See     1
    Software        2
    Technology      1
    The     4
    This    1
    U.S.    1
    Unrestricted    1
    about   1
    algorithms.     1
    and     6
    and/or  1
    another 1
    any     1
    as      1
    asymmetric      1
    at:     2
    both    1
    by      1
    check   1
    classified      1
    code    1
    code.   1
    concerning      1
    country 1
    country's       1
    country,        1
    cryptographic   3
    currently       1
    details 1
    distribution    2
    eligible        1
    encryption      3
    exception       1
    export  1
    following       1
    for     3
    form    1
    from    1
    functions       1
    has     1
    have    1
    http://hadoop.apache.org/core/  1
    http://wiki.apache.org/hadoop/  1
    if      1
    import, 2
    in      1
    included        1
    includes        2
    information     2
    information.    1
    is      1
    it      1
    latest  1
    laws,   1
    libraries       1
    makes   1
    manner  1
    may     1
    more    2
    mortbay.org.    1
    object  1
    of      5
    on      2
    or      2
    our     2
    performing      1
    permitted.      1
    please  2
    policies        1
    possession,     2
    project 1
    provides        1
    re-export       2
    regulations     1
    reside  1
    restrictions    1
    security        1
    see     1
    software        2
    software,       2
    software.       2
    software:       1
    source  1
    the     8
    this    3
    to      2
    under   1
    use,    2
    uses    1
    using   2
    visit   1
    website 1
    which   2
    wiki,   1
    with    1
    written 1
    you     1
    your    1
    
      [hadoop@edydr1p0 ~]$ hadoop fs -rmr output*
    Deleted hdfs://192.168.56.101:9000/user/hadoop/output1
    
      [hadoop@edydr1p0 ~]$ hadoop fs -lsr
    drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:10 /user/hadoop/input
    -rw-r--r--   1 hadoop supergroup       1366 2019-04-19 17:10 /user/hadoop/input/README.txt    
      * hadoop performance tuning을 검색해 보세요.
    
      [hadoop@edydr1p0 ~]$ vi kor.txt
     
        원하는 내용 붙여넣기
    
      [hadoop@edydr1p0 ~]$ hadoop fs -put kor.txt input
    
      [hadoop@edydr1p0 ~]$ hadoop jar $HADOOP_HOME/hadoop-examples*.jar wordcount input/kor.txt output2
    
      [hadoop@edydr1p0 ~]$ hadoop fs -cat output2/part-r-00000
    
      [hadoop@edydr1p0 ~]$ hadoop fs -get output2/part-r-00000 .

<br>
<br>
<br>

# [6] 맵리듀스
> MapReduce

## [6-1] 맵리듀스(MapReduce) 이해하기
###
                Input   	Output
      Map   	<k1, v1>	list (<k2, v2>)             # k1(문장), v1(한줄)           -> Map    함수 ->  list{k2(단어), v2(개수)}  -> 셔플
      Reduce	<k2, list(v2)>	list (<k3, v3>)         # k2(단어), list(v2(개수))     -> Reduce 함수 ->  list{k3(), v3()}
    
      https://www.slideshare.net/gruter/mapreduce-tuning에서 4번 슬라이드
      https://www.tutorialspoint.com/hadoop/hadoop_mapreduce.htm
      http://www.yes24.com/24/Goods/9146116

## [6-2] WordCount 소스 분석 : http://me2.do/5Z4aYa6k

<br>
<br>
<br>

# [7] 하둡 스트리밍
> Streaming

## [7-1] 하둡 스트리밍
* 참고 http://blog.acronym.co.kr/606
###
      [orcl:~]$ su - hadoop
      Password: 
    
      [hadoop@edydr1p0 ~]$ cd $HADOOP_HOME/contrib
      [hadoop@edydr1p0 contrib]$ cd streaming/
      [hadoop@edydr1p0 streaming]$ ls
      hadoop-streaming-1.2.1.jar
    
      [hadoop@edydr1p0 streaming]$


























## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>