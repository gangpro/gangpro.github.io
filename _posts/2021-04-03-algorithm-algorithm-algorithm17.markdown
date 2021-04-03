---
layout: post
title: '[알고리즘] 소수 찾기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-03 18:30:17 +0900
lastmod: 2021-04-03 18:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 소수 찾기<br />

# 알고리즘 문제 풀이 : 소수 찾기

## 문제 
```text
<문제 설명>
1부터 입력받은 숫자 n 사이에 있는 소수의 개수를 반환하는 함수, solution을 만들어 보세요.
소수는 1과 자기 자신으로만 나누어지는 수를 의미합니다.
(1은 소수가 아닙니다.)

<제한 조건>
n은 2이상 1000000이하의 자연수입니다.

<입출력 예>
n    result
10   4
5    3

<입출력 예 설명>
입출력 예 #1
1부터 10 사이의 소수는 [2,3,5,7] 4개가 존재하므로 4를 반환
입출력 예 #2
1부터 5 사이의 소수는 [2,3,5] 3개가 존재하므로 3를 반환
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12921)



## 나의 풀이
수학의 에라토스테네스의 체를 이용하여 소수 찾기

```javascript
function solution(n) {
    let answer = 0;
    let arr = [];
    
    // 1은 소수가 아니므로 
    // 2부터 소수를 구하고자하는 모든 수 나열
    for(let i = 2; i <= n; i++) {
        arr[i] = i;
    }
    
    // 
    for(let i = 2; i <= n; i++) {
        // 
        if(arr[i] == 0) {
            continue;
        }
        // 
        for(let j = i + i; j <= n; j += i) {
            arr[j] = 0;
        }
    }
    
    // 
    for(let i = 2; i <= n; i++) {
        // 소수 개수 카운
        if(arr[i] != 0) {
            answer++;
        }
    }
    return answer;
}
```



## 공부하기
- [에라토스테네스의 체](https://ko.wikipedia.org/wiki/에라토스테네스의_체)
수학에서 에라토스테네스의 체는 소수를 찾는 방법이다. 고대 그리스 수학자 에라토스테네스가 발견하였다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)