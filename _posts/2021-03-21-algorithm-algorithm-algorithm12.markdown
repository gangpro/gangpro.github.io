---
layout: post
title: '[알고리즘] 문자열 다루기 기본'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 19:40:17 +0900
lastmod: 2021-03-21 19:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 문자열 다루기 기본<br />

# 알고리즘 문제 풀이 : 문자열 다루기 기본

## 문제 
```text
<문제 설명>
문자열 s의 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수, 
solution을 완성하세요. 

예를 들어 s가 "a234"이면 False를 리턴하고 
"1234"라면 True를 리턴하면 됩니다.

<제한 사항>
s는 길이 1 이상, 길이 8 이하인 문자열입니다.

<입출력 예>
s        return
"a234"   false
"1234"   true
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12918](https://programmers.co.kr/learn/courses/30/lessons/12918)



## 나의 풀이
문자열을 숫자로 변환하여
변환한 값이 숫자이면서 길이가 4,6 이면 true 리턴 아니면 false 리턴

```javascript
function solution(s) {
    // [참고] 정규식 표현 방법
    // var regex = /^\d{6}$|^\d{4}$/;
    // return regex.test(s);
    
    const number = parseInt(s);
    let result = number == s && (s.length === 4 || s.length === 6) ? true : false;

    return result;
}
```



## 공부하기
- [parseInt() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
parseInt() 함수는 문자열 인자를 구문분석하여 특정 진수(수의 진법 체계에 기준이 되는 값)의 정수를 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)