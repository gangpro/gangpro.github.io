---
layout: post
title: '[알고리즘] 서울에서 김서방 찾기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 18:10:17 +0900
lastmod: 2021-03-21 18:10:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 서울에서 김서방 찾기<br />

# 알고리즘 문제 풀이 : 서울에서 김서방 찾기

## 문제 
```text
<문제 설명>
String형 배열 seoul의 element중 "Kim"의 위치 x를 찾아, 
"김서방은 x에 있다"는 String을 반환하는 함수, solution을 완성하세요. 
seoul에 "Kim"은 오직 한 번만 나타나며 잘못된 값이 입력되는 경우는 없습니다.

<제한 사항>
seoul은 길이 1 이상, 1000 이하인 배열입니다.
seoul의 원소는 길이 1 이상, 20 이하인 문자열입니다.
"Kim"은 반드시 seoul 안에 포함되어 있습니다.

<입출력 예>
seoul             return
["Jane", "Kim"]   "김서방은 1에 있다"
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12919](https://programmers.co.kr/learn/courses/30/lessons/12919)


## 나의 풀이
Kim의 위치를 찾아 answer에 대입 후 출력

```javascript
    function solution(seoul) {
        let answer = '';
        let idx = seoul.indexOf("Kim");
        
        answer = `김서방은 ${idx}에 있다`
        
        return answer;
    }
```



## 공부하기
- [indexOf() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)
indexOf() 메서드는 호출한 String 객체에서 주어진 값과 일치하는 첫 번째 인덱스를 반환합니다. 일치하는 값이 없으면 -1을 반환합니다. 



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)