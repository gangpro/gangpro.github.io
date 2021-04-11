---
layout: post
title: '[알고리즘] 정수 제곱근 판별'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-11 18:00:17 +0900
lastmod: 2021-04-11 18:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 정수 제곱근 판별<br />

# 알고리즘 문제 풀이 : 정수 제곱근 판별

## 문제 
```text
<문제 설명>
임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.
n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, 
n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

<제한 사항>
n은 1이상, 50000000000000 이하인 양의 정수입니다.

<입출력 예>
n     return
121   144
3     -1

<입출력 예 설명>
입출력 예#1
121은 양의 정수 11의 제곱이므로, (11+1)를 제곱한 144를 리턴합니다.
입출력 예#2
3은 양의 정수의 제곱이 아니므로, -1을 리턴합니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12934)



## 나의 풀이
```javascript
function solution(n) {
    
    let num = Math.sqrt(n);
    let answer = Number.isInteger(num) ? Math.pow(num + 1, 2) : -1;
    
    return answer;
    
}
```



## 공부하기
- [Math.pow() 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/pow)
Math.pow() 함수는 숫자에 거듭제곱횟수의 값을 반환합니다.
- [Math.sqrt() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt)
Math.sqrt() 함수는 숫자의 제곱근을 반환합니다.
- [Math.floor() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)
Math.floor() 함수는 주어진 숫자와 같거나 작은 정수 중에서 가장 큰 수를 반환합니다.
- [Number.isInteger() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/isInteger)
Number.isInteger() 메서드는 주어진 값이 정수인지 판별합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)