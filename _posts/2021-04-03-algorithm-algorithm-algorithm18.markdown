---
layout: post
title: '[알고리즘] 이상한 문자 만들기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-03 19:00:17 +0900
lastmod: 2021-04-03 19:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 이상한 문자 만들기<br />

# 알고리즘 문제 풀이 : 이상한 문자 만들기

## 문제 
```text
<문제 설명>
문자열 s는 한 개 이상의 단어로 구성되어 있습니다. 
각 단어는 하나 이상의 공백문자로 구분되어 있습니다. 
각 단어의 짝수번째 알파벳은 대문자로, 
홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴하는 함수, solution을 완성하세요.

<제한 사항>
문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단해야합니다.
첫 번째 글자는 0번째 인덱스로 보아 짝수번째 알파벳으로 처리해야 합니다.

<입출력 예>
s                   return
"try hello world"   "TrY HeLlO WoRlD"

<입출력 예 설명>
"try hello world"는 세 단어 "try", "hello", "world"로 구성되어 있습니다. 
각 단어의 짝수번째 문자를 대문자로, 홀수번째 문자를 소문자로 바꾸면 
"TrY", "HeLlO", "WoRlD"입니다. 
따라서 "TrY HeLlO WoRlD" 를 리턴합니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12930)



## 나의 풀이
띄어쓰기로 문자를 잘라 배열에 담은 후 
배열 순서대로 홀수는 대문자 짝수는 소문자로 변경 후 
마지막에 " " 띄어쓰기를 넣은 후 루프를 돈다.
모든 루프가 다 돈 후 마지막 띄어쓰기를 제거 후 출력

```javascript
function solution(s) {
    let answer = '';
    let arr = s.split(" ");
    
    for(let i in arr) {
        for(let j in arr[i]) {
            let str = arr[i][j];
            answer += (j % 2 == 0) ? str.toUpperCase() : str.toLowerCase(); 
        }
        answer += " ";
        
    }
    answer = answer.slice(0, answer.length - 1);
    
    return answer;
}
```



## 공부하기
- [split() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split)
split() 메서드는 String 객체를 지정한 구분자를 이용하여 여러 개의 문자열로 나눕니다.
- [map() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.
- [join() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
join() 메서드는 배열의 모든 요소를 연결해 하나의 문자열로 만듭니다.
- [toUpperCase() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)
toUpperCase() 메서드는 문자열을 대문자로 변환해 반환합니다.
- [toLowerCase() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)
toLowerCase() 메서드는 문자열을 소문자로 변환해 반환합니다.


## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)