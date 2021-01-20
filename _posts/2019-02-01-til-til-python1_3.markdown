---
layout: post
title: '[python] 파이썬 데이터 타입 - List'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 14:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 데이터 타입 - List<br />


# Python Data Type
1. 숫자형 (number)
2. 문자열 (string)
3. **리스트 (List)** : 파이썬에서는 순수 배열이 없다. #자바에서 Arraylist와 유사한 형태가 바로 파이썬 list
4. 튜플 (tuple) : 자바에는 없는 존재
5. 딕셔너리 (dictionary) : key와 value가 쌍으로 구성이 되어 있는것.  #자바에서 해쉬맵과 같은 형태
6. 집합 (Set) : 자바에서 Set과 유사
7. 논리형 (bool) : True와 False가 있다. true와 false가 아니다. 첫글자는 무조건 대문자!
8. 날짜 (date)     
<br>
<br>

# 3. 리스트 (List)
* 기본 python(라이브러리를 사용하지 않는)에는 배열이 없다.
* Java의 ArrayList와 상당히 유사
* 임의의 객체를 순서대로 저장하는 집합 자료형
* 표현방법은 [] 를 이용 (대괄호)
###
    my_list = list()         # list() 내장함수를 이용해서 list 생성
    print(type(my_list))     # list란 클래스의 인스턴스라는 의미     #결과값 : <class 'list'>
    print(my_list)           # list의 data type을 확인     #결과값 : []
    
## list의 요소는 다른 데이터 타입을 사용할 수 있다.
    my_list = [1, 2, 3, 4]                 # 결과값 : [1, 2, 3, 4]
    my_list = [1, 3.14, "Hello", True]     # 결과값 : [1, 3.14, 'Hello', True] #list의 요소는 다른 데이터 타입을 사용할 수 있다.
    my_list = [1, 2, ["Show", "me", "the", "Money!!"], 3.14, True]     # list 안에 list가 올 수 있다. (이중 리스트)
    print(my_list) 
    # 결과값 : [1, 2, ['Show', 'me', 'the', 'Money!!'], 3.14, True]    
    
## list에 대해서 indexing을 할 수 있다.
    my_list = [1, 2, ["Show", "me", "the", "Money!!"], 3.14, True]
    print(type(my_list[0]))     #결과값 : 1 #타입 : class int
    
    # list에 대해서 slicing을 할 수 있다.
    my_list = [1, 2, ["Show", "me", "the", "Money!!"], 3.14, True]
    print(my_list[0:1])     #list 형식으로 slicing 된다. #앞은 인클루시브 뒤는 익스클루시브 그래서 결과값은 [1] 
    
## 여기서 문제: !! 두개만 출력하고자 한다. 어떻게 하면 될까?
    my_list = [1, 2, ["Show", "me", "the", "Money!!"], 3.14, True]
    
    print(my_list[2])   #결과값 : ["Show", "me", "the", "Money!!"]
    print(my_list[2][3]) #결과값 : Money!!
    print(my_list[2][3][-2:])     #결과값 : !!
    
## Range()
* 영역객체의 "Range()" 함수. list와 같이 많이 사용되는 내장 함수 
###
    my_range = range(10)     # 0부터 9까지 1씩 증가하는 range     #결과값 : [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    my_range = range(1, 10)     # 1부터 10까지 1씩 증가하는 range     #결과값 : [1, 2, 3, 4, 5, 6, 7, 8, 9]
    my_range = range(1, 10, 3)  # 1부터 10까지 3씩 증가하는 range     #결과값 : [1, 4, 7] 
    my_list = list(my_range)
    print(my_list)
    # 결과값 : [1, 4, 7]
    
## 리스트 (List)의 기본 연산
    a = [1, 2, 3]
    b = [4, 5, 6]
    
    result = a + b     # '+'연산은 'list'에서도 연결의 의미(2개의 리스트를 더하기)
    print(result)      # 결과값 : [1, 2, 3, 4, 5, 6]
    
    result = a * 3     # '*'연산 역시 연결의 의미
    print(result)      # 결과값 : [1, 2, 3, 1, 2, 3, 1, 2, 3]
###
    a = [1, 2, 3, 4, 5]
    a[0] = [9, 9, 9]     # [1, 2, 3, 4, 5]란 a 리스트의 0번째인 1을 [9,9,9]로 교체
    print(a)             # 결과값 : [[9, 9, 9], 2, 3, 4, 5]   
###
    a = [1, 2, 3, 4, 5]
    a[0:1] = [9,9,9]     # [1, 2, 3, 4, 5]란 a 리스트의 0번째인 1에 [9,9,9]값을 넣는다.
    print(a)             # 결과값 : [9, 9, 9, 2, 3, 4, 5]
###
    a = [1, 2, 3, 4, 5]
    a[0:-1] = []     # a[0:-1]은 [1, 2, 3, 4]을 [] 공백으로 처리해라
    print(a)         # 결과값 : [5]
###
    a = [1, 2, 3, 4, 5]
    del a[0]         # indexing을 이용해서 삭제
    print(a)         # 결과값 : [2, 3, 4, 5]
###
    a = [1, 2, 3, 4, 5]
    del a[0:-1]         # indexing을 이용해서 삭제
    print(a)         # 결과값 : [5]
    
## 리스트 (List)가 가지고 있는 함수들
* .append()
* .extend()
* .sort()
* .reverse()
* .index()

# .append() 리스트 추가(※ 무조건 마지막 요소에 추가)
    my_list = [1, 2, 3]
    my_list.append(4)         # my_list 값인 [1, 2, 3]에 마지막 요소에 (4)를 추가
    print(my_list)            # 결과값 : [1, 2, 3, 4]

    my_list = [1, 2, 3]
    my_list.append([9, 9, 9]) # my_list 값인 [1, 2, 3]에 마지막 요소에 ([9, 9, 9]) 리스트를 추가
    print(my_list)            # 결과값 : [1, 2, 3, [9, 9, 9]]

# .extend() 리스트 확장
    my_list = [1, 2, 3]
    my_list.extend([9, 9, 9]) # my_list 값인 [1, 2, 3]에 마지막 요소에 ([9, 9, 9]) 리스트 확장 처리(값을 넣는다.)
    print(my_list)            # 결과값 : [1, 2, 3, 9, 9, 9]

# .sort() 정렬(기본은 오름차순 정렬)
    my_list = [5, 8, 2, 9, 4, 7]
    result = my_list.sort()     # 기본은 오름차순 정렬     # 소팅된 결과를 리턴하지 않는다. 그래서 list원본을 정렬해야한다.
    print(result)     #결과값 : None                  
    print(my_list)    #결과값 : [2, 4, 5, 7, 8, 9]

# .reverse() 리스트 역순
    my_list = [5, 8, 2, 9, 4, 7]
    my_list.reverse()     # list안의 요소를 역순으로 다시 배치     #리턴하지 않고 원본을 변경
    print(my_list)    #결과값 : [9, 8, 7, 5, 4, 2]

# .index() 리스트 내 원하는 정보 찾기
    my_list = [5, 8, 2, 9, 4, 7]
    print(my_list.index(9))     #결과값 : 3 -> 0,1,2,3 3번에 9가 있다는 뜻

## 리스트 (List)가 가지고 있는 추가적인 함수들
* .insert()
* .remove()
* .pop())
* id()

# .insert() : 원하는 위치에 원하는 값을 넣을 수 있다.
    a = [1, 2, 3]
    a.insert(0, 100)     #0번째에 100을 넣는다.
    print(a)             #결과값 : [100, 1, 2, 3]

# .remove() : 처음 발견한 값만 삭제
    a = [1, 2, 3, 1, 2, 3, 1, 2, 3]
    a.remove(3)        #list 안에서 삭제하고자 하는 것을 처음 발견한 값만 삭제
    print(a)           #결과값 : [1, 2, 1, 2, 3, 1, 2, 3]

# .pop() : 스택에서 가장 나중에 있는 요소의 값만 뜯어오는 함수. 
    a = [1, 2, 3]
    result = a.pop()    # 끝에있는 3번만 가져온다.
    print(result)        #결과값 : 3
    print(a)            #결과값 : [1, 2]

# id() : reference 값을 알아내는 함수
    a = [1, 2, 3]    # [1, 2, 3] -> list 객체 -> 메모리 어딘가에(Heap 영역) 저장되어 있다.
                     # 이를 사용하기 위해서는 변수가 필요하다.
                     # a는 list객체의 시작 메모리 주소에 대한 reference를 가지고 있다.
                     # a는 reference variable이라고 한다.            
    # id(a)   # 결과값 : 메모리 어느 한 수순의 이상한 숫자가 나온다. 


    b = a
    print(a is b)    # a랑 b랑 같니?라는 의미    #결과값 : True
                     # a와 b가 같은 객체 reference
        
        
    b = a[:] #처음부터 끝까지 슬라이싱
    print(a is b)    # a와 b가 서로 다른 객체이기 때문에 #결과값 : False
    

# copy : 
    from copy import copy #copy라는 외장 모듈에 copy라는 함수를 사용하는 방법
    a = [1, 2, 3]   
    b = copy(a) #a라는 리스트를 b라는 리스트에 복사
    print(a is b)    # a와 b가 서로 다른 객체이기 때문에 #결과값 : False


## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>