---
layout: post
title: '[알고리즘] 문자열 내 마음대로 정렬하기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-11 00:30:17 +0900
lastmod: 2021-04-11 00:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 문자열 내 마음대로 정렬하기<br />

# 알고리즘 문제 풀이 : 문자열 내 마음대로 정렬하기

## 문제 
```text
<문제 설명>
문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 
각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다. 
예를 들어 strings가 ["sun", "bed", "car"]이고 n이 1이면 
각 단어의 인덱스 1의 문자 "u", "e", "a"로 strings를 정렬합니다.

<제한 조건>
strings는 길이 1 이상, 50이하인 배열입니다.
strings의 원소는 소문자 알파벳으로 이루어져 있습니다.
strings의 원소는 길이 1 이상, 100이하인 문자열입니다.
모든 strings의 원소의 길이는 n보다 큽니다.
인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

<입출력 예>
strings                   n   return
["sun", "bed", "car"]     1   ["car", "bed", "sun"]
["abce", "abcd", "cdx"]   2   ["abcd", "abce", "cdx"]

<입출력 예 설명>
입출력 예 1
"sun", "bed", "car"의 1번째 인덱스 값은 각각 "u", "e", "a" 입니다. 
이를 기준으로 strings를 정렬하면 ["car", "bed", "sun"] 입니다.
입출력 예 2
"abce"와 "abcd", "cdx"의 2번째 인덱스 값은 "c", "c", "x"입니다. 
따라서 정렬 후에는 "cdx"가 가장 뒤에 위치합니다. 
"abce"와 "abcd"는 사전순으로 정렬하면 "abcd"가 우선하므로, 답은 ["abcd", "abce", "cdx"] 입니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12915)



## 나의 풀이
```javascript
function solution(strings, n) {
    let answer = [];
    
    strings.sort(function(a, b) {
        
        let first = a[n];
        let second = b[n];
        
        return first == second ? (a > b) - (a < b) : (first > second) - (first < second);
        
    });
    
    return strings;
}
```



## 공부하기
- [sort() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
sort() 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 정렬은 stable sort가 아닐 수 있습니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다. 정렬 속도와 복잡도는 각 구현방식에 따라 다를 수 있습니다.
- [localeCompare() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)
The localeCompare() 메서드는 기준 문자열과 비교했을 때 비교 대상 문자열이 정렬상 전에 오는지, 후에 오는지 혹은 같은 순서에 배치되는지를 알려주는 숫자를 리턴합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)