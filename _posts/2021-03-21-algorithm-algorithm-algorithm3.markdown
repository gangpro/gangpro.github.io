---
layout: post
title: '[알고리즘] 문자열을 정수로 바꾸기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 15:30:17 +0900
lastmod: 2021-03-21 15:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 문자열을 정수로 바꾸기<br />

# 알고리즘 문제 풀이 : 문자열을 정수로 바꾸기

## 문제 
```text
<문제 설명>
문자열 s를 숫자로 변환한 결과를 반환하는 함수, solution을 완성하세요.

<제한 조건>
s의 길이는 1 이상 5이하입니다.
s의 맨앞에는 부호(+, -)가 올 수 있습니다.
s는 부호와 숫자로만 이루어져있습니다.
s는 "0"으로 시작하지 않습니다.

<입출력 예>
예를들어 str이 "1234"이면 1234를 반환하고, "-1234"이면 -1234를 반환하면 됩니다.
str은 부호(+,-)와 숫자로만 구성되어 있고, 잘못된 값이 입력되는 경우는 없습니다.
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12925](https://programmers.co.kr/learn/courses/30/lessons/12925)



## 나의 풀이
문자열 s를 숫자 타입으로 변환하여 출력

```javascript
function solution(s) {
    return Number(s);
}
```



## 공부하기
- [Number() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number)
Number 객체는 숫자 값으로 작업할 수 있게 해주는 래퍼wrapper 객체입니다. 
Number 객체는 Number() 생성자를 사용하여 만듭니다. 
원시 숫자 자료형은 Number() 함수를 사용해 생성합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)