---
layout: post
title: '[알고리즘] 문자열 내림차순으로 배치하기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 19:30:17 +0900
lastmod: 2021-03-21 19:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 문자열 내림차순으로 배치하기<br />

# 알고리즘 문제 풀이 : 문자열 내림차순으로 배치하기

## 문제 
```text
<문제 설명>
문자열 s에 나타나는 문자를 큰것부터 작은 순으로 정렬해 
새로운 문자열을 리턴하는 함수, solution을 완성해주세요.
s는 영문 대소문자로만 구성되어 있으며, 
대문자는 소문자보다 작은 것으로 간주합니다.

<제한 사항>
str은 길이 1 이상인 문자열입니다.

<입출력 예>
s           return
"Zbcdefg"	  "gfedcbZ"
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12917](https://programmers.co.kr/learn/courses/30/lessons/12917)



## 나의 풀이
문자열을 쪼갠 후, 정렬하고, 반전시킨 후 배열 합쳐서 리턴

```javascript
function solution(s) {
    var answer = s.split("").sort().reverse().join("");
    return answer;
}
```



## 공부하기
- [split() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split)
split() 메서드는 String 객체를 지정한 구분자를 이용하여 여러 개의 문자열로 나눕니다.
- [sort() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
sort() 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 
- [reverse() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
reverse() 메서드는 배열의 순서를 반전합니다. 첫 번째 요소는 마지막 요소가 되며 마지막 요소는 첫 번째 요소가 됩니다.
- [join() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
join() 메서드는 배열의 모든 요소를 연결해 하나의 문자열로 만듭니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)