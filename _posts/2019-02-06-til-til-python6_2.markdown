---
layout: post
title: '[python] 파이썬 Pandas DataFrame'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-06 00:12:17 +0900
lastmod: 2019-02-06 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Pandas DataFrame에 대해서 <br />

# Pandas DataFrame
> pandas : python의 data analysis의 핵심 library<br>
> pandas는 고유하게 정의된 두개의 자료구조를 이용(시리즈, 데이터프레임)<br>

> pandas DataFrame : numpy의 2차원 배열과 유사하다.<br> 
> ※ 3차원 배열과 유사한건 없음.<br>
>  - Series를 여러개 컬럼형태로 모아 놓은게 DataFrame
>  - 다른 데이터 타입을 가져도 된다.

## import pandas as pd
* pandas를 사용하기 위해서는 module을 import 사용해야한다.
* 일반적으로 as(얼리아싱)을 써서 pd라고 정해서 사용한다.(전세계적으로 pd를 쓴다.)

# Pandas DataFrame
* DataFrame : 가장 흔하게 사용하는 자료구조
* 2차원 배열, 행과 열로 구성되어 있는 Table형태의 자료형
* DataFrame 생성(Dictionary를 이용해서)
```
    import numpy as np
    import pandas as pd
    #                 ▼ 값에 리스트 또는 집합형태의 자료는 모두 가능
    #        {키값  :  값[리스트]}       # 리스트 개수가 똑같아야 한다. #ex ) 5개로 표시   #행표시
    data = {"names" : ["KIM","LEE","PARK","LEE","KIM"],
            "year" : [2015,2016,2017,2015,2019],
            "points" : [1.5,2.3,3.1,4.0,4.5]}
    
    df = pd.DataFrame(data)
    display(df)
```
<img width="210" alt="Screen Shot 2019-03-25 at 5 30 04 PM" src="https://user-images.githubusercontent.com/46523571/54905161-aad44b00-4f23-11e9-9e79-2601dfea5c39.png">

```
    import numpy as np
    import pandas as pd
    
    data = {"names" : ["KIM","LEE","PARK","LEE","KIM"],
            "year" : [2015,2016,2017,2015,2019],
            "points" : [1.5,2.3,3.1,4.0,4.5]}
    
    df = pd.DataFrame(data)
    
    # DataFrame의 index 정보
    print(df.index)    # 결과값 : RangeIndex(start=0, stop=5, step=1)
    
    # DataFrame이 가지고 있는 값들을 numpy array 2차 배열로 표현 
    print(df.values)   # 데이터타입이 제 각각이라 object로 잡힌다.
    
    # DataFrame의 차원 체크
    print(df.ndim)     # 결과값 : 2   -> 2차원을 의미
    
    # DataFrame의 형태
    print(df.shape)    # 결과값 : (5,3)   -> 튜플형태의 5행 3열, 2차원 형태
    
    # DataFrame 전체 요소의 개수
    print(df.size)     # 결과값 : 15   -> 전체 요소의 개수
```

## 표로 표시 방법
```
    import numpy as np
    import pandas as pd

    data = {"names" : ["KIM","LEE","PARK","LEE","KIM"],
            "year" : [2015,2016,2017,2015,2019],
            "points" : [1.5,2.3,3.1,4.0,4.5]}
    
    df = pd.DataFrame(data)
    display(df)   # column에 대한 정보도 확인이 가능
```
<img width="222" alt="Screen Shot 2019-03-25 at 5 32 30 PM" src="https://user-images.githubusercontent.com/46523571/54905272-fbe43f00-4f23-11e9-833a-0eb64e66ecc2.png">

```
    print(df.index)   # 결과값 : RangeIndex(start=0, stop=5, step=1)
    df.columns        # column에 대한 기본 정보를 알려줌
                      # Index(['names', 'year', 'points'], dtype='object')
    df.index.name = "학생순번"     # 인덱스명 정의
    df.columns.name = "학생정보"   # 컬럼명 정의
    display(df)
```
<img width="258" alt="Screen Shot 2019-03-25 at 5 33 04 PM" src="https://user-images.githubusercontent.com/46523571/54905305-0f8fa580-4f24-11e9-983e-a40f7213b3a4.png">

## 데이터 표준(CSV, XML, JSON)
```
    # 일반적으로 데이터를 주고받기 위해서 데이터를 표현하는 표준
    # 1. CSV방식(comma seperated value)
    #    - ','를 기준으로 데이터를 분리해서 표현하는 방식
    #    - 예 10,홍길동,서울,20,김길동,인천,30,최길동,40
    #    - 장점 : 부가적인 데이터가 상대적으로 작다. 많은 양의 데이터를 표현하기에 적합
    #    - 단점 : 데이터 핸들링(처리)이 쉽지 않다.
    
    # 2. XML방식(Extended Markup Language)
    #    - 예) <person><age>10</age><name>홍길동</name><address>서울</address></person>
    #    - 장점 : 유지보수성이 좋다. 프로그램 처리가 쉽다.
    #    - 단점 : 부가적인 데이터가 많다.
    
    # 3. JSON(JavaScript Object Notation)
    #    예) {age:10, name:홍길동, address:서울}      ※ 파이썬 딕셔너리와 비슷
    #    - 장점 : 유지보수성이 좋다. XML보다 데이터 크기도 작다.
```

## CSV
```
    # CSV파일로부터 데이터를 읽어서 DataFrame을 생성
    
    import numpy as np
    import pandas as pd
    
    df = pd.read_csv("./data/sample/student.csv")   # data 폴더 참고
    print(df)
    display(df)
```
<img width="243" alt="Screen Shot 2019-03-25 at 5 35 32 PM" src="https://user-images.githubusercontent.com/46523571/54905454-68f7d480-4f24-11e9-8e8a-4a08ddb2f44e.png">

###
```
    # CSV파일로부터 데이터를 읽어서 DataFrame을 생성
    
    import numpy as np
    import pandas as pd
    
    df = pd.read_csv("./data/movie/movies.csv")   # data 폴더 참고
    #display(df)   # 전체 추출
    display(df.head(5)) #앞에서 5개만 추출
    display(df.tail(5)) #뒤에서 5개만 추출
    print(df.shape)  # 일반적으로 전체 row가 몇개인지 확인   # 결과값 : (9742, 3)
    
    #  ※ 참고로 numpy의 loadtxt로 불러들이면 단점은 ,가 중간에 있기 때문에 오류가 발생할 수 있다.
    #ratings = np.loadtxt("./data/movie/ratings.csv",   # 파일 경로
    #                    delimiter=",",        # delimiter 구분자 뭐야?
    #                    skiprows=1)           # 첫번째줄은 스킵해줘
```
<img width="677" alt="Screen Shot 2019-03-25 at 5 37 55 PM" src="https://user-images.githubusercontent.com/46523571/54905587-becc7c80-4f24-11e9-991b-2f2c575598b4.png">

## 중간 정리
```
    # Pandas
    # 1. Series
    #    - sumpy 1차원 array와 유사
    #    - index를 Series 자체에 저장
    #    - 기본적인 숫자 index(0~ )를 사용할 수 있다.
    # 2. DataFrame
    #    - 여러개의 Series를 모아놓은 Table형식의 자료구조
    #    - Dictionary를 이용한 DataFrame 생성
```

## Dictionary를 이용한 DataFrame 생성
```
    import numpy as np
    import pandas as pd
    
    # my_dict = {1,2,3,4}           # set 구조
    # my_dict = {"key" : "value"}   # dictionary 구조
    
    # my_dict = dict()   # 내용이 없는 empty dictionary 생성(1)
    # my_dict = {}       # 내용이 없는 empty dictionary 생성(2)
    
    # 학생정보 dictionary 생성(이름, 학과, 학년, 평점)
    # dictionary구조  # key=str구조  # value=list구조
    my_dict = {"이름" : ["이지안","박동훈","홍길동"],   
               "학과" : ["컴퓨터","철학과","수학과"],
               "학년" : [1,2,3],
               "평점" : [1.5,4.0,2.3]}
    
    print(my_dict)               # 결과값 : {'이름': ['이지안', '박동훈', '홍길동'], '학과': ['컴퓨터', '철학과', '수학과'], '학년': [1, 2, 3], '평점': [1.5, 4.0, 2.3]}
    print(my_dict["학과"][1])     # 결과값 : 철학과
    
    df = pd.DataFrame(my_dict)   # 자동으로 key값을 column으로 변환, value값은 row로 변환, index는 지정해주지 않으면 0부터 시작
    display(df)
```
<img width="234" alt="Screen Shot 2019-03-25 at 5 40 07 PM" src="https://user-images.githubusercontent.com/46523571/54905729-0d7a1680-4f25-11e9-8bd7-6364eb93a5b2.png">

## DataFrame의 index와 column 재정의
```
    #DataFrame의 index와 column을 재정의하려면 어떻게?
    import numpy as np
    import pandas as pd
    
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "평점" : [1.5,4.0,2.3,4.3]}
    display(data)
    
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
```
<img width="471" alt="Screen Shot 2019-03-25 at 6 01 59 PM" src="https://user-images.githubusercontent.com/46523571/54907117-220bde00-4f28-11e9-93d2-a1a021736727.png">

## DataFrame 기본분석함수 describe()
```
    #DataFrame은 기본 분석 함수를 가지고 있다.
    import numpy as np
    import pandas as pd
    
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    
    #DataFrame에 대해 기본 집계함수를 실행해서 결과를 출력
    display(df.describe())
```
<img width="232" alt="Screen Shot 2019-03-25 at 6 02 48 PM" src="https://user-images.githubusercontent.com/46523571/54907179-3c45bc00-4f28-11e9-9383-cd519df899f8.png">

## DataFrame 특정 column 출력방법
```
    # 특정 컬럼만 출력하고 싶다.
    import numpy as np
    import pandas as pd
    
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","기계","수학과","컴퓨터"],
               "학년" : [1,1,2,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
    
    # 하나의 특정 컬럼만 선택
    #(1) 일반적으로 많이 쓰는 표현
    df["이름"]   #하나의 시리즈형태로 출력된다고 생각하면 된다.
    #(2) 이렇게도 표현 가능함
    df.이름
    
    type(df["이름"])   # 결과값 : pandas.core.series.Series
```
<img width="403" alt="Screen Shot 2019-03-25 at 6 03 27 PM" src="https://user-images.githubusercontent.com/46523571/54907221-4ebff580-4f28-11e9-8f1e-e7d8fd87189b.png">

```
    # DataFrame에서 특정 column을 출력하면 view가 된다.
    # View를 사용하지 않고 새로운 Series를 생성하려면 copy()를 이용
    
    import numpy as np
    import pandas as pd
    
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    
    # new_name = df["이름"]   # view와 원본 둘다 바꾼다.
    # new_name["three"] = "유관순"   # three 홍길동 -> three 유관순 으로 변경
    # display(new_name)
    # display(df)
    
    new_name = df["이름"].copy()   # 새로운 series로 만들기 때문에 원본은 변경되지 않는다.
    new_name["three"] = "유관순"   # three 홍길동 -> three 유관순 으로 변경
    display(new_name)
    display(df)
```
<img width="310" alt="Screen Shot 2019-03-25 at 6 04 14 PM" src="https://user-images.githubusercontent.com/46523571/54907269-69926a00-4f28-11e9-91c5-e8626f2e2686.png">

## indexcing 3가지 & slicing
```
    # indexing에는 3가지 방법이 있다.
    # 일반 indexing, boolean indexing, fancy indexing & slicing
    
    import numpy as np
    
    arr = np.array([1,2,3,4,5,6])
    print(type(arr))    # 결과값 : <class 'numpy.ndarray'>
    
    # 일반 indexing 방법
    print(arr[0])       # 결과값 : 1
    print(arr[-1])      # 결과값 : 6
    print("\n")
    
    # slicing - list, numpy에서 많이 사용한 방법
    print(arr[1:3])     # 결과값 : [2 3]
    print(arr[1:-1])    # 결과값 : [2 3 4 5]
    print("\n")
    
    # boolean indexing 
    # - 해당 배열에 대해서 특정 조건을 부여해 각각의 요소를 True/False로 매핑한 논리비교하는 array만드는 방법
    # - array에 대해 논리연산을 수행하면 boolean mask가 생성
    # - boolean indexing은 index 위치에 boolean mask를 이용하는 indexing 방법이다.
    print(arr > 3)      # 결과값 : [False False False  True  True  True]
    print(arr[arr > 3]) # 결과값 : [4 5 6]    # True의 위치값만 출력
    print("\n")
    
    # fancy indexing - 연결되어 있지 않은 값들을 indexing할 때 사용
    # - index 부분에 index 배열을 사용하는 indexing 방법
    print("fancy indexing---------------------------------")
    print(arr[[1,4]])   # 결과값 : [2 5]
```

## DataFrame에 fancy indexing 사용 방법
```
    # DataFrame에 대해서 fancy indexing을 사용할 수 있다.
    import numpy as np
    import pandas as pd
    
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    
    display(df)
    display(df["이름"])   #"이름"의 Series만 가져온다.
    display(df[["이름","학년"]])   #"이름"과 "학년" 두개를 fancy indexing 했으니
                                 #Series가 아닌 DataFrame으로 나온다.
```
<img width="333" alt="Screen Shot 2019-03-25 at 6 06 43 PM" src="https://user-images.githubusercontent.com/46523571/54907404-c3932f80-4f28-11e9-8c19-8c4b0010291a.png">

## 특정 column 값 추가하는 방법
```
    # indexcing을 이용해 DataFrame에서 특정 column을 선택할 수 있다.
    import numpy as np
    import pandas as pd
    
    print("기본")
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
    
    print("column에 등급 값 넣는 방법")
    #df["등급"] = "A"   # 등급 값 전체를 바꿀때 
    df["등급"] = ["A","C","A","F"]  # 등급에 값을 각각 바꿀때
    display(df)
    
    print("fancy indexing 방법")
    df[["이름","등급"]] = [["홍길동", "A"],
                         ["김길동", "F"],
                         ["김길동", "F"],
                         ["김길동", "F"]]
                        
    display(df)   
```     
<img width="325" alt="Screen Shot 2019-03-25 at 6 07 19 PM" src="https://user-images.githubusercontent.com/46523571/54907445-dc034a00-4f28-11e9-91e5-489d77e47074.png">

## 새로운 column 추가 방법 4가지
```
    # 기존 DataFrame에 새로운 column을 추가해 보기
    # 새로운 column을 정의하고 그 값을 명시
    # scalar, python list, numpy array, pandas Series를 이용
    
    import numpy as np
    import pandas as pd
    
    print("▼ 기본")
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
    
    print("▼ scalar를 이용해서 column 추가 방법")
    df["나이"] = 20    # 나이 column이 없으니 에러가 발생하는게 아니라 새로운 column을 추가한다.
    display(df)
    
    print("▼ python list를 이용해서 column 추가 방법")
    df["나이"] = [20,22,23,21]
    display(df)
    
    print("▼ numpy array 이용해서 column 추가 방법")
    df["나이"] = np.array([20,22,23,21])
    display(df)
    
    # scalar, python list, numpy array 순서기반이라 다 들어가는데 
    # pandas Series의 경우는 index기반이라 값이 안들어간다.
    # 이유는 index를 기반으로 DataFrame과 매핑하기 때문에 
    # 현재 값이 one, two, three., index값이 다르기 때문에 불가능하다.
    print("▼ pandas Series 이용해서 column 추가 방법 - 값이 안들어가고 NaN 처리된다.")   
    df["나이"] = pd.Series([20,22,23,21])
    display(df)
    
    # 그래서 pandas Series의 경우 값을 넣고 싶다면 index 값도 설정해야 한다.
    # 추가적으로 index기반이라 index의 값을 다르게 해도 된다. -> index 값이 다른곳은 NaN으로 표시
    print("▼ pandas Series 이용해서 column 추가 방법 - 값이 안들어가기 때문에 index 처리를 해야한다.")   
    df["나이"] = pd.Series([20,22,21],
                          index=["one","two","four"])
    display(df)    
```
<img width="421" alt="Screen Shot 2019-03-25 at 6 08 27 PM" src="https://user-images.githubusercontent.com/46523571/54907504-09e88e80-4f29-11e9-9cd0-c63ff35f8784.png">
<img width="851" alt="Screen Shot 2019-03-25 at 6 08 40 PM" src="https://user-images.githubusercontent.com/46523571/54907519-0fde6f80-4f29-11e9-9f8e-8111427a6b96.png">

## 기존 column에 연산을 해서 새로운 column 생성
```
    # 기존 DataFrame에 새로운 column을 추가해 보기
    # 기존 column에서 연산을 해서 새로운 column을 생성할 수 있다.
    
    import numpy as np
    import pandas as pd
    
    print("▼ 기본")
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
    
    
    # 기존 column을 연산을 통해서 새롭게 만들 수 있다.
    print("▼ 학점을 1점씩 올려준다고 가정")
    df["학점"] = df["학점"] + 1
    display(df)
    
    # 학점 가지고 장학여부를 확인 할 수 있다.
    print("▼ 학점 가지고 장학여부를 확인 할 수 있다.")
    df["장학여부"] = df["학점"] > 5.0
    display(df)
```
<img width="396" alt="Screen Shot 2019-03-25 at 6 09 35 PM" src="https://user-images.githubusercontent.com/46523571/54907566-2ab0e400-4f29-11e9-9fa4-4d01aa3bc3f4.png">

## 특정 column 삭제 방법(1),(2-1),(2-2)
```
    # DataFrame에 특정 column을 삭제할 수 있다.
    import numpy as np
    import pandas as pd
    
    print("▼ 기본")
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
    
    
    # DataFrame 원본이 변경된다.
    # 그러나 이러한 방법은 거의 사용되지 않는다.
    # print("▼ del을 써서 특정 column이 삭제된 상태(1)")
    # del df["등급"]
    # display(df)
    
    
    # print("▼ .drop()함수를 써서 특정 column이 삭제된 상태(2-1)")
    # df.drop("등급", axis = 1, inplace = True)   #2차원에서 축이 1이니깐 열방향을 뜻함.  
    #             #inplace=True(df원본삭제 그래서 return받지 않음 그래서 None 표시됨),
    # display(df)
    
    print("▼ .drop()함수를 써서 특정 column이 삭제된 상태(2-2)")
    new_df = df.drop("등급", axis = 1, inplace = False)  
                #inplace=False(df원본은 건들지 말고 삭제처리한 결과df를 return 하라는 의미(복사본)
    display(df)
```
<img width="495" alt="Screen Shot 2019-03-25 at 6 10 19 PM" src="https://user-images.githubusercontent.com/46523571/54907607-46b48580-4f29-11e9-8de3-7ca4c465107e.png">

## DataFrame의 행 제어
```
    # row(행) 제어할 때 index를 사용할 수 없다.
    # row(행) 제어할 때 slicing은 사용할 수 없다.   #원본에서 부분을 view로 뽑아오기 때문에 가능
    
    import numpy as np
    import pandas as pd
    
    print("기본")
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
    
    #print("숫자 indexing")
    #df[0]   # 행을 선택하기 위해서 숫자index를 바로 이용할 수 없다.   #에러가 발생한다. #반면 slicing은 가능
    
    print("숫자 slicing")
    display(df[0:1])  # slicing은 가능   # slicing은 원본에서 부분을 view로 뽑아오는 것이기 때문에 결과는 DataFrame
    
    #print("값 indexing")
    #df["one"]   # 행을 선택하기 위해서 index값을 바로 이용할 수 없다.
    
    print("값 slicing")
    display(df["one":"three"])
```
<img width="306" alt="Screen Shot 2019-03-25 at 6 11 24 PM" src="https://user-images.githubusercontent.com/46523571/54907671-6ba8f880-4f29-11e9-93f5-689ea7c038b7.png">

## DataFrame의 행 제어 loc
```
    import numpy as np
    import pandas as pd
    
    print("▼ 기본")
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
    
    print("▼ .loc 사용하여 일반 indexing 방법")
    display(df.loc["one"]) # 일반 indexing 방법 -> 결과는 Series 값으로 출력
    
    print("▼ .loc 사용하여 fancy indexing 방법")
    display(df.loc[["one","two","three"]])  # fancy indexcing 방법 -> 결과는 DataFrame 값으로 출력
    
    print("▼ .loc 사용하여 boolean indexing 방법")
    display(df["학점"] > 3.0 )   # boolean mask
    display(df.loc[df["학점"] > 3.0])   # boolean indexing 방법
    
    print("▼ .loc 사용하여 boolean indexing을 사용하여 특정값(이름만) 출력 방법")
    display(df.loc[df["학점"] > 3.0, "이름"])   #성적이 3.0 이상인 애들의 이름만 출력 방법
    
    print("▼ .loc 사용하여 boolean indexing을 사용하여 특정값들(이름과 학점) 출력 방법")
    display(df.loc[df["학점"] > 3.0, ["이름","학점"]])  #성적이 3.0 이상인 애들의 이름과 학점 출력 방법
```
<img width="397" alt="Screen Shot 2019-03-25 at 6 12 44 PM" src="https://user-images.githubusercontent.com/46523571/54907749-9bf09700-4f29-11e9-8740-5dcedba1ca39.png">
<img width="695" alt="Screen Shot 2019-03-25 at 6 12 50 PM" src="https://user-images.githubusercontent.com/46523571/54907755-a01cb480-4f29-11e9-85ea-62cdda7315df.png">

## 중간정리
```
    # Pandas
    # 1. Series
    # 2. DataFrame (일반적으로 사용하는 자료구조)
    
    # DataFrame 생성 (Dictionary)
    import numpy as np
    import pandas as pd
    
    data = {"이름" : ["이지안", "박동훈", "홍길동"],
           "학과" : pd.Series(["컴퓨터", "철학", "기계"],index = ["2019001", "2019002", "2019003"]),  #numpy array나 list의 경우는 상관없지만 Series의 경우는 index의 기반이라 아래 Data index를 바꿨으니 Series의 index 정보를 추가해야 결과값이 보인다.      
           "학년" : np.array([1, 2, 3])}   #리스트 형태의 자료구조는 다 된다. # list, Series
    df = pd.DataFrame(data,
                      columns = ["학년", "학과", "이름", "학점"],   # columns의 정보 변경
                      index = ["2019001", "2019002", "2019003"])   # 연산을 할필요가 없으니 숫자라고 해도 문자열로 잡는게 좋음
    
    display(df)       # DataFrame의 일반적인 확인(출력)
    print(df.shape)   # DataFrame의 행과 열의 개수를 확인 # (3, 3)
    print(df.columns) # DataFrame의 column의 정보 확인  # Index(['학년', '학과', '이름', '학점'], dtype='object')
    print(df.index)   # DataFrame의 index의 정보 확인   # Index(['2019001', '2019002', '2019003'], dtype='object')
    #df.columns.name = "학생정보"   # columns의 이름을 지정
    #df.index.name = "학번"        # index의 이름을 지정
    #display(df)
    
    ### DataFrame을 실제로 생성하는 방법
    # 1. 파일처리 (CSV, XML, JSON)
    # 2. Database에서 data를 SELECTION해서 DataFrame을 생성
    # 3. open API처럼 외부 프로그램을 통해 DataFrame을 생성(JSON)
    
    ### DataFrame의 column의 CRUD 처리 (CRUD : Creat Read Update Delete)
    #df[2]      # 특정 column 추출할 땐 열의 번호가 아닌 값을 넣어야 한다.
    df["이름"]   # 특정 column 정보만 Series 형태로 값으로 return
    df[["학과", "이름"]]   # fancy indexing 2차원 형태의 Dataframe 값으로 return
    df["학과"] = "국문학과" # 특정 column 정보를 특정 값으로 변경 # indexing의 결과는 view 생성됨
                        # column의 값을 수정(update)처리
    new_dept = df["학과"].copy()  # 복사본 생성
    df["장학여부"] = True   # 없는 column을 만들면 DataFrame에 새로운 값이 생성된다.(create)
    
    # 용도에 따라 삭제 방법
    new_df = df.drop("학점", axis = 1, inplace = False)
    df
    
    df.drop("학점", axis = 1, inplace = True)   # 2차원에서 axis=0 행방향, axis=1 열방향
                                               # inplace=True 원본지우기, inplace=False 원본보존
    df
    
    ### DataFrame의 row의 CRUD 처리 (CRUD : Creat Read Update Delete)
    #df[0]   # 행을 read할 때 일반적인 indexing 방법은 사용할 수 없다.
    df[0:2]  # 행을 선택할 때 slicing은 사용할 수 있다.   #view로 return
    #df["2019001"]  # 불가능, column 처리 방식이다. 
    df["2019001":"2019002"]  # 사용가능
    
    # row을 선택하고 사용하는게 너무 불편해서 loc 방법으로 기억하자
    #df.loc[0]   # 숫자 indexing 불가능 #loc는 indexing을 사용할 수 있도록 제공 
                #DataFrame이 가지고 있는 숫자 index는 불가능
    df.loc["2019001"]   #행 선택이 가능하고 결과는 Series로 return
    df.iloc[0]   #iloc = index location의 약자 # 숫자 index를 이용해서 행 선택 가능
    df.loc[["2019001","2019003"]]   # fancy indexing 가능
    df.loc["2019001","이름"]   # 특정 행에대한 열 값 출력
    df.loc["2019001",["이름", "학과"]]   # 특정 행에대한 열 값 출력
```

## DataFrame에 새로운 행 추가
```
    import numpy as np
    import pandas as pd
    
    print("▼ 기본")
    data = {"이름" : ["이지안","박동훈","홍길동", "강감찬"],
               "학과" : ["컴퓨터","철학과","수학과","컴퓨터"],
               "학년" : [1,2,3,4],
               "학점" : [1.5,4.0,2.3,4.3]}
    df = pd.DataFrame(data,
                     columns=["학과","이름","학점","학년","등급"],
                     index=["one","two","three","four"])
    display(df)
    
    print("▼ 새로운 행 추가(fancy indexing)")
    df.loc["five",["이름","학년"]] = ["최길동",3]   # 새로운 행 추가 # fancy indexcing
    # 숫자 3의 경우 정수지만 값이 생성되면서 실수로 처리한다. 그래서 모든 타입을 실수로 바꾼다.
    display(df)
    
    print("▼ 새로운 행 추가(값에 code상으로 NaN을 추가할 수 있다.)")  # np가지고 있는 상수값 nan 사용
    df.loc["five",["이름","학년","등급"]] = ["최길동",3,np.nan]   # 새로운 행 추가 # fancy indexcing
    # 숫자 3의 경우 정수지만 nan 값이 실수라 전체가 실수로 처리한다. 그래서 모든 타입을 실수로 바꾼다.
    display(df)
    
    print("▼ 새로운 행 추가")
    df.loc["five",:] = ["기계","최길동",4.0,3,"A"]   # 새로운 행 추가 # 값을 순서에 맞게 명시
    display(df)
    
    print("▼ 행 삭제")
    df.drop("one",axis=0,inplace=True)   # axis=0 행방향으로 삭제
    display(df)
```
<img width="512" alt="Screen Shot 2019-03-25 at 6 14 24 PM" src="https://user-images.githubusercontent.com/46523571/54907838-d6f2ca80-4f29-11e9-89c6-9e6146fa80b1.png">
<img width="329" alt="Screen Shot 2019-03-25 at 6 14 29 PM" src="https://user-images.githubusercontent.com/46523571/54907851-dfe39c00-4f29-11e9-8f5c-8b463b9b1d46.png">

## NaN 값의 처리(1) 처리
```
    # NaN값의 처리
    
    import numpy as np
    import pandas as pd
    
    np.random.seed(4)
    arr = np.random.randint(0,10,(4,3))
    
    df = pd.DataFrame(arr)
    display(arr)
    df.columns = ["A","B","C"]    # column값 변경
    #df.index = pd.date_range("20190101", "20190104") 아래와 같은 말
    df.index = pd.date_range("20190101", periods=4)   # 특정 날짜 간격을 지정할 때
    
    df["D"] = [7,np.nan,4,np.nan]   # "D" column을 추가하고 값을 넣기  # np.nan은 실수
    # nan을 쉽게 생각하면 홈페이지 가입할 때 빈칸 냅두고 작성 한 것을 nan 처리한다고 생각하면 된다.
    display(df)
```
<img width="245" alt="Screen Shot 2019-03-25 at 6 24 45 PM" src="https://user-images.githubusercontent.com/46523571/54908454-4a490c00-4f2b-11e9-9ccf-9ced66ddef44.png">

## NaN 값의 처리(2) 삭제
```
    # 삭제
    # 결측값을 제거
    # NaN이 포함된 행을 모두 삭제(#nan 삭제 방법)
    
    #(1) - how="any"
    #df.dropna(how="any", inplace=True)   #how="any"는 NaN이 단 한개라도 포함된 행은 무조건 삭제
                                        # inplace=True 원본지우기, inplace=False 원본보존
    #display(df)
    
    #(2) - how="all"
    #df.dropna(how="all", inplace=True) #how="all"는 모든 column이 NaN이면 해당 행을 삭제
                                        # inplace=True 원본지우기, inplace=False 원본보존 
    #display(df)
```

## NaN 값의 처리(3) 대체
```
    # 대체
    # 다른 값으로 대체 처리(NaN이 많은 경우 다른 값으로 대체 처리할때가 있다.)
    df.fillna(value=0, inplace=True)     # 기본값은 False이다.   
    # value=에 대체하길 원하는 값을 넣으면 된다.
    
    # DataFrame에 대해 NaN에 대한 boolean mask를 생성할 수 있다.
    #(1)
    #display(df.isnull())   # 전체 True/False 값으로 변경하여 NaN값만 True로 변경 나머진 False
    
    #(2)
    #display(df.isnull()["D"]) # 해당 열에서만 True/False 값으로 변경하여 NaN값만 True로 변경 나머진 False 
``` 

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>