---
layout: post
title: '[python] 파이썬에 대해서'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 13:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬에 대해서<br />

# Python (programming language)
> 파이썬(영어: Python)은 1991년 프로그래머인 귀도 반 로섬(Guido van Rossum)이 발표한 고급 프로그래밍 언어로, 플랫폼 독립적이며 인터프리터식, 객체지향적, 동적 타이핑(dynamically typed) 대화형 언어이다. 파이썬이라는 이름은 귀도가 좋아하는 코미디 〈Monty Python's Flying Circus〉에서 따온 것이다. 파이썬은 비영리의 파이썬 소프트웨어 재단이 관리하는 개방형, 공동체 기반 개발 모델을 가지고 있다. C언어로 구현된 C파이썬 구현이 사실상의 표준이다. [[위키백과]](https://ko.wikipedia.org/wiki/%ED%8C%8C%EC%9D%B4%EC%8D%AC)<br>

<br>

## Cell in jupyter notebook
* cell 의미 : 각각 칸을 cell이라고 부른다.
* 셀 단축키 a : cell을 누르면 초록색에서 파란색으로 되고 a를 누르면 해당 색 위에 cell이 생성된다.
* 셀 단축키 b : cell을 누르면 초록색에서 파란색으로 되고 b를 누르면 해당 색 아래로 cell이 생성된다.
* 셀 단축키 d : 지우고 싶은 cell을 누른 후에 dd를 누르면 cell이 지워진다.
* 명령창 단축키 shift + enter : 현재 cell을 실행시키고 아래쪽에 cell을 생성
* 왼쪽 In [N] 의미는 N번째로 실행 됐다는 표시. 이는 각 셀이 나뉘어있지만 시스템 내에서는 연결이 되어 있다는 뜻.
<br>

## 주석 처리 방법
    * 1줄 주석 : # 
    
    * 여러줄 주석1 : <br>
    """ <br>
    내용 <br>
    """ <br>
    
    * 여러줄 주석2 : <br>
    ''' <br>
    내용 <br>
    ''' <br>
    
    * 주석처리 하고 싶은 코드 클릭 드래그 후 : ctrl + / 
    
    * ※ 큰따옴표와 작은따옴표 혼용해서 쓰지 말자. 나의 경우 큰따옴표를 쓰도록 하자.
<br>

## Python

* python은 weak data type을 지향
* python은 변수를 선언할 때 data type을 명시하지 않는다. 
  * (참고) Java는 strong data type(변수를 지정한다.)<br>

* 아래와 같이 사용 가능
  * a = 10     (숫자) 
  * b = "Hello"     (문자) 
* python은 기본형이 존재하지 않는다. 그래서 모두 reference data type이다(즉, 모두 class이고 개체(entity)이다).
* 다시 말해 int, double 등등이 필요 없고 안 쓴다.
* 자바에서 int a => 4 byte 공간 확보( -21억 ~ + 21억 )
* 자바에서 double b => 8 byte 공간 확보
* 파이썬에서 int, double과 같은 개체 분류를 할 필요가 없다.
<br>

## python은 문자의 개념이 존재하지 않는다.
    a = 'Hello World'
    b = "Hello World"
* python은 작은따옴표('), 큰따옴표(")의 차이가 없다.
* 둘 중 어느것을 사용해도 상관이 없지만 혼용해서 쓰면 헷갈리기 때문에 일관성 있게 쓰자.
* 나의 경우는 큰따옴표만 사용할 것이다.
<br>

## 파이썬에서 단항 증감연산자는 존재하지 않는다.
    a = 100
    a += 1          a++ 문법은 에러 발생
    a -= 1          a-- 문법은 에러 발생
* ++, -- 연산자가 없어서 => +=, -=로 사용해야 한다.<br>
<br>

## 여러 변수에 값을 쉽게 할당(assign) 할 수 있다.
    a, b, c = 10, 20, 30
<br>

## 두개의 변수 값을 바꿀 때 swap 처리
    a, b = 10, 20
    b, a = a, b
    print(a,b)
<br>

## del 변수명 (변수 삭제)
    a = 100    #변수 지정
    print(a)   #변수 사용
    del a      #변수 삭제
    print(a)   #변수가 없기 때문에 에러 발생
<br>

## print() 함수의 특징
    print("이것은")
    print("소리 없는")
    print("아우성!!")

    # 결과값
    """
    이것은
    소리 없는
    아우성!!
    """    


    print("이것은", end=" ")
    print("소리 없는", end=" ")
    print("아우성!!")

    # 결과값
    이것은 소리 없는 아우성!!
<br>

## print() 와 display() 차이

    import numpy as np     # numpy는 추후 배울 내용으로 예시를 위해 잠시 사용

    arr = np.arange(0,12,1)   # 0~12(0~11까지 표시됨)까지 1씩 증가하는 숫자 생성
    print(arr)    # 결과값 : [ 0  1  2  3  4  5  6  7  8  9 10 11] 
    display(arr)  # 결과값 : array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])
                  # 콘솔 출력 형식  
                  # 아웃풋 파라미터와 똑같이 출력됨
                  
    arr           # 결과값 : array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])
                  # 콘솔 출력 형식
                  # ※ cell 안에서 맨 마지막에 있을 때만 실행 가능
<br>

## cell 실행시키는데 걸리는 시간 확인
    %%time
    # 결과값 : 
    # CPU times: user 3 µs, sys: 1e+03 ns, total: 4 µs
    # Wall time: 6.91 µs
    
    # ※ 주의할 점은 실해하고자 하는 cell 안에서 맨 위에 적어야하며 옆으로 주석처리도 하면 안됨.
    # 이런 오류를 볼 수 있음.. UsageError: Line magic function `%%time` not found.

## input()
* 입력 받은 내용은 무조건 문자열로 처리된다.
###
    a = input("이메일 주소를 입력하세요 : ")
    print(a)
<img width="902" alt="input()" src="https://user-images.githubusercontent.com/46523571/54869450-c9c7c580-4ddb-11e9-9e30-6c40c38e9a33.png">


## %%time - 해당 cell을 실행시키는데 걸리는 시간
    # 집계함수가 필요한 이유
    # 수학식을 이용하면 모든 집계함수를 프로그램적으로 구현 할 수 있다.
    # 우리가 다루는 데이터는 상당히 크기가 큰 데이터이다.
    
    import numpy as np
    
    arr = np.arange(100000000)
###
    %%time
    # 해당 cell을 실행시키는데 걸리는 시간을 알 수 있다.(1)
    
    result = 0.0
    for tmp in arr:
        result += tmp
        
    print(tmp)
    # 결과값 : 
            99999999
            CPU times: user 17.6 s, sys: 16.1 ms, total: 17.6 s
            Wall time: 17.6 s
###
    %%time
    # 해당 cell을 실행시키는데 걸리는 시간을 알 수 있다.(2)
    
    print(arr.sum())
    # 결과값 : 
            4999999950000000
            CPU times: user 58.8 ms, sys: 911 µs, total: 59.7 ms
            Wall time: 58.6 ms








## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>