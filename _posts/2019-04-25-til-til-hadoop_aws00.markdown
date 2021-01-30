---
layout: post
title: '[AWS] 00. AWS에 하둡(hadoop) 개발환경구축 목차'
subtitle: 
categories: til
tags: til aws hadoop
comments: true
date: 2019-04-25 00:12:17 +0900
lastmod: 2019-04-25 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

AWS를 이용하여 하둡(hadoop) 개발환경구축<br />

# AWS에 Hadoop 설치
> Install Hadoop using AWS <br>
> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> AWS : Amazon Web Services with EC2(Amazon Linux AMI)<br>
> JDK : jdk-7u51-linux-x64<br>
> Hadoop : Hadoop 1.2.1<br>


## 목차
01. AWS EC2 인스턴스 생성
02. AWS 탄력적 IP(고정 IP) 설정
03. Mac 터미널을 통해 AWS 리눅스 접속
04. AWS EC2 인스턴스에 Oracle jdk 다운로드 및 설치
05. SSH 공개키 생성, 복사, 접속 확인
06. AWS EC2 인스턴스 방화벽 설정
07. Hadoop 다운로드 및 설치, 환경 설정 
08. Hadoop Cluster 시작과 정지 
09. Hadoop 설치 확인

<br>
<br>
<br>

## 참고
* 하둡(혹은 빅데이터)과 클라우드
  - AWS에서 빅데이터 프로젝트 시작하기 : https://www.slideshare.net/awskorea/launching-your-big-data-project-on-aws-aws
  - Amazon EMR : https://docs.aws.amazon.com/ko_kr/emr/latest/ManagementGuide/emr-gs.html

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



## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>