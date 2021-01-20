---
layout: post
title: '[python] 파이썬 데이터 타입 - bool'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 18:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 데이터 타입 - bool<br />


# Python Data Type
1. 숫자형 (number)
2. 문자열 (string)
3. 리스트 (List) : 파이썬에서는 순수 배열이 없다. #자바에서 Arraylist와 유사한 형태가 바로 파이썬 list
4. 튜플 (tuple) : 자바에는 없는 존재
5. 딕셔너리 (dictionary) : key와 value가 쌍으로 구성이 되어 있는것.  #자바에서 해쉬맵과 같은 형태
6. 집합 (Set) : 자바에서 Set과 유사
7. **논리형 (bool)** : True와 False가 있다. true와 false가 아니다. 첫글자는 무조건 대문자!
8. 날짜 (date)     
<br>
<br>

# 7. 논리형 (bool)
* 논리형 (bool)에는 True와 False가 있다.
* python은 다음과 같은 경우에 False로 간주한다.

## False keyword
* 숫자 0은 False, 나머지 숫자는 True로 간주
* 빈 문자열 (" ") -> False로 간주
  - 문자열("N") -> True로 간주.
* 빈 list ( [] ) -> False로 간주
  - list([N]) -> True로 간주.
* 빈 tuple ( () ) -> False로 간주
  - tuple((N)) -> True로 간주.
* 빈 dict ( { } ) -> False로 간주
  - dict({N}) -> True로 간주.
* None -> False로 간주

## if 문의 형식(python는 swich가 없다.)
    area = ["서울", "인천", "부산"] 
       
    if "제주" in area:
        pass     #참일 경우 무조건 code 작성
    elif "부산" in area:
        print("부산은 있어요!")
    else:
        print("부산은 없어요!")
    



## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>