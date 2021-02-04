---
layout: post
title: '[til] 플로터(flutter) - 프로젝트 폴더와 기본 코드 이해하기'
subtitle: 
categories: til
tags: til flutter
comments: true
date: 2021-02-03 23:12:17 +0900
lastmod: 2021-02-03 23:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

플로터(flutter) 프로젝트 폴더와 기본 코드 이해하기<br />


# Flutter 프로젝트 폴더와 기본 코드 이해하기

## 1.  Flutter 프로젝트 폴더의 구성

### 프로젝트 구성

- pubspec.yaml : 프로젝트 버전, 프로젝트 사용 환경, dart 버전, 각종 디팬던시, 서드파티 라이브러리 등을 정의
- android/ios 폴더 : 각 플랫폼에 맞게 앱을 배포하기 위한 정보를 담고 있음
- test 폴더 : 각종 dart 관련 코드를 테스트를 해 볼 수 있음
- lib 폴더 내 main.dart 파일 : 앱을 만들 때 99% 이상 사용하는 파일

### main.dart

- 플로터 앱을 만들기 위해서는 material 라이브러리 import 필수!

```dart
import 'package:flutter/material.dart';

// 아무것도 반환하지 않음 void
// 앱의 시작점
            // => : main 함수가 다른 함수를 호출한다는 의미 
void main() => runApp(MyApp()); // runApp(app) : 플로터에서 최상위 함수. runApp 함수 안에는 app, 즉 위젯이 들어온다 생각하면 됨.

-------------------------------------------------------------------------------------
// 함수 : 소문자로 시작
main(), runApp()
// 클래스 : 대문자로 시작
MyApp()
-------------------------------------------------------------------------------------
```

## 2. 코드의 기본 내용 이해하기 / 앱 페이지 기본 구성 공식화 해보기

### main.dart - 1

```dart
import 'package:flutter/material.dart';

// 아무것도 반환하지 않음 void
// 앱의 시작점
// => : main 함수가 다른 함수를 호출한다는 의미
void main() => runApp(
    MyApp()); // runApp(app) : 플로터에서 최상위 함수. runApp 함수 안에는 app, 즉 위젯이 들어온다 생각하면 됨.

// stl 자동완성
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      // MaterialApp : 플로터에서 제공하고 있는 모든 위젯과 디자인 테마 사용 할 수 있다.
      title: 'First app',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: MyHomePage(), // 앱이 정상적으로 실행되었을 때 가장 먼저 보여주는 화면 경로
    );
  }
}

// stl 자동완성
class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // sca 자동완성 // 앱 화면을 디자인하기 위함
      appBar: AppBar(
        title: Text('First app'), // MyApp에서는
      ),
      body: Center(
        child: Column(
          children: <Widget>[
            Text('Hello'),
            Text('world.'),
            Text('Flutter!!')
          ],
        ),
      ),
    );
  }
}
```

### main.dart - 2 : home에서 scaffold 위젯 바로 사용

```dart
import 'package:flutter/material.dart';

// 아무것도 반환하지 않음 void
// 앱의 시작점
// => : main 함수가 다른 함수를 호출한다는 의미
void main() => runApp(
    MyApp()); // runApp(app) : 플로터에서 최상위 함수. runApp 함수 안에는 app, 즉 위젯이 들어온다 생각하면 됨.

// stl 자동완성
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      // MaterialApp : 플로터에서 제공하고 있는 모든 위젯과 디자인 테마 사용 할 수 있다.
      title: 'First app',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: Scaffold(
        appBar: AppBar(
          title: Text('First app'), // MyApp에서는
        ),
        body: Center(
          child: Column(
            children: <Widget>[
              Text('Hello'),
              Text('world.'),
              Text('Flutter!')
            ],
          ),
        ),
      ), // 앱이 정상적으로 실행되었을 때 가장 먼저 보여주는 화면 경로
    );
  }
}
```

### 기본 앱 화면
<img width="483" alt="Screen Shot 2021-02-03 at 23 57 26" src="https://user-images.githubusercontent.com/59175609/106919308-6172fa80-674d-11eb-802e-91e4dd940dc9.png">


## 참고
- 유튜브 강의 : [https://youtu.be/b5wbsJFXVTM](https://youtu.be/b5wbsJFXVTM)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>