---
layout: post
title: '[python] 파이썬 데이터 타입 - Dictionary'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 16:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 데이터 타입 - Dictionary<br />


# Python Data Type
1. 숫자형 (number)
2. 문자열 (string)
3. 리스트 (List) : 파이썬에서는 순수 배열이 없다. #자바에서 Arraylist와 유사한 형태가 바로 파이썬 list
4. 튜플 (tuple) : 자바에는 없는 존재
5. **딕셔너리 (dictionary)** : key와 value가 쌍으로 구성이 되어 있는것.  #자바에서 해쉬맵과 같은 형태
6. 집합 (Set) : 자바에서 Set과 유사
7. 논리형 (bool) : True와 False가 있다. true와 false가 아니다. 첫글자는 무조건 대문자!
8. 날짜 (date)



# 5. 딕셔너리 (dictionary)
* python에서 사용되는 Map  # hashmap #key value key value
* python에서는 dictinary라고 표현
* dictionary는 {} 중괄호로 표현하고, key와 value의 쌍으로 표현
  - ※ 리스트 - [], 딕셔너리 - {}, 튜플 - ()
* literal의 형태가 'JSON'처럼 생김.
```
    my_dict = {"name" : "홍길동", "address" : "서울"}
    print(type(my_dict))
    #결과값 : <class 'dict'>
```

## key 값을 추가 할 수 있다.
* 연산 배열 행태로 사용하면 된다.
```
    my_dict["age"] = 50     # 배열상태로 사용 가능, 동적으로 key와 value를 추가, 수정, 삭제 할 수 있다.
    print(my_dict)
    # 결과값 : {'name': '홍길동', 'address': '서울', 'age': 50}
    
    del my_dict["address"]   # "address" key값을 삭제 할 수 있다.
    print(my_dict)
    # 결과값 : {'name': '홍길동', 'age': 50}
    
    my_dict["name"]
    # key가 중복 될 수는 없다. 하나는 무시. 
    my_dict = {"name" : "홍길동", "age" : "20", "age" : "40"}
    print(my_dict)
    # 결과값 : {'name': '홍길동', 'age': '40'}
```
    
## 딕셔너리 (dictionary)의 일반적인 사용법
* 3가지 method를 이용해서 사용한다.
  - .keys()
  - .values()
  - .items())

### .keys()
* dictionary의 key들을 dict_keys라는 type으로 return 받는다. 
* list와 상당히 유사하지만, list 함수는 아니다.
```
    # 형태 my_dict = {"key1" : "value1", "key2" : "value2", "key3" : "value3"}
    my_dict = {"name" : "홍길동", "age" : "20", "address" : "서울"}
    my_dict.keys()  # <-list로 간주 할 수 있다.

    * python에서 block은 들여쓰기(indentation)로 표시
    * 들여쓰기 스페이스 4칸으로 표현
    #for 사용할 변수 in 집합 자료(리스트, 튜플 같은거):
```
    
### .keys()라는 함수는 dictionary에서 key만 추출
```
    for tmp in my_dict.keys():
        print("{}".format(tmp))
        # 결과값 : name
                  age
                  address
```
        
### .values()라는 함수는 dictionary에서 value만 추출
```
    for tmp in my_dict.values():
        print("{}".format(tmp))
        # 결과값 : 홍길동
                  20
                  서울
```
        
### .items()라는 함수는 dictionarydptj (key, value) 추출
```
    for (key, value) in my_dict.items():
        print("{}, {}".format(key, value))
        # 결과값 : name, 홍길동
                  age, 20
                  address, 서울    
```
        
## dictionary 값 추출  
* dictionary에서 값을 추출할 때는 2가지 방식(my_dict["추출값"]     .get())
```
    my_dict = {"name" : "홍길동", "age" : "20", "address" : "서울"}
    print(my_dict["name"])       #결과값 : 홍길동     #
    print(my_dict.get("name"))   #결과값 : 홍길동     #함수를 이용해서 사용
    print(my_dict.get("name11", "이름 없음"))   #결과값 : 이름 없음     #바로 위 함수를 활용해 defaul값을 사용할 수 있음
    print("age" in my_dict)     #결과값: True     #my_dict의 key 중에 age가 있니? 있으면 True 없으면 False
```
    
## .clear()
```
    my_dict.clear()     #모두 삭제
```


## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>