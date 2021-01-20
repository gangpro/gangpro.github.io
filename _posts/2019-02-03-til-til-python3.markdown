---
layout: post
title: '[python] 파이썬 함수에 대해서'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-03 00:12:17 +0900
lastmod: 2019-02-03 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 함수에 대해서 <br />


# Python Function
> 파이썬 함수에 대해서
<br>
<br>

## python은 독립적인 함수를 사용자가 정의해서 사용할 수 있다. 
* 함수의 정의
* def 함수이름(인자):
* _ _ _ _4칸들여쓰기 후 함수가 하는 일(로직코드)을 적으면 된다.
###
    def my_sum(a, b):
        result = a + b
        return result
    
    print(my_sum(10, 20))
    # 결과값 : 30

## 아무런 일도 하지 않는 함수를 선언하는 경우
    def do_nothing():
        pass
        
    # 결과값 아무것도 없음.
    
## python 함수는 내장함수와 사용자정의함수(user define functio) 목차
* <내장함수>
* abs() -> x의 절대값을 구하는 함수
* all() -> 반복 가능한 대상(list)에 대해 모든 것이 True면 True
* any() -> 반복 가능한 대상(list)에 대해 True가 1개라도 있으면 True
* eval() -> 문자열로 입력된 수식을 직접 계산
* int() -> 입력값을 정수로 변환시키는 함수
* str() -> 입력값을 문자열로 변환시키는 함수
* len() -> 입력 parameter에 대한 길이를 구하는 함수
* list() -> 리스트를 생성하거나 리스트로 변환시키는 함수
* tuple() -> tuple로 변환시키는 함수
* type() -> 데이터타입을 알아내는 함수
* dir() -> 사용가능한 변수와 함수를 list로 반환시키는 함수(property&method를 알려준다.)
* id() -> 입력 객체의 고유 주소값(reference)을 return
* divmod(a,b) -> a를 b로 나눠서 몫과 나머지를 tuple로 리턴
* join() -> 리스트를 문자열로 변환할 때 많이 사용    
* max(), min(), pow() -> 최대값, 최소값, 제곱값
###
* <사용자정의함수>
* 가장 일반적인 형태(인자의 개수를 맞춰야 한다.)
* 인자의 개수에 영향을 받지 않도록 지정
* python은 두개 이상의 값을 return할 수 있다.(실제로는 tuple로 만들어서 return하는 거다.)
* default값을 이용할 수 있다.
* 외부변수를 사용할 수 있다. (scope에 대한 얘기)
<br>
<br>

# 내장함수
## 내장함수     abs()
    # abs(x) -> x의 절대값을 구하는 함수
    
    print(abs(-100))
    # 결과값 : 100
    
## 내장함수     all()
    # all() -> 반복 가능한 대상(list)에 대해 모든 것이 True면 True

    print(all([1,2,3,4,5]))   # 결과값 : true
    print(all([1,2,3,4,0]))   # 결과값 : false #0은 false를 뜻하기 때문에

## 내장함수     any()
    # any() -> 반복 가능한 대상(list)에 대해 True가 1개라도 있으면 True
    print(any([1,2,3,4,5]))   # 결과값 : true
    print(any([1,2,3,4,0]))   # 결과값 : true #0은 false를 뜻하지만 any()함수기 때문에 전체적으로는 true를 뜻함
    print(any(["",0,False,"Hello World"]))   # 결과값 : true
    
## 내장함수     eval()
    # eval() -> 문자열로 입력된 수식을 직접 계산
    print(eval("1+3*4"))
    # 결과값 :13

## 내장함수     int()
    # int() -> 입력값을 정수로 변환시키는 함수

## 내장함수     str()
    # str() -> 입력값을 문자열로 변환시키는 함수

## 내장함수     len()
    # len() -> 입력 parameter에 대한 길이를 구하는 함수

## 내장함수     list()
    # list() -> 리스트를 생성하거나 리스트로 변환시키는 함수

## 내장함수     tuple()
    # tuple() -> tuple로 변환시키는 함수

## 내장함수     type()
    # type() -> 데이터타입을 알아내는 함수
    print(type(1))          # 결과값 : <class 'int'>
    print(type("Hello"))    # 결과값 : <class 'str'>

## 내장함수     dir()
    # dir() -> 사용가능한 변수와 함수를 list로 반환시키는 함수(property&method를 알려준다.)
    print(dir("Hello"))
    # 결과값 : ['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']

## 내장함수     id()
    # id() -> 입력 객체의 고유 주소값(reference)을 return
    print(id("Hello"))     # 결과값 : 4511031680     숫자를 직접적으로 사용할 일은 없으나, 추후 데이터의 값사이를 비교할 때

## 내장함수     divmod(a,b)
    # divmod(a,b) -> a를 b로 나눠서 몫과 나머지를 tuple로 리턴
    divmod(10, 3)     #결과값 : (3, 1)    (몫:3, 1:나머지)

## 내장함수     join()
    # join() -> 리스트를 문자열로 변환할 때 많이 사용    
    my_list = ["Hello","World","!!"]     # 리스트안에 있는 문자열들을 "".join() 사용
    "".join(my_list)     # 결과값 : 'HelloWorld!!'

## 내장함수     max()     min()     pow()
    # max(), min(), pow() -> 최대값, 최소값, 제곱값
    max([9,5,2,10,6,7,8])     # 결과값 : 10
    min([9,5,2,10,6,7,8])     # 결과값 : 2
    pow(2, 10)     # 결과값 : 1024     # 2의 10제곱, 2의 10승
     
## 내장함수 참고     x.sort()   &   sorted(x)
    my_list = [7,4,5,9,2,3]
    result = my_list.sort()    # my_list가 갖고 있는 메소드
    print(result)     # 결과값 : None
    print(my_list)    # 결과값 :[2, 3, 4, 5, 7, 9]     my_list원본 변환
    
    my_list = [7,4,5,9,2,3]
    result = sorted(my_list)    #sorted란 메소드
    print(result)     # 결과값 : None
    print(my_list)    # 결과값 :[7, 4, 5, 9, 2, 3]     my_list 원본을 변환하지 않고 정렬된걸 리턴값만 표시한다.
<br>
<br>

# 사용자 정의 함수(user define function)
## 가장 일반적인 형태(인자의 개수를 맞춰야 한다.)
    def my_calc(a, b):
        result = a + b
        return result
    
    my_calc(100, 200)
    결과값 : 300

## 인자의 개수에 영향을 받지 않도록 지정
    def my_calc(*args):     #파라미터 부분에 (*args = 인자 여러개 받아서 tuple로 처리 의미)
        result = 0
        for tmp in args:     #tmp 변수
            result += tmp
        return result
    
    my_calc(1, 2, 3, 4, 5)
    결과값 : 15
    
## python은 두개 이상의 값을 return할 수 있다.(실제로는 tuple로 만들어서 return하는 거다.)
    def my_calc(a, b):
        result1 = a + b
        result2 = a * b
        return result1, result2
    
    a, b = my_calc(10, 20)
    print(a, b)
    # 결과값 : 30 200
    
## default값을 이용할 수 있다.
* ※ 주의사항 : default 값은 파라미터의 맨 끝에만 나올 수 있다. 그래서 중간에 하나 넣는건 안된다. 
* def my_calc(a, b = False, c):   안됨
* def my_calc(a, b = False, c = False):   이건 됨
###
    def my_calc(a, b, c = False):     # 마지막 c값은 default값
        if c:
            return a
        else:
            return b
        
    my_calc(10, 20)         # 결과값 : 20
    my_calc(10, 20, True)   # 결과값 : 10

## 외부변수를 사용할 수 있다. (scope에 대한 얘기)
    result = 0
    
    def my_calc(a, b):
        result += (a + b)     # (a + b)를 해서 result에 넣어야하는데 초기 위에 있는 result값이 없기 때문에 누적시킬 수가 없어서 에러가 발생
        return result
    
    my_calc(10, 20)
    print(result)     # error 발생
    # 결과값 : 
    ---------------------------------------------------------------------------
    UnboundLocalError                         Traceback (most recent call last)
    <ipython-input-47-471fafde2634> in <module>
          8     return result
          9 
    ---> 10 my_calc(10, 20)
         11 print(result)
    
    <ipython-input-47-471fafde2634> in my_calc(a, b)
          5 
          6 def my_calc(a, b):
    ----> 7     result += (a + b)     # (a + b)를 해서 result에 넣어라
          8     return result
          9 
    
    UnboundLocalError: local variable 'result' referenced before assignment

## 외부변수를 사용할 수 있다. (scope에 대한 얘기)
    result = 0
    
    def my_calc(a, b):    
        global result     
        result += (a + b)     # (a + b)를 해서 result에 넣어야하는데 초기 위에 있는 result값이 없기 때문에 누적시킬 수가 없어서 에러가 발생
        return result
    
    my_calc(10, 20)
    print(result)       # 함수 정의하면서 global result를 쓰면
    # 결과값 : 30        # 외부에 있는 위에 result를 함수 내에서 사용하겠어라는 의미 
                        # 코드가 함수밖(result = 0)과 함수안(result += (a + b)이 서로 연결되어 있기 때문에 유지보수가 어려움
                        # 지양하는 방법







## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>