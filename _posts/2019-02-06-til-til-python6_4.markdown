---
layout: post
title: '[python] 파이썬 Pandas with MySQL'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-06 00:12:17 +0900
lastmod: 2019-02-06 14:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Pandas with MySQL에 대해서 <br />

# Pandas with MySQL
> MySQL 개발환경 구축된 상태에서 진행 가능합니다.

# PyMySQL
> MySQL을 사용하기 위해 module<br>

## **PyMySQL 설치**

* 터미널 실행


        $ KANGs-MacBook-Pro ~ $ cd ~
          ↳ cd ~ : 최상위 폴더로 이동

* 가상환경 접속


        $ KANGs-MacBook-Pro ~ $ source activate data_env

* pymysql 설치


        $ (data_env) KANGs-MacBook-Pro:~ kang$ conda install pymysql
        Solving environment: done
        
        
        ==> WARNING: A newer version of conda exists. <==
          current version: 4.5.12
          latest version: 4.6.4
        
        Please update conda by running
        
            $ conda update -n base -c defaults conda
        
        
        
        ## Package Plan ##
        
          environment location: /anaconda3/envs/data_env
        
          added / updated specs: 
            - pymysql
        
        
        The following packages will be downloaded:
        
            package                    |            build
            ---------------------------|-----------------
            idna-2.8                   |           py36_0         133 KB
            pymysql-0.9.3              |           py36_0          83 KB
            asn1crypto-0.24.0          |           py36_0         154 KB
            cffi-1.11.5                |   py36h6174b99_1         205 KB
            cryptography-2.5           |   py36ha12b0ac_0         590 KB
            pycparser-2.19             |           py36_0         173 KB
            ------------------------------------------------------------
                                                   Total:         1.3 MB
        
        The following NEW packages will be INSTALLED:
        
            asn1crypto:   0.24.0-py36_0        
            cffi:         1.11.5-py36h6174b99_1
            cryptography: 2.5-py36ha12b0ac_0   
            idna:         2.8-py36_0           
            pycparser:    2.19-py36_0          
            pymysql:      0.9.3-py36_0         
        
        
        Proceed ([y]/n)? y
         ↳ 설치를 원할 시 y 버튼을 클릭하고 엔터!
        
        
        Downloading and Extracting Packages
        idna-2.8             | 133 KB    | ##################################### | 100% 
        pymysql-0.9.3        | 83 KB     | ##################################### | 100% 
        asn1crypto-0.24.0    | 154 KB    | ##################################### | 100% 
        cffi-1.11.5          | 205 KB    | ##################################### | 100% 
        cryptography-2.5     | 590 KB    | ##################################### | 100% 
        pycparser-2.19       | 173 KB    | ##################################### | 100% 
        Preparing transaction: done
        Verifying transaction: done
        Executing transaction: done
        (data_env) KANGs-MacBook-Pro:~ kang$ 


 
* 이제 jupyter notebook에서 pymysql import 사용 가능.



<br>
<br>

## MySQL Connection(1. 접속 및 출력방법)
* Database로부터 Data를 읽어서 DataFrame으로 생성
* mysql database를 이용
* pymysql module을 이용
###
    import pymysql.cursors   # mysql을 사용하기 위해 module import
    import numpy as np
    import pandas as pd
    
    # 1. mysql을 사용하려면 port번호를 알아야한다. 그래야 database를 접근 할 수 있다.
    # 2. ID, PW를 알아야 database에 접근을 할 수 있다.
    # 3. 어떤 database에 접근할 것인지 지정해줘야 한다.
    # 4. 그래야 Table 사용 가능
    
    # mysql, oracle, DB2, informix, cybase : DMBS라고 하며 Database는 아니다.
    # DBMS : 데이터의 논리적인 집합체를 관리하는 툴을 DBMS라고 한다.
    # Database : 데이터의 논리적인 집합체
    # MySQL Database connection 방법
    conn = pymysql.connect(host = "localhost",      # mysql이 설치된 컴퓨터 IP에 접속  #함수
                           user = "python",         # 사용자 user ID
                           password = "python",     # 사용자 password
                           db = "library",          # 사용할 database
                           charset = "utf8")        # 사용할 character set
    # 결과값 : <pymysql.connections.Connection at 0x10594ec88> 이렇게 나오면 연결 완료(커넥션객체 나옴)
    
    # SQL을 작성
    # SQL 예약어는 관용적으로 대문자를 쓴다.
    # 강사님이 제공한 script 파일에 books란 table이 존재한다.
    # table에서 책제목(btitle) 중 특정 keyword 추출
    keyword = input("키워드를 입력하세요")   # 사용자가 키워드를 넣어서 검색 할 수 있다.
    #keyword = "java"      # java가 들어간 키워드
    
    sql = "SELECT bisbn, btitle, bauthor, bprice \
           FROM books \
           WHERE btitle \
           LIKE '%{}%'".format(keyword)     # %는 0개 이상의 문자열을 나타냄
    # pandas가 sql을 실행시켜서 그 결과를 DataFrame으로 생성
    df = pd.read_sql(sql, con = conn)   # sql을 연결된 데이터베이스(conn)을 연결해(con)
    display(df)
    conn.close()   # Database 연결 종료 해야한다. #종료 안하면 메모리 누수가 발생해 튕김현상이 일어난다.
<img width="707" alt="Screen Shot 2019-03-25 at 5 51 18 PM" src="https://user-images.githubusercontent.com/46523571/54906472-a3627100-4f26-11e9-95b0-aadde804a199.png">
<img width="1020" alt="Screen Shot 2019-03-25 at 5 51 26 PM" src="https://user-images.githubusercontent.com/46523571/54906483-aa897f00-4f26-11e9-9e74-27ed95960543.png">

## MySQL Connection(2. JSON 파일로 저장 방법)
* mysql -> JSON 파일형식으로 저장방법
* mysql을 통해서 나온 결과값은 메모리에 저장되어 있기 때문에 프로그램 종료시 없어진다.
* 그래서 이렇게 Database로부터 얻어낸 data를 JSON 파일 형태로 저장할 수 있다.
    
* Database로부터 Data를 읽어서 DataFrame으로 생성
* mysql database를 이용
* pymysql module을 이용
###    
    import pymysql.cursors   #mysql을 사용하기 위해 module import
    import numpy as np
    import pandas as pd
    
    # 1. mysql을 사용하려면 port번호를 알아야한다. 그래야 database를 접근 할 수 있다.
    # 2. ID, PW를 알아야 database에 접근을 할 수 있다.
    # 3. 어떤 database에 접근할 것인지 지정해줘야 한다.
    # 4. 그래야 Table 사용 가능
    
    # mysql, oracle, DB2, informix, cybase : DMBS라고 하며 Database는 아니다.
    # DBMS : 데이터의 논리적인 집합체를 관리하는 툴을 DBMS라고 한다.
    # Database : 데이터의 논리적인 집합체
    # MySQL Database connection 방법
    conn = pymysql.connect(host = "localhost",      # mysql이 설치된 컴퓨터 IP에 접속  #함수
                           user = "python",         # 사용자 user ID
                           password = "python",     # 사용자 password
                           db = "library",          # 사용할 database
                           charset = "utf8")        # 사용할 character set
    # 결과값 : <pymysql.connections.Connection at 0x10594ec88> 이렇게 나오면 연결 완료(커넥션객체 나옴)
    
    # SQL을 작성
    # SQL 예약어는 관용적으로 대문자를 쓴다.
    # 강사님이 제공한 script 파일에 books란 table이 존재한다.
    # table에서 책제목(btitle) 중 특정 keyword 추출
    keyword = input("키워드를 입력하세요")   # 사용자가 키워드를 넣어서 검색 할 수 있다.
    #keyword = "java"      # java가 들어간 키워드
    
    sql = "SELECT bisbn, btitle, bauthor, bprice \
           FROM books \
           WHERE btitle \
           LIKE '%{}%'".format(keyword)     # %는 0개 이상의 문자열을 나타냄
    # pandas가 sql을 실행시켜서 그 결과를 DataFrame으로 생성
    df = pd.read_sql(sql, con = conn)   #sql을 연결된 데이터베이스(conn)을 연결해(con)
    #이렇게 만들어진 DataFrame을 파일에 JSON형태로 저장
    df.to_json("./data/sample/books.json")   #파일을 저장할 경로
    conn.close()   #Database 연결 종료 해야한다. #종료 안하면 메모리 누수가 발생해 튕김현상이 일어난다.
    
    #./data/sample/books.json 경로에 파일이 생성됨.

## MySQL JSON 파일 다루는법
    # JSON file로부터 데이터를 읽어서 dictionary로 만들어보자.
    # JSON이라는 데이터표현형태는 python의 dictionary와 같은 형태를 가진다.
    
    import numpy as np
    import pandas as pd
    import json
    
    # (1)
    # 일단 python의 파일처리기능을 이용해서 file을 열고 데이터를 읽어들인다.
    file = open("./data/sample/books.json", "r")   #(경로, (읽을목적(r), 쓸목적(w)) 용도를 적는다.
    my_dict = json.load(file)
    file.close()
    
    for tmp in my_dict["btitle"].values():
        print(tmp)
    
    df = pd.DataFrame(my_dict)
    display(df)
<img width="1014" alt="Screen Shot 2019-03-25 at 5 54 20 PM" src="https://user-images.githubusercontent.com/46523571/54906625-081dcb80-4f27-11e9-9d0b-c2967d1c211c.png">


## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>