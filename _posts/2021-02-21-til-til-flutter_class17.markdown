---
layout: post
title: '[TIL] 플러터(flutter) - BuildContext 이해하기'
subtitle: 
categories: til
tags: til flutter
comments: true
date: 2021-02-21 23:55:17 +0900
lastmod: 2021-02-21 23:55:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

플러터(Flutter) BuildContext 이해하기 (강좌 17)<br />

# 플러터(Flutter) BuildContext 이해하기

## BuildContext 1

```
"A handle to the location of a widget in the widget tree."
"widget tree에서 현재 widget의 위치를 알 수 있는 정보"
```

```dart
int addNumber(int a, int b) {
	return a + b;
}

여기서
int는 타입
addNumber 함수 안에 int 타입의 a, b 인자값을 받아서
결과값으로 a + b를 리턴 한다는 의미

class MyPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold();
	}
}

여기서 
Widget은 타입
build 함수 안에 BuildContext 타입의 context 인자값을 받아오며
결과값으로 Scaffold()를 리턴한다는 의미
```

## BuildContext 2

```
"Each widget has its own BuildContext, 
which becomes the parent of the widget 
returned by the StatelessWidget.build or State.build function."

"이 BuildContext는 stateless 위젯이나 state 빌드 메서드에 의해서
리턴 된 위젯의 부모가 된다."
```

```dart
class MyPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold();
	}
}

여기서
StatelessWiget으로 MyPage라는 커스텀 위젯을 생성했으며,
이 MyPage라는 커스텀 위젯도 자신만의 BuildContext 타입의 context를 가지고 있다.
build 메소드를 통해서 결과값으로 Scaffold() 위젯이 리턴되었으며 
Scaffold() 위젯은 부모인 MyPage의 context를 그대로 물려받게 된다는 의미
```



## 참고
- 유튜브 강의 : [코딩셰프](https://www.youtube.com/channel/UC_2ge45JCuJH1z6VYt4iCgQ)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>