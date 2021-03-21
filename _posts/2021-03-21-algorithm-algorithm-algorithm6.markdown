---
layout: post
title: '[알고리즘] 두 정수 사이의 합'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 18:00:17 +0900
lastmod: 2021-03-21 18:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 두 정수 사이의 합<br />

# 알고리즘 문제 풀이 : 두 정수 사이의 합

## 문제 
```text
<문제 설명>
두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 
모든 정수의 합을 리턴하는 함수, solution을 완성하세요. 
예를 들어 a = 3, b = 5인 경우, 
3 + 4 + 5 = 12이므로 12를 리턴합니다.

<제한 조건>
a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.
a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.
a와 b의 대소관계는 정해져있지 않습니다.

<입출력 예>
a	b	return
3	5	12
3	3	3
5	3	12
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12912](https://programmers.co.kr/learn/courses/30/lessons/12912)



## 나의 풀이
a와 b의 크기에 따라 분기 후 해당 길이만큼 더함

```javascript
function solution(a, b) {
    let answer = 0;
    
    if(a <= b) {
        for(let i = a; i <= b; i++) {
            answer = answer + i;
        }
    } else {
        for(let i = b; i <= a; i++) {
            answer = answer + i;
        }
    }
    
    return answer;
}
```



## 공부하기
- [Math.abs() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/abs)
Math.abs() 함수는 주어진 숫자의 절대값을 반환합니다



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)