---
layout: post
title: '[TIL] 플러터(flutter) - 캐릭터 페이지 디자인'
subtitle: 
categories: til
tags: til flutter
comments: true
date: 2021-02-20 21:00:17 +0900
lastmod: 2021-02-20 21:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

플러터(flutter) 캐릭터 페이지 디자인 강좌 9~11<br />

# Flutter 캐릭터 카드 페이지 만들기

## 강좌9 : 캐릭터 페이지 디자인 1

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Charactor carrd',
      home: MyCard(),
    );
  }
}

class MyCard extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('BBANTO'),
        centerTitle: true, // 앱바 타이틀 가운데 정렬 true || false
        backgroundColor: Colors.redAccent, // 앱바 배경색 변경
        elevation: 0.0, // 앱바 높이 조절(붕떠 있는 느낌)
      ),
      body: Center(
        // 위젯 가로 정렬 할 때 쓰임
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center, // 위젯 세로 정렬 할 때 쓰임
          children: <Widget>[
            Text('Hello'),
            Text('Hello'),
            Text('Hello'),
          ],
        ),
      ),
    );
  }
}
```

## 강좌 10 : 캐릭터 페이지 디자인 2

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

// 단축어 : stl
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'BBANTO',
      home: Grade(),
    );
  }
}

// 모양이 변하는 모습이 없으므로 StatelessWidget
class Grade extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.amber[800], // 위젯 배경색
      appBar: AppBar(
        title: Text('BBANTO'),
        backgroundColor: Colors.amber[700], // 앱바 배경색
        centerTitle: true, // 앱바 중앙 정렬
        elevation: 0.0, // 앱바 입체감 없애기
      ),
      body: Padding(
        padding: EdgeInsets.fromLTRB(30.0, 40.0, 0.0, 0.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start, // 단축어 cro // 정렬
          children: <Widget>[
            Text(
              'NAME',
              style: TextStyle(color: Colors.white, letterSpacing: 2.0),
            ),
            SizedBox(
              // 위 아래 텍스트 간 간격을 위해 추가함
              height: 10.0,
            ),
            Text(
              'BBANTO',
              style: TextStyle(
                  color: Colors.white,
                  letterSpacing: 2.0,
                  fontSize: 28.0,
                  fontWeight: FontWeight.bold),
            ),
          ],
        ),
      ),
    );
  }
}
```

## 강좌 11 : 캐릭터 페이지 디자인 3

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

// 단축어 : stl
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false, // 디버스 빨간띠 제거
      title: 'LITTLE PRINCE',
      home: Grade(),
    );
  }
}

// 모양이 변하는 모습이 없으므로 StatelessWidget
class Grade extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.amber[800], // 위젯 배경색
      appBar: AppBar(
        title: Text('어린왕자'),
        backgroundColor: Colors.amber[700], // 앱바 배경색
        centerTitle: true, // 앱바 중앙 정렬
        elevation: 0.0, // 앱바 입체감 없애기
      ),
      body: Padding(
        padding: EdgeInsets.fromLTRB(30.0, 40.0, 0.0, 0.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start, // 단축어 cro // 정렬
          children: <Widget>[
            // 단축어 cir
            // 이미지 넣기
            Center(
              child: CircleAvatar(
                backgroundImage:
                    AssetImage('assets/little_prince.gif'), // 이미지 불러오기
                radius: 60.0, // 서클의 크기를 조절 할 수 있음
              ),
            ),
            // 구분
            Divider(
              height: 60.0, // 위 이미지 30과 아래 텍스트 30이 합쳐서 60이라는 말
              color: Colors.grey[850],
              thickness: 1.0, // 선의 두께
              endIndent: 30.0, // 끝에서 부터 어느정도 떨어져야 할지 지정
            ),
            Text(
              'NAME',
              style: TextStyle(color: Colors.white, letterSpacing: 2.0),
            ),
            SizedBox(
              // 위 아래 텍스트 간 간격을 위해 추가함
              height: 10.0,
            ),
            Text(
              'LITTLE PRINCE',
              style: TextStyle(
                  color: Colors.white,
                  letterSpacing: 2.0,
                  fontSize: 28.0,
                  fontWeight: FontWeight.bold),
            ),
            SizedBox(
              height: 30.0,
            ),
            Text(
              'LITTLE PRINCE POWER LEVEL',
              style: TextStyle(color: Colors.white, letterSpacing: 2.0),
            ),
            SizedBox(
              height: 10.0,
            ),
            Text(
              '14',
              style: TextStyle(
                  color: Colors.white,
                  letterSpacing: 2.0,
                  fontSize: 28.0,
                  fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 30.0),
            Row(
              children: <Widget>[
                Icon(Icons.check_circle_outline),
                SizedBox(width: 10.0),
                Text('using lightsaber',
                    style: TextStyle(fontSize: 16.0, letterSpacing: 1.0))
              ],
            ),
            Row(
              children: <Widget>[
                Icon(Icons.check_circle_outline),
                SizedBox(width: 10.0),
                Text('face hero tatto',
                    style: TextStyle(fontSize: 16.0, letterSpacing: 1.0))
              ],
            ),
            Row(
              children: <Widget>[
                Icon(Icons.check_circle_outline),
                SizedBox(width: 10.0),
                Text('fire flames',
                    style: TextStyle(fontSize: 16.0, letterSpacing: 1.0))
              ],
            ),
            Center(
              child: CircleAvatar(
                backgroundImage: AssetImage('assets/little_prince_rose.png'),
                radius: 40.0,
                backgroundColor: Colors.amber[800],
              ),
            )
          ],
        ),
      ),
    );
  }
}
```

프로젝트 내 이미지 사용방법

```dart
1. 프로젝트 내 asset 폴더 생성 후 jpg, gif 파일 옮겨놓기
2. pubspec.yaml 수정
(기존) : 주석 처리되어 있음
	# assets:
  #   - images/a_dot_burr.jpeg
  #   - images/a_dot_ham.jpeg
(변경) : 생성한 폴더 내 파일명 적기
	assets:
    - assets/little_prince_rose.png
    - assets/little_prince.gif

```



### 구현 화면
<img width="352" alt="Screen Shot 2021-02-20 at 21 36 05" src="https://user-images.githubusercontent.com/59175609/108595531-b9536780-73c3-11eb-9d20-56fec42d4fa9.png">


## 참고
- 유튜브 강의 : [코딩셰프](https://www.youtube.com/channel/UC_2ge45JCuJH1z6VYt4iCgQ)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>