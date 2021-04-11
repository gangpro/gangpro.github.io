---
layout: post
title: '[알고리즘] 행렬의 덧셈'
subtitle: 
categories: algorithm
tags: algorithm
comments: true
date: 2021-04-11 16:40:17 +0900
lastmod: 2021-04-11 16:40:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

알고리즘 문제 풀이 : 행렬의 덧셈<br />

# 알고리즘 문제 풀이 : 행렬의 덧셈

## 문제 
```text
<문제 설명>
행렬의 덧셈은 행과 열의 크기가 같은 두 행렬의 같은 행, 같은 열의 값을 서로 더한 결과가 됩니다. 
2개의 행렬 arr1과 arr2를 입력받아, 행렬 덧셈의 결과를 반환하는 함수, solution을 완성해주세요.

<제한 조건>
행렬 arr1, arr2의 행과 열의 길이는 500을 넘지 않습니다.

<입출력 예>
arr1            arr2            return
[[1,2],[2,3]]   [[3,4],[5,6]]   [[4,6],[7,9]]
[[1],[2]]       [[3],[4]]       [[4],[6]]
```

* [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12950)



## 나의 풀이
```javascript
function solution(arr1, arr2) {
    let answer = [];
    
    for(let i = 0; i < arr1.length; i++) {
        
        let temp = [];
        for(let j = 0; j < arr1[i].length; j++) {
            
            console.log(`arr1 = ${arr1[i][j]}, arr2 = ${arr2[i][j]}`);
            temp.push(arr1[i][j] + arr2[i][j]);     
            
        }
        answer.push(temp);
        
    }
    
    return answer;
}
```



## 공부하기
- [push() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 
push() 메서드는 배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환합니다.
- [map() 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.



## 참고
- 알고리즘 문제 : [프로그래머스](https://programmers.co.kr)