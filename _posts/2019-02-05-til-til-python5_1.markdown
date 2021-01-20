---
layout: post
title: '[python] 파이썬 Numpy'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-05 00:12:17 +0900
lastmod: 2019-02-05 11:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Numpy에 대해서 <br />



# Numpy
> NumPy - 행렬 연산을 위한 핵심 라이브러리<br> 
> NumPy Library<br>
> NP, Numerical Python의 약자(수치적인 처리를 하는 파이썬)(수치 계산에 특화, 1차원 백터, 다차원 매트릭스)<br>
> Vector(1차원 배열), Matrix(다차원 배열)연산에 상당한 편의성 제공<br>
> Pandas, matplotlib의 기반이 되는 library<br>

> Pandas Library(데이터분석을 하며, Pandas가 사용하는 자료구조를 NumPy이용)<br>
> matplotlib Library(데이터를 그림으로 표현할 때 사용)<br>

> 자료가 구조가 딱 1개이다. 그건 바로,<br>
> NumPy는 ndarray라는 배열을 사용<br>
> ndarray(n-dimension array) : 1차원 배열, 2차원 배열, 다차원 배열<br>
> 배열의 가장 큰 특성은 같은 데이터 타입(data type)을 가진다는 것이다.<br>
> 사용방법은 list와 거의 비슷하다.<br>
> 하지만 실행속도면이나 메모리용량 등에서 비교할 수 없이 좋다<br>

<br>

## import numpy as np
* numpy를 사용하기 위해서는 module을 import 사용해야한다.
* 일반적으로 as(얼리아싱)을 써서 np라고 정해서 사용한다.(전세계적으로 np를 쓴다.)
###
    import numpy as np
    
    # python list
    a = [1,2,3,4]     # python list
    print(type(a))    # python list의 type을 출력 -> 결과값 : <class 'list'>
    print("a[0] : {}, a[0]의 type : {}".format(a[0], type(a[0]))) # 결과값 : a[0] : 1, a[0]의 type : <class 'int'>
    print(a)          # list 출력 -> 결과값 : [1, 2, 3, 4]
    
    # numpy array
    arr = np.array([1,2,3,4])
    print(arr)       # numpy array 출력 -> 결과값 : [1 2 3 4]
    print(type(arr)) # numpy array type 출력 -> 결과값 : <class 'numpy.ndarray'>
    print(arr[0])    # numpy array 요소 출력 -> 결과값 : 1
    print(type(arr[0]))     # numpy array 요소 타입 출력 -> 결과값 : <class 'numpy.int64'>
    print(arr.dtype) # 결과값 : int64   #dtype : date type을 지칭     # 배열이다보니 데이터타입은 한개만 가질 수 있다.
    
    # 2차원 리스트(리스트 안에 리스트가 있는 구조)를 2차원 배열구조로 변경
    my_list = [[1,2,3],[4,5,6]]
    print(my_list)     # 결과값 : [[1, 2, 3], [4, 5, 6]]
    arr = np.array(my_list)
    print(arr)     # 결과값 : [[1 2 3]
                   #         [4 5 6]]     2행 3열짜리 2차원 배열구조( 수학적 매트릭스 )
        
    # 다시 말해, list를 이용해서 numpy array를 생성할 수 있다.


## list를 이용해서 numpy array를 생성할 수 있다.
* Q. data type이 가지각색인 list를 이용해서 numpy array를 만들면 어떻게 되는가?
* A. 여러개의 data type이 섞여 있는 경우 모든 데이터를 문자열 형태로 바꾼다.
###
    import numpy as np
    
    my_list = [100,3.14,"홍길동",True]     # 다른 데이터 타입의 리스트
    arr = np.array(my_list)
    arr     # 출력값 - >array(['100', '3.14', '홍길동', 'True'], dtype='<U32')

    # 문자열을 기반으로 한 numpy array가 생성
    # 다른데이터 타입이 섞여 있는 경우 모든 데이터를 문자열 형태로 바꾼다. U32:유니코드

## indexing 표현법
* list indexing & array indexing
###
    import numpy as np
    
    # 5를 출력해보자
    my_list = [[1,2,3],[4,5,6]]
    print(my_list)           # 결과값 : [[1, 2, 3], [4, 5, 6]]
    print(my_list[1][1])     # # 결과값 : 5   # list의 indexing   
    
    arr = np.array(my_list)
    print(arr)               # 결과값 : [[1 2 3]
                                        [4 5 6]]
    print(arr[1,1])          # 결과값 : 5   # array의 indexing    # 행과 열의 순서로 콤마로 구분


## numpy array 생성시 data type을 지정할 수 있다.(정수를 실수로, 실수를 정수로 등)
* 데이터타입을 잘못 선택하면 데이터 이상현상이 일어날 수 있다. 소수점을 날려버린다든가 등~
###
    import numpy as np
    
    my_list = [[1,2,3],[4,5,6]]
    
    arr = np.array(my_list, dtype=np.float64) #ex)float64
    print(arr)        # 결과값 : [[1. 2. 3.]   # 배열의 내용을 출력
                                 [4. 5. 6.]]
    print(arr[1,1])   # 결과값 : 5.0
    
    arr = np.array(my_list, dtype=np.str) #ex)str
    print(arr)        # 결과값 : [['1' '2' '3']   # 배열의 내용을 출력
                                 ['4' '5' '6']]
    print(arr[1,1])   # 결과값 : 5

## numpy array의 차원의 개수, 크기
* .ndim
* .shape
* .size
###
    import numpy as np
    
    my_list = [1,2,3,4,5]
    arr = np.array(my_list)
    print(arr)          # 배열의 내용을 출력
    print(arr.ndim)     # 배열의 차원을 출력, .ndim 사용   #결과값 : 1 <- 1차원이라는 의미
    print(arr.shape)    # 배열의 형태(행과 열의 개수) tuple로 출력   # 1개 나오면 1차원(행,)

    my_list = [[1,2,3],[4,5,6]]
    arr = np.array(my_list)
    print(arr)          # 배열의 내용을 출력
    print(arr.ndim)     # 배열의 차원을 출력, .ndim 사용   #결과값 : 2 <- 2차원이라는 의미
    print(arr.shape)    # 배열의 형태(행과 열의 개수) tuple로 출력   #결과값 : <- 2개 나오면 2차원(행, 열)
    print(len(arr))     # 2차원이지만 1차원 형태의 길이를 알아옴  #결과값 : 2 <- 행의 개수만 출력 #그래서잘안씀
    print(arr.size)     # 배열 안의 전체 요소의 개수   #결과값 : 6 < -행, 열 구분없이 요소만 출력

## numpy array의 axis(축))
* axis는 차원에 영향을 받는다.
* nbarray가 1차원이면 axis = 0
* nbarray가 2차원이면 axis = 0, axis = 1
* nbarray가 3차원이면 axis = 0, axis = 1, axis = 2
###
* nbarray가 1차원이면 axis = 0
* [1 2 3 4 5 6] 열 진행방향으로 진행하는군아라고 인식하면 된다.
###
* nbarray가 2차원이면 axis = 0, axis = 1
* axis=0은 행의 방향(위에서 아래로), axis=1은 행의 방향(왼쪽에서 오른쪽으로)
* [[1 2 3] 
*  [4 5 6] 

## numpy array는 형태를 내가 원하는 차원으로 변환시킬 수 있다.
    import numpy as np
    
    arr = np.array([1,2,3,4,5,6,7,8,9,10,11,12])
    print(arr)          # 결과값 : [ 1  2  3  4  5  6  7  8  9 10 11 12]
    print(arr.ndim)     # 결과값 : 1   -> 1차원이라는 의미
    print(arr.shape)    # 결과값 : (12,)   -> 1차원이고 12개의 원소가 있다는 의미
    print(arr.size)     # 결과값 : 12   -> 차원에 상관없이 해당 array 안에 들어있는 원소 총 개수
    print(type(arr))

    # 1차원을 2차원으로 바꿀 수 있다.
    arr.shape = (3,4)   # 1차원을 2차원의 3행 4열 배열 형태로 바꾸기 #튜플이니 arr.shape = 3,4 이렇게도 표현
    print(arr)          # 결과값 : [[ 1  2  3  4]         # 배열의 원본형태가 변환 된다.
                                # [ 5  6  7  8]
                                # [ 9 10 11 12]]
    # arr.shape = (3,5)   -> ValueError: cannot reshape array of size 12 into shape (3,5)
    # arr의 값이 12개이기 때문에 3행 5열의 형태로는 불가능해서 에러가 발생한다.


    # 1차원을 3차원으로 바꿀 수 있다.
    arr.shape = (3,2,2)   # 1차원을 3차원의 3,2,2로 배열 형태로 바꾸기
    print(arr)          # 결과값 : [[[ 1  2]             # 배열의 원본형태가 변환 된다.
                                #  [ 3  4]]
                                #
                                # [[ 5  6]
                                #  [ 7  8]]
                                #
                                # [[ 9 10]
                                #  [11 12]]]

## numpy array의 type을 다른 type으로 변환
    import numpy as np
    
    # 기본값
    arr = np.array([1,2,3,4,5])   #1차원 형태   #dtype : int64으로 자동으로 잡힘
    print(arr)        # 결과값 : [1 2 3 4 5]
    print(arr.dtype)  # 결과값 : int64
    
    # 실수로 dtype 변환
    arr1 = arr.astype(np.float64) #위쪽에서 만든 arr를 astype()을 이용해서 뒤에 있는 float64 타입으로 변경
    print(arr1)       # 결과값 : [1. 2. 3. 4. 5.]
    print(arr1.dtype) # 결과값 : float64
    
    # 문자열로 dtype 변환
    arr2 = arr.astype(np.str) #위쪽에서 만든 arr를 astype()을 이용해서 뒤에 있는 str 타입으로 변경
    print(arr2)       # 결과값 : ['1' '2' '3' '4' '5']
    print(arr2.dtype) # 결과값 : <U21 : 유니코드를 의미

## 다양한 생성 함수 .zeros
    import numpy as np
    
    # 형태1
    # arr = np.array([1,2,3]) # 기본값은 int64
    # arr = np.array([1,2,3], dtype=np.float64)
    # print(arr)     #결과값 : [1 2 3]
    
    # 형태2
    # 괄호안에 내가 만들 튜플 형태로 shape에 넣는다. 0으로 다 채워라
    arr = np.zeros((3,4))   # shape을 지정해야 한다.
                            # arrya의 모든 요소를 0으로 세팅 default dtype은 float 64
    print(arr)
    # 결과값 : [[0. 0. 0. 0.]
               [0. 0. 0. 0.]
               [0. 0. 0. 0.]]
    
    # default dtype값이 float64이기 때문에 int64로 바꾸고 싶으면 아래와 같이.
    arr = np.zeros((3,4), dtype=np.int64)

## 다양한 생성함수들
* .ones
* .empty
* .full
###
    import numpy as np
    
    
    arr = np.ones((2,3), dtype=np.int64) # 1로 채운다.
    arr = np.empty((3,3))   # 초기값을 안주고 공간만 만들기. 쓰레기값이 있을 수 있다.
    arr = np.full((3,4),10) # 내가 원하는 값으로 초기화
    arr1 = np.full_like(arr.20)   # 특정 array의 shape와 같은 array를 만든다.
    # zeros_like   ones_like   empty_like   full_like

## python range()와 numpy arrange()
* python -> range()
* numpy -> arrange()
* 0이상 10미만의 1의 크기로 증가하는 요소를 가지는 numpy array 생성
###
    import numpy as np
    
    arr = np.arange(0,10,1) 
    print(arr)
    # 결과값 : [0 1 2 3 4 5 6 7 8 9]

## random 함수는 실행시킬 때마다 다른 값을 추출한다.
* random 값 도출도 하나의 알고리즘을 이용한 순열을 프로그램적으로 뽑아내느 것.
* 이 알고리즘을 사용할 때 특정 초기값을 이용해서 알고리즘을 돌린다.
* (seed값) 만약 이 seed값이 동일하다면 random 알고리즘은 당연히 같은 난수를 도출한다.(난수의 재연)
###
    import numpy as np
    
    np.random.seed(5)   #seed 값 고정을 통한 난수 재연
    arr = np.random.randint(0,100,(3,4))     #0부터 99까지 숫자를 3행 4열로 표현
    print(arr)
    # 결과값 : [[99 78 61 16]
               [73  8 62 27]
               [30 80  7 76]]

## (중간 정리) NumPy 특징
1. NumPy 특징
   - Vector, Matrix 계산에 특화된 python library
   - 빠른 속도, 적은 메모리, ndarrary라는 배열이용
   - 다른 library의 기반이 되는 library
2. NumPy array(ndarray) 생성
   - np.array(list, dtype=np.float64)
3. NumPy array 특징
   - arr.ndim, arr.shape, arr.size, axis,
4. NumPy array 다양한 생성 함수
   - zeros(), ones(), full(), empty(),
   - zeros_like(), arange, linspace()
   - random기반 생성 함수

## numpy의 파일 입출력은 크게 2가지로 나누어진다.
1. binary 형태로 저장하고 불러올 수 있다. (눈으로 확인 불가능)
2. text 형태로 저장하고 불러올 수 있다. (눈으로 확인 가능)
###
    # 1. binary 형태로 저장하고 불러올 수 있다. (눈으로 확인 불가능)
    import numpy as py
    
    # 사용데이터, 정수 난수 추출
    arr1 = np.random.randint(0,100,(5,4))   #0~100구간에서 5행 4열, 즉 20개 뽑아서 저장해
    arr2 = np.random.randint(0,100,(2,7))    #0~100구간에서 2행 7열, 즉 14개 뽑아서 저장해
    
    # 1-1. 배열 1개를 파일 1개에 저장 (binary형태로)
    # 파일의 확장자가 정해져 있다.(.npy)
    np.save("./save_bin",arr1)   #./현재폴더 안에,  save_bin 임의파일이름, arr1이름 배열을 저장
    
    # 1-2. 배열 2개 이상을 파일 1개에 저장(binary형태로)
    # 파일의 확장자가 정해져 있다.(.npz)
    np.savez("./savez_bin",arr1,arr2)   #./현재폴더 안에,  save_bin 임의파일이름, arr1, arr2 배열을 저장
    
    # 1-3. binary형태의 파일에서 배열 1개짜리 numpy array를 읽어들이기
    new_arr1 = np.load("./save_bin.npy")
    
    # 1-4. binary형태의 파일에서 배열 2개짜리 numpy array를 읽어들이기
    new_arr1 = np.load("./savez_bin.npz")
    new_arr1.files   # 출력값 : ['arr_0', 'arr_1']
    
    new_arr1["arr_0"]   #배열을 여러개 읽어들였을 때 각각의 배열에 대한 reference 사용법
    new_arr1["arr_1"]   #new_arr1.files를 검색 후 이렇게 2개를 쓸 수 있다
###
    # 2. text 형태로 저장하고 불러올 수 있다. (눈으로 확인 가능)
    import numpy as py
    
    # 사용데이터
    arr = np.random.randn(5,4)   #표준정규분포로 5행 4열짜리
    
    # 2-1. 파일 저장
    # csv(csv:comma seperated value : 콤마 구분된 데이터)파일에 저장
    #./현재폴더 안에,  savetxt.csv 임의파일이름, arr이름 배열을 저장
    # delimiter=","  ,(콤마)를 기준으로 데이터가 
    # fmt="%.3f" 형식도 지정 가능(소수점 세번째 자리까지)
    np.savetxt("./savetxt.csv", arr, delimiter=",", fmt="%.3f") 
    
    # 2-2. 파일 로드
    # text file에서 데이터를 불러오기 위해서는
    new_arr = np.loadtxt("./savetxt.csv", delimiter=",", dtype=np.float64)
    print(new_arr)

## skiprows, dtype
    import numpy as py
    
    # skiprows = 1 위에서 1줄을 날려버리고 아래를 출력(필드나 설명같은게 첫번째 오기때문에 필요없을때 스킵)
    # dtype={ keys : values}
    new_arr = np.loadtxt("./savetxt.csv", 
                         delimiter=",", 
                         skiprows=1, 
                         dtype={
                             "names" : ("name","dept","year","grade"),
                                "formats" : ("S10","S10","i","f")})
    print(new_arr)
    # 결과값 : 
    [(b'"Lee"', b'"CS"', 3, 3.7) (b'"Park"', b'"MECHANIC"', 1, 2.2)
    (b'"Kim"', b'"MATH"', 2, 4.1)]
    
    print((new_arr[1][1]).decode())
    # 결과값 : "MECHANIC"





## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>