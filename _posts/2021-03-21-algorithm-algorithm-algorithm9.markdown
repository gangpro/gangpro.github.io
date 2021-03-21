---
layout: post
title: '[알고리즘] 핸드폰 번호 가리기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 18:40:17 +0900
lastmod: 2021-03-21 18:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 핸드폰 번호 가리기<br />

# 알고리즘 문제 풀이 : 핸드폰 번호 가리기

## 문제 
```text
<문제 설명>
프로그래머스 모바일은 개인정보 보호를 위해 고지서를 보낼 때 
고객들의 전화번호의 일부를 가립니다.
전화번호가 문자열 phone_number로 주어졌을 때, 
전화번호의 뒷 4자리를 제외한 나머지 숫자를 전부 *으로 가린 
문자열을 리턴하는 함수, solution을 완성해주세요.

<제한 조건>
s는 길이 4 이상, 20이하인 문자열입니다.

<입출력 예>
phone_number    return
"01033334444"   "*******4444"
"027778888"     "*****8888"
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12948](https://programmers.co.kr/learn/courses/30/lessons/12948)


## 나의 풀이
핸드폰 번호 길이에서 마지막 4자리를 제외한 숫자에 "*" 표시를 한 후에
핸드폰 번호 마지막 4자리 자른 후 붙여서 리턴

```javascript
function solution(phone_number) {
    let answer = '';
    let phoneNumber = phone_number;
    let phoneLength = phone_number.length;
    
    answer = "*".repeat(phoneLength - 4) + phoneNumber.slice(-4);
    
    return answer;
}
```



## 공부하기
- [repeat() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)
repeat() 메서드는 문자열을 주어진 횟수만큼 반복해 붙인 새로운 문자열을 반환합니다.
- [slice() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
splice() 메서드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.


## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)