---
layout: post
title: '[til] 플로터(flutter) - 위젯이란?'
subtitle: 
categories: til
tags: til flutter
comments: true
date: 2021-02-03 22:12:17 +0900
lastmod: 2021-02-03 22:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

플로터(flutter) - 위젯이란?<br />

# 플로터에서 제일 중요하다는 위젯이란 무엇일까요?

## 1. Flutter 상에서의 위젯이란?

### Widget

1. 독립적으로 실행되는 작은 프로그램
2. 주로 바탕화면 등에서 날씨나 뉴스, 생활정보 등을 보여줌
3. 그래픽이나 데이터 요소를 처리하는 함수를 가지고 있음

### What is Widget in flutter?

1. UI를 만들고 구성하는 모든 기본 단위 요소
2. 눈에 보이지 않는 요소들까지 위젯
3. Everything is a widget

## 2. Stateless widgets vs. Stateful widgets

### Stateless와 Stateful의 일반적인 의미

1. Stateful ⇒ Value 값을 지속적으로 추적 보존
2. Stateless ⇒ 이전 상호작용의 어떠한 값도 저장하지 않음

### Type of widgets

1. Stateless Widgets : 상태가 없는 정적인 위젯
2. Stateful Widgets : 계속 움직임이 있는 동적인 위젯

### Stateless Widgets

1. 스크린상에 존재만 할 뿐 아무것도 하지 않음
2. 어떠한 실시간  데이터도 저장하지 않음
3. 어떤 변화(모양, 상태)를 유발시키는 value 값을 갖지 않음
4. ex: 앱 타이틀, 앱 로고 등

### Stateful Widgets

1. 사용자의 interaction에 따라서 모양이 바뀜
2. 데이터를 받게 되었을 때 모양이 바뀜
3. ex: 체크박스, 라디오 버튼, 로그인 input 등

## 3. Widget Tree

1. Widgets들은 tree 구조로 정리될 수 있음
2. 한 Widget 내에 얼마든지 다른 widget들이 포함될 수 있음
3. Widget은 부모 위젯과 지식 위젯으로 구성
4. Parent widget을 widget container라고 부르기도 함
5. ex: 구성 요소
```
MyApp 
> MaterialApp 
> MyHomePage(custom)(앱 디자인, 앱 기능) 
> Scaffold(앱 화면 기능과 구성을 하는 위젯) 
> UI, 앱 구성요소(텍스트, 이미지, 패딩, 마진 등등) 
> AppBar > Text // > Center > Column > Image, TextField, Button
```

## 참고
- 유튜브 강의 : [https://youtu.be/jI4kqLdqXic](https://youtu.be/jI4kqLdqXic)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>