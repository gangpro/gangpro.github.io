---
layout: post
title: '[AWS] 08. Hadoop Cluster 시작과 종료'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 08:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Hadoop Cluster 시작과 종료<br />
# Hadoop Cluster 시작 및 종료
> Install Hadoop on AWS <br>

## Hadoop Cluster의 시작과 정지
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

## HDFS & 맵리듀스 모두 시작
```
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
```

```
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
```

# 6. Hadoop 설치 확인
```
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
```
<img width="637" alt="Screen Shot 2019-04-23 at 8 02 55 PM" src="https://user-images.githubusercontent.com/46523571/56576250-d1f56980-6602-11e9-8de2-e2772b483652.png">

## 확인 2
* 크롬 - http://namenode:50070 접속
* HDFS 는 namenode 의 50070 포트에서 모니터링할 수 있다.
![Screen Shot 2019-04-23 at 8 05 45 PM](https://user-images.githubusercontent.com/46523571/56576392-331d3d00-6603-11e9-88cf-d87037bf61f8.png)

* MapReduce namenode의 50030 포트에서 모니터링할 수 있다.
![Screen Shot 2019-04-23 at 8 07 18 PM](https://user-images.githubusercontent.com/46523571/56576464-6b248000-6603-11e9-9045-f94f70b51fd3.png)

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>