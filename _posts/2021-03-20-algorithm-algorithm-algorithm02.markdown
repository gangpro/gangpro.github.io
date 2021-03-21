---
layout: post
title: '[알고리즘] 수박수박수박수박수박수?'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-20 20:20:17 +0900
lastmod: 2021-03-20 20:20:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 수박수박수박수박수박수?<br />

# 알고리즘 문제 풀이 : 수박수박수박수박수박수?

## 문제 
```text
<문제 설명>

길이가 n이고, "수박수박수박수...."와 같은 패턴을 유지하는 문자열을 리턴하는 함수, 
solution을 완성하세요. 예를들어 n이 4이면 "수박수박"을 리턴하고 3이라면 
"수박수"를 리턴하면 됩니다.

<제한 조건>
n은 길이 10,000이하인 자연수입니다.
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12922#](https://programmers.co.kr/learn/courses/30/lessons/12922#)


## 나의 풀이
answer에 n의 길이에 따라 짝수면 수, 홀수면 박을 더한 후 return

```javascript
  function solution(n) {
      var answer = '';
      
      for(let i = 0; i < n; i++) {
          answer += i % 2 === 0 ? '수' : '박';
      }
      
      return answer;
  }
```



## 공부하기
- 문자열 메소드 다루는 법
- [repeat() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)
repeat() 메서드는 문자열을 주어진 횟수만큼 반복해 붙인 새로운 문자열을 반환합니다.


## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)