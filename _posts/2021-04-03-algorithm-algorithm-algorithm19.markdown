---
layout: post
title: '[알고리즘] 자릿수 더하기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-04 02:40:17 +0900
lastmod: 2021-04-04 02:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 자릿수 더하기<br />

# 알고리즘 문제 풀이 : 자릿수 더하기

## 문제 
```text
<문제 설명>
자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 
return 하는 solution 함수를 만들어 주세요.
예를들어 N = 123이면 1 + 2 + 3 = 6을 return 하면 됩니다.

<제한사항>
N의 범위 : 100,000,000 이하의 자연수

<입출력 예>
N     answer
123   6
987   24

<입출력 예 설명>
입출력 예 #1
문제의 예시와 같습니다.
입출력 예 #2
9 + 8 + 7 = 24이므로 24를 return 하면 됩니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12931)



## 나의 풀이
숫자를 문자열로 변경한 후에
한개씩 자른 후 문자열을 다시 숫자로 변환한 후에
answer에 더한 후 출력

```javascript
function solution(n) {
    let answer = 0;
    let str = n.toString();
    let str_split = str.split("");
    
    for(let i in str) {
        answer += parseInt(str_split[i]);
    }

    return answer;
}
```



## 공부하기
- [split() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split)
split() 메서드는 String 객체를 지정한 구분자를 이용하여 여러 개의 문자열로 나눕니다.
- [parseInt() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
parseInt() 함수는 문자열 인자를 구문분석하여 특정 진수(수의 진법 체계에 기준이 되는 값)의 정수를 반환합니다.
- [reduce() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
reduce() 메서드는 배열의 각 요소에 대해 주어진 리듀서(reducer) 함수를 실행하고, 하나의 결과값을 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)