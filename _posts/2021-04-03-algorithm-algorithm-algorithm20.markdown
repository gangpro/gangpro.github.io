---
layout: post
title: '[알고리즘] 시저 암호'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-04 03:10:17 +0900
lastmod: 2021-04-04 03:10:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 시저 암호<br />

# 알고리즘 문제 풀이 : 시저 암호

## 문제 
```text
<문제 설명>
어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 
다른 알파벳으로 바꾸는 암호화 방식을 시저 암호라고 합니다. 

예를 들어 "AB"는 1만큼 밀면 "BC"가 되고, 3만큼 밀면 "DE"가 됩니다. 
"z"는 1만큼 밀면 "a"가 됩니다. 

문자열 s와 거리 n을 입력받아 s를 n만큼 민 암호문을 만드는 함수, 
solution을 완성해 보세요.

<제한 조건>
공백은 아무리 밀어도 공백입니다.
s는 알파벳 소문자, 대문자, 공백으로만 이루어져 있습니다.
s의 길이는 8000이하입니다.
n은 1 이상, 25이하인 자연수입니다.

<입출력 예>
s         n   result
"AB"      1   "BC"
"z"       1   "a"
"a B z"   4   "e F d"
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12926)



## 나의 풀이
다시...

```javascript
function solution(str, n) {
    
    let answer= "";
    let upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    let lower = "abcdefghijklmnopqrstuvwxyz";
    
    for(let i in str) {
        
        var text = str[i];
        
        if(text == " ") {
            answer += " "; 
            continue;
        }
        
        var textArr = upper.includes(text) ? upper : lower;
        var index = textArr.indexOf(text) + n;
        
        if(index >= textArr.length) index -= textArr.length;
        
        answer += textArr[index];
        
    }
    
    return answer;
}
```



## 공부하기
- [includes() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)
includes() 메서드는 배열이 특정 요소를 포함하고 있는지 판별합니다.
- [indexOf() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)
indexOf() 메서드는 호출한 String 객체에서 주어진 값과 일치하는 첫 번째 인덱스를 반환합니다. 일치하는 값이 없으면 -1을 반환합니다. 
- [charCodeAt() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)
charCodeAt() 메서드는 주어진 인덱스에 대한 UTF-16 코드를 나타내는 0부터 65535 사이의 정수를 반환합니다.
- [String.fromCharCode() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode)
String.fromCharCode() 메서드는 UTF-16 코드 유닛의 시퀀스로부터 문자열을 생성해 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)