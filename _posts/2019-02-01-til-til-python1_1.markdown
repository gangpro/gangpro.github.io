---
layout: post
title: '[python] 파이썬 데이터 타입 - Number'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 데이터 타입 - Number<br />


# Python Data Type
1. **숫자형 (number)**
2. 문자열 (string)
3. 리스트 (List) : 파이썬에서는 순수 배열이 없다. #자바에서 Arraylist와 유사한 형태가 바로 파이썬 list
4. 튜플 (tuple) : 자바에는 없는 존재
5. 딕셔너리 (dictionary) : key와 value가 쌍으로 구성이 되어 있는것.  #자바에서 해쉬맵과 같은 형태
6. 집합 (Set) : 자바에서 Set과 유사
7. 논리형 (bool) : True와 False가 있다. true와 false가 아니다. 첫글자는 무조건 대문자!
8. 날짜 (date)     
<br>
<br>

# 1. 숫자형 (number)
* 자바에서 int, double, float 등이 있지만, 파이썬에서는 없다. 그냥 쓰면 된다.
* 정수, 실수, 복소수, 8진수, 16진수 사용 가능.
```
    a = 100                 # 정수
    b = 3.14159265352898    # 실수
    b2 = 3.1415926535E3     # 지수형태로도 사용 가능    #E3 = 10의 3제곱을 표현
    c = 1 + 2j              # 복소수
    d = 0o34                # 8진수
    e = 0xAA                # 16진수

    div = 3 / 4             # 정수 나누기 정수 = 정수가 나와야 하지만,
    print(div)              # 정수 나누기 정수라 해도 파이썬에서는 소수점까지 다 나온다.
    #파이썬 2.x 버전에서는 결과가 0이 나오고
    #파이썬 3.x 버전에서는 결과가 0.75가 나온다.
```

## 제곱표현(지수승 표현)
```
    result = 2 ** 5     # 2의 5제곱(2의 5승)
    print(result)
    # 결과값 : 32
```

## 몫만 구하는 연산
```
    result = 10 // 3
    print(result) 
    # 결과값 : 3
```
    
## 나머지 구하는 연산
```
    result = 10 % 3
    print(result)
    # 결과값 : 1
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>