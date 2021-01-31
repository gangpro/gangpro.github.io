---
layout: post
title: '[hadoop] 하둡(hadoop) 에코시스템 - pig'
subtitle: 
categories: til
tags: til env hadoop
comments: true
date: 2019-04-24 13:12:17 +0900
lastmod: 2019-04-24 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

하둡(hadoop) 에코시스템 pig<br />

# PIG
> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> VM : Enterprise Linux<br>
> JDK : jdk1.8.0_211<br>
> Hadoop 1.2.1<br>
> hive 1.2.1<br>
> pig 0.12.0<br>

## [0] intro
* Pig Philosophy : https://stayrelevant.globant.com/en/pig/
* Pig Latin Statemnts : https://pig.apache.org/docs/latest/start.html#pl-statements

<br>
<br>
<br>

## [1] Stable 버전 확인
* http://pig.apache.org/releases.html#Download 에서 적절한 버전을 확인하세요.

<br>
<br>
<br>

## [2] putty로 접속해서 설치 및 간단한 테스트
* (1) 설치
  - release 0.12.0 : This release works with Hadoop 0.20.X, 1.X, 0.23.X and 2.X
### 
    [hadoop@edydr1p0 ~]$ whoami
    hadoop
    
    [hadoop@edydr1p0 ~]$ start-all.sh
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out
     
    [hadoop@edydr1p0 ~]$ jps
    27634 JobTracker
    27523 SecondaryNameNode
    27867 Jps
    27772 TaskTracker
    27374 DataNode
    27231 NameNode
    
    [hadoop@edydr1p0 ~]$ cd
    [hadoop@edydr1p0 ~]$ mkdir pig-install      <- 디렉토리 생성
    [hadoop@edydr1p0 ~]$ cd pig-install
###
    # pig 다운로드 
    [hadoop@edydr1p0 pig-install]$ wget https://archive.apache.org/dist/pig/pig-0.12.0/pig-0.12.0.tar.gz
    --2019-04-24 14:44:26--  https://archive.apache.org/dist/pig/pig-0.12.0/pig-0.12.0.tar.gz
    Resolving archive.apache.org... 163.172.17.199
    Connecting to archive.apache.org|163.172.17.199|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 59941262 (57M) [application/x-gzip]
    Saving to: `pig-0.12.0.tar.gz'
    
    100%[======================================>] 59,941,262  2.96M/s   in 23s     
    
    2019-04-24 14:44:51 (2.51 MB/s) - `pig-0.12.0.tar.gz' saved [59941262/59941262] 
    [hadoop@edydr1p0 pig-install]$ 
###
    # pig 압축 해제
    [hadoop@edydr1p0 pig-install]$ tar xvfz pig-0.12.0.tar.gz
    
    [hadoop@edydr1p0 pig-install]$ cd
    
    [hadoop@edydr1p0 ~]$ vi + .bash_profile
    # vi 편집기 실행    
    export PIG_HOME=/home/hadoop/pig-install/pig-0.12.0
    export PATH=$PIG_HOME/bin:$PATH
    # vi 편집기 종료
![Screen Shot 2019-04-24 at 2 47 11 PM](https://user-images.githubusercontent.com/46523571/56635228-de7bcf80-669f-11e9-8982-e9be5623570f.png)
    
    [hadoop@edydr1p0 ~]$ . .bash_profile
    
    [hadoop@edydr1p0 ~]$ pig -x local
    2019-04-24 14:47:28,910 [main] INFO  org.apache.pig.Main - Apache Pig version 0.12.0 (r1529718) compiled Oct 07 2013, 12:20:14
    2019-04-24 14:47:28,911 [main] INFO  org.apache.pig.Main - Logging error messages to: /home/hadoop/pig_1556084848908.log
    2019-04-24 14:47:28,952 [main] INFO  org.apache.pig.impl.util.Utils - Default bootup file /home/hadoop/.pigbootup not found
    2019-04-24 14:47:29,184 [main] INFO  org.apache.pig.backend.hadoop.executionengine.HExecutionEngine - Connecting to hadoop file system at: file:///
    grunt> 
    
    grunt> help
    Commands:
    <pig latin statement>; - See the PigLatin manual for details: http://hadoop.apache.org/pig
    File system commands:
        fs <fs arguments> - Equivalent to Hadoop dfs command: http://hadoop.apache.org/common/docs/current/hdfs_shell.html
    Diagnostic commands:
        describe <alias>[::<alias] - Show the schema for the alias. Inner aliases can be described as A::B.
        explain [-script <pigscript>] [-out <path>] [-brief] [-dot|-xml] [-param <param_name>=<param_value>]
            [-param_file <file_name>] [<alias>] - Show the execution plan to compute the alias or for entire script.
            -script - Explain the entire script.
            -out - Store the output into directory rather than print to stdout.
            -brief - Don't expand nested plans (presenting a smaller graph for overview).
            -dot - Generate the output in .dot format. Default is text format.
            -xml - Generate the output in .xml format. Default is text format.
            -param <param_name - See parameter substitution for details.
            -param_file <file_name> - See parameter substitution for details.
            alias - Alias to explain.
        dump <alias> - Compute the alias and writes the results to stdout.
    Utility Commands:
        exec [-param <param_name>=param_value] [-param_file <file_name>] <script> - 
            Execute the script with access to grunt environment including aliases.
            -param <param_name - See parameter substitution for details.
            -param_file <file_name> - See parameter substitution for details.
            script - Script to be executed.
        run [-param <param_name>=param_value] [-param_file <file_name>] <script> - 
            Execute the script with access to grunt environment. 
            -param <param_name - See parameter substitution for details.
            -param_file <file_name> - See parameter substitution for details.
            script - Script to be executed.
        sh  <shell command> - Invoke a shell command.
        kill <job_id> - Kill the hadoop job specified by the hadoop job id.
        set <key> <value> - Provide execution parameters to Pig. Keys and values are case sensitive.
            The following keys are supported: 
            default_parallel - Script-level reduce parallelism. Basic input size heuristics used by default.
            debug - Set debug on or off. Default is off.
            job.name - Single-quoted name for jobs. Default is PigLatin:<script name>
            job.priority - Priority for jobs. Values: very_low, low, normal, high, very_high. Default is normal
            stream.skippath - String that contains the path. This is used by streaming.
            any hadoop property.
        help - Display this message.
        history [-n] - Display the list statements in cache.
            -n Hide line numbers. 
        quit - Quit the grunt shell.

    grunt> quit
    
    OS] pig
    
    ... Connecting to map-reduce job tracker at: localhost:9001
    
    grunt> quit

* 참고하세요.
  - Pig runs locally on your machine or your gateway machine. All of the parsing, checking, and planning is done locally. Pig then executes MapReduce jobs in your cluster.
  - When I say "your gateway machine," I mean the machine from which you are launching Pig jobs. Usually this will be one or more machines that have access to your Hadoop cluster. However, depending on your configuration, it could be your local machine as well.
  -  => 피그의 conf 경로에 pig.properties 파일을 생성하고 NameNode와 JobTracker를 설정하면 됩니다.
  -  => fs.default.name=hdfs://localhost/mapred.job.tracker=localhost:8021
###
    [hadoop@edydr1p0 ~]$ pig -x local       <-리눅스 일반 경로
    
    grunt> A = LOAD '/home/hadoop/ml-1m/movies2.dat' USING PigStorage(',');        <-- 계획  
    grunt> B = FOREACH A GENERATE $0,$1;         <-- 계획                           
    grunt> DUMP B;      <--A, B 계획 후 여기와서 실행
    grunt> STORE B INTO 'pig_output1' USING PigStorage(','); 
    grunt> quit
    
    [hadoop@edydr1p0 ~]$ ls pig_output*
    part-m-00000  _SUCCESS

    [hadoop@edydr1p0 ~]$ more pig_output1/p*

* (3) 맵리듀스 모드
###    
    [hadoop@edydr1p0 ~]$ hadoop fs -mkdir /movie_rating_data
    [hadoop@edydr1p0 ~]$ hadoop fs -put ml-1m/*.dat /movie_rating_data
    [hadoop@edydr1p0 ~]$ hadoop fs -ls / 
    Found 1 items
    drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:10 /user/hadoop/input
    
    [hadoop@edydr1p0 ~]$ pig 
    2019-04-24 14:54:51,516 [main] INFO  org.apache.pig.Main - Apache Pig version 0.12.0 (r1529718) compiled Oct 07 2013, 12:20:14
    2019-04-24 14:54:51,517 [main] INFO  org.apache.pig.Main - Logging error messages to: /home/hadoop/pig_1556085291514.log
    2019-04-24 14:54:51,559 [main] INFO  org.apache.pig.impl.util.Utils - Default bootup file /home/hadoop/.pigbootup not found
    2019-04-24 14:54:51,996 [main] INFO  org.apache.pig.backend.hadoop.executionengine.HExecutionEngine - Connecting to hadoop file system at: hdfs://192.168.56.101:9000
    2019-04-24 14:54:52,433 [main] INFO  org.apache.pig.backend.hadoop.executionengine.HExecutionEngine - Connecting to map-reduce job tracker at: 192.168.56.101:9001
    
    grunt> A = LOAD '/movie_rating_data/movies2.dat' USING PigStorage(',');       <-하둡 안에 경로
    grunt> B = FOREACH A GENERATE $0,$1;                             
    grunt> STORE B INTO '/pig_output1' USING PigStorage(','); 
    2019-04-24 14:55:22,629 [main] INFO  org.apache.pig.tools.pigstats.ScriptState - Pig features used in the script: UNKNOWN
    2019-04-24 14:55:22,670 [main] INFO  org.apache.pig.newplan.logical.optimizer.LogicalPlanOptimizer - {RULES_ENABLED=[AddForEach, ColumnMapKeyPrune, DuplicateForEachColumnRewrite, GroupByConstParallelSetter, ImplicitSplitInserter, LimitOptimizer, LoadTypeCastInserter, MergeFilter, MergeForEach, NewPartitionFilterOptimizer, PartitionFilterOptimizer, PushDownForEachFlatten, PushUpFilter, SplitFilter, StreamTypeCastInserter], RULES_DISABLED=[FilterLogicExpressionSimplifier]}
    2019-04-24 14:55:22,788 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MRCompiler - File concatenation threshold: 100 optimistic? false
    2019-04-24 14:55:22,813 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MultiQueryOptimizer - MR plan size before optimization: 1
    2019-04-24 14:55:22,814 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MultiQueryOptimizer - MR plan size after optimization: 1
    2019-04-24 14:55:23,096 [main] INFO  org.apache.pig.tools.pigstats.ScriptState - Pig script settings are added to the job
    2019-04-24 14:55:23,156 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - mapred.job.reduce.markreset.buffer.percent is not set, set to default 0.3
    2019-04-24 14:55:23,160 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - creating jar file Job7442325799582477221.jar
    2019-04-24 14:55:26,089 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - jar file Job7442325799582477221.jar created
    2019-04-24 14:55:26,102 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - Setting up single store job
    2019-04-24 14:55:26,108 [main] INFO  org.apache.pig.data.SchemaTupleFrontend - Key [pig.schematuple] is false, will not generate code.
    2019-04-24 14:55:26,108 [main] INFO  org.apache.pig.data.SchemaTupleFrontend - Starting process to move generated code to distributed cache
    2019-04-24 14:55:26,109 [main] INFO  org.apache.pig.data.SchemaTupleFrontend - Setting key [pig.schematuple.classes] with classes to deserialize []
    2019-04-24 14:55:26,114 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - Map only job, skipping reducer estimation
    2019-04-24 14:55:26,151 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - 1 map-reduce job(s) waiting for submission.
    2019-04-24 14:55:26,557 [JobControl] INFO  org.apache.hadoop.mapreduce.lib.input.FileInputFormat - Total input paths to process : 1
    2019-04-24 14:55:26,557 [JobControl] INFO  org.apache.pig.backend.hadoop.executionengine.util.MapRedUtil - Total input paths to process : 1
    2019-04-24 14:55:26,642 [JobControl] INFO  org.apache.hadoop.util.NativeCodeLoader - Loaded the native-hadoop library
    2019-04-24 14:55:26,642 [JobControl] WARN  org.apache.hadoop.io.compress.snappy.LoadSnappy - Snappy native library not loaded
    2019-04-24 14:55:26,654 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - 0% complete
    2019-04-24 14:55:26,659 [JobControl] INFO  org.apache.pig.backend.hadoop.executionengine.util.MapRedUtil - Total input paths (combined) to process : 1
    2019-04-24 14:55:28,552 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - HadoopJobId: job_201904241443_0001
    2019-04-24 14:55:28,552 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - Processing aliases A,B
    2019-04-24 14:55:28,552 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - detailed locations: M: A[1,4],B[2,4] C:  R: 
    2019-04-24 14:55:28,552 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - More information at: http://192.168.56.101:50030/jobdetails.jsp?jobid=job_201904241443_0001
    2019-04-24 14:55:38,282 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - 50% complete
    2019-04-24 14:55:43,409 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - 100% complete
    2019-04-24 14:55:43,411 [main] INFO  org.apache.pig.tools.pigstats.SimplePigStats - Script Statistics: 
    
    HadoopVersion   PigVersion      UserId  StartedAt       FinishedAt      Features
    1.2.1   0.12.0  hadoop  2019-04-24 14:55:23     2019-04-24 14:55:43     UNKNOWN
    
    Success!
    
    Job Stats (time in seconds):
    JobId   Maps    Reduces MaxMapTime      MinMapTIme      AvgMapTime      MedianMapTime   MaxReduceTime   MinReduceTime   AvgReduceTime   MedianReducetime       Alias    Feature Outputs
    job_201904241443_0001   1       0       5       5       5       5       n/a    n/a      n/a     n/a     A,B     MAP_ONLY        /pig_output1,
    
    Input(s):
    Successfully read 3883 records (163917 bytes) from: "/movie_rating_data/movies2.dat"
    
    Output(s):
    Successfully stored 3883 records (101483 bytes) in: "/pig_output1"
    
    Counters:
    Total records written : 3883
    Total bytes written : 101483
    Spillable Memory Manager spill count : 0
    Total bags proactively spilled: 0
    Total records proactively spilled: 0
    
    Job DAG:
    job_201904241443_0001
    
    
    2019-04-24 14:55:43,418 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - Success!

    grunt> quit
###    
    [hadoop@edydr1p0 ~]$ hadoop fs -ls / 
    Found 1 items
    drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:10 /user/hadoop/input

    [hadoop@edydr1p0 ~]$ hadoop fs -ls /pig_output*
    Found 3 items
    -rw-r--r--   1 hadoop supergroup          0 2019-04-24 14:55 /pig_output1/_SUCCESS
    drwxr-xr-x   - hadoop supergroup          0 2019-04-24 14:55 /pig_output1/_logs
    -rw-r--r--   1 hadoop supergroup     101483 2019-04-24 14:55 /pig_output1/part-m-00000

    [hadoop@edydr1p0 ~]$ hadoop fs -cat /pig_output1/p* | head 
    1,Toy Story (1995)
    2,Jumanji (1995)
    3,Grumpier Old Men (1995)
    4,Waiting to Exhale (1995)
    5,Father of the Bride Part II (1995)
    6,Heat (1995)
    7,Sabrina (1995)
    8,Tom and Huck (1995)
    9,Sudden Death (1995)
    10,GoldenEye (1995)
    cat: Unable to write to output stream.
    
    [hadoop@edydr1p0 ~]$ vi a.pig
    # vi 편집기 편집
    A = LOAD '/movie_rating_data/movies2.dat' USING PigStorage(','); 
    B = FOREACH A GENERATE $0,$1;
    STORE B INTO '/pig_output2' USING PigStorage(',');
    # vi 편집기 종료
![Screen Shot 2019-04-24 at 2 58 33 PM](https://user-images.githubusercontent.com/46523571/56635699-74642a00-66a1-11e9-97fc-f7c217225548.png)
###
    [hadoop@edydr1p0 ~]$ pig a.pig
    2019-04-24 14:58:34,412 [main] INFO  org.apache.pig.Main - Apache Pig version 0.12.0 (r1529718) compiled Oct 07 2013, 12:20:14
    2019-04-24 14:58:34,413 [main] INFO  org.apache.pig.Main - Logging error messages to: /home/hadoop/pig_1556085514410.log
    2019-04-24 14:58:34,882 [main] INFO  org.apache.pig.impl.util.Utils - Default bootup file /home/hadoop/.pigbootup not found
    2019-04-24 14:58:35,067 [main] INFO  org.apache.pig.backend.hadoop.executionengine.HExecutionEngine - Connecting to hadoop file system at: hdfs://192.168.56.101:9000
    2019-04-24 14:58:36,108 [main] INFO  org.apache.pig.backend.hadoop.executionengine.HExecutionEngine - Connecting to map-reduce job tracker at: 192.168.56.101:9001
    2019-04-24 14:58:36,774 [main] INFO  org.apache.pig.tools.pigstats.ScriptState - Pig features used in the script: UNKNOWN
    2019-04-24 14:58:36,814 [main] INFO  org.apache.pig.newplan.logical.optimizer.LogicalPlanOptimizer - {RULES_ENABLED=[AddForEach, ColumnMapKeyPrune, DuplicateForEachColumnRewrite, GroupByConstParallelSetter, ImplicitSplitInserter, LimitOptimizer, LoadTypeCastInserter, MergeFilter, MergeForEach, NewPartitionFilterOptimizer, PartitionFilterOptimizer, PushDownForEachFlatten, PushUpFilter, SplitFilter, StreamTypeCastInserter], RULES_DISABLED=[FilterLogicExpressionSimplifier]}
    2019-04-24 14:58:36,999 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MRCompiler - File concatenation threshold: 100 optimistic? false
    2019-04-24 14:58:37,033 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MultiQueryOptimizer - MR plan size before optimization: 1
    2019-04-24 14:58:37,034 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MultiQueryOptimizer - MR plan size after optimization: 1
    2019-04-24 14:58:37,260 [main] INFO  org.apache.pig.tools.pigstats.ScriptState - Pig script settings are added to the job
    2019-04-24 14:58:37,269 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - mapred.job.reduce.markreset.buffer.percent is not set, set to default 0.3
    2019-04-24 14:58:37,271 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - creating jar file Job3095847377096572183.jar
    2019-04-24 14:58:40,469 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - jar file Job3095847377096572183.jar created
    2019-04-24 14:58:40,502 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - Setting up single store job
    2019-04-24 14:58:40,508 [main] INFO  org.apache.pig.data.SchemaTupleFrontend - Key [pig.schematuple] is false, will not generate code.
    2019-04-24 14:58:40,508 [main] INFO  org.apache.pig.data.SchemaTupleFrontend - Starting process to move generated code to distributed cache
    2019-04-24 14:58:40,509 [main] INFO  org.apache.pig.data.SchemaTupleFrontend - Setting key [pig.schematuple.classes] with classes to deserialize []
    2019-04-24 14:58:40,509 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.JobControlCompiler - Map only job, skipping reducer estimation
    2019-04-24 14:58:40,548 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - 1 map-reduce job(s) waiting for submission.
    2019-04-24 14:58:41,051 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - 0% complete
    2019-04-24 14:58:41,276 [JobControl] INFO  org.apache.hadoop.mapreduce.lib.input.FileInputFormat - Total input paths to process : 1
    2019-04-24 14:58:41,277 [JobControl] INFO  org.apache.pig.backend.hadoop.executionengine.util.MapRedUtil - Total input paths to process : 1
    2019-04-24 14:58:41,288 [JobControl] INFO  org.apache.hadoop.util.NativeCodeLoader - Loaded the native-hadoop library
    2019-04-24 14:58:41,289 [JobControl] WARN  org.apache.hadoop.io.compress.snappy.LoadSnappy - Snappy native library not loaded
    2019-04-24 14:58:41,295 [JobControl] INFO  org.apache.pig.backend.hadoop.executionengine.util.MapRedUtil - Total input paths (combined) to process : 1
    2019-04-24 14:58:42,432 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - HadoopJobId: job_201904241443_0002
    2019-04-24 14:58:42,433 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - Processing aliases A,B
    2019-04-24 14:58:42,433 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - detailed locations: M: A[1,4],B[2,4] C:  R: 
    2019-04-24 14:58:42,433 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - More information at: http://192.168.56.101:50030/jobdetails.jsp?jobid=job_201904241443_0002
    2019-04-24 14:58:48,496 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - 50% complete
    2019-04-24 14:58:52,062 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - 100% complete
    2019-04-24 14:58:52,064 [main] INFO  org.apache.pig.tools.pigstats.SimplePigStats - Script Statistics: 
    
    HadoopVersion   PigVersion      UserId  StartedAt       FinishedAt      Features
    1.2.1   0.12.0  hadoop  2019-04-24 14:58:37     2019-04-24 14:58:52     UNKNOWN
    
    Success!
    
    Job Stats (time in seconds):
    JobId   Maps    Reduces MaxMapTime      MinMapTIme      AvgMapTime      MedianMapTime   MaxReduceTime   MinReduceTime   AvgReduceTime   MedianReducetime       Alias    Feature Outputs
    job_201904241443_0002   1       0       3       3       3       3       n/a    n/a      n/a     n/a     A,B     MAP_ONLY        /pig_output2,
    
    Input(s):
    Successfully read 3883 records (163917 bytes) from: "/movie_rating_data/movies2.dat"
    
    Output(s):
    Successfully stored 3883 records (101483 bytes) in: "/pig_output2"
    
    Counters:
    Total records written : 3883
    Total bytes written : 101483
    Spillable Memory Manager spill count : 0
    Total bags proactively spilled: 0
    Total records proactively spilled: 0
    
    Job DAG:
    job_201904241443_0002
    
    
    2019-04-24 14:58:52,072 [main] INFO  org.apache.pig.backend.hadoop.executionengine.mapReduceLayer.MapReduceLauncher - Success!

    [hadoop@edydr1p0 ~]$ hadoop fs -ls /pig_output*
    -rw-r--r--   1 hadoop supergroup          0 2019-04-24 14:55 /pig_output1/_SUCCESS
    drwxr-xr-x   - hadoop supergroup          0 2019-04-24 14:55 /pig_output1/_logs
    -rw-r--r--   1 hadoop supergroup     101483 2019-04-24 14:55 /pig_output1/part-m-00000
    -rw-r--r--   1 hadoop supergroup          0 2019-04-24 14:58 /pig_output2/_SUCCESS
    drwxr-xr-x   - hadoop supergroup          0 2019-04-24 14:58 /pig_output2/_logs
    -rw-r--r--   1 hadoop supergroup     101483 2019-04-24 14:58 /pig_output2/part-m-00000

    [hadoop@edydr1p0 ~]$ hadoop fs -cat /pig_output2/p* | head 
    1,Toy Story (1995)
    2,Jumanji (1995)
    3,Grumpier Old Men (1995)
    4,Waiting to Exhale (1995)
    5,Father of the Bride Part II (1995)
    6,Heat (1995)
    7,Sabrina (1995)
    8,Tom and Huck (1995)
    9,Sudden Death (1995)
    10,GoldenEye (1995)
    cat: Unable to write to output stream.


<br>
<br>
<br>

## [3] EmbeddedPig(pig를 Java나 Python에 내포 시킬 수 있다.)
* Embedding Pig In Java Programs       : http://wiki.apache.org/pig/EmbeddedPig
* Embedded Pig - Python and JavaScript : http://pig.apache.org/docs/r0.9.1/cont.html#Usage+Examples

## References
<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>