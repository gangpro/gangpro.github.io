---
layout: post
title: '[TIL] 하둡(hadoop) 에코시스템 - hive'
subtitle: 
categories: til
tags: til env hadoop
comments: true
date: 2019-04-24 12:12:17 +0900
lastmod: 2019-04-24 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

하둡(hadoop) 에코시스템 hive<br />

# HIVE
> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> VM : Enterprise Linux<br>
> JDK : jdk1.8.0_211<br>
> Hadoop 1.2.1<br>
> hive 1.2.1<br>

## [0] data 준비 : ml-1m 데이터 준비
    [orcl:~]$ su - hadoop
    Password: 
    [hadoop@edydr1p0 ~]$ 
    
    # 데이터 파일 다운로드
    [hadoop@edydr1p0 ~]$ wget http://www.grouplens.org/system/files/ml-1m.zip --no-check-certificate
    --2019-04-24 13:47:59--  http://www.grouplens.org/system/files/ml-1m.zip
    Resolving www.grouplens.org... 128.101.34.235
    Connecting to www.grouplens.org|128.101.34.235|:80... connected.
    HTTP request sent, awaiting response... 301 Moved Permanently
    Location: https://grouplens.org/system/files/ml-1m.zip [following]
    --2019-04-24 13:48:00--  https://grouplens.org/system/files/ml-1m.zip
    Resolving grouplens.org... 128.101.34.235
    Connecting to grouplens.org|128.101.34.235|:443... connected.
    WARNING: cannot verify grouplens.org's certificate, issued by `/C=US/O=Let's Encrypt/CN=Let's Encrypt Authority X3':
      Unable to locally verify the issuer's authority.
    HTTP request sent, awaiting response... 301 Moved Permanently
    Location: http://files.grouplens.org/papers/ml-1m.zip [following]
    --2019-04-24 13:48:01--  http://files.grouplens.org/papers/ml-1m.zip
    Resolving files.grouplens.org... 128.101.34.235
    Connecting to files.grouplens.org|128.101.34.235|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 5917549 (5.6M) [application/zip]
    Saving to: `ml-1m.zip'
    
    100%[======================================>] 5,917,549   10.8K/s   in 9m 22s  
    
    2019-04-24 13:57:24 (10.3 KB/s) - `ml-1m.zip' saved [5917549/5917549]
    
    [hadoop@edydr1p0 ~]$ 
###    
    # 데이터 파일 압축 해제
    [hadoop@edydr1p0 ~]$ unzip ml-1m.zip
    Archive:  ml-1m.zip
       creating: ml-1m/
      inflating: ml-1m/movies.dat        
      inflating: ml-1m/ratings.dat       
      inflating: ml-1m/README            
      inflating: ml-1m/users.dat         
    [hadoop@edydr1p0 ~]$ 
###
    [hadoop@edydr1p0 ~]$ cd ml-1m
    [hadoop@edydr1p0 ml-1m]$ ls -al
    total 24372
    drwxr-x--- 2 hadoop hadoop     4096 Jan 30  2016 .
    drwx------ 6 hadoop hadoop     4096 Apr 24 13:58 ..
    -rw-r----- 1 hadoop hadoop   171308 Mar 27  2003 movies.dat
    -rw-r----- 1 hadoop hadoop 24594131 Mar  1  2003 ratings.dat
    -rw-r----- 1 hadoop hadoop     5577 Jan 30  2016 README
    -rw-r----- 1 hadoop hadoop   134368 Mar  1  2003 users.dat
    [hadoop@edydr1p0 ml-1m]$ 
    
    # 데이터 구분이 ::로 되어 있는걸 ,로 변경
    [hadoop@edydr1p0 ml-1m]$ sed s/::/,/g movies.dat > movies2.dat      
    [hadoop@edydr1p0 ml-1m]$ sed s/::/,/g ratings.dat > ratings2.dat 
    [hadoop@edydr1p0 ml-1m]$ sed s/::/,/g users.dat > users2.dat  
    [hadoop@edydr1p0 ml-1m]$ ls
    movies2.dat  ratings2.dat  README      users.dat
    movies.dat   ratings.dat   users2.dat
    
    # 방대한 데이터 확인 할 수 있음
    [hadoop@edydr1p0 ml-1m]$ more users2.dat
    
    [hadoop@edydr1p0 ml-1m]$ cd
    
    [hadoop@edydr1p0 ~]$ ls ml-1m
    movies2.dat  movies.dat  ratings2.dat  ratings.dat  README  users2.dat  users.dat

<br>
<br>
<br>

## [1] 적절한 버전 확인
    http://hive.apache.org/downloads.html에서 적절한 버전을 확인하세요.

<br>
<br>
<br>

## [2] 설치 및 간단한 테스트
    [hadoop@edydr1p0 ~]$ start-all.sh  
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out
       
    [hadoop@edydr1p0 ~]$ jps
    24034 NameNode
    24662 Jps
    24166 DataNode
    24312 SecondaryNameNode
    24570 TaskTracker
    24431 JobTracker
    
    [hadoop@edydr1p0 ~]$ cd
    [hadoop@edydr1p0 ~]$ mkdir hive-install             <- 디렉토리 생성
    [hadoop@edydr1p0 hive-install]$ cd hive-install
###    
    # hive 다운로드
    [hadoop@edydr1p0 hive-install]$ wget https://archive.apache.org/dist/hive/hive-1.2.1/apache-hive-1.2.1-bin.tar.gz
    --2019-04-24 14:03:22--  https://archive.apache.org/dist/hive/hive-1.2.1/apache-hive-1.2.1-bin.tar.gz
    Resolving archive.apache.org... 163.172.17.199
    Connecting to archive.apache.org|163.172.17.199|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 92834839 (89M) [application/x-gzip]
    Saving to: `apache-hive-1.2.1-bin.tar.gz'
    
    100%[======================================>] 92,834,839  6.01M/s   in 23s     
    
    2019-04-24 14:03:46 (3.79 MB/s) - `apache-hive-1.2.1-bin.tar.gz' saved [92834839/92834839]
###    
    # hive 압축 해제 
    [hadoop@edydr1p0 hive-install]$ tar xvfz apache-hive-1.2.1-bin.tar.gz
    
    [hadoop@edydr1p0 hive-install]$ cd apache-hive-1.2.1-bin
    [hadoop@edydr1p0 apache-hive-1.2.1-bin]$ cd conf
    
    [hadoop@edydr1p0 conf]$ cp hive-env.sh.template hive-env.sh
    
    [hadoop@edydr1p0 conf]$ echo $HADOOP_HOME
    /opt/hadoop/hadoop
    
    [hadoop@edydr1p0 conf]$ vi hive-env.sh
    
    # vi 편집기 실행        
    HADOOP_HOME=/opt/hadoop/hadoop
    # vi 편집기 종료
![Screen Shot 2019-04-24 at 2 06 18 PM](https://user-images.githubusercontent.com/46523571/56633694-25ff5d00-669a-11e9-82cc-ffd0a8243605.png)
    
    [hadoop@edydr1p0 conf]$ cd
    
    [hadoop@edydr1p0 ~]$ vi + .bash_profile
    
    export HIVE_HOME=/home/hadoop/hive-install/apache-hive-1.2.1-bin
    export PATH=$HIVE_HOME/bin:$PATH
    
    [hadoop@edydr1p0 ~]$ source .bash_profile
![Screen Shot 2019-04-24 at 2 08 07 PM](https://user-images.githubusercontent.com/46523571/56633756-6e1e7f80-669a-11e9-8314-88053c315b15.png)
    
    [hadoop@edydr1p0 ~]$ hive           <- 에러 발생
    Caused by: java.lang.RuntimeException: The root scratch dir: 
    /tmp/hive on HDFS should be writable. Current permissions are: rwx--x--x
    
    [hadoop@edydr1p0 ~]$ hadoop fs -chmod 777 /tmp/hive   <- 권한 조정
    
    [hadoop@edydr1p0 ~]$ hive
    Logging initialized using configuration in jar:file:/home/hadoop/hive-install/apache-hive-1.2.1-bin/lib/hive-common-1.2.1.jar!/hive-log4j.properties
    hive> 
###
    hive> show tables; 
    OK
    Time taken: 1.009 seconds
    
    hive> DROP TABLE rating;
    OK
    Time taken: 0.126 seconds
    
    hive> CREATE TABLE rating
         (userid INT, 
          movieid INT, 
          rating FLOAT,
          ds STRING)
          ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
          STORED AS TEXTFILE;
    OK
    Time taken: 0.428 seconds  
      
    hive> show tables; 
    OK
    rating
    Time taken: 0.034 seconds, Fetched: 1 row(s)
    
    hive> describe rating; 
    OK
    userid                  int                                         
    movieid                 int                                         
    rating                  float                                       
    ds                      string                                      
    Time taken: 1.411 seconds, Fetched: 4 row(s)
    
    hive> LOAD DATA LOCAL INPATH '/home/hadoop/ml-1m/ratings2.dat' OVERWRITE INTO TABLE rating;
    Loading data to table default.rating
    Table default.rating stats: [numFiles=1, numRows=0, totalSize=21593504, rawDataSize=0]
    OK
    Time taken: 2.987 seconds

    hive> select * from rating limit 10;
    OK
    1       1193    5.0     978300760
    1       661     3.0     978302109
    1       914     3.0     978301968
    1       3408    4.0     978300275
    1       2355    5.0     978824291
    1       1197    3.0     978302268
    1       1287    5.0     978302039
    1       2804    5.0     978300719
    1       594     4.0     978302268
    1       919     4.0     978301368
    Time taken: 0.312 seconds, Fetched: 10 row(s)
       
    hive> select count(*) 
          from rating r;
    Query ID = hadoop_20190424141223_79603caa-6e71-4849-8f3c-75233292d7e0
    Total jobs = 1
    Launching Job 1 out of 1
    Number of reduce tasks determined at compile time: 1
    In order to change the average load for a reducer (in bytes):
      set hive.exec.reducers.bytes.per.reducer=<number>
    In order to limit the maximum number of reducers:
      set hive.exec.reducers.max=<number>
    In order to set a constant number of reducers:
      set mapred.reduce.tasks=<number>
    Starting Job = job_201904241402_0001, Tracking URL = http://192.168.56.101:50030/jobdetails.jsp?jobid=job_201904241402_0001
    Kill Command = /opt/hadoop/hadoop-1.2.1/libexec/../bin/hadoop job  -kill job_201904241402_0001
    Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
    2019-04-24 14:12:29,876 Stage-1 map = 0%,  reduce = 0%
    2019-04-24 14:12:35,153 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.78 sec
    2019-04-24 14:12:43,241 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 3.77 sec
    MapReduce Total cumulative CPU time: 3 seconds 770 msec
    Ended Job = job_201904241402_0001
    MapReduce Jobs Launched: 
    Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 3.77 sec   HDFS Read: 21600246 HDFS Write: 8 SUCCESS
    Total MapReduce CPU Time Spent: 3 seconds 770 msec
    OK
    1000209
    Time taken: 21.76 seconds, Fetched: 1 row(s)
    
    hive> select * from rating r where r.userid = '2';
    OK
    2       1357    5.0     978298709
    2       3068    4.0     978299000
    2       1537    4.0     978299620
    2       647     3.0     978299351
    2       2194    4.0     978299297
    2       648     4.0     978299913
    2       2268    5.0     978299297
    2       2628    3.0     978300051
    2       1103    3.0     978298905
    2       2916    3.0     978299809
    2       3468    5.0     978298542
    2       1210    4.0     978298151
    2       1792    3.0     978299941
    2       1687    3.0     978300174
    2       1213    2.0     978298458
    2       3578    5.0     978298958
    2       2881    3.0     978300002
    2       3030    4.0     978298434
    2       1217    3.0     978298151
    2       3105    4.0     978298673
    2       434     2.0     978300174
    2       2126    3.0     978300123
    2       3107    2.0     978300002
    2       3108    3.0     978299712
    2       3035    4.0     978298625
    2       1253    3.0     978299120
    2       1610    5.0     978299809
    2       292     3.0     978300123
    2       2236    5.0     978299220
    2       3071    4.0     978299120
    2       902     2.0     978298905
    2       368     4.0     978300002
    2       1259    5.0     978298841
    2       3147    5.0     978298652
    2       1544    4.0     978300174
    2       1293    5.0     978298261
    2       1188    4.0     978299620
    2       3255    4.0     978299321
    2       3256    2.0     978299839
    2       3257    3.0     978300073
    2       110     5.0     978298625
    2       2278    3.0     978299889
    2       2490    3.0     978299966
    2       1834    4.0     978298813
    2       3471    5.0     978298814
    2       589     4.0     978299773
    2       1690    3.0     978300051
    2       3654    3.0     978298814
    2       2852    3.0     978298958
    2       1945    5.0     978298458
    2       982     4.0     978299269
    2       1873    4.0     978298542
    2       2858    4.0     978298434
    2       1225    5.0     978298391
    2       2028    4.0     978299773
    2       515     5.0     978298542
    2       442     3.0     978300025
    2       2312    3.0     978299046
    2       265     4.0     978299026
    2       1408    3.0     978299839
    2       1084    3.0     978298813
    2       3699    2.0     978299173
    2       480     5.0     978299809
    2       1442    4.0     978299297
    2       2067    5.0     978298625
    2       1265    3.0     978299712
    2       1370    5.0     978299889
    2       1193    5.0     978298413
    2       1801    3.0     978300002
    2       1372    3.0     978299941
    2       2353    4.0     978299861
    2       3334    4.0     978298958
    2       2427    2.0     978299913
    2       590     5.0     978299083
    2       1196    5.0     978298730
    2       1552    3.0     978299941
    2       736     4.0     978300100
    2       1198    4.0     978298124
    2       593     5.0     978298517
    2       2359    3.0     978299666
    2       95      2.0     978300143
    2       2717    3.0     978298196
    2       2571    4.0     978299773
    2       1917    3.0     978300174
    2       2396    4.0     978299641
    2       3735    3.0     978298814
    2       1953    4.0     978298775
    2       1597    3.0     978300025
    2       3809    3.0     978299712
    2       1954    5.0     978298841
    2       1955    4.0     978299200
    2       235     3.0     978299351
    2       1124    5.0     978299418
    2       1957    5.0     978298750
    2       163     4.0     978299809
    2       21      1.0     978299839
    2       165     3.0     978300002
    2       2321    3.0     978299666
    2       1090    2.0     978298580
    2       380     5.0     978299809
    2       2501    5.0     978298600
    2       349     4.0     978299839
    2       457     4.0     978299773
    2       1096    4.0     978299386
    2       920     5.0     978298775
    2       459     3.0     978300002
    2       1527    4.0     978299839
    2       3418    4.0     978299809
    2       1385    3.0     978299966
    2       3451    4.0     978298924
    2       3095    4.0     978298517
    2       780     3.0     978299966
    2       498     3.0     978299418
    2       2728    3.0     978298881
    2       2002    5.0     978300100
    2       1962    5.0     978298813
    2       1784    5.0     978298841
    2       2943    4.0     978298372
    2       2006    3.0     978299861
    2       318     5.0     978298413
    2       1207    4.0     978298478
    2       1968    2.0     978298881
    2       3678    3.0     978299250
    2       1244    3.0     978299143
    2       356     5.0     978299686
    2       1245    2.0     978299200
    2       1246    5.0     978299418
    2       3893    1.0     978299535
    2       1247    5.0     978298652
    Time taken: 0.152 seconds, Fetched: 129 row(s)
    
    hive> exit;
    
    [hadoop@edydr1p0 ~]$ hadoop fs -lsr /user/hive/warehouse
    drwxr-xr-x   - hadoop supergroup          0 2019-04-24 14:11 /user/hive/warehouse/rating
    -rw-r--r--   1 hadoop supergroup   21593504 2019-04-24 14:11 /user/hive/warehouse/rating/ratings2.dat

    [hadoop@edydr1p0 ~]$ hadoop fs -cat /user/hive/warehouse/rating/ratings*
    
    [hadoop@edydr1p0 ~]$ vi a.sql  
    # vi 편집기 실행  
    select count(*) from rating r;
    # vi 편집기 종료
![Screen Shot 2019-04-24 at 2 15 25 PM](https://user-images.githubusercontent.com/46523571/56634041-6dd2b400-669b-11e9-8a69-ccbb33492c47.png)
###
    [hadoop@edydr1p0 ~]$ hive -f a.sql    <- 스크립트 파일 수행 방법 1
    Logging initialized using configuration in jar:file:/home/hadoop/hive-install/apache-hive-1.2.1-bin/lib/hive-common-1.2.1.jar!/hive-log4j.properties
    Query ID = hadoop_20190424141545_2fc4be17-2b40-4839-a6a5-24c10e526431
    Total jobs = 1
    Launching Job 1 out of 1
    Number of reduce tasks determined at compile time: 1
    In order to change the average load for a reducer (in bytes):
      set hive.exec.reducers.bytes.per.reducer=<number>
    In order to limit the maximum number of reducers:
      set hive.exec.reducers.max=<number>
    In order to set a constant number of reducers:
      set mapred.reduce.tasks=<number>
    Starting Job = job_201904241402_0002, Tracking URL = http://192.168.56.101:50030/jobdetails.jsp?jobid=job_201904241402_0002
    Kill Command = /opt/hadoop/hadoop-1.2.1/libexec/../bin/hadoop job  -kill job_201904241402_0002
    Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
    2019-04-24 14:15:52,794 Stage-1 map = 0%,  reduce = 0%
    2019-04-24 14:15:58,046 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 2.88 sec
    2019-04-24 14:16:06,124 Stage-1 map = 100%,  reduce = 33%, Cumulative CPU 3.8 sec
    2019-04-24 14:16:07,135 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 4.87 sec
    MapReduce Total cumulative CPU time: 4 seconds 870 msec
    Ended Job = job_201904241402_0002
    MapReduce Jobs Launched: 
    Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 4.87 sec   HDFS Read: 21600246 HDFS Write: 8 SUCCESS
    Total MapReduce CPU Time Spent: 4 seconds 870 msec
    OK
    1000209
    Time taken: 23.109 seconds, Fetched: 1 row(s)
    
    [hadoop@edydr1p0 ~]$ hive
    Logging initialized using configuration in jar:file:/home/hadoop/hive-install/apache-hive-1.2.1-bin/lib/hive-common-1.2.1.jar!/hive-log4j.properties
    hive> 
      
    hive> ! ls -l;
    total 5836
    -rw-rw-r-- 1 hadoop hadoop      31 Apr 24 14:14 a.sql
    -rw-rw-r-- 1 hadoop hadoop   21058 Apr 24 14:16 derby.log
    drwxrwxr-x 2 hadoop hadoop    4096 Apr 19 15:29 download
    drwxrwxr-x 3 hadoop hadoop    4096 Apr 24 14:04 hive-install
    drwxrwxr-x 5 hadoop hadoop    4096 Apr 24 14:16 metastore_db
    drwxr-x--- 2 hadoop hadoop    4096 Apr 24 13:59 ml-1m
    -rw-rw-r-- 1 hadoop hadoop 5917549 Jan 30  2016 ml-1m.zip
###
    hive> source a.sql;                   <- 스크립트 파일 수행 방법 2
    Query ID = hadoop_20190424141706_5a5067eb-ccfb-41b6-9114-6474090a5c80
    Total jobs = 1
    Launching Job 1 out of 1
    Number of reduce tasks determined at compile time: 1
    In order to change the average load for a reducer (in bytes):
      set hive.exec.reducers.bytes.per.reducer=<number>
    In order to limit the maximum number of reducers:
      set hive.exec.reducers.max=<number>
    In order to set a constant number of reducers:
      set mapred.reduce.tasks=<number>
    Starting Job = job_201904241402_0003, Tracking URL = http://192.168.56.101:50030/jobdetails.jsp?jobid=job_201904241402_0003
    Kill Command = /opt/hadoop/hadoop-1.2.1/libexec/../bin/hadoop job  -kill job_201904241402_0003
    Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
    2019-04-24 14:17:15,295 Stage-1 map = 0%,  reduce = 0%
    2019-04-24 14:17:18,320 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.66 sec
    2019-04-24 14:17:26,398 Stage-1 map = 100%,  reduce = 33%, Cumulative CPU 1.66 sec
    2019-04-24 14:17:27,426 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 4.01 sec
    MapReduce Total cumulative CPU time: 4 seconds 10 msec
    Ended Job = job_201904241402_0003
    MapReduce Jobs Launched: 
    Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 4.01 sec   HDFS Read: 21600246 HDFS Write: 8 SUCCESS
    Total MapReduce CPU Time Spent: 4 seconds 10 msec
    OK
    1000209
    Time taken: 23.631 seconds, Fetched: 1 row(s)

    hive> exit;

<br>
<br>
<br>

## 예제 하나 더
* https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/EMRforDynamoDB.Tutorial.LoadDataIntoHDFS.html

<br>
<br>
<br>

## Documentation
* Getting Started  : https://cwiki.apache.org/confluence/display/Hive/GettingStarted
* Hive Tutorial    : https://cwiki.apache.org/confluence/display/Hive/Tutorial

## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>