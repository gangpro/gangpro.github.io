---
layout: post
title: '[알고리즘] 정수 내림차순으로 배치하기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 18:20:17 +0900
lastmod: 2021-03-21 18:20:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 정수 내림차순으로 배치하기<br />

# 알고리즘 문제 풀이 : 정수 내림차순으로 배치하기

## 문제 
```text
<문제 설명>
함수 solution은 정수 n을 매개변수로 입력받습니다. 
n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요. 
예를들어 n이 118372면 873211을 리턴하면 됩니다.

<제한 조건>
n은 1이상 8000000000 이하인 자연수입니다.

<입출력 예>
n       return
118372  873211
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12933#](https://programmers.co.kr/learn/courses/30/lessons/12933#)


## 나의 풀이
숫자를 문자열로 변경 후 모든 문자를 자른 후 정렬한 다음 배열을 반전 시킨 후
배열 내 문자를 합친 후 문자를 숫자로 변환

```javascript
function solution(n) {
    let arr = n.toString().split("").sort().reverse();
    let answer = parseInt(arr.join(""));
    return answer;
}
```



## 공부하기
- [toString() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
toString() 은 문자열을 반환하는 object의 대표적인 방법이다.
- [split() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split)
split() 메서드는 String 객체를 지정한 구분자를 이용하여 여러 개의 문자열로 나눕니다.
- [sort() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
sort() 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 
- [reverse() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
reverse() 메서드는 배열의 순서를 반전합니다. 첫 번째 요소는 마지막 요소가 되며 마지막 요소는 첫 번째 요소가 됩니다.
- [parseInt() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
parseInt() 함수는 문자열 인자를 구문분석하여 특정 진수(수의 진법 체계에 기준이 되는 값)의 정수를 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)