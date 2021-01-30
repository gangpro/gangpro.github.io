---
layout: post
title: '[AWS] 07. Hadoop 다운로드 및 설치, 환경 설정'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 07:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Hadoop 다운로드 및 설치, 환경 설정<br />

# Hadoop 다운로드 및 설치, 환경 설정
> Install Hadoop on AWS <br>

> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> AWS : Amazon Web Services with EC2(Amazon Linux AMI)<br>
> JDK : jdk-7u51-linux-x64<br>
> Hadoop : Hadoop 1.2.1<br>

## 하둡 다운로드 및 설치
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
    
## 하둡 설치파일 압축 해제
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

## namenode에서 다른 노드(snamenode, data01, data02, data03)들로 hadoop-1.2.1.tar.gz을 scp 명령어로 복사한다.
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

##
###
각각의 snamenode, data01, data02, data03 노드에 로그인하여 아래와 같이 hadoop-1.2.1.tar.gz 파일의 압축을 풀어주고, 위 과정처럼 환경변수를 확인한다.
###
    $ cd
    $ tar xvfz hadoop-1.2.1.tar.gz 
    $ ln -s hadoop-1.2.1 hadoop 
    $ vi .bash_profile
    $ source .bash_profile

## Hadoop 환경 설정 파일 수정(1) - hadoop-env.sh
###
    [ec2-user@namenode ~]$ cd $HADOOP_HOME/conf
    [ec2-user@namenode conf]$ vi hadoop-env.sh
    vi 편집기 실행 
    export JAVA_HOME=/usr/java/jdk1.7.0_51/jre
    export HADOOP_HOME=/home/ec2-user/hadoop
    export HADOOP_HOME_WARN_SUPPRESS=1
    vi 편집기 종료
<img width="837" alt="Screen Shot 2019-04-23 at 5 18 21 PM" src="https://user-images.githubusercontent.com/46523571/56565625-d06c7700-65eb-11e9-8639-f5c8da0af998.png">

## Hadoop 환경 설정 파일 수정(2) - masters
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

<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>