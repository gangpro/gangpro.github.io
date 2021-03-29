---
layout: post
title: '[알고리즘] 나누어 떨어지는 숫자 배열'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-29 23:40:17 +0900
lastmod: 2021-03-29 23:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 나누어 떨어지는 숫자 배열<br />

# 알고리즘 문제 풀이 : 나누어 떨어지는 숫자 배열

## 문제 
```text
<문제 설명>
array의 각 element 중 divisor로 나누어 떨어지는 값을 
오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요.
divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.

<제한사항>
arr은 자연수를 담은 배열입니다.
정수 i, j에 대해 i ≠ j 이면 arr[i] ≠ arr[j] 입니다.
divisor는 자연수입니다.
array는 길이 1 이상인 배열입니다.

<입출력 예>
arr	            divisor	  return
[5, 9, 7, 10]	  5	        [5, 10]
[2, 36, 1, 3]	  1	        [1, 2, 3, 36]
[3,2,6]	        10        [-1]

<입출력 예 설명>
입출력 예#1
arr의 원소 중 5로 나누어 떨어지는 원소는 5와 10입니다. 따라서 [5, 10]을 리턴합니다.
입출력 예#2
arr의 모든 원소는 1으로 나누어 떨어집니다. 원소를 오름차순으로 정렬해 [1, 2, 3, 36]을 리턴합니다.
입출력 예#3
3, 2, 6은 10으로 나누어 떨어지지 않습니다. 나누어 떨어지는 원소가 없으므로 [-1]을 리턴합니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12910)



## 나의 풀이
정말 단순히 배열 n값을 divisor로 나누어 0이면 answer 리스트에 넣고
그런 다음 오름차순으로 정렬한 다음 answer 리스트가 아무 값도 없으면
-1을 넣어서 출력

```javascript
function solution(arr, divisor) {
    var answer = [];
    
    for(var i = 0; i < arr.length; i++) {

        if(arr[i] % divisor == 0 ) {
            answer.push(arr[i]);
        } 
        /*
        if(Number.isInteger(arr[i] / divisor) == true) {
            answer.push(arr[i]);
        }
        */
        
    }
    
    answer.sort(function(a, b) {
        return a - b;  
    })

    if(answer.length == 0) {
        answer.push(-1);
    }
    
    return answer;
}
```



## 공부하기
- [isInteger() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/isInteger)
Number.isInteger() 메서드는 주어진 값이 정수인지 판별합니다.
- [filter() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
filter() 메서드는 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환합니다.
- [map() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)