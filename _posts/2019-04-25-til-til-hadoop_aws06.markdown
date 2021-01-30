---
layout: post
title: '[AWS] 06. AWS EC2 인스턴스 방화벽 설정'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 06:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

AWS EC2 인스턴스 방화벽 설정<br />

# AWS EC2 인스턴스 방화벽 설정
> Install Hadoop on AWS <br>

> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> AWS : Amazon Web Services with EC2(Amazon Linux AMI)<br>
> JDK : jdk-7u51-linux-x64<br>
> Hadoop : Hadoop 1.2.1<br>

## EC2 인스턴스 방화벽 설정
* EC2 인스턴스는 기본적으로 SSH 프로토콜 사용을 위한 22번 포트를 제외하고는 모든 포트들이 막혀있다. 따라서 Hadoop이 사용하는 포트의 방화벽을 풀어줘야 한다.
* 네트워크 및 보안 - 보안 그룹 - launch-wizard-1 체크 박스 선택 - 인바운드 탭 - '편집' 버튼 클릭
<img width="1680" alt="Screen Shot 2019-04-23 at 4 27 35 PM" src="https://user-images.githubusercontent.com/46523571/56562643-d1e67100-65e4-11e9-811a-5d1aa6e53817.png">

* 인바운드 규칙 편집 - 규칙 추가
<img width="1680" alt="Screen Shot 2019-04-23 at 4 31 11 PM" src="https://user-images.githubusercontent.com/46523571/56562814-44575100-65e5-11e9-9331-f7be7128de43.png">

* 아래와 같이 편집
  - 유형 : 사용자 지정 TCㅈP 규칙 
  - 프로토콜 : TCP
  - 포트 범위 : 
  - 소스 :
  - '저장' 버튼 클릭
<img width="1680" alt="Screen Shot 2019-04-23 at 4 35 05 PM" src="https://user-images.githubusercontent.com/46523571/56563091-d95a4a00-65e5-11e9-9908-15c7856bdc19.png">

* 아래와 같이 변경 된걸 확인 할 수 있다.
  - 나의 경우 하둡관련 인스턴스가 launch-wizard-6이어서 6에 추가했다.
<img width="1680" alt="Screen Shot 2019-04-23 at 4 37 22 PM" src="https://user-images.githubusercontent.com/46523571/56563241-1f171280-65e6-11e9-89d1-ba5dddcfeed6.png">


















# 하둡 다운로드 및 설치
* 하둡 다운로드
* http://mirror.apache-kr.org/hadoop/common에서 1.2.X stable 버전을 사용
###
    [ec2-user@namenode ~]$ cd
    [ec2-user@namenode ~]$ wget https://archive.apache.org/dist/hadoop/core/hadoop-1.2.1/hadoop-1.2.1.tar.gz
    --2019-04-23 07:43:01--  https://archive.apache.org/dist/hadoop/core/hadoop-1.2.1/hadoop-1.2.1.tar.gz
    Resolving archive.apache.org (archive.apache.org)... 163.172.17.199
    Connecting to archive.apache.org (archive.apache.org)|163.172.17.199|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 63851630 (61M) [application/x-gzip]
    Saving to: 'hadoop-1.2.1.tar.gz'
    
    hadoop-1.2.1.tar.gz    100%[============================>]  60.89M  2.39MB/s    in 31s     
    
    2019-04-23 07:43:34 (1.95 MB/s) - 'hadoop-1.2.1.tar.gz' saved [63851630/63851630]
    
    [ec2-user@namenode ~]$ 
    
* 하둡 설치파일 압축 해제
###
    # hadoop 압축 해제
    [ec2-user@namenode ~]$ tar xvfz hadoop-1.2.1.tar.gz
    엄청 길게 실행 됨+_+
      
    # 심볼릭 링크 생성
    [ec2-user@namenode ~]$ ln -s hadoop-1.2.1 hadoop
    
    [ec2-user@namenode ~]$ cd 
    
    [ec2-user@namenode ~]$ vi + .bash_profile
    가장 아래쪽에 추가하세요.
    vi 편집기 실행
    export HADOOP_HOME=/home/ec2-user/hadoop 
    export PATH=$HADOOP_HOME/bin:$PATH 
    export PATH=$HADOOP_HOME/sbin:$PATH
    vi 편집기 종료
    
<img width="837" alt="Screen Shot 2019-04-23 at 5 13 08 PM" src="https://user-images.githubusercontent.com/46523571/56565329-170da180-65eb-11e9-9715-1478cbcf9d76.png">

    
    [ec2-user@namenode ~]$ . .bash_profile
    
    [ec2-user@namenode ~]$ env|grep HOME
    HADOOP_HOME=/home/ec2-user/hadoop
    EC2_AMITOOL_HOME=/opt/aws/amitools/ec2
    EC2_HOME=/opt/aws/apitools/ec2
    JAVA_HOME=/usr/java/jdk1.7.0_51
    AWS_CLOUDWATCH_HOME=/opt/aws/apitools/mon
    HOME=/home/ec2-user
    AWS_AUTO_SCALING_HOME=/opt/aws/apitools/as
    AWS_ELB_HOME=/opt/aws/apitools/elb
    
    [ec2-user@namenode ~]$ pwd
    /home/ec2-user

* namenode에서 다른 노드(snamenode, data01, data02, data03)들로 hadoop-1.2.1.tar.gz을 scp 명령어로 복사한다.
###
    [ec2-user@namenode ~]$ scp hadoop-1.2.1.tar.gz ec2-user@snamenode:
    hadoop-1.2.1.tar.gz                                       100%   61MB 114.7MB/s   00:00    
    [ec2-user@namenode ~]$ scp hadoop-1.2.1.tar.gz ec2-user@data01:
    hadoop-1.2.1.tar.gz                                       100%   61MB 114.3MB/s   00:00    
    [ec2-user@namenode ~]$ scp hadoop-1.2.1.tar.gz ec2-user@data02:
    hadoop-1.2.1.tar.gz                                       100%   61MB  56.6MB/s   00:01    
    [ec2-user@namenode ~]$ scp hadoop-1.2.1.tar.gz ec2-user@data03:
    hadoop-1.2.1.tar.gz                                       100%   61MB 113.4MB/s   00:00    
    [ec2-user@namenode ~]$ 
<img width="834" alt="Screen Shot 2019-04-23 at 4 51 07 PM" src="https://user-images.githubusercontent.com/46523571/56563979-07408e00-65e8-11e9-89be-e41830b1b938.png">

* 각각의 snamenode, data01, data02, data03 노드에 로그인하여 아래와 같이 hadoop-1.2.1.tar.gz 파일의 압축을 풀어주고, 위 과정처럼 환경변수를 확인한다.
###
    $ cd
    $ tar xvfz hadoop-1.2.1.tar.gz 
    $ ln -s hadoop-1.2.1 hadoop 
    $ vi .bash_profile
    $ source .bash_profile

* Hadoop 환경 설정 파일 수정(1) - hadoop-env.sh
###
    [ec2-user@namenode ~]$ cd $HADOOP_HOME/conf
    [ec2-user@namenode conf]$ vi hadoop-env.sh
    vi 편집기 실행 
    export JAVA_HOME=/usr/java/jdk1.7.0_51/jre
    export HADOOP_HOME=/home/ec2-user/hadoop
    export HADOOP_HOME_WARN_SUPPRESS=1
    vi 편집기 종료
<img width="837" alt="Screen Shot 2019-04-23 at 5 18 21 PM" src="https://user-images.githubusercontent.com/46523571/56565625-d06c7700-65eb-11e9-8639-f5c8da0af998.png">

* Hadoop 환경 설정 파일 수정(2) - masters
###
    [ec2-user@namenode conf]$ vi masters
    [ec2-user@namenode conf]$ 
    vi 편집기 실행 
    namenode-internal
    snamenode-internal 
    vi 편집기 종료
<img width="838" alt="Screen Shot 2019-04-23 at 5 21 07 PM" src="https://user-images.githubusercontent.com/46523571/56565845-440e8400-65ec-11e9-86d5-5b5437833090.png">

* Hadoop 환경 설정 파일 수정(3) - slaves
###
    [ec2-user@namenode conf]$ vi slaves
    [ec2-user@namenode conf]$ 
    vi 편집기 실행 
    data01-internal
    data02-internal
    data03-internal
    vi 편집기 종료
<img width="836" alt="Screen Shot 2019-04-23 at 5 22 47 PM" src="https://user-images.githubusercontent.com/46523571/56565931-6f916e80-65ec-11e9-8d48-51abc6181291.png">

* Hadoop 환경 설정 파일 수정(4) - core-site.xml
###
    [ec2-user@namenode conf]$ vi core-site.xml
    [ec2-user@namenode conf]$ 
    vi 편집기 실행 
    <configuration>
        <property>
            <name>fs.default.name</name>
            <value>hdfs://namenode-internal:9000</value>
        </property>
        <property>
            <name>hadoop.tmp.dir</name>
            <value>/home/ec2-user/hadoop-tmp-dir/</value>
        </property>
    </configuration>
    vi 편집기 종료
    
    # 아래 디렉토리들은 하둡 실행시에 자동으로 생성된다.
    /home/ec2-user/hadoop-tmp-dir
<img width="837" alt="Screen Shot 2019-04-23 at 5 24 25 PM" src="https://user-images.githubusercontent.com/46523571/56566001-a9627500-65ec-11e9-95f6-612d68b3bc25.png">

* Hadoop 환경 설정 파일 수정(5) - hdfs-site.xml
###
    [ec2-user@namenode conf]$ vi hdfs-site.xml
    [ec2-user@namenode conf]$ 
    vi 편집기 실행 
    <configuration>
        <property>
            <name>dfs.replication</name>
            <value>3</value>
        </property>
        <property>
            <name>dfs.http.address</name>
            <value>namenode-internal:50070</value>
        </property>
        <property>
            <name>dfs.secondary.http.address</name>
            <value>snamenode-internal:50090</value>
        </property>
        <property>
            <name>dfs.name.dir</name>
            <value>/home/ec2-user/hadoop-tmp-dir/dfs/name</value>
        </property>
        <property>
            <name>dfs.name.edits.dir</name>
            <value>${dfs.name.dir}</value>
        </property>
        <property>
            <name>dfs.data.dir</name>
            <value>/home/ec2-user/hadoop-tmp-dir/dfs/data</value>
        </property>
    </configuration>
    vi 편집기 종료
    
    # 아래 디렉토리들은 하둡 실행시에 자동으로 생성된다.
    /home/ec2-user/hadoop-tmp-dir/dfs
    /home/ec2-user/hadoop-tmp-dir/dfs/name
    /home/ec2-user/hadoop-tmp-dir/dfs/data
<img width="837" alt="Screen Shot 2019-04-23 at 5 27 15 PM" src="https://user-images.githubusercontent.com/46523571/56566199-0eb66600-65ed-11e9-914c-0ea6b4c2ff0d.png">

* Hadoop 환경 설정 파일 수정(6) - mapred-site.xml
###
    [ec2-user@namenode conf]$ vi mapred-site.xml
    [ec2-user@namenode conf]$ 
    vi 편집기 실행
    <configuration>
        <property>
            <name>mapred.job.tracker</name>
            <value>namenode-internal:9001</value>
        </property>
    </configuration>
    vi 편집기 종료
<img width="835" alt="Screen Shot 2019-04-23 at 5 29 11 PM" src="https://user-images.githubusercontent.com/46523571/56566327-54732e80-65ed-11e9-9967-3f967d7f6bc5.png">

* 위와 같이 namenode에서 수정한 파일들을 다른 노드들에게도 scp 명령어로 복사해 준다.
###
    [ec2-user@namenode conf]$ scp hadoop-env.sh masters slaves core-site.xml hdfs-site.xml mapred-site.xml ec2-user@snamenode:/home/ec2-user/hadoop/conf
    hadoop-env.sh                                             100% 2554     3.6MB/s   00:00    
    masters                                                   100%   37    64.4KB/s   00:00    
    slaves                                                    100%   48    96.3KB/s   00:00    
    core-site.xml                                             100%  415   753.4KB/s   00:00    
    hdfs-site.xml                                             100%  877     1.5MB/s   00:00    
    mapred-site.xml                                           100%  294   484.2KB/s   00:00    
    
    [ec2-user@namenode conf]$ scp hadoop-env.sh masters slaves core-site.xml hdfs-site.xml mapred-site.xml ec2-user@data01:/home/ec2-user/hadoop/conf
    hadoop-env.sh                                             100% 2554     3.8MB/s   00:00    
    masters                                                   100%   37    61.9KB/s   00:00    
    slaves                                                    100%   48   103.4KB/s   00:00    
    core-site.xml                                             100%  415   812.4KB/s   00:00    
    hdfs-site.xml                                             100%  877     1.7MB/s   00:00    
    mapred-site.xml                                           100%  294   540.3KB/s   00:00    
    
    [ec2-user@namenode conf]$ scp hadoop-env.sh masters slaves core-site.xml hdfs-site.xml mapred-site.xml ec2-user@data02:/home/ec2-user/hadoop/conf
    hadoop-env.sh                                             100% 2554     3.3MB/s   00:00    
    masters                                                   100%   37    63.8KB/s   00:00    
    slaves                                                    100%   48    89.6KB/s   00:00    
    core-site.xml                                             100%  415   641.6KB/s   00:00    
    hdfs-site.xml                                             100%  877     1.5MB/s   00:00    
    mapred-site.xml                                           100%  294   556.6KB/s   00:00    
    
    [ec2-user@namenode conf]$ scp hadoop-env.sh masters slaves core-site.xml hdfs-site.xml mapred-site.xml ec2-user@data03:/home/ec2-user/hadoop/conf
    hadoop-env.sh                                             100% 2554     3.6MB/s   00:00    
    masters                                                   100%   37    66.9KB/s   00:00    
    slaves                                                    100%   48    91.6KB/s   00:00    
    core-site.xml                                             100%  415   812.1KB/s   00:00    
    hdfs-site.xml                                             100%  877     1.6MB/s   00:00    
    mapred-site.xml                                           100%  294   616.2KB/s   00:00    
    
    [ec2-user@namenode conf]$ 




# Hadoop Cluster의 HDFS 포맷
* Hadoop을 실행하기 전에 아래와 같이 namenode에서 HDFS(Hadoop Distributed File System)을 포맷해야 한다.
* 포맷은 Hadoop Cluster set-up 단계에서 한 번만 실행하며, 추후 필요 시 재포맷 할 수 있다.
###
    [ec2-user@namenode conf]$ hadoop namenode -format
    19/04/23 08:34:59 INFO namenode.NameNode: STARTUP_MSG: 
    /************************************************************
    STARTUP_MSG: Starting NameNode
    STARTUP_MSG:   host = namenode.localdomain/127.0.0.1
    STARTUP_MSG:   args = [-format]
    STARTUP_MSG:   version = 1.2.1
    STARTUP_MSG:   build = https://svn.apache.org/repos/asf/hadoop/common/branches/branch-1.2 -r 1503152; compiled by 'mattf' on Mon Jul 22 15:23:09 PDT 2013
    STARTUP_MSG:   java = 1.7.0_51
    ************************************************************/
    19/04/23 08:35:00 INFO util.GSet: Computing capacity for map BlocksMap
    19/04/23 08:35:00 INFO util.GSet: VM type       = 64-bit
    19/04/23 08:35:00 INFO util.GSet: 2.0% max memory = 1013645312
    19/04/23 08:35:00 INFO util.GSet: capacity      = 2^21 = 2097152 entries
    19/04/23 08:35:00 INFO util.GSet: recommended=2097152, actual=2097152
    19/04/23 08:35:00 INFO namenode.FSNamesystem: fsOwner=ec2-user
    19/04/23 08:35:00 INFO namenode.FSNamesystem: supergroup=supergroup
    19/04/23 08:35:00 INFO namenode.FSNamesystem: isPermissionEnabled=true
    19/04/23 08:35:00 INFO namenode.FSNamesystem: dfs.block.invalidate.limit=100
    19/04/23 08:35:00 INFO namenode.FSNamesystem: isAccessTokenEnabled=false accessKeyUpdateInterval=0 min(s), accessTokenLifetime=0 min(s)
    19/04/23 08:35:00 INFO namenode.FSEditLog: dfs.namenode.edits.toleration.length = 0
    19/04/23 08:35:00 INFO namenode.NameNode: Caching file names occuring more than 10 times 
    19/04/23 08:35:01 INFO common.Storage: Image file /home/ec2-user/hadoop-tmp-dir/dfs/name/current/fsimage of size 114 bytes saved in 0 seconds.
    19/04/23 08:35:01 INFO namenode.FSEditLog: closing edit log: position=4, editlog=/home/ec2-user/hadoop-tmp-dir/dfs/name/current/edits
    19/04/23 08:35:01 INFO namenode.FSEditLog: close success: truncate to 4, editlog=/home/ec2-user/hadoop-tmp-dir/dfs/name/current/edits
    19/04/23 08:35:01 INFO common.Storage: Storage directory /home/ec2-user/hadoop-tmp-dir/dfs/name has been successfully formatted.
    19/04/23 08:35:01 INFO namenode.NameNode: SHUTDOWN_MSG: 
    /************************************************************
    SHUTDOWN_MSG: Shutting down NameNode at namenode.localdomain/127.0.0.1
    ************************************************************/
    
    [ec2-user@namenode conf]$ 
    
* 추후 namenode 재포맷할 시
  - Re-format filesystem in /home/oracle/hadoop/hadoop/dfs/name ? (Y or N) <- 반드시 대문자 Y 를 사용!


# Hadoop Cluster의 시작과 정지
* Hadoop Cluster 시작은 두 단계로 이루어져 있다.
* (1) Hadoop Cluster start
  - 1-1. HDFS daemon들을 start
  - 마스터 노드에 NameNode 데몬 프로세스가 기동되고 다른 모든 슬레이브 노드에는 DataNod 데몬이 기동된다.
  - HDFS 데몬 프로세스들을 기동시키기 위해서 네임노드에서 start-dfs.sh 쉘 스크립트 명령어를 실행시킨다.
  - 1-2. MapReduce daemon 들을 start
  - 마스터 노드(여기서는 namenode)에 JobTracker 데몬 프로세스가 기동되고 다른 모든 슬레이브 노드에는 TraskTracker 데몬 프로세스가 기동된다.
  - MapReduce 데몬 프로세스들을 기동시키기 위해서 네임노드(또는 JobTracker 를 기동시키기 원하는 노드)에서 start-mapred.sh 쉘 스크립트 명령어를 실행시킨다.
  - start-mapred.sh 를 실행시킨 노드에 JobTracker 데몬프로세스가 start 되고, slaves 파일에 기록된 노드에 TaskTraker 들이 start 되어 MapReduce Cluser 가 기동된다.
  - 1-3. HDFS 와 MapReduce 를 함께 start
  - HDFS 와 MapReduce 를 모두 start 시키기 위해서는 start-all.sh 쉘 스크립트 명령어를 실행시킨다.
* (2) Hadoop Cluster stop
  - 하둡 클러스터를 stop 하는 것도 두 단계로 이루어 진다. 그러나 stop 시키는 것은 start 하는 것과 반대의 순서로 한다.
  - 2-1. MapReduce 데몬 프로세스들을 stop
  - JobTracker 가 실행되는 노드(여기서는 namenode)에서 stop-mapred.sh 명령어를 실행한다.
  - stop-mapred.sh 를 실행한 노드에서 실행중인 JobTracker 데몬과 slave 파일에 기술된 노드들에서 실행중인 TaskTracker 데몬들을 stop 시키고 MapReduce 클러스터를 정지한다.
  - Master 노드(여기서는 namenode)에 JobTracker 데몬 프로세스가 stop 되고, 다른 모든 Slave 노드에 TraskTracker 데몬 프로세스가 stop 된다.
  - 2-2. HDFS 데몬 프로세스들을 stop
  - NameNode 데몬이 실행중인 namenode 에서 stop-dfs.sh 쉘 스크립트 명령어를 실행한다.
  - stop-dfs.sh 를 실행한 노드에서 실행중인 NameNode 데몬가 stop 되고, 다른 모든 Slave 노드에 DataNode 데몬 프로세스들이 stop 된다.
  - master 노드인 namenode 에 NameNode 데몬 프로세스가 stop 되고 다른 모든 slave 노드에 DataNode

* (3) 하둡 클러스터의 시작과 정지 예
  - jps 명령어로 하둡 클러스터가 실행 중인지 확인한다.
  $ jps
  - 하둡 클러스터 데몬 프로세스들이 실행중이 아니면 다음 명령어로 하둡 클러스터를 start 시킨다. - HDFS 와 MapRed 를 각각 start 시키고 stop 시켜 본다.


* HDFS & 맵리듀스 모두 시작
###
    # hdfs & mapred 시작
    [ec2-user@namenode conf]$ start-all.sh
    starting namenode, logging to /home/ec2-user/hadoop/logs/hadoop-ec2-user-namenode-namenode.localdomain.out
    data02-internal: starting datanode, logging to /home/ec2-user/hadoop/logs/hadoop-ec2-user-datanode-data02.localdomain.out
    data03-internal: starting datanode, logging to /home/ec2-user/hadoop/logs/hadoop-ec2-user-datanode-data03.localdomain.out
    data01-internal: starting datanode, logging to /home/ec2-user/hadoop/logs/hadoop-ec2-user-datanode-data01.localdomain.out
    namenode-internal: starting secondarynamenode, logging to /home/ec2-user/hadoop/logs/hadoop-ec2-user-secondarynamenode-namenode.localdomain.out
    snamenode-internal: secondarynamenode running as process 2733. Stop it first.
    jobtracker running as process 3042. Stop it first.
    data03-internal: tasktracker running as process 2800. Stop it first.
    data02-internal: tasktracker running as process 2826. Stop it first.
    data01-internal: tasktracker running as process 2820. Stop it first.
###
    # namenode jps 확인
    [ec2-user@namenode conf]$ jps
    3689 NameNode
    3989 Jps
    3042 JobTracker
    
    # snamenode jps 확인
    [ec2-user@snamenode ~]$ jps
    2733 SecondaryNameNode
    3071 Jps
    
    # data01 jps 확인 
    [ec2-user@data01 ~]$ jps
    2820 TaskTracker
    3181 DataNode
    3304 Jps
    
    # data02 jps 확인 
    [ec2-user@data02 ~]$ jps
    3118 DataNode
    3239 Jps
    2826 TaskTracker
    
    # data03 jps 확인 
    [ec2-user@data03 ~]$ jps
    3069 DataNode
    3190 Jps
    2800 TaskTracker
    [ec2-user@data03 ~]$ 
    
    # hdfs & mapred 종료
    [ec2-user@namenode conf]$ stop-all.sh
    stopping jobtracker
    data02-internal: stopping tasktracker
    data03-internal: stopping tasktracker
    data01-internal: stopping tasktracker
    stopping namenode
    data03-internal: stopping datanode
    data02-internal: stopping datanode
    data01-internal: stopping datanode
    namenode-internal: no secondarynamenode to stop
    snamenode-internal: stopping secondarynamenode
    [ec2-user@namenode conf]$ 





# 6. Hadoop 설치 확인
    [ec2-user@namenode conf]$ hadoop dfsadmin -report
    Safe mode is ON
    Configured Capacity: 24956350464 (23.24 GB)
    Present Capacity: 19177766912 (17.86 GB)
    DFS Remaining: 19177652224 (17.86 GB)
    DFS Used: 114688 (112 KB)
    DFS Used%: 0%
    Under replicated blocks: 0
    Blocks with corrupt replicas: 0
    Missing blocks: 0
    
    -------------------------------------------------
    Datanodes available: 3 (3 total, 0 dead)
    
    Name: 172.31.28.203:50010
    Decommission Status : Normal
    Configured Capacity: 8318783488 (7.75 GB)
    DFS Used: 36864 (36 KB)
    Non DFS Used: 1919713280 (1.79 GB)
    DFS Remaining: 6399033344(5.96 GB)
    DFS Used%: 0%
    DFS Remaining%: 76.92%
    Last contact: Tue Apr 23 11:02:19 UTC 2019
    
    
    Name: 172.31.30.98:50010
    Decommission Status : Normal
    Configured Capacity: 8318783488 (7.75 GB)
    DFS Used: 40960 (40 KB)
    Non DFS Used: 1920548864 (1.79 GB)
    DFS Remaining: 6398193664(5.96 GB)
    DFS Used%: 0%
    DFS Remaining%: 76.91%
    Last contact: Tue Apr 23 11:02:19 UTC 2019
    
    
    Name: 172.31.17.175:50010
    Decommission Status : Normal
    Configured Capacity: 8318783488 (7.75 GB)
    DFS Used: 36864 (36 KB)
    Non DFS Used: 1938321408 (1.81 GB)
    DFS Remaining: 6380425216(5.94 GB)
    DFS Used%: 0%
    DFS Remaining%: 76.7%
    Last contact: Tue Apr 23 11:02:19 UTC 2019
    
    
    [ec2-user@namenode conf]$ 
<img width="637" alt="Screen Shot 2019-04-23 at 8 02 55 PM" src="https://user-images.githubusercontent.com/46523571/56576250-d1f56980-6602-11e9-8de2-e2772b483652.png">





## 확인 2
* 크롬 - http://namenode:50070 접속
* HDFS 는 namenode 의 50070 포트에서 모니터링할 수 있다.
![Screen Shot 2019-04-23 at 8 05 45 PM](https://user-images.githubusercontent.com/46523571/56576392-331d3d00-6603-11e9-88cf-d87037bf61f8.png)


* MapReduce namenode의 50030 포트에서 모니터링할 수 있다.
![Screen Shot 2019-04-23 at 8 07 18 PM](https://user-images.githubusercontent.com/46523571/56576464-6b248000-6603-11e9-9045-f94f70b51fd3.png)


<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>