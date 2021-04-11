---
layout: post
title: '[알고리즘] 최대공약수와 최소공배수'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-11 17:40:17 +0900
lastmod: 2021-04-11 17:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 최대공약수와 최소공배수<br />

# 알고리즘 문제 풀이 : 최대공약수와 최소공배수

## 문제 
```text
<문제 설명>
두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환하는 함수, solution을 완성해 보세요. 
배열의 맨 앞에 최대공약수, 그다음 최소공배수를 넣어 반환하면 됩니다. 
예를 들어 두 수 3, 12의 최대공약수는 3, 최소공배수는 12이므로 
solution(3, 12)는 [3, 12]를 반환해야 합니다.

<제한 사항>
두 수는 1이상 1000000이하의 자연수입니다.

<입출력 예>
n   m    return
3   12   [3, 12]
2   5    [1, 10]

<입출력 예 설명>
입출력 예 #1
위의 설명과 같습니다.
입출력 예 #2
자연수 2와 5의 최대공약수는 1, 최소공배수는 10이므로 [1, 10]을 리턴해야 합니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12940)



## 나의 풀이
```javascript
function solution(a, b) {
    
    let min = Math.min(a, b);
    let max = Math.max(a, b);
    let answer = [gcd(min, max), lcm(min, max)];
    
    return answer;
    
}

function gcd(min, max) {
    return (min % max) == 0 ? max : gcd(max, min % max);
}

function lcm(min, max) {
    return min * max / gcd(min, max);
}
```



## 공부하기
- [유클리드 호제법](https://ko.wikipedia.org/wiki/유클리드_호제법)
유클리드 호제법(Euclidean algorithm) 또는 유클리드 알고리즘은 2개의 자연수 또는 정식(整式)의 최대공약수를 구하는 알고리즘의 하나이다.
- [Math.min() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/min)
Math.min() 함수는 주어진 숫자들 중 가장 작은 값을 반환합니다.
- [Math.max() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/max)
Math.max() 함수는 입력값으로 받은 0개 이상의 숫자 중 가장 큰 숫자를 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)