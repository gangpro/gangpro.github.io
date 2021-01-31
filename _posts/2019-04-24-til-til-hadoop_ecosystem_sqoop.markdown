---
layout: post
title: '[hadoop] 하둡(hadoop) 에코시스템 - sqoop'
subtitle: 
categories: til
tags: til env hadoop
comments: true
date: 2019-04-24 14:12:17 +0900
lastmod: 2019-04-24 20:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

하둡(hadoop) 에코시스템 sqoop<br />

# SQOOP
> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> VM : Enterprise Linux<br>
> JDK : jdk1.8.0_211<br>
> Hadoop 1.2.1<br>
> hive 1.2.1<br>
> pig 0.12.0<br>
> MySQL : 5.7<br>
> sqoop 1.4.4<br>

## [1] Homebrew 설치
* [Homebrew URL](https://brew.sh/) 사이트 참고
###
    # 터미널에서 아래 코드 실행
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
###
    @KANG ~ $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ==> This script will install:
    /usr/local/bin/brew
    /usr/local/share/doc/homebrew
    /usr/local/share/man/man1/brew.1
    /usr/local/share/zsh/site-functions/_brew
    /usr/local/etc/bash_completion.d/brew
    /usr/local/Homebrew
    ==> The following new directories will be created:
    /usr/local/etc
    /usr/local/include
    /usr/local/lib
    /usr/local/sbin
    /usr/local/share
    /usr/local/var
    /usr/local/opt
    /usr/local/share/zsh
    /usr/local/share/zsh/site-functions
    /usr/local/var/homebrew
    /usr/local/var/homebrew/linked
    /usr/local/Cellar
    /usr/local/Caskroom
    /usr/local/Homebrew
    /usr/local/Frameworks
    ==> The Xcode Command Line Tools will be installed.
    ----------------------------------------------------------
    Press RETURN to continue or any other key to abort
    ----------------------------------------------------------
    ==> /usr/bin/sudo /bin/mkdir -p /usr/local/etc /usr/local/include /usr/local/lib /usr/local/sbin /usr/local/share /usr/local/var /usr/local/opt /usr/local/share/zsh /usr/local/share/zsh/site-functions /usr/local/var/homebrew /usr/local/var/homebrew/linked /usr/local/Cellar /usr/local/Caskroom /usr/local/Homebrew /usr/local/Frameworks
    ----------------------------------------------------------
    Password:
    ----------------------------------------------------------
    ==> /usr/bin/sudo /bin/chmod g+rwx /usr/local/etc /usr/local/include /usr/local/lib /usr/local/sbin /usr/local/share /usr/local/var /usr/local/opt /usr/local/share/zsh /usr/local/share/zsh/site-functions /usr/local/var/homebrew /usr/local/var/homebrew/linked /usr/local/Cellar /usr/local/Caskroom /usr/local/Homebrew /usr/local/Frameworks
    ==> /usr/bin/sudo /bin/chmod 755 /usr/local/share/zsh /usr/local/share/zsh/site-functions
    ==> /usr/bin/sudo /usr/sbin/chown kang /usr/local/etc /usr/local/include /usr/local/lib /usr/local/sbin /usr/local/share /usr/local/var /usr/local/opt /usr/local/share/zsh /usr/local/share/zsh/site-functions /usr/local/var/homebrew /usr/local/var/homebrew/linked /usr/local/Cellar /usr/local/Caskroom /usr/local/Homebrew /usr/local/Frameworks
    ==> /usr/bin/sudo /usr/bin/chgrp admin /usr/local/etc /usr/local/include /usr/local/lib /usr/local/sbin /usr/local/share /usr/local/var /usr/local/opt /usr/local/share/zsh /usr/local/share/zsh/site-functions /usr/local/var/homebrew /usr/local/var/homebrew/linked /usr/local/Cellar /usr/local/Caskroom /usr/local/Homebrew /usr/local/Frameworks
    ==> /usr/bin/sudo /bin/mkdir -p /Users/kang/Library/Caches/Homebrew
    ==> /usr/bin/sudo /bin/chmod g+rwx /Users/kang/Library/Caches/Homebrew
    ==> /usr/bin/sudo /usr/sbin/chown kang /Users/kang/Library/Caches/Homebrew
    ==> /usr/bin/sudo /bin/mkdir -p /Library/Caches/Homebrew
    ==> /usr/bin/sudo /bin/chmod g+rwx /Library/Caches/Homebrew
    ==> /usr/bin/sudo /usr/sbin/chown kang /Library/Caches/Homebrew
    ==> Searching online for the Command Line Tools
    ==> /usr/bin/sudo /usr/bin/touch /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress
    ==> Installing Command Line Tools (macOS Mojave version 10.14) for Xcode-10.1
    ==> /usr/bin/sudo /usr/sbin/softwareupdate -i Command\ Line\ Tools\ (macOS\ Mojave\ version\ 10.14)\ for\ Xcode-10.1
    Software Update Tool
    
    
    Downloading Command Line Tools (macOS Mojave version 10.14) for Xcode
    Downloaded Command Line Tools (macOS Mojave version 10.14) for Xcode
    Installing Command Line Tools (macOS Mojave version 10.14) for Xcode
    Done with Command Line Tools (macOS Mojave version 10.14) for Xcode
    Done.
    ==> /usr/bin/sudo /bin/rm -f /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress
    ==> /usr/bin/sudo /usr/bin/xcode-select --switch /Library/Developer/CommandLineTools
    ==> Downloading and installing Homebrew...
    remote: Enumerating objects: 2, done.
    remote: Counting objects: 100% (2/2), done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 116067 (delta 0), reused 1 (delta 0), pack-reused 116065
    Receiving objects: 100% (116067/116067), 27.31 MiB | 3.24 MiB/s, done.
    Resolving deltas: 100% (84761/84761), done.
    From https://github.com/Homebrew/brew
     * [new branch]          master     -> origin/master
     * [new tag]             0.1        -> 0.1
     * [new tag]             0.2        -> 0.2
     * [new tag]             0.3        -> 0.3
     * .........             ...        -> ...
     * [new tag]             1.8.4      -> 1.8.4
     * [new tag]             1.8.5      -> 1.8.5
     * [new tag]             1.8.6      -> 1.8.6
    HEAD is now at fdffb3951 Merge pull request #5450 from jonchang/fix-update-test
    ==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
      https://github.com/Homebrew/brew#donations
    ==> Tapping homebrew/core
    Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'...
    remote: Enumerating objects: 4887, done.
    remote: Counting objects: 100% (4887/4887), done.
    remote: Compressing objects: 100% (4691/4691), done.
    remote: Total 4887 (delta 48), reused 301 (delta 4), pack-reused 0
    Receiving objects: 100% (4887/4887), 4.02 MiB | 8.57 MiB/s, done.
    Resolving deltas: 100% (48/48), done.
    Tapped 2 commands and 4671 formulae (4,929 files, 12.5MB).
    ==> Migrating /Library/Caches/Homebrew to /Users/kang/Library/Caches/Homebrew...
    ==> Deleting /Library/Caches/Homebrew...
    Already up-to-date.
    ==> Installation successful!
    
    ==> Homebrew has enabled anonymous aggregate formulae and cask analytics.
    Read the analytics documentation (and how to opt-out) here:
      https://docs.brew.sh/Analytics
    
    ==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
      https://github.com/Homebrew/brew#donations
    ==> Next steps:
    - Run `brew help` to get started
    - Further documentation: 
        https://docs.brew.sh
    @KANG ~ $ 


# [2] Install MySQL 5.7 on macOS
## [2-1] MySQL 5.7 버전 체크  
    터미널에서 아래 코드 실행
    brew info mysql@5.7
###  
    @KANG ~ $brew info mysql@5.7
    mysql@5.7: stable 5.7.25 (bottled) [keg-only]
    Open source relational database management system
    https://dev.mysql.com/doc/refman/5.7/en/
    Not installed
    From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/mysql@5.7.rb
    ==> Dependencies
    Build: cmake ✘
    Required: openssl ✘
    ==> Caveats
    We've installed your MySQL database without a root password. To secure it run:
        mysql_secure_installation
    
    MySQL is configured to only allow connections from localhost by default
    
    To connect run:
        mysql -uroot
    
    A "/etc/my.cnf" from another install may interfere with a Homebrew-built
    server starting up correctly.
    
    mysql@5.7 is keg-only, which means it was not symlinked into /usr/local,
    because this is an alternate version of another formula.
    
    
    To have launchd start mysql@5.7 now and restart at login:
      brew services start mysql@5.7
    Or, if you don't want/need a background service you can just run:
      /usr/local/opt/mysql@5.7/bin/mysql.server start
    ==> Analytics
    install: 21,662 (30 days), 64,880 (90 days), 214,112 (365 days)
    install_on_request: 21,614 (30 days), 64,680 (90 days), 213,591 (365 days)
    build_error: 0 (30 days)

<br>
<br>
<br>

## [2-2] MySQL 5.7 설치
    # 터미널에서 아래 코드 실행
    brew install mysql@5.7
###
    @KANG ~ $brew install mysql@5.7
    ==> Installing dependencies for mysql@5.7: openssl
    ==> Installing mysql@5.7 dependency: openssl
    ==> Downloading https://homebrew.bintray.com/bottles/openssl-1.0.2r.mojave.bottle.tar.gz
    ==> Downloading from https://akamai.bintray.com/c1/c1f8c06740398325c7028213b20b18c5de39763fb
    ######################################################################## 100.0%
    ==> Pouring openssl-1.0.2r.mojave.bottle.tar.gz
    ==> Caveats
    A CA file has been bootstrapped using certificates from the SystemRoots
    keychain. To add additional certificates (e.g. the certificates added in
    the System keychain), place .pem files in
      /usr/local/etc/openssl/certs
    
    and run
      /usr/local/opt/openssl/bin/c_rehash
    
    openssl is keg-only, which means it was not symlinked into /usr/local,
    because Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries.
    
    If you need to have openssl first in your PATH run:
      echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile
    
    For compilers to find openssl you may need to set:
      export LDFLAGS="-L/usr/local/opt/openssl/lib"
      export CPPFLAGS="-I/usr/local/opt/openssl/include"
    
    ==> Summary
    🍺  /usr/local/Cellar/openssl/1.0.2r: 1,795 files, 12.1MB
    ==> Installing mysql@5.7
    ==> Downloading https://homebrew.bintray.com/bottles/mysql@5.7-5.7.25.mojave.bottle.tar.gz
    ==> Downloading from https://akamai.bintray.com/40/408e41e6b7db830398597db177df21ea28b55d379
    ######################################################################## 100.0%
    ==> Pouring mysql@5.7-5.7.25.mojave.bottle.tar.gz
    ==> /usr/local/Cellar/mysql@5.7/5.7.25/bin/mysqld --initialize-insecure --user=kang --basedi
    ==> Caveats
    We've installed your MySQL database without a root password. To secure it run:
        mysql_secure_installation
    
    MySQL is configured to only allow connections from localhost by default
    
    To connect run:
        mysql -uroot
    
    A "/etc/my.cnf" from another install may interfere with a Homebrew-built
    server starting up correctly.
    
    mysql@5.7 is keg-only, which means it was not symlinked into /usr/local,
    because this is an alternate version of another formula.
    
    If you need to have mysql@5.7 first in your PATH run:
      echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.bash_profile
    
    For compilers to find mysql@5.7 you may need to set:
      export LDFLAGS="-L/usr/local/opt/mysql@5.7/lib"
      export CPPFLAGS="-I/usr/local/opt/mysql@5.7/include"
    
    
    To have launchd start mysql@5.7 now and restart at login:
      brew services start mysql@5.7
    Or, if you don't want/need a background service you can just run:
      /usr/local/opt/mysql@5.7/bin/mysql.server start
    ==> Summary
    🍺  /usr/local/Cellar/mysql@5.7/5.7.25: 319 files, 234MB
    ==> `brew cleanup` has not been run in 30 days, running now...
    Removing: /Users/kang/Library/Caches/Homebrew/git-lfs--2.7.1.mojave.bottle.tar.gz... (4MB)
    Removing: /usr/local/Cellar/openssl/1.0.2q... (1,794 files, 12.1MB)
    Removing: /Users/kang/Library/Caches/Homebrew/openssl--1.0.2q.mojave.bottle.tar.gz... (3.7MB)
    Removing: /Users/kang/Library/Caches/Homebrew/pcre2--10.32.mojave.bottle.tar.gz... (1.8MB)
    Removing: /Users/kang/Library/Logs/Homebrew/git-lfs... (64B)
    Removing: /Users/kang/Library/Logs/Homebrew/gettext... (64B)
    Removing: /Users/kang/Library/Logs/Homebrew/pcre2... (64B)
    Removing: /Users/kang/Library/Logs/Homebrew/git... (64B)
    Pruned 1 symbolic links and 2 directories from /usr/local
    ==> Caveats
    ==> openssl
    A CA file has been bootstrapped using certificates from the SystemRoots
    keychain. To add additional certificates (e.g. the certificates added in
    the System keychain), place .pem files in
      /usr/local/etc/openssl/certs
    
    and run
      /usr/local/opt/openssl/bin/c_rehash
    
    openssl is keg-only, which means it was not symlinked into /usr/local,
    because Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries.
    
    If you need to have openssl first in your PATH run:
      echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile
    
    For compilers to find openssl you may need to set:
      export LDFLAGS="-L/usr/local/opt/openssl/lib"
      export CPPFLAGS="-I/usr/local/opt/openssl/include"
    
    ==> mysql@5.7
    We've installed your MySQL database without a root password. To secure it run:
        mysql_secure_installation
    
    MySQL is configured to only allow connections from localhost by default
    
    To connect run:
        mysql -uroot
    
    A "/etc/my.cnf" from another install may interfere with a Homebrew-built
    server starting up correctly.
    
    mysql@5.7 is keg-only, which means it was not symlinked into /usr/local,
    because this is an alternate version of another formula.
    
    If you need to have mysql@5.7 first in your PATH run:
      echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.bash_profile
    
    For compilers to find mysql@5.7 you may need to set:
      export LDFLAGS="-L/usr/local/opt/mysql@5.7/lib"
      export CPPFLAGS="-I/usr/local/opt/mysql@5.7/include"
    
    
    To have launchd start mysql@5.7 now and restart at login:
      brew services start mysql@5.7
    Or, if you don't want/need a background service you can just run:
      /usr/local/opt/mysql@5.7/bin/mysql.server start

<br>
<br>
<br>

## [2-3] MySQL 기타 설정
    @KANG ~ $brew tap homebrew/services
    
    @KANG ~ $brew services list
    Name      Status  User Plist
    mysql@5.7 stopped      
    
    @KANG ~ $brew link mysql@5.7 --force
    Linking /usr/local/Cellar/mysql@5.7/5.7.25... 87 symlinks created
    
    If you need to have this software first in your PATH instead consider running:
      echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.bash_profile
    
    @KANG ~ $mysql -V
    mysql  Ver 14.14 Distrib 5.7.25, for osx10.14 (x86_64) using  EditLine wrapper

<br>
<br>
<br>

## [2-4] MySQL 서버 시작과 종료
    @KANG ~ $brew services start mysql@5.7
    ==> Successfully started `mysql@5.7` (label: homebrew.mxcl.mysql@5.7)
    
    @KANG ~ $launchctl list | grep mysql
    22363	0	homebrew.mxcl.mysql@5.7
    
    @KANG ~ $tail /usr/local/var/mysql/kezzico.local.err 
    tail: /usr/local/var/mysql/kezzico.local.err: No such file or directory
    
    @KANG ~ $sudo mkdir /var/run/mysqld
    Password: 맥북 비밀번호
    
    @KANG ~ $sudo chmod 777 /var/run/mysqld
    
    @KANG ~ $mysql -V
    mysql  Ver 14.14 Distrib 5.7.25, for osx10.14 (x86_64) using  EditLine wrapper

    @KANG ~ $brew services stop mysql@5.7
<br>
<br>
<br>

## [2-5] MySQL root 계정 비밀번호 설정
    @KANG ~ $mysqladmin -u root password '비밀번호'
    mysqladmin: [Warning] Using a password on the command line interface can be insecure.
    Warning: Since password will be sent to server in plain text, use ssl connection to ensure password safety.

<br>
<br>
<br>

## [2-6] MySQL root 계정 접속
    @KANG ~ $mysql -u root -p
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 5
    Server version: 5.7.25 Homebrew
    
    Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> 

<br>
<br>
<br>

## [2-7] MySQL 기초 사용법
    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | sys                |
    +--------------------+
    4 rows in set (0.00 sec)
###
    mysql> use information_schema
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A
    
    Database changed
###
    mysql> show tables;
    +---------------------------------------+
    | Tables_in_information_schema          |
    +---------------------------------------+
    | CHARACTER_SETS                        |
    | COLLATIONS                            |
    | COLLATION_CHARACTER_SET_APPLICABILITY |
    | COLUMNS                               |
    | COLUMN_PRIVILEGES                     |
    | ENGINES                               |
    | EVENTS                                |
    | FILES                                 |
    | GLOBAL_STATUS                         |
    | GLOBAL_VARIABLES                      |
    | KEY_COLUMN_USAGE                      |
    | OPTIMIZER_TRACE                       |
    | PARAMETERS                            |
    | PARTITIONS                            |
    | PLUGINS                               |
    | PROCESSLIST                           |
    | PROFILING                             |
    | REFERENTIAL_CONSTRAINTS               |
    | ROUTINES                              |
    | SCHEMATA                              |
    | SCHEMA_PRIVILEGES                     |
    | SESSION_STATUS                        |
    | SESSION_VARIABLES                     |
    | STATISTICS                            |
    | TABLES                                |
    | TABLESPACES                           |
    | TABLE_CONSTRAINTS                     |
    | TABLE_PRIVILEGES                      |
    | TRIGGERS                              |
    | USER_PRIVILEGES                       |
    | VIEWS                                 |
    | INNODB_LOCKS                          |
    | INNODB_TRX                            |
    | INNODB_SYS_DATAFILES                  |
    | INNODB_FT_CONFIG                      |
    | INNODB_SYS_VIRTUAL                    |
    | INNODB_CMP                            |
    | INNODB_FT_BEING_DELETED               |
    | INNODB_CMP_RESET                      |
    | INNODB_CMP_PER_INDEX                  |
    | INNODB_CMPMEM_RESET                   |
    | INNODB_FT_DELETED                     |
    | INNODB_BUFFER_PAGE_LRU                |
    | INNODB_LOCK_WAITS                     |
    | INNODB_TEMP_TABLE_INFO                |
    | INNODB_SYS_INDEXES                    |
    | INNODB_SYS_TABLES                     |
    | INNODB_SYS_FIELDS                     |
    | INNODB_CMP_PER_INDEX_RESET            |
    | INNODB_BUFFER_PAGE                    |
    | INNODB_FT_DEFAULT_STOPWORD            |
    | INNODB_FT_INDEX_TABLE                 |
    | INNODB_FT_INDEX_CACHE                 |
    | INNODB_SYS_TABLESPACES                |
    | INNODB_METRICS                        |
    | INNODB_SYS_FOREIGN_COLS               |
    | INNODB_CMPMEM                         |
    | INNODB_BUFFER_POOL_STATS              |
    | INNODB_SYS_COLUMNS                    |
    | INNODB_SYS_FOREIGN                    |
    | INNODB_SYS_TABLESTATS                 |
    +---------------------------------------+
    61 rows in set (0.00 sec)
###
    mysql> create database mydb;
    Query OK, 1 row affected (0.00 sec)
###
    mysql> drop database mydb;
    Query OK, 0 rows affected (0.00 sec)
###
    mysql> create database morcl;
    Query OK, 1 row affected (0.00 sec)
###
    mysql> create user mscott;
    Query OK, 0 rows affected (0.00 sec)
###
    mysql> select * from user where user = 'mscott';
    ERROR 1109 (42S02): Unknown table 'user' in information_schema
###
    mysql> use mysql
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A
    
    Database changed
###
    mysql> select * from user where user = 'mscott'; 
    +------+--------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+-----------------------+-----------------------+------------------+-----------------------+-------------------+----------------+
    | Host | User   | Select_priv | Insert_priv | Update_priv | Delete_priv | Create_priv | Drop_priv | Reload_priv | Shutdown_priv | Process_priv | File_priv | Grant_priv | References_priv | Index_priv | Alter_priv | Show_db_priv | Super_priv | Create_tmp_table_priv | Lock_tables_priv | Execute_priv | Repl_slave_priv | Repl_client_priv | Create_view_priv | Show_view_priv | Create_routine_priv | Alter_routine_priv | Create_user_priv | Event_priv | Trigger_priv | Create_tablespace_priv | ssl_type | ssl_cipher | x509_issuer | x509_subject | max_questions | max_updates | max_connections | max_user_connections | plugin                | authentication_string | password_expired | password_last_changed | password_lifetime | account_locked |
    +------+--------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+-----------------------+-----------------------+------------------+-----------------------+-------------------+----------------+
    | %    | mscott | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N          | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N                | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | mysql_native_password |                       | N                | 2019-04-24 20:47:56   |              NULL | N              |
    +------+--------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+------------+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+-----------------------+-----------------------+------------------+-----------------------+-------------------+----------------+
    1 row in set (0.00 sec)
###
    mysql> drop user mscott;
    Query OK, 0 rows affected (0.00 sec)
###
    mysql> create user mscott@'%';
    Query OK, 0 rows affected (0.00 sec)
###
    mysql> GRANT ALL privileges ON morcl.* TO mscott@'%' IDENTIFIED BY 'tiger';
    Query OK, 0 rows affected, 1 warning (0.00 sec)
###
    mysql> \q
    Bye
###
    @KANG ~ $mysql -u mscott -p
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 6
    Server version: 5.7.25 Homebrew
    
    Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> 
###
    mysql> create table t1 (no int);
    ERROR 1046 (3D000): No database selected
    mysql> exit
    Bye
###
    @KANG ~ $mysql -u mscott -p morcl
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 7
    Server version: 5.7.25 Homebrew
    
    Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> 
###
    mysql> select database();
    +------------+
    | database() |
    +------------+
    | morcl      |
    +------------+
    1 row in set (0.00 sec)
###
    mysql> create table t1 (no int);
    Query OK, 0 rows affected (0.01 sec)
###
    mysql> CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20), species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
    Query OK, 0 rows affected (0.02 sec)
###
    mysql> CREATE TABLE event (name VARCHAR(20), date DATE, type VARCHAR(15), remark VARCHAR(255));
    Query OK, 0 rows affected (0.01 sec)
###
    mysql> SHOW TABLES;
    +-----------------+
    | Tables_in_morcl |
    +-----------------+
    | event           |
    | pet             |
    | t1              |
    +-----------------+
    3 rows in set (0.00 sec)
###
    mysql> DESCRIBE pet;
    +---------+-------------+------+-----+---------+-------+
    | Field   | Type        | Null | Key | Default | Extra |
    +---------+-------------+------+-----+---------+-------+
    | name    | varchar(20) | YES  |     | NULL    |       |
    | owner   | varchar(20) | YES  |     | NULL    |       |
    | species | varchar(20) | YES  |     | NULL    |       |
    | sex     | char(1)     | YES  |     | NULL    |       |
    | birth   | date        | YES  |     | NULL    |       |
    | death   | date        | YES  |     | NULL    |       |
    +---------+-------------+------+-----+---------+-------+
    6 rows in set (0.01 sec)
###
    mysql> desc event;
    +--------+--------------+------+-----+---------+-------+
    | Field  | Type         | Null | Key | Default | Extra |
    +--------+--------------+------+-----+---------+-------+
    | name   | varchar(20)  | YES  |     | NULL    |       |
    | date   | date         | YES  |     | NULL    |       |
    | type   | varchar(15)  | YES  |     | NULL    |       |
    | remark | varchar(255) | YES  |     | NULL    |       |
    +--------+--------------+------+-----+---------+-------+
    4 rows in set (0.00 sec)
###
    mysql> \q
    Bye
    @KANG ~ $
###
    @KANG ~ $mysql -u mscott -p morcl
    Enter password: 
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A
    
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 8
    Server version: 5.7.25 Homebrew
    
    Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> 
###
    mysql> LOAD DATA LOCAL INFILE '/Users/kang/Documents/PycharmProjects/TIL/HadoopEcosystem/data/pet.txt' INTO TABLE pet;
    Query OK, 8 rows affected, 9 warnings (0.00 sec)
    Records: 8  Deleted: 0  Skipped: 0  Warnings: 9
###
    mysql> LOAD DATA LOCAL INFILE '/Users/kang/Documents/PycharmProjects/TIL/HadoopEcosystem/data/event.txt' INTO TABLE event;
    Query OK, 10 rows affected (0.00 sec)
    Records: 10  Deleted: 0  Skipped: 0  Warnings: 0
###
    mysql> select * from pet;
    +--------+--------+---------+------+------------+-------+
    | name   | owner  | species | sex  | birth      | death |
    +--------+--------+---------+------+------------+-------+
    | Fluffy | Harold | cat     | f    | 1993-02-04 | NULL  |
    | Claws  | Gwen   | cat     | m    | 1994-03-17 | NULL  |
    | Buffy  | Harold | dog     | f    | 1989-05-13 | NULL  |
    | Fang   | Benny  | dog     | m    | 1990-08-27 | NULL  |
    | Bowser | Diane  | dog     | m    | 0000-00-00 | NULL  |
    | Chirpy | Gwen   | bird    | f    | 1998-09-11 | NULL  |
    | White  | Gwen   | bird    |      | 1997-12-09 | NULL  |
    | Slim   | Benny  | snake   | m    | 1996-04-29 | NULL  |
    +--------+--------+---------+------+------------+-------+
    8 rows in set (0.00 sec)
###
    mysql> select * from event;
    +--------+------------+----------+-----------------------------+
    | name   | date       | type     | remark                      |
    +--------+------------+----------+-----------------------------+
    | Fluffy | 1995-05-15 | litter   | 4 kittens, 3 female, 1 male |
    | Buffy  | 1993-06-23 | litter   | 5 puppies, 2 female, 3 male |
    | Buffy  | 1994-06-19 | litter   | 3 puppies, 3 female         |
    | Chirpy | 1999-03-21 | vet      | needed beak straightened    |
    | Slim   | 1997-08-03 | vet      | broken rib                  |
    | Bowser | 1991-10-12 | kennel   |                             |
    | Fang   | 1991-10-12 | kennel   |                             |
    | Fang   | 1998-08-28 | birthday | Gave him a new chew toy     |
    | Claws  | 1998-03-17 | birthday | Gave him a new flea collar  |
    | White  | 1998-12-09 | birthday | First birthday              |
    +--------+------------+----------+-----------------------------+
    10 rows in set (0.00 sec)
###
    mysql> INSERT INTO pet VALUES ('Puffball','Diane','hamster','f','1999-03-30',NULL);
    Query OK, 1 row affected (0.00 sec)
###
    mysql> UPDATE pet SET birth = '1989-08-31' WHERE name = 'Bowser';
    Query OK, 1 row affected (0.00 sec)
    Rows matched: 1  Changed: 1  Warnings: 0
###
    mysql> select * from pet;
    +----------+--------+---------+------+------------+-------+
    | name     | owner  | species | sex  | birth      | death |
    +----------+--------+---------+------+------------+-------+
    | Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL  |
    | Claws    | Gwen   | cat     | m    | 1994-03-17 | NULL  |
    | Buffy    | Harold | dog     | f    | 1989-05-13 | NULL  |
    | Fang     | Benny  | dog     | m    | 1990-08-27 | NULL  |
    | Bowser   | Diane  | dog     | m    | 1989-08-31 | NULL  |
    | Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL  |
    | White    | Gwen   | bird    |      | 1997-12-09 | NULL  |
    | Slim     | Benny  | snake   | m    | 1996-04-29 | NULL  |
    | Puffball | Diane  | hamster | f    | 1999-03-30 | NULL  |
    +----------+--------+---------+------+------------+-------+
    9 rows in set (0.00 sec)
###
    mysql> SELECT * FROM pet WHERE name = 'Bowser';
    +--------+-------+---------+------+------------+-------+
    | name   | owner | species | sex  | birth      | death |
    +--------+-------+---------+------+------------+-------+
    | Bowser | Diane | dog     | m    | 1989-08-31 | NULL  |
    +--------+-------+---------+------+------------+-------+
    1 row in set (0.00 sec)
###
    mysql> SELECT * FROM pet WHERE birth >= '1998-1-1';
    +----------+-------+---------+------+------------+-------+
    | name     | owner | species | sex  | birth      | death |
    +----------+-------+---------+------+------------+-------+
    | Chirpy   | Gwen  | bird    | f    | 1998-09-11 | NULL  |
    | Puffball | Diane | hamster | f    | 1999-03-30 | NULL  |
    +----------+-------+---------+------+------------+-------+
    2 rows in set (0.00 sec)
###
    mysql> SELECT * FROM pet WHERE species = 'dog' AND sex = 'f';
    +-------+--------+---------+------+------------+-------+
    | name  | owner  | species | sex  | birth      | death |
    +-------+--------+---------+------+------------+-------+
    | Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
    +-------+--------+---------+------+------------+-------+
    1 row in set (0.00 sec)
###
    mysql> SELECT * FROM pet WHERE species = 'snake' OR species = 'bird';
    +--------+-------+---------+------+------------+-------+
    | name   | owner | species | sex  | birth      | death |
    +--------+-------+---------+------+------------+-------+
    | Chirpy | Gwen  | bird    | f    | 1998-09-11 | NULL  |
    | White  | Gwen  | bird    |      | 1997-12-09 | NULL  |
    | Slim   | Benny | snake   | m    | 1996-04-29 | NULL  |
    +--------+-------+---------+------+------------+-------+
    3 rows in set (0.01 sec)
###
    mysql> SELECT * FROM pet 
        -> WHERE (species = 'cat' AND sex = 'm')
        -> OR (species = 'dog' AND sex = 'f');
    +-------+--------+---------+------+------------+-------+
    | name  | owner  | species | sex  | birth      | death |
    +-------+--------+---------+------+------------+-------+
    | Claws | Gwen   | cat     | m    | 1994-03-17 | NULL  |
    | Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
    +-------+--------+---------+------+------------+-------+
    2 rows in set (0.00 sec)
###
    mysql> help
    
    For information about MySQL products and services, visit:
       http://www.mysql.com/
    For developer information, including the MySQL Reference Manual, visit:
       http://dev.mysql.com/
    To buy MySQL Enterprise support, training, or other products, visit:
       https://shop.mysql.com/
    
    List of all MySQL commands:
    Note that all text commands must be first on line and end with ';'
    ?         (\?) Synonym for `help'.
    clear     (\c) Clear the current input statement.
    connect   (\r) Reconnect to the server. Optional arguments are db and host.
    delimiter (\d) Set statement delimiter.
    edit      (\e) Edit command with $EDITOR.
    ego       (\G) Send command to mysql server, display result vertically.
    exit      (\q) Exit mysql. Same as quit.
    go        (\g) Send command to mysql server.
    help      (\h) Display this help.
    nopager   (\n) Disable pager, print to stdout.
    notee     (\t) Don't write into outfile.
    pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
    print     (\p) Print current command.
    prompt    (\R) Change your mysql prompt.
    quit      (\q) Quit mysql.
    rehash    (\#) Rebuild completion hash.
    source    (\.) Execute an SQL script file. Takes a file name as an argument.
    status    (\s) Get status information from the server.
    system    (\!) Execute a system shell command.
    tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
    use       (\u) Use another database. Takes database name as argument.
    charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
    warnings  (\W) Show warnings after every statement.
    nowarning (\w) Don't show warnings after every statement.
    resetconnection(\x) Clean session context.
    
    For server side help, type 'help contents'
###
    mysql> ?
    
    For information about MySQL products and services, visit:
       http://www.mysql.com/
    For developer information, including the MySQL Reference Manual, visit:
       http://dev.mysql.com/
    To buy MySQL Enterprise support, training, or other products, visit:
       https://shop.mysql.com/
    
    List of all MySQL commands:
    Note that all text commands must be first on line and end with ';'
    ?         (\?) Synonym for `help'.
    clear     (\c) Clear the current input statement.
    connect   (\r) Reconnect to the server. Optional arguments are db and host.
    delimiter (\d) Set statement delimiter.
    edit      (\e) Edit command with $EDITOR.
    ego       (\G) Send command to mysql server, display result vertically.
    exit      (\q) Exit mysql. Same as quit.
    go        (\g) Send command to mysql server.
    help      (\h) Display this help.
    nopager   (\n) Disable pager, print to stdout.
    notee     (\t) Don't write into outfile.
    pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
    print     (\p) Print current command.
    prompt    (\R) Change your mysql prompt.
    quit      (\q) Quit mysql.
    rehash    (\#) Rebuild completion hash.
    source    (\.) Execute an SQL script file. Takes a file name as an argument.
    status    (\s) Get status information from the server.
    system    (\!) Execute a system shell command.
    tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
    use       (\u) Use another database. Takes database name as argument.
    charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
    warnings  (\W) Show warnings after every statement.
    nowarning (\w) Don't show warnings after every statement.
    resetconnection(\x) Clean session context.
    
    For server side help, type 'help contents'

# [3] Sqoop with MySQL
## [3-1] Sqoop 설치 전 상태 확인
* 내용 확인 : http://sqoop.apache.org
* Apache sqoop : https://www.slideshare.net/pavan5780/apache-sqoop-72298037
###
    # 실습을 위해 hadoop 유저로 로그인 뒤 reboot
    
    [hadoop@edydr1p0 ~]$ su -
    [root@edydr1p0 ~]# reboot
###
    # 필요한 구성요소 시작 및 상태 파악
    [orcl:~]$ su - oracle
    Password: 비밀번호
###
    [orcl:~]$ lsnrctl start   
    LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 24-APR-2019 21:00:30
    
    Copyright (c) 1991, 2009, Oracle.  All rights reserved.
    
    Starting /u01/app/oracle/product/11.2.0/dbhome_1/bin/tnslsnr: please wait...
    
    TNSLSNR for Linux: Version 11.2.0.1.0 - Production
    System parameter file is /u01/app/oracle/product/11.2.0/dbhome_1/network/admin/listener.ora
    Log messages written to /u01/app/oracle/diag/tnslsnr/edydr1p0/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=edydr1p0.us.oracle.com)(PORT=1521)))
    
    Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of the LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 11.2.0.1.0 - Production
    Start Date                24-APR-2019 21:00:30
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Parameter File   /u01/app/oracle/product/11.2.0/dbhome_1/network/admin/listener.ora
    Listener Log File         /u01/app/oracle/diag/tnslsnr/edydr1p0/listener/alert/log.xml
    Listening Endpoints Summary...
      (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=edydr1p0.us.oracle.com)(PORT=1521)))
    Services Summary...
    Service "orcl" has 1 instance(s).
      Instance "orcl", status UNKNOWN, has 1 handler(s) for this service...
    The command completed successfully
###    
    [orcl:~]$ sqlplus / as sysdba
    
    SQL*Plus: Release 11.2.0.1.0 Production on Wed Apr 24 21:00:39 2019
    
    Copyright (c) 1982, 2009, Oracle.  All rights reserved.
    
    Connected to an idle instance.
    
    SQL> 
###
    SQL> startup force
    ORACLE instance started.
    
    Total System Global Area 1489829888 bytes
    Fixed Size                  1336624 bytes
    Variable Size             872418000 bytes
    Database Buffers          603979776 bytes
    Redo Buffers               12095488 bytes
    Database mounted.
    Database opened.
###
    SQL> exit
    Disconnected from Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - Production
    With the Partitioning, OLAP, Data Mining and Real Application Testing options
###
    [orcl:~]$ exit
    logout
###
    [orcl:~]$ su - hadoop
    Password: 비밀번호
###
    [hadoop@edydr1p0 ~]$ start-all.sh
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out
###
    [hadoop@edydr1p0 ~]$ ps -ef|grep lsnr
    oracle    5215     1  0 21:00 ?        00:00:00 /u01/app/oracle/product/11.2.0/dbhome_1/bin/tnslsnr LISTENER -inherit
    hadoop    6110  5360  0 21:02 pts/1    00:00:00 grep lsnr
###    
    [hadoop@edydr1p0 ~]$ ps -ef|grep smon
    oracle    5269     1  0 21:00 ?        00:00:00 ora_smon_orcl
    hadoop    6112  5360  0 21:03 pts/1    00:00:00 grep smon
###
    [hadoop@edydr1p0 ~]$ jps
    6113 Jps
    5798 JobTracker
    5447 NameNode
    5578 DataNode
    5709 SecondaryNameNode
    5934 TaskTracker

## [3-2] Sqoop 설치
    [hadoop@edydr1p0 ~]$ cd
    [hadoop@edydr1p0 ~]$ whoami
    hadoop
    [hadoop@edydr1p0 ~]$ mkdir sqoop-install
    [hadoop@edydr1p0 ~]$ cd sqoop-install
    [hadoop@edydr1p0 sqoop-install]$ 
###
    # sqoop 1.4.4 다운로드 
    [hadoop@edydr1p0 sqoop-install]$ wget https://archive.apache.org/dist/sqoop/1.4.4/sqoop-1.4.4.bin__hadoop-1.0.0.tar.gz
    --2019-04-25 01:04:14--  https://archive.apache.org/dist/sqoop/1.4.4/sqoop-1.4.4.bin__hadoop-1.0.0.tar.gz
    Resolving archive.apache.org... 163.172.17.199
    Connecting to archive.apache.org|163.172.17.199|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 5266542 (5.0M) [application/x-gzip]
    Saving to: `sqoop-1.4.4.bin__hadoop-1.0.0.tar.gz'
    
    100%[======================================>] 5,266,542    106K/s   in 42s     
    
    2019-04-25 01:04:58 (121 KB/s) - `sqoop-1.4.4.bin__hadoop-1.0.0.tar.gz' saved [5266542/5266542]
    
###
    # sqoop 압축 해제 
    [hadoop@edydr1p0 sqoop-install]$ tar zxvf sqoop-1.4.4.bin__hadoop-1.0.0.tar.gz
###
    [hadoop@edydr1p0 sqoop-install]$ vi + ../.bash_profile
    # vi 편집기 실행
    export SQOOP_HOME=/home/hadoop/sqoop-install/sqoop-1.4.4.bin__hadoop-1.0.0
    export SQOOP_CONF_DIR=/home/hadoop/sqoop-install/sqoop-1.4.4.bin__hadoop-1.0.0/conf
    export PATH=$PATH:$SQOOP_HOME/bin
    # vi 편집기 종료
<img width="1083" alt="Screen Shot 2019-04-24 at 9 05 48 PM" src="https://user-images.githubusercontent.com/46523571/56671308-5e7d5600-66ef-11e9-92ab-3de16309bc6c.png">

###
    [hadoop@edydr1p0 sqoop-install]$ source ../.bash_profile
###
    [hadoop@edydr1p0 sqoop-install]$ cd $SQOOP_HOME/conf
###
    [hadoop@edydr1p0 conf]$ pwd
    /home/hadoop/sqoop-install/sqoop-1.4.4.bin__hadoop-1.0.0/conf
###
    [hadoop@edydr1p0 conf]$ ls $HADOOP_HOME/*core*
    /opt/hadoop/hadoop/hadoop-core-1.2.1.jar
###
    [hadoop@edydr1p0 conf]$ ls $HADOOP_HOME/bin/h*
    /opt/hadoop/hadoop/bin/hadoop            /opt/hadoop/hadoop/bin/hadoop-daemon.sh
    /opt/hadoop/hadoop/bin/hadoop-config.sh  /opt/hadoop/hadoop/bin/hadoop-daemons.sh
###
    [hadoop@edydr1p0 conf]$ cp sqoop-env-template.sh sqoop-env.sh
    [hadoop@edydr1p0 conf]$ vi + sqoop-env.sh
    # vi 편집기 실행해서 아래 내용 아래 코드 추가
    #Set path to where bin/hadoop is available
    export HADOOP_COMMON_HOME=$HADOOP_HOME
    #Set path to where hadoop-*-core.jar is available
    export HADOOP_MAPRED_HOME=$HADOOP_HOME
    # vi 편집기 종료

    [hadoop@edydr1p0 conf]$ cd
<img width="1081" alt="Screen Shot 2019-04-24 at 9 08 51 PM" src="https://user-images.githubusercontent.com/46523571/56675295-b8cde500-66f6-11e9-8ce0-5301e4fd6923.png">

<br>
<br>
<br>

## [3-3] Sqoop import 테스트 : from MySQL
    [hadoop@edydr1p0 ~]$ sqoop import --connect jdbc:mysql://70.12.114.169/morcl --table pet --username mscott -m 1
    Warning: /usr/lib/hbase does not exist! HBase imports will fail.
    Please set $HBASE_HOME to the root of your HBase installation.
    Warning: /usr/lib/hcatalog does not exist! HCatalog jobs will fail.
    Please set $HCAT_HOME to the root of your HCatalog installation.
    19/04/24 21:09:50 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
    19/04/24 21:09:50 INFO tool.CodeGenTool: Beginning code generation
    19/04/24 21:09:50 ERROR sqoop.Sqoop: Got exception running Sqoop: java.lang.RuntimeException: Could not load db driver class: com.mysql.jdbc.Driver
    java.lang.RuntimeException: Could not load db driver class: com.mysql.jdbc.Driver
            at org.apache.sqoop.manager.SqlManager.makeConnection(SqlManager.java:772)
            at org.apache.sqoop.manager.GenericJdbcManager.getConnection(GenericJdbcManager.java:52)
            at org.apache.sqoop.manager.SqlManager.execute(SqlManager.java:660)
            at org.apache.sqoop.manager.SqlManager.execute(SqlManager.java:683)
            at org.apache.sqoop.manager.SqlManager.getColumnTypesForRawQuery(SqlManager.java:240)
            at org.apache.sqoop.manager.SqlManager.getColumnTypes(SqlManager.java:223)
            at org.apache.sqoop.manager.ConnManager.getColumnTypes(ConnManager.java:347)
            at org.apache.sqoop.orm.ClassWriter.getColumnTypes(ClassWriter.java:1277)
            at org.apache.sqoop.orm.ClassWriter.generate(ClassWriter.java:1089)
            at org.apache.sqoop.tool.CodeGenTool.generateORM(CodeGenTool.java:96)
            at org.apache.sqoop.tool.ImportTool.importTable(ImportTool.java:396)
            at org.apache.sqoop.tool.ImportTool.run(ImportTool.java:502)
            at org.apache.sqoop.Sqoop.run(Sqoop.java:145)
            at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:65)
            at org.apache.sqoop.Sqoop.runSqoop(Sqoop.java:181)
            at org.apache.sqoop.Sqoop.runTool(Sqoop.java:220)
            at org.apache.sqoop.Sqoop.runTool(Sqoop.java:229)
            at org.apache.sqoop.Sqoop.main(Sqoop.java:238)
    --> 에러 발생 Could not load db driver class : JDBC 드라이버 설정 필요.
###
    [hadoop@edydr1p0 ~]$ cd $SQOOP_HOME
    [hadoop@edydr1p0 sqoop-1.4.4.bin__hadoop-1.0.0]$ pwd
    /home/hadoop/sqoop-install/sqoop-1.4.4.bin__hadoop-1.0.0
###
    [hadoop@edydr1p0 sqoop-1.4.4.bin__hadoop-1.0.0]$ ls
    bin            COMPILING.txt  ivy      LICENSE.txt  README.txt            src
    build.xml      conf           ivy.xml  NOTICE.txt   sqoop-1.4.4.jar       testdata
    CHANGELOG.txt  docs           lib      pom-old.xml  sqoop-test-1.4.4.jar
###
    [hadoop@edydr1p0 sqoop-1.4.4.bin__hadoop-1.0.0]$ wget http://70.12.114.169/Share/mysql-connector-java-5.1.28.tar.gz
    [hadoop@edydr1p0 sqoop-1.4.4.bin__hadoop-1.0.0]$ tar zxvf mysql-connector-java-5.1.28.tar.gz
    [hadoop@edydr1p0 sqoop-1.4.4.bin__hadoop-1.0.0]$ cp mysql-connector-java-5.1.28/mysql-connector-java-5.1.28-bin.jar $SQOOP_HOME/lib
    [hadoop@edydr1p0 sqoop-1.4.4.bin__hadoop-1.0.0]$ cd
    
    [hadoop@edydr1p0 ~]$ sqoop help import





  [hadoop@edydr1p0 ~]$ sqoop import --connect jdbc:mysql://70.12.114.169/morcl --table pet --username mscott -P -m 1

  [hadoop@edydr1p0 ~]$ hadoop fs -lsr

  [hadoop@edydr1p0 ~]$ hadoop fs -cat /user/hadoop/pet/part-m-00000

  [hadoop@edydr1p0 ~]$ hive




## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>