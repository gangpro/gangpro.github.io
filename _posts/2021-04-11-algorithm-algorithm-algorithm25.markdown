---
layout: post
title: '[알고리즘] 2016년'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-11 15:40:17 +0900
lastmod: 2021-04-11 15:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 2016년<br />

# 알고리즘 문제 풀이 : 2016년

## 문제 
```text
<문제 설명>
2016년 1월 1일은 금요일입니다. 
2016년 a월 b일은 무슨 요일일까요? 두 수 a ,b를 입력받아 
2016년 a월 b일이 무슨 요일인지 리턴하는 함수, solution을 완성하세요. 
요일의 이름은 일요일부터 토요일까지 각각 SUN,MON,TUE,WED,THU,FRI,SAT
입니다. 예를 들어 a=5, b=24라면 5월 24일은 화요일이므로 문자열 "TUE"를 반환하세요.

<제한 조건>
2016년은 윤년입니다.
2016년 a월 b일은 실제로 있는 날입니다. (13월 26일이나 2월 45일같은 날짜는 주어지지 않습니다)

<입출력 예>
a   b   result
5   24  "TUE"
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12901)



## 나의 풀이
```javascript
function solution(a, b) {
    
    const weak = ["SUN", "MON", "TUE", "WED", "THU", "FRI", "SAT"];
    let date = new Date(`2016, ${a}, ${b}`);
    
    return weak[date.getDay()];
}
```



## 공부하기
- [Date 생성자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date)
Date 생성자는 시간의 특정 지점을 나타내는 Date 객체를 생성합니다. Date 객체는 1970년 1월 1일 UTC(국제표준시) 00:00으로부터 지난 시간을 밀리초로 나타내는 유닉스 타임스탬프를 사용합니다.
- [getDay() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)
getDay() 메서드는 주어진 날짜의 현지 시간 기준 요일을 반환합니다. 0은 일요일을 나타냅니다. 현재의 일을 반환하려면 Date.prototype.getDate()를 사용하세요.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)