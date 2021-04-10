---
layout: post
title: '[알고리즘] 두 개 뽑아서 더하기'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-10 23:30:17 +0900
lastmod: 2021-04-10 23:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 두 개 뽑아서 더하기<br />

# 알고리즘 문제 풀이 : 두 개 뽑아서 더하기

## 문제 
```text
<문제 설명>
정수 배열 numbers가 주어집니다. 
numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 
만들 수 있는 모든 수를 배열에 오름차순으로 담아 
return 하도록 solution 함수를 완성해주세요.

<제한사항>
numbers의 길이는 2 이상 100 이하입니다.
numbers의 모든 수는 0 이상 100 이하입니다.

<입출력 예>
numbers       result
[2,1,3,4,1]   [2,3,4,5,6,7]
[5,0,2,7]     [2,5,7,9,12]

<입출력 예 설명>
입출력 예 #1
2 = 1 + 1 입니다. (1이 numbers에 두 개 있습니다.)
3 = 2 + 1 입니다.
4 = 1 + 3 입니다.
5 = 1 + 4 = 2 + 3 입니다.
6 = 2 + 4 입니다.
7 = 3 + 4 입니다.
따라서 [2,3,4,5,6,7] 을 return 해야 합니다.

입출력 예 #2
2 = 0 + 2 입니다.
5 = 5 + 0 입니다.
7 = 0 + 7 = 5 + 2 입니다.
9 = 2 + 7 입니다.
12 = 5 + 7 입니다.
따라서 [2,5,7,9,12] 를 return 해야 합니다.
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/68644)



## 나의 풀이
```javascript
function solution(num) {
    let answer = [];
    
    for(let i = 0; i < num.length -1; i++) {
        
        for(let j = i + 1; j < num.length; j++) {
            
            let sum = num[i] + num[j];
            
            if(answer.indexOf(sum) == -1) {
                answer.push(sum);
            }
            
        }
    }
    answer.sort((a, b) => a - b);
    
    return answer;
}
```



## 공부하기
- [indexOf() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)
indexOf() 메서드는 호출한 String 객체에서 주어진 값과 일치하는 첫 번째 인덱스를 반환합니다. 일치하는 값이 없으면 -1을 반환합니다. 
- [push() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/push)
push() 메서드는 배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환합니다.
- [sort() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
sort() 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 정렬은 stable sort가 아닐 수 있습니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다. 정렬 속도와 복잡도는 각 구현방식에 따라 다를 수 있습니다.
- [set 객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set)
Set 객체는 자료형에 관계 없이 원시 값과 객체 참조 모두 유일한 값을 저장할 수 있습니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)