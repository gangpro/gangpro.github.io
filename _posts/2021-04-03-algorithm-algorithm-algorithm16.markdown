---
layout: post
title: '[알고리즘] 자연수 뒤집어 배열로 만들기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-03 15:30:17 +0900
lastmod: 2021-04-03 15:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 자연수 뒤집어 배열로 만들기<br />

# 알고리즘 문제 풀이 : 자연수 뒤집어 배열로 만들기

## 문제 
```text
<문제 설명>
자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 
예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.

<제한 조건>
n은 10,000,000,000이하인 자연수입니다.

<입출력 예>
n       return
12345   [5,4,3,2,1]
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12932)



## 나의 풀이
n 정수를 str 문자열로 변경한 후에
한 자리씩 잘라서 숫자로 변환 후 
arr 리스트에 담은 다음
마지막에 역순으로 변경 후 출력

```javascript
function solution(n) {
    var arr = [];
    let str = n.toString();
    
    for(let i = 0; i < str.length; i++) {
        
        arr.push(parseInt(str.substring(i, i+1)));
    }
    
    let answer = arr.reverse();
    
    return answer;
}
```



## 공부하기
- [substring() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/substring)
substring()메소드는 string 객체의 시작 인덱스로 부터 종료 인덱스 전 까지 문자열의 부분 문자열을 반환합니다. 
- [substr() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/substr)
substr() 메서드는 문자열에서 특정 위치에서 시작하여 특정 문자 수 만큼의 문자들을 반환합니다.
- [parseInt() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
parseInt() 함수는 문자열 인자를 구문분석하여 특정 진수(수의 진법 체계에 기준이 되는 값)의 정수를 반환합니다.
- [push() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 
push() 메서드는 배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환합니다.
- [reverse() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
reverse() 메서드는 배열의 순서를 반전합니다. 첫 번째 요소는 마지막 요소가 되며 마지막 요소는 첫 번째 요소가 됩니다.
- [Number() 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number)
Number 객체는 숫자 값으로 작업할 수 있게 해주는 래퍼wrapper 객체입니다. 
Number 객체는 Number() 생성자를 사용하여 만듭니다. 
원시 숫자 자료형은 Number() 함수를 사용해 생성합니다.
- [map() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)