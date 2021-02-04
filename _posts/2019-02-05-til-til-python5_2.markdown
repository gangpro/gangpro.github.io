---
layout: post
title: '[python] 파이썬 Numpy Array'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-05 00:12:17 +0900
lastmod: 2019-02-05 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Numpy Array에 대해서 <br />

# Numpy Array
> NumPy - 행렬 연산을 위한 핵심 라이브러리<br> 
> Numpy Array - 배열 처리

## numpy array의 형태변환
1. reshape(), View를 생성하는 개념, 데이터 공유(원본과 뷰)
2. ravel(), View를 생성 다차원 -> 1차원 numpy array 변환
3. resize(), 새로운 배열이 생성, 원소의 개수가 달라도 변환이 가능
   - 원본을 변환 또는 새로운 배열 생성 두가지 방법이 있다.

## numpy array의 .shape을 조절해 보자.
* shape을 쓰면 -> x차원과 배열의 형태, 원소의 개수를 알 수 있다.
```
    import numpy as np
    
    # numpay array 생성
    arr = np.arange(0,12,1)   # 0~12(0~11까지 표시됨)까지 1씩 증가하는
    print(arr)         # 결과값 : [ 0  1  2  3  4  5  6  7  8  9 10 11]
    print(arr.shape)   # 결과값 : (12,)   #원소가 하나이기 때문에 1차원 배열이다.   # 튜플로 되어 있음
    
    arr1 = arr.reshape(4,3)   # 원래 arr에서 형태를 4행 3열로 변환시키자.
    print(arr1)
    # 결과값 : 
    [[ 0  1  2]
     [ 3  4  5]
     [ 6  7  8]
     [ 9 10 11]]

    # ※ reshape할 때 조심해야 할 점은, 새로 생성된 numpy배열이 view로 생성된다.
    # 다시 말해, 진짜 새로운 배열이 만들어 지는게 아니라 보이는 형태만 나타낸다.
    # 즉, 하나의 데이터(arr)를 공유한다. 예시는 아래와 같다.

    arr[0] = 100   # arr원본 데이터 0을 100으로 바꾸면 arr과 arr1의 데이터 둘다 바뀐다. 왜? 데이터를 공유하니깐
    print(arr)     # 결과값 : [100   1   2   3   4   5   6   7   8   9  10  11]
    print(arr1)
    # 결과값 : 
    [[100   1   2]
     [  3   4   5]
     [  6   7   8]
     [  9  10  11]]
    
    # 위와 같은 형태를 View라고 한다.
    # View는 원본 배열과 데이터를 공유하기 때문에 
    # 둘중 하나가 바뀌면 원본이 변하든, View가 변하든 둘다 데이터에 영향을 미친다.
```

```
    if arr1.base is arr:      # .base는 만약 view일 경우 원본데이터를 어디서 땡겼니? 라는 함수
        print("데이터가 같아요!")
    else:
        print("데이터가 달라요!")
        
    # 결과값 : 데이터가 같아요!
```

## .reshape을 하고 원본 데이터를 공유하고 싶지 않을 때
```
    import numpy as np
    
    arr = np.arange(0,12,1)
    arr2 = arr.reshape(2,6).copy()   # 해당 원본데이터(arr)에 copy()를 써서 새로운 데이터를 만든다.
                                     # 새로운 배열(데이터 공유하지 않는다.) # view가 아니다.

    print(arr)   
    # 결과값 : [ 0  1  2  3  4  5  6  7  8  9 10 11]
    print(arr2)
    # 결과값 : 
    [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]]
        
    arr[0] = 100
    print(arr)
    # 결과값 : [100   1   2   3   4   5   6   7   8   9  10  11]
    print(arr2)
    # 결과값 : 
    [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]]
```

## reshape에 대해서 +@
* 자동 행과 열 계산
* 차원의 원소개수를 -1로 지정하면 배열의 전체 원소 개수와 확장된 차원 크기를 기반으로 계산을 통해서 배열을 생성.
```
    import numpy as np
    
    arr = np.arange(0,12,1)   #요소가 12개인 numpay array #0,1,2,3,4,5,6,7,8,9,10,11 1씩증가.
    arr1 = arr.reshape(-1,4)   #행은 아직 정해지지 않았는데 열은 '4열로 구성해'라는 의미.
                               #그럼 결과적으로 자동 계산되어 3행이 자동(auto)으로 결정된다. 
        
    arr1 = arr.reshape(4,-1)   #열은 아직 정해지지 않았는데 행은 '4행으로 구성해'라는 의미.
                               #그럼 결과적으로 자동 계산되어 3열이 자동(auto)으로 결정된다. 
```

## 다차원배열을 1차원 배열로 변환   .ravle()
```
    import numpy as np

    arr2 = np.random.randint(0,12,(3,4))   #균등분포로 되어있는 난수 추출, 2차원 형태의 3행 4열짜리 
    print(arr2)
    # 결과값 : 
    [[ 4  0  3  3]
     [11  7  5  8]
     [ 8  0  1  5]]
    
    arr3 = arr2.ravel()   #ravle() 다차원함수를 1차원 함수로 데이터를 추출
    print(arr3)           # 데이터를 공유하는 view 형태로 출력
    # 결과값 : [ 4  0  3  3 11  7  5  8  8  0  1  5]

    
    arr3 = arr2.ravel().copy()   #ravle() 다차원함수를 1차원 함수로 데이터를 추출
    print(arr3)           # copy()를 써서 데이터를 공유하지 않는 view 형태로 출력
    # 결과값 : [ 4  0  3  3 11  7  5  8  8  0  1  5]
```

## reshape에 대해서 +@
* .resize()는 .reshape()과 유사
* 2가지 방법으로 변환할 수 있다. 원본변경 방법, view로 리턴 방법
```
    import numpy as np
    
    arr = np.arange(0,12,1)
    print(arr)
    # 결과값 : [ 0  1  2  3  4  5  6  7  8  9 10 11]

    #arr1 = arr.reshape(3,5)   # arr의 12개요소에서 3행 5열은 못 만든다. 요소 개수가 틀려 에러가 난다.
    arr2 = arr.resize(2,6)  # resize로 shape을 변경
    print(arr2)    # 결과값 : None   # 형태를 바꾼 후 원본 데이터로 바꾼다. 
    print(arr)     # 그래서 원본인 arr의 데이터가 1차원에서 2차원으로 변경 됐다.
    결과값 : 
    [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]]
     
    # 배열 resize()하면 원본을 변경, return은 None
    arr = np.arange(0,12,1)
    arr2 = np.resize(arr,(2,6)) #resize로 shape을 변경
    print(arr2)   #이런 경우 view로 리턴되고 원본이 바꾸지 않는다.
    # 결과값 : 
    [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]]
    print(arr)
    # 결과값 : [ 0  1  2  3  4  5  6  7  8  9 10 11]
    
    # 원본을 변경할 것인지 혹은
    # 원본 변함없이 view를 return 받을 것이지 선택 해야한다.
    
    arr = np.arange(0,12,1)
    arr.resize(3,5)   # arr의 12개 요소를 가지고 3행 5열의 15개 요소로 만들 수 있다.
    print(arr)        # 모자른 원소는 0을 채워 넣는다.
    # 결과값 : 
    [[ 0  1  2  3  4]
     [ 5  6  7  8  9]
     [10 11  0  0  0]]
    
    arr.resize(2,2)   # 크기가 작을때는 앞에것을 남기고 뒤에것 나머지를 버린다.
    print(arr)
    # 결과값 : 
    [[0 1]
     [2 3]]
```
 
## numpy array의 결합
1. vstack(), hstack() : 두개의 array를 결합
* numpy array 연결(2개의 array를 행, 열 방향으로 연결)(두가지 방법으로 연결시킬 수 있다.)
* 행 방향으로 붙이고 싶다면 열의 개수가 같아야한다. .vstack()
* 열 방향으로 붙이고 깊다면 행의 개수가 같아야한다. .hstack()
```
    import numpy as np
    
    arr1 = np.array([[1,2,3],
                     [4,5,6]])   # 2행 3열짜리 2차원 배열
    arr2 = np.array([[1,1,1],
                     [2,2,2],
                     [3,3,3],
                     [4,4,4]])   # 4행 3열짜리 2차원 배열
    print(arr1)
    # 결과값 : 
    [[1 2 3]
     [4 5 6]]
     
    print(arr2)
    # 결과값 : 
    [[1 1 1]
     [2 2 2]
     [3 3 3]
     [4 4 4]]


    # 두개의 배열을 연결하려고 하는데
    # 행 방향(수직방향)으로 연결하려면 두 배열의 열의 개수가 같아야한다. 대신 행의 개수는 상관 없다.
    result = np.vstack((arr1,arr2))  # 2개의 인자를 받아서 튜플형태로
    print(result)   # 결과로 새로운 배열이 return 된다.(view가 아니다.)
    # 결과값 : 
    [[1 2 3]
     [4 5 6]
     [1 1 1]
     [2 2 2]
     [3 3 3]
     [4 4 4]]


    arr1 = np.array([[1,2,3],
                     [4,5,6]])   # 2행 3열짜리 2차원 배열
    arr2 = np.array([[9,9,9,9],
                     [9,9,9,9]]) # 2행 4열짜리 2차원 배열

    # 두개의 배열을 연결하려고 하는데
    # 열 방향(수평방향)으로 연결하려면 두 배열의 행의 개수가 같아야한다. 대신 열의 개수는 상관 없다.
    result = np.hstack((arr1,arr2))  # 2개의 인자를 받아서 튜플형태로
    print(result)
    # 결과값 : 
    [[1 2 3 9 9 9 9]
     [4 5 6 9 9 9 9]]
```

## numpy array의 참조¶
1. indexing : 원하는 요소만 가져오기
2. slicing : slicing해서 가져온 게 view를 생성(새로운 배열이 생성되는게 아니다 X)
3. boolean indexing(True/False 값중 True 값만 추출)
4. index 배열을 이용해서 참조 : fancy indexing

## 1차원 numpy array의 indexing, slicing
```
    import numpy as np
    
    arr = np.arange(10,20,1)
    
    for item in (arr):                # 요소 값이 나온다.
                                      # ex) 10, 11, 12, 13 ~~ (값만 나옴)
        print("값 : {}".format(item))
        # 결과값 : 
                값 : 10
                값 : 11
                값 : 12
                값 : 13
                값 : 14
                값 : 15
                값 : 16
                값 : 17
                값 : 18
                값 : 19
    
    for idx,item in enumerate(arr):   #enmerate를 쓰면 인덱스값과 요소값이 같이 나온다.
                                      #ex) 0 10, 1 11, 2 12, 3 13 ~~ (튜플형태로 나온다.)
        print("인덱스 : {}, 값 : {}".format(idx,item))
        # 결과값 : 
                인덱스 : 0, 값 : 10
                인덱스 : 1, 값 : 11
                인덱스 : 2, 값 : 12
                인덱스 : 3, 값 : 13
                인덱스 : 4, 값 : 14
                인덱스 : 5, 값 : 15
                인덱스 : 6, 값 : 16
                인덱스 : 7, 값 : 17
                인덱스 : 8, 값 : 18
                인덱스 : 9, 값 : 19    
```

```
    import numpy as np
        
    arr = np.arange(10,20,1)
    print(arr)
    # 결과값 : [10 11 12 13 14 15 16 17 18 19]
    
    print(arr[0])
    # 인덱싱 결과값 : 10
    print(arr[0:3]) 
    # 슬라이싱 결과값 : [10 11 12]     # view 형태로 만든다.
```

```
    import numpy as np    
    
    arr = np.arange(10,20,1)
    print(arr)
    # 결과값 : [10 11 12 13 14 15 16 17 18 19]
    
    tmp = arr[0:3]
    print(tmp)
    # 결과값 : [10 11 12]
    
    tmp[0] = 1000    # 0번째 인덱스값의 요소를 1000으로 변경
    print(tmp)
    # 결과값 : [1000   11   12]
    
    print(arr)   # numpy array의 slicing은 view를 생성
    # 결과값 : [1000   11   12   13   14   15   16   17   18   19]
```

```
    import numpy as np    
        
    arr = np.arange(10,20,1)
    print(arr[1:-1])     # 결과값: [11 12 13 14 15 16 17 18]
    print(arr[0:-1])     # 결과값: [10 11 12 13 14 15 16 17 18]
    print(arr[0:-1:2])   # 결과값: [10 12 14 16 18]   # 0부터 -1까지 2칸씩 가라
    print(arr[::3])      # 결과값: [10 13 16 19]   # 처음부터 끝까지 3칸식
    print(arr[::-1])     # 결과값: [19 18 17 16 15 14 13 12 11 10]   #배열 역순처리  #reverse 효과를지닌다. 
```

## 2차원 numpy array의 대한 slicing
```
    import numpy as np
    
    arr = np.array([[1,2,3],
                    [4,5,6],
                    [7,8,9]])
    
    print(arr[0,0])   #numpy array에서는 콤마를 기준으로 행열을 나눔
    # 결과값 : 1
    
    print(arr[:,0])
    # 결과값 : [1 4 7]
    
    print(arr[:,0:2])
    # 결과값 : 
            [[1 2]
             [4 5]
             [7 8]]

    print(arr[1:,:2])
    # 결과값 : 
            [[4 5]
             [7 8]]
             
    # 참고로 print(arr[0][0])  #python list에서는 []기준으로 행열을 나눔
```

## mask와 boolean indexing
* numpy는 기본적으로 boolean indexing을 지원
```
    import numpy as np
    
    np.random.seed(1)   # random값을 seed함수를 써서 값이 고정된다.
    arr = np.random.randint(0,10,(10,))
    print(arr)
    # 결과값 : [5 8 9 5 0 0 1 7 6 9]
    
    print(arr+1)   # 이항연산 가능
    # 결과값 : [ 6  9 10  6  1  1  2  8  7 10]
    
    print(arr % 2 == 0)  #논리연산 가능 그래서 결과값은 True 또는 False가 나온다.
    # 결과값 : [False  True False False  True  True False False  True False]

    tmp = (arr % 2 == 0) # boolean mask 
    print(arr[tmp])   # True 해당 요소만 뽑는다.   # boolean indexing
    # 결과값 : [8 0 0 6]
```

## Fancy indexing 
* 배열의 index배열을 전달해서 배열요소를 참조하는 방식
```
    import numpy as np
    
    np.random.seed(0)
    arr = np.random.randint(0,10,(4,5))   # 0~9까지 숫자를 이용해 4행 5열 형태로
    print(arr)
    # 결과값 : [[5 0 3 3 7]
               [9 3 5 2 4]
               [7 6 8 8 1]
               [6 7 7 8 1]]
    
    print(arr[0,[1,3]])   # 열과 행 부분에 배열이 들어가고 그 안에 index가 들어간다.  
    # 결과값 : [0,3]
    
    print(arr[3,[0,1]])   
    # 결과값 : [6 7]
```

## (정리) 배열 사용 시 총 4가지 방식
1. indexing
2. slicing
3. boolean index
4. fancy indexing(True/False 중 True값만 추출)

## numpy array 정리
* numpy array의 형태변환
  1. reshape(), View를 생성하는 개념
  2. ravel(), View를 생성 다차원 -> 1차원 numpy array 변환
  3. resize(), 새로운 배열이 생성, 원소의 개수가 달라도 변환이 가능
   - 원본을 변환 또는 새로운 배열 생성 두가지 방법이 있다.

* numpy array의 결합
  1. vstack(), hstack() : 두개의 array를 결합

* numpy array의 참조
  1. indexing : 원하는 요소만 가져오기
  2. slicing : slicing해서 가져온 게 view를 생성(새로운 배열이 생성되는게 아니다 X)
  3. boolean indexing(True/False 값중 True 값만 추출)
  4. index 배열을 이용해서 참조 : fancy indexing

## numpy array의 이항연산( 사칙연산 + - * / )
* 배열의 shape가 같아야 이항연산이 가능하다.
```
    import numpy as np
    
    np.random.seed(0)   # 랜덤값 고정
    arr1 = np.random.randint(0,10,(2,3))   # 0~9까지의 임의의수의 2행 3열
    arr2 = np.random.randint(0,10,(2,3))   # 0~9까지의 임의의수의 2행 3열
    print(arr1)
    # 결과값 : 
            [[5 0 3]
             [3 7 9]]
    print(arr2)
    # 결과값 : 
            [[3 5 2]
             [4 7 6]]
    print(arr1 + arr2)   # 해당 인덱스 값끼리 덧셈한다. 
    # 5 0 3 + 3 5 2 = 8 5 5
    # 3 7 9 + 4 7 6 = 7 14 15
    # 결과값 : 
    [[ 8  5  5]
     [ 7 14 15]]
    # 즉 shape이 같아야 사칙연산이 가능하다.
```

## numpy array의 행열곱(dot product)   .dot()
* ex) (1)2행3열과 (2)3행2열의 곱 = (1)의 열 숫자와 (2)의 행 숫자가 같아야함 = 결과는 2행 2열로 나온다. 
```
    import numpy as np
    
    np.random.seed(0)   # 랜덤값 고정
    arr1 = np.random.randint(0,10,(2,3))   #0~9까지의 임의의수의 2행 3열
    arr2 = np.random.randint(0,10,(2,3))   #0~9까지의 임의의수의 2행 3열
    arr3 = np.random.randint(0,10,(3,2))   #0~9까지의 임의의수의 3행 3열
    print(arr1)
    # 결과값 : 
    [[5 0 3]
     [3 7 9]]
         
    print(arr2)
    # 결과값 : 
    [[3 5 2]
     [4 7 6]]
    
    print(arr3)
    # 결과값 : 
    [[8 8]
     [1 6]
     [7 7]]
    
    print(np.dot(arr1,arr3))
    # 5 0 3               8 8
    # 3 7 9     행렬곱     1 6         (1)2행3열과 (2)3행2열의 곱
    #                     7 7         = (1)의 열 숫자와 (2)의 행 숫자가 같아야함 = 결과는 2행 2열로 나온다.
    # 결과값 : 
    [[ 61  61]
     [ 94 129]]
```

## numpy array의 브로드캐스팅 작업이 일어나면 이항연산(사칙연산 + - * / 가능)이 가능해진다.
* (1)4행2열의 2차원 배열과 (2)1행2열의 1차원 배열을 행열곱하면 
* 자동적으로 (2) 1차원 배열이 2차원 배열로 된다.
```
    #[[1 2],         [1 1]      1 2     1 1
    # [3 4],   작업          ->  3 4     1 1    브로드캐스팅 작업이 이루어 진다.
    # [5 6],                    5 6     1 1
    # [7 8]]                    7 8     1 1

    import numpy as np
    
    arr1 = np.array([[1,2,3],
                    [4,5,6]])
    arr2 = np.array([7,8,9])
    
    print(arr1)
    # 결과값 : 
            [[1 2 3]
             [4 5 6]]
     
    print(arr2)
    # 결과값 : 
            [7 8 9]

    print(arr1+arr2)   #broadcasting이 일어나고 연산이 가능해진다.
                       #(2)자동으로 형태를 만들 수 있으면 브로드캐스팅이 일어나고 안되면 안된다.
                       # 2행3열과 1행2열의 경우는 자동으로 형태를 만들 수 없기 때문에 브로드캐스팅 작업 안일어남 
    # 결과값 : 
            [[ 7 16 27]
             [28 40 54]]
```

## 두개의 배열이 내용이 같은지 전체비교하는 함수
* np.array_equal(N,M)
```
    import numpy as np
    
    arr1 = np.arange(10)
    arr2 = np.arange(10)
    
    print(arr1)   # 결과값 : [0 1 2 3 4 5 6 7 8 9]
    print(arr2)   # 결과값 : [0 1 2 3 4 5 6 7 8 9]
    
    if arr1 is arr2:
        print("두개가 같다.")
    else:
        print("두개가 다르다.")
    # 결과값 : 두개가 다르다.
    # 객체 arr1, arr2로 다르게 만들었기 때문에 배열값은 똑같지만 객체는 다르다.
    
    print(np.array_equal(arr1,arr2))   # 배열안의 내용 비교
    # 결과값 : True
```

## numpy array의 1차원 집계함수(계산하는 함수)
* .sum()   # 합
* .mean()  # 평균
* .max()   # 최대값
* .min()   # 최소값
```
    import numpy as np
    
    np.random.seed(0)
    arr = np.random.randint(0,10,(5,))   # 0~9까지 난수 정수형 5개 뽑기
    print(arr)
    # 결과값 : [5 0 3 3 7]
    
    # 두가지로 출력 가능
    print(arr.sum())    # 결과값: 18   # 배열에 대해 sum해도 되고
    print(np.sum(arr))  # 결과값: 18   # np에 sum이 있는데 배열(arr)로 해도 된다.
    
    print(arr.mean())   # 결과값: 3.6   # 평균
    print(np.mean(arr)) # 결과값: 3.6
    
    print(arr.max())    # 결과값 : 7    # 최대값
    print(np.max(arr))  # 결과값 : 7
    
    print(arr.min())    # 결과값 : 0    # 최소값
    print(np.min(arr))  # 결과값 : 0
```
    
## numpy array의 2차원 집계함수와 축(axis)에 대해
* 1차원 배열로 결과값이 나온다.
```
    import numpy as np
    
    np.random.seed(0)
    arr = np.random.randint(0,10,(5,))   # 0~9까지 난수 정수형 5개 뽑기
    print(arr)   # 결과값 : [5 0 3 3 7]
    
    arr = np.random.randint(0,10,(3,2))
    print(arr)
    # 결과값 : 
            [[9 3]
             [5 2]
             [4 7]]
             
    print(arr.sum())   # 만약 sum의 괄호안에 axis= 가 없으면 None으로 간주된다.
    # 결과값: 30        # 연산의 대상이 배열 전체가 된다.
                       # 2차원 배열에서 axis=0 -> 행방향
                       # 2차원 배열에서 axis=1 -> 열방향
                       
    print(arr.sum(axis=0))   #결과적으로 행방향으로 값은 두개가 나오며 1차원 배열로 나온다.
    # 결과값 : [18 12]

    print(arr.sum(axis=1))   #결과적으로 열방향으로 값은 세개가 나오며 1차원 배열로 나온다.
    # 결과값 : [12  7 11]
    
    
    # <참고>
    # 1차원 배열에서는 axis=0 만 존재하며 -> 열 방향을 뜻한다.
    
    # 2차원 배열에서 axis가 x,y 2개의 축이 존재
    #              axis=0 -> 행 방향
    #              axis=1 -> 열 방향
    
    # 3차원 배열에서 axis가 x,y,z 3개의 축이 존재
    #              axis=0 -> 깊이
    #              axis=1 -> 행 방향
    #              axis=2 -> 열 방향
```

## Mask활용
```
    import numpy as np
    
    arr = np.array([True,False,True,False,False])
    print(arr)         # 결과값 : [ True False  True False False]
    print(arr.sum())   # 결과값 : 2   ->  True=1, False=0 이렇게 계산해서 결과값이 2는 True가 2개다라는 의미
                       # 배열안에 True가 몇개있는지 개수를 계산할때 사용한다.
        
    arr = np.random.normal(3,1,(4,5))  # 평균=3, 표준편차=1, 4행5열의 20개 추출을 의미
    (arr > 3.0).sum()   # 만들어진 배열안에 3.0초과하는 수를 True, False로 싹다 변경 후 .sum()
    # 결과값 : 10        # 조건을 만족하는 개수를 count할 때 사용
```

## 정렬관련 함수 2가지
* numpy.sort(배열,axis=n) : 원본은 변화 X, 정렬된 결과가 return된다.
* 배열.sort()             : 원본은 정렬 됨, return = None
```
    import numpy as np
    
    np.random.seed(0)
    arr = np.random.randint(0,10,(3,3))
    print("원본출력")
    print(arr)
    # 결과값 : 
            [[5 0 3]
             [3 7 9]
             [3 5 2]]    
    print(arr.sort())   # axis를 명시하지 않으면 axis=-1로 지정된다.   -1 = 마지막 축을 의미
    # 결과값 : None      # axis=1을 의미
                        # 원본이 변한다. 열 방향으로 오름차순 정렬
    print("배열 열방향 정렬 출력")
    print(arr)
    # 결과값 : 
            [[0 3 5]
             [3 7 9]
             [2 3 5]]
    
    print(arr.sort(axis=0))   # 원본이 변한다. 행 방향으로 오름차순 정렬
    # 결과값 : None
    
    print("배열 행방향 정렬 출력")
    print(arr)
    # 결과값 : 
            [[0 3 5]
             [2 3 5]
             [3 7 9]]
```

## 중복된거 배제하고 유니크한 것만 추출
* .unique() 함수
* 중복된거 배제하고 유니크한것만 추출
```
    import numpy as np
    
    np.random.seed(0)
    arr = np.random.randint(0,9,(7,))
    print(arr)       # 결과값 : [5 0 3 3 7 3 5]
    np.unique(arr)   # 결과값 : [0 3 5 7]   # 유일한 값만 추출
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>