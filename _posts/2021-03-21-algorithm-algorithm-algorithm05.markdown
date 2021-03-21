---
layout: post
title: '[알고리즘] 문자열 내 p와 y의 개수'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-03-21 16:50:17 +0900
lastmod: 2021-03-21 16:50:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 문자열 내 p와 y의 개수<br />

# 알고리즘 문제 풀이 : 문자열 내 p와 y의 개수

## 문제 
```text
<문제 설명>
대문자와 소문자가 섞여있는 문자열 s가 주어집니다. 
s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 
다르면 False를 return 하는 solution를 완성하세요. 
'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다. 
단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.
예를 들어 s가 "pPoooyY"면 true를 return하고 "Pyy"라면 false를 return합니다.

<제한사항>
문자열 s의 길이 : 50 이하의 자연수
문자열 s는 알파벳으로만 이루어져 있습니다.

<입출력 예>
s	        answer
"pPoooyY"	true
"Pyy"	    false

<입출력 예 설명>
입출력 예 #1
'p'의 개수 2개, 'y'의 개수 2개로 같으므로 true를 return 합니다.
입출력 예 #2
'p'의 개수 1개, 'y'의 개수 2개로 다르므로 false를 return 합니다.
```

* 문제 출처 : [https://programmers.co.kr/learn/courses/30/lessons/12916#](https://programmers.co.kr/learn/courses/30/lessons/12916#)


## 나의 풀이
문자열을 대문자로 바꾼 후 해당 문자열 개수를 체크해서 비교하여 
같으면 true 리턴, 다르면 false 리턴

```javascript
  function solution(s){
      let answer = true;
      let input = s.toUpperCase();
      let findP = input.indexOf("P");
      let findY = input.indexOf("Y");
      
      // 문자열 P 개수
      let countP = 0;
      while (findP !== -1) {
          countP++;
          findP = input.indexOf("P", findP + 1);
      }
      console.log(countP);
      
      // 문자열 Y 개수
      let countY = 0;
      while (findY !== -1) {
          countY++;
          findY = input.indexOf("Y", findY + 1);
      }
      console.log(countY);
      
      // if문
      answer = countP === countY ? true : false;

      return answer;
  }
```



## 공부하기
- [toUpperCase() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)
toUpperCase() 메서드는 문자열을 대문자로 변환해 반환합니다.
- [toLowerCase() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)
toLowerCase() 메서드는 문자열을 소문자로 변환해 반환합니다.
- [split() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split)
split() 메서드는 String 객체를 지정한 구분자를 이용하여 여러 개의 문자열로 나눕니다.
- [match() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/match)
match() 메서드는 문자열이 정규식과 매치되는 부분을 검색합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)