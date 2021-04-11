---
layout: post
title: '[알고리즘] x만큼 간격이 있는 n개의 숫자'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-11 16:00:17 +0900
lastmod: 2021-04-11 16:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : x만큼 간격이 있는 n개의 숫자<br />

# 알고리즘 문제 풀이 : x만큼 간격이 있는 n개의 숫자

## 문제 
```text
<문제 설명>
함수 solution은 정수 x와 자연수 n을 입력 받아, 
x부터 시작해 x씩 증가하는 숫자를 n개 지니는 리스트를 리턴해야 합니다. 
다음 제한 조건을 보고, 조건을 만족하는 함수, solution을 완성해주세요.

<제한 조건>
x는 -10000000 이상, 10000000 이하인 정수입니다.
n은 1000 이하인 자연수입니다.

<입출력 예>
x    n   answer
2    5   [2,4,6,8,10]
4    3   [4,8,12]
-4   2   [-4, -8]
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12954)



## 나의 풀이
```javascript
function solution(x, n) {
    let answer = [];
    
    for(let i = 1; i <= n; i++) {
        answer.push(x * i);
    }
    return answer;
    
}
```



## 공부하기
- [push() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 
push() 메서드는 배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환합니다.
- [Array 객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array)
JavaScript Array 전역 객체는 배열을 생성할 때 사용하는 리스트 형태의 고수준 객체입니다.
- [fill() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)
fill() 메서드는 배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채웁니다.
- [map() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.
- [keys() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/keys)
keys() 메서드는 배열의 각 인덱스를 키 값으로 가지는 새로운 Array Iterator 객체를 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)