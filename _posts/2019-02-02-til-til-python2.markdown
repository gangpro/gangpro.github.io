---
layout: post
title: '[python] 파이썬 Control Statement에 대해서'
subtitle: 
categories: til
tags: til python
comments: true
date: 2019-02-02 00:12:17 +0900
lastmod: 2019-02-02 12:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

파이썬 Control Statement에 대해서 <br />

# Python Control Statement
> Python Control Statement에 대해서    
<br>
<br>

## 기본 for문 (1)
    my_list = [5, 8, 2, 4, 10]
    
    sum = 0
    for tmp in my_list:     #tmp는 변수
        sum += tmp
        
    print("리스트의 총 합은 : {}".format(sum))
    # 결과값 : 리스트의 총 합은 : 29
    
## 기본 for문 (2)
    my_list = [5, 8, 2, 4, 10]
    
    sum = 0
    for idx in range(len(my_list)):     #이렇게 하면 인덱스가 루프를 돔
        sum += my_list[idx]
        
    print("리스트의 총 합은 : {}".format(sum))
    # 결과값 : 리스트의 총 합은 : 29
    
## 기본 for문 (3)
    my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    
    score1 = [ tmp * 2 for tmp in my_list]
    # my_list에 데이터를 tmp 변수에 한개씩 넣어 맨앞 tmp*2에 넣는다.
    
    print(score1)
    # 결과값 : [2, 4, 6, 8, 10, 12, 14, 16, 18]
    
## 기본 for문 (4)
    my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    
    score1 = [ tmp * 2 for tmp in my_list if tmp % 2 == 0]
    # my_list안에 1이 0=짝수가 아니니 false 사용하지 말아라
    # my_list안에 2가 0=짝수이므로 true 사용해라, tmp * 2 사용
    # 짝수는 true이니 사용
    # my_list에 데이터를 tmp 변수에 한개씩 넣어 맨앞 tmp*2에 넣는다.
    
    print(score1)
    # 결과값 : [4, 8, 12, 16]





## References

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>