---
layout: post
title: '[알고리즘] 짝수와 홀수'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-31 22:40:17 +0900
lastmod: 2021-03-31 22:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 짝수와 홀수<br />

# 알고리즘 문제 풀이 : 짝수와 홀수

## 문제 
```text
<문제 설명>
정수 num이 짝수일 경우 "Even"을 반환하고 
홀수인 경우 "Odd"를 반환하는 함수, solution을 완성해주세요.

<제한 조건>
num은 int 범위의 정수입니다.
0은 짝수입니다.

<입출력 예>
num	  return
3     "Odd"
4     "Even
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12937)



## 나의 풀이
num을 2로 나눈 나머지가 0이면 true(짝수), 아니면 false(홀수)로 출력

```javascript
function solution(num) {
    return num % 2 == 0 ? "Even" : "Odd";
}
```



## 공부하기
- [삼항 조건 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)
조건부 삼항 연산자는 JavaScript에서 세 개의 피연산자를 취할 수 있는 유일한 연산자입니다. 보통 if 명령문의 단축 형태로 쓰입니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)