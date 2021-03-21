---
layout: post
title: '[알고리즘] 같은 숫자는 싫어'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-20 20:00:17 +0900
lastmod: 2021-03-20 20:10:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 같은 숫자는 싫어<br />

# 알고리즘 문제 풀이 : 같은 숫자는 싫어

## 문제 
```text
<문제 설명>

배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 
이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 
단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 
예를 들면,
arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.
arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.
배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 
return 하는 solution 함수를 완성해 주세요.

<제한사항>
배열 arr의 크기 : 1,000,000 이하의 자연수
배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12906](https://programmers.co.kr/learn/courses/30/lessons/12906)


## 나의 풀이
들어오는 배열 n 번째와 n+1 번째를 비교하여 다르면 answer 배열에 넣어 출력

```javascript
  function solution(arr) {
      var answer = [];
      
      for(let i = 0; i < arr.length; i++) {
          if(arr[i] !== arr[i+1]) {
            answer.push(arr[i]);
          }
      }
      return answer;
  }
```



## 공부하기
- [push() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 
push() 메서드는 배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환합니다.
- [filter() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
filter() 메서드는 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)