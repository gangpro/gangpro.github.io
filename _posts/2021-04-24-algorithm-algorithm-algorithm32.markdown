---
layout: post
title: '[알고리즘] 가장 큰 수'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-24 17:30:17 +0900
lastmod: 2021-04-24 17:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 가장 큰 수<br />

# 알고리즘 문제 풀이 : 가장 큰 수

## 문제 
```text
<문제 설명>
0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.
예를 들어, 주어진 정수가 [6, 10, 2]라면 
[6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.
0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 
순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 
return 하도록 solution 함수를 작성해주세요.

<제한 사항>
numbers의 길이는 1 이상 100,000 이하입니다.
numbers의 원소는 0 이상 1,000 이하입니다.
정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

<입출력 예>
numbers             return
[6, 10, 2]          "6210"
[3, 30, 34, 5, 9]   "9534330"
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42746)



## 나의 풀이
```javascript
function solution(numbers) {
    
    let answer = numbers.map(c => c + "").sort((a, b) => (b + a) - (a + b)).join("");
    
    return answer[0] === "0" ? "0" : answer;
    
}
```



## 공부하기
- [map() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.
- [sort() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
sort() 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 정렬은 stable sort가 아닐 수 있습니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다. 정렬 속도와 복잡도는 각 구현방식에 따라 다를 수 있습니다.
- [join() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
join() 메서드는 배열의 모든 요소를 연결해 하나의 문자열로 만듭니다.
- [toString() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/toString)
toString() 메서드는 특정한 Number 객체를 나타내는 문자열을 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)