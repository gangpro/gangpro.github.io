---
layout: post
title: '[알고리즘] 제일 작은 수 제거하기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 19:20:17 +0900
lastmod: 2021-03-21 19:20:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 제일 작은 수 제거하기<br />

# 알고리즘 문제 풀이 : 제일 작은 수 제거하기

## 문제 
```text
<문제 설명>
정수를 저장한 배열, arr 에서 가장 작은 수를 제거한 배열을 리턴하는 함수, 
solution을 완성해주세요. 
단, 리턴하려는 배열이 빈 배열인 경우엔 배열에 -1을 채워 리턴하세요. 
예를들어 arr이 [4,3,2,1]인 경우는 [4,3,2]를 리턴 하고, 
[10]면 [-1]을 리턴 합니다.

<제한 조건>
arr은 길이 1 이상인 배열입니다.
인덱스 i, j에 대해 i ≠ j이면 arr[i] ≠ arr[j] 입니다.

<입출력 예>
arr         return
[4,3,2,1]   [4,3,2]
[10]        [-1]
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12935#](https://programmers.co.kr/learn/courses/30/lessons/12935#)



## 나의 풀이
arr 배열에서 최소값을 구해 위치를 찾고
그 위치를 제거한 arr을 리턴

```javascript
function solution(arr) {
    let answer = [];
    let result = [];
    
    // NO
    //result = arr.sort().reverse().slice(0, arr.length - 1);
    //answer = result.length === 0 ? [-1] :  result;
    
    // arr 배열에서 최소값을 구한 다음
    // 배열 위치를 찾아 제거한다.
    
    if(arr.length <= 1) return [-1];
    arr.splice(arr.indexOf(Math.min(...arr)), 1);
    
    return arr;
}
```



## 공부하기
- [Math.min() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/min)
Math.min() 함수는 주어진 숫자들 중 가장 작은 값을 반환합니다.
- [Spread syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
전개 구문을 사용하면 배열이나 문자열과 같이 반복 가능한 문자를 0개 이상의 인수 (함수로 호출할 경우) 또는 요소 (배열 리터럴의 경우)로 확장하여, 0개 이상의 키-값의 쌍으로 객체로 확장시킬 수 있습니다.
- [Rest parameters](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/rest_parameters)
Rest 파라미터 구문은 정해지지 않은 수(an indefinite number, 부정수) 인수를 배열로 나타낼 수 있게 합니다.
- [indexOf() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)
indexOf() 메서드는 호출한 String 객체에서 주어진 값과 일치하는 첫 번째 인덱스를 반환합니다. 일치하는 값이 없으면 -1을 반환합니다. 
- [slice() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
splice() 메서드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)