---
layout: post
title: '[알고리즘] 평균 구하기'
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

알고리즘 문제 풀이 : 평균 구하기<br />

# 알고리즘 문제 풀이 : 평균 구하기

## 문제 
```text
<문제 설명>
정수를 담고 있는 배열 arr의 평균값을 return하는 함수, 
solution을 완성해보세요.

<제한사항>
arr은 길이 1 이상, 100 이하인 배열입니다.
arr의 원소는 -10,000 이상 10,000 이하인 정수입니다.

<입출력 예>
arr	        return
[1,2,3,4]	  2.5
[5,5]       5
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12944)



## 나의 풀이
num 리스트를 개수만큼 total에 더한 다음
total을 num 리스트 개수로 나누어 출력

```javascript
function solution(num) {
    var total = 0;
    
    for(let i in num) {
        total += num[i]
    }
    
    return total / num.length;
}
```



## 공부하기
- [향상된 for문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Loops_and_iteration#for...in_문)
for...in  문은 객체의 열거 속성을 통해 지정된 변수를 반복합니다. 각각의 고유한 속성에 대해, JavaScript는 지정된 문을 실행합니다.
- [reduce() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
reduce() 메서드는 배열의 각 요소에 대해 주어진 리듀서(reducer) 함수를 실행하고, 하나의 결과값을 반환합니다.


## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)