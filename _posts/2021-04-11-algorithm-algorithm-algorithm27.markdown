---
layout: post
title: '[알고리즘] 약수의 합'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-11 16:20:17 +0900
lastmod: 2021-04-11 16:20:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 약수의 합<br />

# 알고리즘 문제 풀이 : 약수의 합

## 문제 
```text
<문제 설명>
정수 n을 입력받아 n의 약수를 모두 더한 값을 리턴하는 함수, solution을 완성해주세요.

<제한 사항>
n은 0 이상 3000이하인 정수입니다.

<입출력 예>
n    return
12   28
5    6

<입출력 예 설명>
입출력 예 #1
12의 약수는 1, 2, 3, 4, 6, 12입니다. 이를 모두 더하면 28입니다.
입출력 예 #2
5의 약수는 1, 5입니다. 이를 모두 더하면 6입니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12928)



## 나의 풀이
```javascript
function solution(n) {
    var answer = 0;
    
    for(let i = 1; i <= n; i++) {
        
        if(n % i == 0) {
            answer += i;
        }
        
    }
    
    return answer;
}
```



## 공부하기



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)