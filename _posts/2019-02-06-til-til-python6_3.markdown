---
layout: post
title: '[python] 파이썬 Pandas Aggregate Function'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-06 00:12:17 +0900
lastmod: 2019-02-06 13:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Pandas Aggregate Function에 대해서 <br />

# Pandas DataReader
> Pandas DataReader Package<br>
> 웹 상의 데이터를 Pandas DataFrame 객체로 만드는 기능을 제공<br>

## **Pandas DataReader Package 설치**

* 터미널 실행


        $ KANGs-MacBook-Pro ~ $ cd ~
          ↳ cd ~ : 최상위 폴더로 이동

* 가상환경 접속


        $ KANGs-MacBook-Pro ~ $ source activate data_env

* pandas_datareader 설치


        $ (data_env) KANGs-MacBook-Pro:~ kang$ pip install pandas_datareader
        Collecting pandas_datareader
          Downloading https://files.pythonhosted.org/packages/cc/5c/ea5b6dcfd0f55c5fb1e37fb45335ec01cceca199b8a79339137f5ed269e0/pandas_datareader-0.7.0-py2.py3-none-any.whl (111kB)
            100% |████████████████████████████████| 112kB 1.3MB/s 
        Requirement already satisfied: pandas>=0.19.2 in /anaconda3/envs/data_env/lib/python3.6/site-packages (from pandas_datareader) (0.24.1)
        Collecting wrapt (from pandas_datareader)
          Downloading https://files.pythonhosted.org/packages/67/b2/0f71ca90b0ade7fad27e3d20327c996c6252a2ffe88f50a95bba7434eda9/wrapt-1.11.1.tar.gz
        Collecting requests>=2.3.0 (from pandas_datareader)
          Downloading https://files.pythonhosted.org/packages/7d/e3/20f3d364d6c8e5d2353c72a67778eb189176f08e873c9900e10c0287b84b/requests-2.21.0-py2.py3-none-any.whl (57kB)
            100% |████████████████████████████████| 61kB 4.6MB/s 
        Collecting lxml (from pandas_datareader)
          Downloading https://files.pythonhosted.org/packages/b5/5c/27b381a7d5b24ee4b9da1b02269a305bdc4fc30a3ea5bb21a5a98194850d/lxml-4.3.1-cp36-cp36m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (8.8MB)
            100% |████████████████████████████████| 8.8MB 5.4MB/s 
        Requirement already satisfied: python-dateutil>=2.5.0 in /anaconda3/envs/data_env/lib/python3.6/site-packages (from pandas>=0.19.2->pandas_datareader) (2.7.5)
        Requirement already satisfied: numpy>=1.12.0 in /anaconda3/envs/data_env/lib/python3.6/site-packages (from pandas>=0.19.2->pandas_datareader) (1.16.1)
        Requirement already satisfied: pytz>=2011k in /anaconda3/envs/data_env/lib/python3.6/site-packages (from pandas>=0.19.2->pandas_datareader) (2018.9)
        Collecting urllib3<1.25,>=1.21.1 (from requests>=2.3.0->pandas_datareader)
          Downloading https://files.pythonhosted.org/packages/62/00/ee1d7de624db8ba7090d1226aebefab96a2c71cd5cfa7629d6ad3f61b79e/urllib3-1.24.1-py2.py3-none-any.whl (118kB)
            100% |████████████████████████████████| 122kB 3.1MB/s 
        Requirement already satisfied: certifi>=2017.4.17 in /anaconda3/envs/data_env/lib/python3.6/site-packages (from requests>=2.3.0->pandas_datareader) (2018.11.29)
        Requirement already satisfied: idna<2.9,>=2.5 in /anaconda3/envs/data_env/lib/python3.6/site-packages (from requests>=2.3.0->pandas_datareader) (2.8)
        Collecting chardet<3.1.0,>=3.0.2 (from requests>=2.3.0->pandas_datareader)
          Downloading https://files.pythonhosted.org/packages/bc/a9/01ffebfb562e4274b6487b4bb1ddec7ca55ec7510b22e4c51f14098443b8/chardet-3.0.4-py2.py3-none-any.whl (133kB)
            100% |████████████████████████████████| 143kB 19.2MB/s 
        Requirement already satisfied: six>=1.5 in /anaconda3/envs/data_env/lib/python3.6/site-packages (from python-dateutil>=2.5.0->pandas>=0.19.2->pandas_datareader) (1.12.0)
        Building wheels for collected packages: wrapt
          Building wheel for wrapt (setup.py) ... done
          Stored in directory: /Users/kang/Library/Caches/pip/wheels/89/67/41/63cbf0f6ac0a6156588b9587be4db5565f8c6d8ccef98202fc
        Successfully built wrapt
        Installing collected packages: wrapt, urllib3, chardet, requests, lxml, pandas-datareader
        Successfully installed chardet-3.0.4 lxml-4.3.1 pandas-datareader-0.7.0 requests-2.21.0 urllib3-1.24.1 wrapt-1.11.1

 
* 이제 jupyter notebook에서 pandas_datareader import 사용 가능.



# Pandas Aggregate Function
> pandas : python의 data analysis의 핵심 library<br>
> pandas는 고유하게 정의된 두개의 자료구조를 이용(시리즈, 데이터프레임)<br>

<br>
<br>

## DataFrame의 집계함수
* 변량 : 숫자들 ~ 평균 : 변량의 합 / 인원 수 편차 : 변량 - 평균 편차제곱 : 편차 * 편차 분산 : 편차의 제곱의 평균 표준편차 : 루트 분산
    - 크다 : 평균을 기준으로 데이터가 흩어져있다라는 의미
    - 작다 : 평균을 기준으로 데이터가 오밀조밀 모여있다는 의미

## DataFrame의 생성과 사용에 기본적인 내용들.
* 평균(mean) : 여러개의 값이 있을 때, 그 값들에 대한 합을 개수로 나눈값, 수학적 확률의 기대치
* 편차(deviation) : 확률변수 X와 평균값에 대한 차이
* 분산(variance) : 편차의 제곱의 평균
* 표준편차(standard deviation) : 분산의 제곱근
###
    import numpy as np
    
    arr = np.array([1,2,3,4,5], dtype=np.int64)
    print(arr.sum())    # 합계    # 결과값 : 15
    print(arr.mean())   # 평균    # 결과값 : 3.0
    print(arr.var())    # 분산    # 결과값 : 2.0
    print(arr.std())    # 표준편차 # 결과값 : 1.4142135623730951

## 공분산(cov, covariance)
* 확률 변수가 2개일 때의 관계를 알아 볼때 공분산을 쓴다.
###
    # 확률변수 X와 Y의 관계를 알기 위해서 사용하는 값
    # X가 변할때 Y가 변하는 방향을 알기 위해서 사용
    # X, Y가 서로 독립이면 공분산 0
    # 공분산이 양수일경우 X가 증가하면 Y도 증가
    # 공분산이 음수일경우 X가 증가하면 Y도 감소
    
    # 양의 상관관계 ex) 삼성이 오르면 대체적으로 코스피도 오른다.
    # 음의 상관관계 ex) 남북경헙주 & 방산주
    
    # 두 확률변수의 관계정도(strength)를 측정하기가 좋지 않다. => 상관계수
    # 양수든 음수든 상관없이 두개의 밀접한 관계를 알 수 있는 건 => 상관계수

## 공분산이 양수인 경우
* 예) KOSPI지수와 삼성전자 주가
###
    # 사이트에서 바로 분석하는 방법
    import numpy as np
    import pandas as pd
    import datetime
    import pandas_datareader.data as pdr
    
    start = datetime.datetime(2018,1,1)
    end = datetime.datetime(2018,12,31)
    
    # Yahoo에서 제공하는 코스피 지수정보를 가져와서 DataFrame
    df_KOSPI = pdr.DataReader("^KS11","yahoo",start,end)   #사이트에서 정보 가져오기
    df_KOSPI.to_json("./data/stock/KOSPI.json")   # 정보를 파일로 저장
    df_KOSPI.head(5)
<img width="712" alt="Screen Shot 2019-03-25 at 6 32 53 PM" src="https://user-images.githubusercontent.com/46523571/54909001-83ce4700-4f2c-11e9-9c03-278b5e84f1c6.png">

## 공분산이 양수인 경우
* 예) KOSPI지수와 삼성전자 주가
* 위의 방법으로 접속 불가능시 아래와 같이 실행
* 파일로 분석하는 방법
### 
    import numpy as np
    import pandas as pd
    import datetime
    import pandas_datareader.data as pdr
    
    start = datetime.datetime(2018,1,1)
    end = datetime.datetime(2018,12,31)
    
    #코스피 주식 가져오기
    #df_KOSPI = pd.read_json("./data/stock/KOSPI.json")   #JSON 파일 정보 전체 가져오기
    df_KOSPI = pd.read_json("./data/stock/KOSPI.json")["Close"]   #"Close" column만 가져오기(Series형태이다.)   
    #print(df_KOSPI.head(5))
    
    #삼성전자 주식 가져오기
    #df_SE = pd.read_json("./data/stock/SE.json")   #JSON 파일 정보 전체 가져오기
    df_SE = pd.read_json("./data/stock/SE.json")["Close"]   #"Close" column만 가져오기(Series형태이다.)   
    #print(df_SE.head(5))
    
    # KOSPI와 SE이 두개의 확률변수를 가지고 공분산(covariance)을 구해보자.
    np.cov(df_KOSPI, df_SE)
    
    # 결과값
    # array([[   24177.23140621,   490222.10530186],
    #        [  490222.10530186, 11919911.50745464]])
    # (0,0) KOSPI KOSPI
    # (0,1) KOSPI & SE
    # (1,0) SE & KOSPI
    # (1,1) SE & SE
    # 값의 의미는 모르겠으나 양수이기 때문에 양의 공분산이라는걸 알 수 있다.

## 공분산이 음수인 경우¶
* 예) 남북경협주와 방산주
###
    # 파일로 분석하는 방법
    
    import numpy as np
    import pandas as pd
    import datetime
    import pandas_datareader.data as pdr
    
    start = datetime.datetime(2018,1,1)
    end = datetime.datetime(2018,12,31)
    
    #부산산업 주식 가져오기
    #df_BUSAN = pd.read_json("./data/stock/부산산업.json")   #JSON 파일 정보 전체 가져오기
    df_BUSAN = pd.read_json("./data/stock/부산산업.json")["Close"]   #"Close" column만 가져오기(Series형태이다.)   
    #print(df_BUSAN.head(5))
    
    #LIG넥스원 주식 가져오기
    #df_LIG = pd.read_json("./data/stock/LIG넥스원.json")   #JSON 파일 정보 전체 가져오기
    df_LIG = pd.read_json("./data/stock/LIG넥스원.json")["Close"]   #"Close" column만 가져오기(Series형태이다.)   
    #print(df_LIG.head(5))
    
    # 부산산업과 LIG넥스원 두개의 확률변수를 가지고 공분산(covariance)을 구해보자.
    np.cov(df_BUSAN, df_LIG)
    
    # 결과값
    # array([[ 4.64762211e+09, -3.86535936e+08],
    #        [-3.86535936e+08,  6.35924170e+07]])
    # (0,0) BUSAN BUSAN
    # (0,1) BUSAN LIG
    # (1,0) LIG BUSAN
    # (1,1) LIG LIG
    # 값의 의미는 모르겠으나 양수이기 때문에 양의 공분산이라는걸 알 수 있다.

## 상관관계(corelation), 상관계수(corelation coefficient)
* 상관관계(corelation)
  - 서로 연관이 있어보이는 관계   ex) 성적과 자존감, 온라인게임과 폭력성
* 상관계수(corelation coefficient)
  - 값이 항상 -1 ~ +1 사이의 실수로 표현한다.
  - 0에 가까울 수록 서로 연관성이 없는 관계를 나타낸다.
  - 절대값이 1에 가까울수록 연관성이 높다.
  - 상관계수는 두개의 확률변수에 대한 연관성에 대한 지표이지 인과관계(원인과 결과)를 설명하지 않는다.
  - 그래서 오류의 소지가 다분하다.

## 상관계수 구해보기(1)
    import numpy as np
    import pandas as pd
    import datetime
    import pandas_datareader.data as pdr
    
    start = datetime.datetime(2018,1,1)
    end = datetime.datetime(2018,12,31)
    
    df_KOSPI = pd.read_json("./data/stock/KOSPI.json")["Close"]      
    df_SE = pd.read_json("./data/stock/SE.json")["Close"]    
    
    np.corrcoef(df_KOSPI, df_SE)
    
    # 결과값
    # array([[1.        , 0.91317306],
    #        [0.91317306, 1.        ]])
    # (0,0) df_KOSPI df_KOSPI
    # (0,1) df_KOSPI df_SE
    # (1,0) df_SE df_KOSPI
    # (1,1) df_SE df_SE
    
    # 0~0.3 약한관계, 0.3~0.7 중간관계, 0.7~1 강한관계를 뜻한다.

## 상관계수 구해보기(2)
    import numpy as np
    import pandas as pd
    import datetime
    import pandas_datareader.data as pdr
    
    start = datetime.datetime(2018,1,1)
    end = datetime.datetime(2018,12,31)
    
    df_BUSAN = pd.read_json("./data/stock/부산산업.json")["Close"]     
    df_LIG = pd.read_json("./data/stock/LIG넥스원.json")["Close"]   
    
    np.corrcoef(df_BUSAN, df_LIG)
    
    # 결과값 
    # array([[ 1.        , -0.71100361],
    #        [-0.71100361,  1.        ]])
    
    # 0~0.3 약한관계, 0.3~0.7 중간관계, 0.7~1 강한관계를 뜻한다.

## pandas 통계적 함수 사용
    # numpy를 이용해서 통계적인 내용을 해왔다.
    # 이번엔 pandas(DataFrame)를 이용한 통계적인 함수 사용법을 알아보자.
    import numpy as np
    import pandas as pd
    
    data = [[2, np.nan],   # 4행 2열의 리스트
            [7, -3],
            [np.nan, np.nan],
            [1, -2]]
    
    df = pd.DataFrame(data,
                      columns=["one","two"],
                      index=["a","b","c","d"])
    display(df)
    display(df.sum())   # axis가 생략, axis=0 default
    display(df.mean())   # default로 NaN은 배제하고 계산
    display(df.sum(axis=1))   # 축을 기준으로 Series형태로 계산된다.
<img width="194" alt="Screen Shot 2019-03-25 at 6 38 41 PM" src="https://user-images.githubusercontent.com/46523571/54909307-39999580-4f2d-11e9-8eab-36c65ddc74b4.png">

## 간단한 문제
* 결측값이 있으면 계산이 불확실해진다.
* 그래서 결륵값을 해결해야 한다. (삭제 또는 다른값으로 대체해야한다.)
* one column의 결측값은 one column의 평균으로 대체
* two column의 결측값은 two column의 최소값으로 대체
* 결측값을 대체하고 DataFrame을 출력
###   
    import numpy as np
    import pandas as pd
    
    data = [[2, np.nan],   # 4행 2열의 리스트
            [7, -3],
            [np.nan, np.nan],
            [1, -2]]
    
    df = pd.DataFrame(data,
                      columns=["one","two"],
                      index=["a","b","c","d"])
    print("▼ 기본")
    display(df)
    
    one_value = df["one"].mean()   # one column의 평균
    two_value = df["two"].min()   # two column의 최소값
    
    df["one"] = df["one"].fillna(value=one_value)
    df["two"] = df["two"].fillna(value=two_value)
    
    print("▼ 결과값")
    display(df)   # 결측값을 대체하고 DataFrame을 출력
<img width="171" alt="Screen Shot 2019-03-25 at 6 40 06 PM" src="https://user-images.githubusercontent.com/46523571/54909391-6cdc2480-4f2d-11e9-9ce7-110920967551.png">

## DataFrame에서 공분산과 상관계수 구하기 +@ JSON 파일 기준으로
    import numpy as np
    import pandas as pd
    
    df_BUSAN = pd.read_json("./data/stock/부산산업.json")["Close"]     
    df_LIG = pd.read_json("./data/stock/LIG넥스원.json")["Close"]   
    df_KOSPI = pd.read_json("./data/stock/KOSPI.json")["Close"]      
    df_SE = pd.read_json("./data/stock/SE.json")["Close"]   
    
    data = {"KOSPI" : df_KOSPI,
            "SE" : df_SE,
            "LIG" : df_LIG,
            "BUSAN" : df_BUSAN}
    df = pd.DataFrame(data)
    df["BUSAN"].cov(df["KOSPI"])   #공분산 구하기       -6113069.91603586
    df["BUSAN"].corr(df["KOSPI"])   #상관계수 구하기     -0.5766876815236214
    
    display(df.cov())    # DataFrame 공분산 구하기
    display(df.corr())   # DataFrame 상관계수 구하기
<img width="574" alt="Screen Shot 2019-03-25 at 6 40 42 PM" src="https://user-images.githubusercontent.com/46523571/54909418-82e9e500-4f2d-11e9-9f35-e704fd80249a.png">

## DataFrame이 제공하는 유용한 함수
*  정렬(1) index를 통해서 정렬
###
    import numpy as np
    import pandas as pd
    
    np.random.seed(0)
    df = pd.DataFrame(np.random.randint(0,10,(6,4)),
                      columns=["A","B","C","D"],
                      index=pd.date_range("20190101",periods=6))
    random_index = np.random.permutation(df.index)   # np가 제공하는 랜덤을 순열(연속적인 값의 집합)
    
    df2 = df.reindex(index=random_index,
                     columns=["B","D","C","A"])   #
    display(df2)
    
    
    # sort_index() 인덱스를 이용해서 재정렬 할 수 있다.
    display(df2.sort_index(axis=0, ascending=False))   #axis=0(행 방향으로 정렬)   #False(내림차순)
    display(df2.sort_index(axis=1, ascending=False))   #axis=1(열 방향으로 정렬)   #False(내림차순)
                                  #default값은 오름차순 그래서 False하면 내림차순이 된다
<img width="229" alt="Screen Shot 2019-03-25 at 6 42 05 PM" src="https://user-images.githubusercontent.com/46523571/54909495-b3318380-4f2d-11e9-8486-aa06f76a9649.png">

## DataFrame이 제공하는 유용한 함수
*  정렬(2) data값을 통해서 정렬
###
    import numpy as np
    import pandas as pd
    
    np.random.seed(0)
    df = pd.DataFrame(np.random.randint(0,10,(6,4)),
                     columns=["A","B","C","D"],
                     index=pd.date_range("20190101",periods=6))
    random_index = np.random.permutation(pd.date_range("20190101",periods=6))   #리스트 안에 있는 요소들을 섞어주는 역할
    df.index = random_index
    df.columns = ["B","A","C","D"]
    
    display(df)
    
    display(df.sort_values(by="B", ascending=False))   #default 값은 오름차순 그래서 False는 내림차순
    display(df.sort_values(by=["B","A"])) #B를 갖고 정렬한다음 값이 같으면 A를 기준으로 정렬해
<img width="225" alt="Screen Shot 2019-03-25 at 6 43 02 PM" src="https://user-images.githubusercontent.com/46523571/54909553-d9572380-4f2d-11e9-8a7c-9925a5b473d5.png">


## DataFrame이 제공하는 유용한 함수
* unique()
* value_counts()
* isin()
* lambda 입력값 : 해야할 일
###
    import numpy as np
    import pandas as pd
    
    np.random.seed(0)
    df = pd.DataFrame(np.random.randint(0,10,(6,4)),
                     columns=["A","B","C","D"],
                     index=pd.date_range("20190101",periods=6))
    
    df["E"] = ["AA","BB","CC","AA","CC","CC"]   # 새로운 column 추가
    display(df)
    
    # unique()
    print(df["E"].unique())   #어떤 종류?값이 있니?
    
    # value_counts()
    print(df["E"].value_counts())   #값의 개수를 카운팅
    
    # isin()
    print(df["E"].isin(["AA","BB"]))   # 어디어디에 포함되어 있니? True/False 값으로 나와서
                                       # boolean indexing 처리가능
    
    df.drop("E",axis=1,inplace=True)   # drop으로 삭제할때 어떤걸 삭제할지랑 axis축 값을 정해야한다.
                                        # inplace=True 원본을 변경해라는 의미
    display("isin",df)
    
    # lambda : 한줄짜리 함수를 만들 때 쓴다.
    # 최대값에서 최소값을 뺀후 E라는 새로운 column에 추가하는 방법
    func = lambda x : x.max() - x.min() #x: 입력값 : 해야할 일 등을 처리 후 return 하겠어라는 의미
    df.apply(func,axis=1)   # 이 함수를 저장해 라는 의미(적용할 함수명, 축값(axis=0 행, axis=1 열))
    display(df.apply(func,axis=1))
    df["Max-Min"] = df.apply(func,axis=1)
    display(df)
<img width="345" alt="Screen Shot 2019-03-25 at 6 44 11 PM" src="https://user-images.githubusercontent.com/46523571/54909622-ff7cc380-4f2d-11e9-961f-ba1926c2bb18.png">
<img width="321" alt="Screen Shot 2019-03-25 at 6 44 15 PM" src="https://user-images.githubusercontent.com/46523571/54909631-04417780-4f2e-11e9-9b01-cf0884655ca3.png">

## DataFrame이 제공하는 유용한 함수 merge(1)
* merge : (1) key값이 같은 경우의 merge
###
    #(1) key값이 같은 경우의 merge
    # Pandas(merge) = SQL(Inner join, Outer join, Left Outer join, Right Outer join)
    # merge() 함수를 이용해서 join 작업을 수행(DataFrame의 결합)
    
    import numpy as np
    import pandas as pd
    
    df1 = pd.DataFrame({"학번" : ["2019001","2019002","2019003"],
                        "이름" : ["이지안","박동훈","홍길동"]})
    df2 = pd.DataFrame({"학번" : ["2019001","2019003","2019004"],
                        "학과" : ["컴퓨터","철학","수학"]})
    
    print("▼ inner join")
    display(pd.merge(df1, df2, on="학번", how="inner"))   # 학번을 기준으로 merge
    
    print("▼ outer join")
    display(pd.merge(df1, df2, on="학번", how="outer"))
    
    print("▼ left join")
    display(pd.merge(df1, df2, on="학번", how="left"))
    
    print("▼ right join")
    display(pd.merge(df1, df2, on="학번", how="right"))
<img width="240" alt="Screen Shot 2019-03-25 at 6 45 08 PM" src="https://user-images.githubusercontent.com/46523571/54909699-22a77300-4f2e-11e9-8284-625763494503.png">

## DataFrame이 제공하는 유용한 함수 merge(2)
* merge : (2) key값이 다른 경우의 merge
###
    import numpy as np
    import pandas as pd
    
    df1 = pd.DataFrame({"학번" : ["2019001","2019002","2019003"],
                        "이름" : ["이지안","박동훈","홍길동"]})
    df2 = pd.DataFrame({"학생학번" : ["2019001","2019003","2019004"],
                        "학과" : ["컴퓨터","철학","수학"]})
    
    # df1과 df2의 키값이 다른 경우 사용법
    pd.merge(df1, df2, left_on="학번", right_on="학생학번", how="inner")
<img width="301" alt="Screen Shot 2019-03-25 at 6 45 32 PM" src="https://user-images.githubusercontent.com/46523571/54909802-56829880-4f2e-11e9-831d-6aeb54e165ad.png">

## DataFrame이 제공하는 유용한 함수 merge(3)
* merge : (3) column기반과 index기반의 merge
###
    import numpy as np
    import pandas as pd
    
    df1 = pd.DataFrame({"학번" : ["2019001","2019002","2019003"],
                        "이름" : ["이지안","박동훈","홍길동"]})
    df2 = pd.DataFrame({"학과" : ["컴퓨터","철학","수학"]},
                       index=["2019001","2019003","2019004"])
    
    display(df1)   #학번이 column으로 잡히고
    display(df2)   #학번이 index로 잡힘
    display(pd.merge(df1, df2, left_on="학번", right_index=True, how="inner"))
<img width="250" alt="Screen Shot 2019-03-25 at 6 47 03 PM" src="https://user-images.githubusercontent.com/46523571/54909848-71550d00-4f2e-11e9-82cb-e563a276bcc7.png">











## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>