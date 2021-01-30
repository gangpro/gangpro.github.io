---
layout: post
title: '[AWS] 09. Hadoop 설치 확인'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 09:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Hadoop 설치 확인<br />


# Hadoop 설치 확인
> Install Hadoop on AWS <br>

> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> AWS : Amazon Web Services with EC2(Amazon Linux AMI)<br>
> JDK : jdk-7u51-linux-x64<br>
> Hadoop : Hadoop 1.2.1<br>

## Hadoop 설치 확인(1)
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

<br>
<br>
<br>

## Hadoop 설치 확인(2)
* 크롬 접속
* HDFS 는 namenode의 50070 포트에서 모니터링할 수 있다.
![Screen Shot 2019-04-23 at 8 05 45 PM](https://user-images.githubusercontent.com/46523571/56576392-331d3d00-6603-11e9-88cf-d87037bf61f8.png)

* MapReduce namenode의 50030 포트에서 모니터링할 수 있다.
![Screen Shot 2019-04-23 at 8 07 18 PM](https://user-images.githubusercontent.com/46523571/56576464-6b248000-6603-11e9-9045-f94f70b51fd3.png)

<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>