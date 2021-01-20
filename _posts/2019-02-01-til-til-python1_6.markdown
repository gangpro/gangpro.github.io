---
layout: post
title: '[python] 파이썬 데이터 타입 - Set'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-01 00:12:17 +0900
lastmod: 2019-02-01 17:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 데이터 타입 - Set<br />


# Python Data Type
1. 숫자형 (number)
2. 문자열 (string)
3. 리스트 (List) : 파이썬에서는 순수 배열이 없다. #자바에서 Arraylist와 유사한 형태가 바로 파이썬 list
4. 튜플 (tuple) : 자바에는 없는 존재
5. 딕셔너리 (dictionary) : key와 value가 쌍으로 구성이 되어 있는것.  #자바에서 해쉬맵과 같은 형태
6. **집합 (Set)** : 자바에서 Set과 유사
7. 논리형 (bool) : True와 False가 있다. true와 false가 아니다. 첫글자는 무조건 대문자!
8. 날짜 (date)     
<br>
<br>

# 6. 집합 (Set)
* {   } : 중괄호를 이용해서 set 표현
* 중복을 배제, 순서가 없는 집합형 자료구조
###
    s = {1, 2, 3}     # set의 literal
    s = set([1, 2, 3, 4, 5])    # 안에 있는 list를 가지고 set을 만들 수도 있다.
    print(s)
    # 결과값 : {1, 2, 3, 4, 5}
    
    s = set("Hello")      
    print(s)
    # 결과값 : {'o', 'l', 'e', 'H'}

## 교집합(&), 합집합(|), 차집합(-))
    s1 = {1, 2, 3, 4, 5, 6}
    s2 = {3, 4, 5, 6, 7, 8, 9}
    
    s3 = s1 & s2     # s1.intersection(s2), 교집합
    print(s5)
    # 결과값 : {3, 4, 5, 6}
    
    s4 = s1 | s2     # s1.union(s2), 합집합
    print(s5)
    # 결과값 : {1, 2, 3, 4, 5, 6, 7, 8, 9}
    
    s5 = s1 - s2     # s1.difference(s2), 차집합
    print(s5)
    # 결과값 : {1, 2}

## .add()
    s = {1, 2, 3}
    s.add(10)     #set에 요소 1개를 추가하고 싶을 때 .add()
    print(s)
    # 결과값 : {10, 1, 2, 3}
    
## .update()
    s = {1, 2, 3}
    s.update([10, 20, 30, 40])     #set에 요소 여러개를 추가하고 싶을 때 .update()
    print(s)
    # 결과값 : {1, 2, 3, 40, 10, 20, 30}
     
## .remove()
    s = {1, 2, 3}
    s.remove(3)     #set에 요소 1개를 삭제
    print(s)
    # 결과값 : {1, 2}



## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>