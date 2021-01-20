---
layout: post
title: '[python] 파이썬 Pandas Series'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-06 00:12:17 +0900
lastmod: 2019-02-06 11:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Pandas Series에 대해서 <br />

# Pandas Series
> pandas : python의 data analysis의 핵심 library<br>
> pandas는 고유하게 정의된 두개의 자료구조를 이용(시리즈, 데이터프레임)<br>

> pandas Series : numpy의 1차원 배열과 유사하다.<br>
> - 시리즈 안에 값들이 같은 데이터 타입을 가진다.
> - ※ numpy는 모든 타입을 넣을 수 있지만 series는 불가능

<br>
<br>

## import pandas as pd
* pandas를 사용하기 위해서는 module을 import 사용해야한다.
* 일반적으로 as(얼리아싱)을 써서 pd라고 정해서 사용한다.(전세계적으로 pd를 쓴다.)

# Numpy Array & Pandas Series 비교
    import numpy as np    # numpy
    import pandas as pd   # pandas 모듈을 쓸때 보통 numpy 모듈과 함께 쓴다.
    
    ## numpy array 만드는 방식--------------------------------------------------------------
    print("<numpy array 만드는 방식>","-"*50)
    
    # numpy 배열(list를 가지고 만드는 예제)
    arr = np.array([-1,5,10,99])
    
    # numpy array의 data type을 확인
    print("타입 :",arr.dtype)   # 결과값 : 타입 : int64
    print("-정수", arr)         # 결과값 : -정수 [-1  5 10 99]
    
    # numpy 배열을 생성할 때 data type(실수)을 지정할 수 있다.   dtype=np.float64
    arr = np.array([-1,5,10,99], dtype=np.float64)   # 해당 데이터 타입으로 지정해서 만들 수 있다.
    arr = np.array([-1,5,10,99], np.float64)   # (첫번째 인자값, 두번째 타입 지정) 그래서 dtype= 생략가능
    print("타입 :",arr.dtype)   # 결과값 : 타입 : float64
    print("-실수", arr)         # 결과값 : -실수 [-1.  5. 10. 99.]
    
    # numpy 배열을 생성할 때 data type(문자열)을 지정할 수 있다.   dtype=np.str
    arr = np.array([-1,5,10,99], dtype=np.str)   #해당 데이터 타입으로 지정해서 만들 수 있다.
    print("타입 :",arr.dtype)   # 결과값 : 타입 : <U2
    print("-문자", arr)         # 결과값 : -문자 ['-1' '5' '10' '99']
    
    # numpy 배열을 생성할 때 data type(객체)을 지정할 수 있다.   dtype=np.object
    arr = np.array([-1,3.14,"Hello",True], dtype=np.object)   #타입이 다 다르니 객체로 만들면 된다.
    print("타입 :",arr.dtype)   # 결과값 : 타입 : object
    print("-객체", arr)         # 결과값 : -객체 [-1 3.14 'Hello' True]
    
    
    
    ## pandas Series 만드는 방식--------------------------------------------------------------
    print("<pandas Series 만드는 방식>","-"*50)
    s = pd.Series([-1,5,10,99], dtype=np.object)   # (데이터, 타입)
    # series는 리스트와 딕셔너리를 합친 것과 같다.
    # 1차원 형태이다. 앞: 인덱스, 뒤: 값     # series는 인덱스와 값을 같이 가지고 있다.
    #                    0    -1
    #                    1     5
    #                    2    10
    #                    3    99
    #                    dtype: object
    display(s)                 # Series를 console에 출력
    print(s.index)             # Series에서 index 정보만 출력  
    # 결과값 : RangeIndex(start=0, stop=4, step=1)
    
    print(s.values)            # Series가 가지고 있는 값의 정보만 출력.  
    # 결과값 : [-1 5 10 99]     # 1차원 형태의 numpy array가 return 된다.
    
    print(type(s.values))      # s.value가 어떤 class의 객체인지 확인가능 
    # 결과값 : <class 'numpy.ndarray'>
    
## Series에 대한 indexing과 slicing
    import numpy as np
    import pandas as pd
    
    # Series에서 인덱스값을 다르게 할 수 있다. 숫자나 문자로 재정의 할 수 있다.
    # ()괄호 안에는list를 쓰는게 편하기 때문에 쓰는거지만, numpy array나 dictionary를 써도 된다.
    s = pd.Series([1,-5,10,99], index=["c","b","a","k"])
    print(s)
    # 기본(숫자) 인덱스(재정의)   값       # 인덱스를 재정의할 수 있으며 숫자 인덱스를 쓸수도 있다.
    #   0         c            1       # index를 재정의 함과 동시에 기존의 숫자 index도 사용 가능
    #   1         b            -5      # 인덱스(key), 값(value) 처럼 쓸 수 있다. 그래서 dictionary같다.
    #   2         a            10
    #   3         k            99
    # dtype: int64
    
    # 위와같이 index를 재정의했음에도 동시에 기존의 숫자 index도 사용 가능  #둘다 혼용해 사용 가능
    print("s[0]의 값은 : {}".format(s[0]))   #-> index 0번째 배열의 값 : 1 출력
    print("s[0]의 값은 : {}".format(s["c"]))   #-> index c번째 배열의 값 : 1 출력
    
    
    
    # slicing(기본 index 값으로 slicing)
    print("s[0:3]의 값은 : \n{}".format(s[0:3]))
    # 결과값 : 
            s[0:3]의 값은 :
            c     1
            b    -5
            a    10
            dtype: int64
    
    # slicing(재정의한 index 값으로 slicing)
    # 재정의한 index는 [앞:뒤] 모두 inclusive ※ 주의해야한다.
    print("s['c':'a']의 값은 : \n{}".format(s["c":"a"])) 
    # 결과값 : 
            s['c':'a']의 값은 : 
            c     1
            b    -5
            a    10
            dtype: int64

## Series 안의 데이터 출력 방법(1)&(2)
    import numpy as np
    import pandas as pd
    
    # Series안의 데이터를 모두 합해서 출력(지양) - (1) 좋지 않은 방법
    result = 0.0      # ※ result 변수에 sum 등의 함수값을 변수이름으로 적으면 좋지 않다.
    for tmp in s:
        result += tmp
    print(result)     # 결과값 : 105.0
    
    # Series안의 집계함수를 써서 출력(지향) - (2) 좋은 방법
    print("Series안의 데이터 총합 : {}".format(s.sum()))
    # 결과값 : Series안의 데이터 총합 : 105

## Series 생성하는 방법 list() & dictionary{ : }, 이름부여
* Series를 생성하는 방법 중에 list를 이용하는 방법이 있다.
* ex) s = pd.Series([1,2,3,4,5])
* Series를 dictionary를 이용해서 만들 수 있다.
###
    import numpy as np
    import pandas as pd
    
    my_dict = {"서울" : 3000, "인천" : 5000, "제주" : 2000}
    s = pd.Series(my_dict)
    print(s)
    # 결과값 : 
            서울    3000
            인천    5000
            제주    2000
            dtype: int64
    
    # Series에 이름을 부여할 수 있다.
    s.name = "지역별 가격 데이터"
    print(s)
    # 결과값 : 
            서울    3000
            인천    5000
            제주    2000
            Name: 지역별 가격 데이터, dtype: int64
    
    # Series index의 이름을 부여할 수 있다.
    s.index.name = "지역명"
    print(s)
    # 결과값 : 
            지역명
            서울    3000
            인천    5000
            제주    2000
            Name: 지역별 가격 데이터, dtype: int64
    
    # index를 수정할 수 있다.
    idx = ["SEOUL","INCHEON","JEJU"]
    s.index = idx
    print(s)
    # 결과값 : 
            SEOUL      3000
            INCHEON    5000
            JEJU       2000
            Name: 지역별 가격 데이터, dtype: int64



## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>