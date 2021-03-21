---
layout: post
title: '[알고리즘] 가운데 글자 가져오기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 15:50:17 +0900
lastmod: 2021-03-21 15:50:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 가운데 글자 가져오기<br />

# 알고리즘 문제 풀이 : 가운데 글자 가져오기

## 문제 
```text
<문제 설명>
단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요. 
단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.

<제한사항>
s는 길이가 1 이상, 100이하인 스트링입니다.

<입출력 예>
s         return
"abcde"   "c"
"qwer"    "we"
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12903)



## 나의 풀이
문자열 길이를 체크하여 짝수와 홀수에 맞게 글자열 추출

```javascript
function solution(s) {
    let answer = '';
    let len = s.length;
    
    //answer = len % 2 === 0 ? console.log("짝수") : console.log("홀수");
    answer = len % 2 === 0 ? s.substr(len/2-1, 2) : s.substr(len/2, 1);
    
    return answer;
}
```



## 공부하기
- [substr() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/substr)
substr() 메서드는 문자열에서 특정 위치에서 시작하여 특정 문자 수 만큼의 문자들을 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)