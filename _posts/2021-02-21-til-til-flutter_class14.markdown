---
layout: post
title: '[TIL] 플러터(flutter) - 앱바 메뉴 아이콘 추가하기'
subtitle: 
categories: til
tags: til flutter
comments: true
date: 2021-02-21 01:20:17 +0900
lastmod: 2021-02-21 01:20:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

플러터(Flutter) 앱바 메뉴 아이콘 추가하기 강좌 14<br />

# 플러터(Flutter) 앱바 메뉴 아이콘 추가하기

## App bar icon button

- leading: 아이콘 버튼이나 간단한 위젯을 왼쪽에 배치할 때
- actions: 복수의 앙이콘 버튼 등을 오른쪽에 배치할 때
- onPressed: 함수의 형태로 일반 버튼이나 아이콘 버튼을 터치했을 때 일어나는 이벤트를 정의하는 곳

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

// 단축어 stl
// MyApp
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Appbar',
      theme: ThemeData(primarySwatch: Colors.red),
      home: MyPage(),
    );
  }
}

// MyPage
class MyPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Appbar icon menu'),
        centerTitle: true, // 중앙 정렬
        elevation: 0.0,
        leading: IconButton(
          icon: Icon(Icons.menu), // 햄버거버튼 생성
          onPressed: () {
            // 아이콘 버튼 실행
            print('menu button is clicked');
          },
        ),
        actions: <Widget>[
          IconButton(
            icon: Icon(Icons.shopping_cart), // 햄버거버튼 생성
            onPressed: () {
              // 아이콘 버튼 실행
              print('Shopping cart button is clicked');
            },
          ),
          IconButton(
            icon: Icon(Icons.search), // 햄버거버튼 생성
            onPressed: () {
              // 아이콘 버튼 실행
              print('Search button is clicked');
            },
          ),
        ],
      ),
    );
  }
}
```

### 구현 화면
<img width="348" alt="Screen Shot 2021-02-21 at 1 18 22" src="https://user-images.githubusercontent.com/59175609/108602038-3b06bd80-73e3-11eb-9943-986be51cbec7.png">
<img width="339" alt="Screen Shot 2021-02-21 at 1 18 34" src="https://user-images.githubusercontent.com/59175609/108602045-40640800-73e3-11eb-920e-306997967fa6.png">



## 참고
- 유튜브 강의 : [코딩셰프](https://www.youtube.com/channel/UC_2ge45JCuJH1z6VYt4iCgQ)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>