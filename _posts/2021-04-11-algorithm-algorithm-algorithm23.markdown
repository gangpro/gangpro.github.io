---
layout: post
title: '[알고리즘] 완주하지 못한 선수'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-11 01:00:17 +0900
lastmod: 2021-04-11 01:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 완주하지 못한 선수<br />

# 알고리즘 문제 풀이 : 완주하지 못한 선수

## 문제 
```text
<문제 설명>
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 
단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.
마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 
완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 
완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

<제한사항>
마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
completion의 길이는 participant의 길이보다 1 작습니다.
참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
참가자 중에는 동명이인이 있을 수 있습니다.

<입출력 예>
participant                                         completion                                 return
["leo", "kiki", "eden"]                             ["eden", "kiki"]                           "leo"
["marina", "josipa", "nikola", "vinko", "filipa"]   ["josipa", "filipa", "marina", "nikola"]   "vinko"
["mislav", "stanko", "mislav", "ana"]	["stanko", "ana", "mislav"]	"mislav"

<입출력 예 설명>
예제 #1
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.
예제 #2
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.
예제 #3
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42576)



## 나의 풀이
```javascript
function solution(participant, completion) {
    var answer = '';
    
    participant.sort();
    completion.sort();
    
    for(let i in participant) {
        if(participant[i] != completion[i]) {
            return participant[i];
        }        
    }
}
```



## 공부하기
- [sort() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
sort() 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 정렬은 stable sort가 아닐 수 있습니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다. 정렬 속도와 복잡도는 각 구현방식에 따라 다를 수 있습니다.
- [find() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
find() 메서드는 주어진 판별 함수를 만족하는 첫 번째 요소의 값을 반환합니다. 그런 요소가 없다면 undefined를 반환합니다.
- [forEach() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
forEach() 메서드는 주어진 함수를 배열 요소 각각에 대해 실행합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)