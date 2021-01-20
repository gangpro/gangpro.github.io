---
layout: post
title: '[python] 파이썬 데이터 타입 - Tuple'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 15:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 데이터 타입 - Tuple<br />


# Python Data Type
1. 숫자형 (number)
2. 문자열 (string)
3. 리스트 (List) : 파이썬에서는 순수 배열이 없다. #자바에서 Arraylist와 유사한 형태가 바로 파이썬 list
4. **튜플 (tuple)** : 자바에는 없는 존재
5. 딕셔너리 (dictionary) : key와 value가 쌍으로 구성이 되어 있는것.  #자바에서 해쉬맵과 같은 형태
6. 집합 (Set) : 자바에서 Set과 유사
7. 논리형 (bool) : True와 False가 있다. true와 false가 아니다. 첫글자는 무조건 대문자!
8. 날짜 (date)     
<br>
<br>

# 4. 튜플 (tuple)
* python의 자료구조 중 list와 상당히 유사
* 기호 : ()  (튜플은 (소괄호) 사용, 리스트는 [대괄호] 사용)
* tuple은 tuple안의 값의 수정과 삭제가 불가능 X   -> read only
* list는 list 안의 값의 수정과 삭제가 가능 O
* 형태
###
    my_tuple = ()                   # 내용이 empty인 tuple
    my_tuple = (1, 2, 3)            # tuple의 literal
    my_tuple = (1,)                 # 원소가 1개 있는 튜플의 literal
    my_tuple = (1, 3.14, "Hello")   # 일반적인 경우

## tuple은 소괄호()를 생략할 수 있다.
    my_tuple = (1, 2, 3)
    my_tuple = 1, 2, 3
    
    (a, b) = (10, 20)
    a, b = 10, 20
    
    my_tuple = 1, 2, 3, 4, 5, 6
    print(my_tuple[1])   # tuple에 대해서 indexing
    #결과값 : 2
    
    print(my_tuple[1:3])   # tuple에 대한 slicing 그 결과도 tuple
    #결과값 : (2, 3)
    
    #my_tuple[0] = 100    # tuple은 값의 수정이 안된다.

    my_tuple = 1, 2, 3
    result = my_tuple + (4, 5, 6)   #튜플 내 데이터는 수정 삭제는 안되지만 +, * 등의 연산자를 사용하면 확장은 가능
    print(result)                   #기존 튜플(my_tuple = 1, 2, 3)이 확장되는게 아니라 
                                    #새로운 튜플(1, 2, 3, 4, 5, 6)이 확장된게 만들어 진거다.

## list와 tuple 간의 변환
* list -> tuple 가능 O
* tuple -> list 가능 O
###
    my_list = [1, 2, 3]
    my_tuple = tuple(my_list)
    print(my_tuple)
    # 결과 튜플 형태 값 : (1, 2, 3)
    # list -> tuple 가능 O

###
    my_tuple = (1, 2, 3)
    my_list = list(my_tuple)
    print(my_list)
    # 결과 리스트 형태 값 : [1, 2, 3]
    # tuple -> list 가능 O


## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>