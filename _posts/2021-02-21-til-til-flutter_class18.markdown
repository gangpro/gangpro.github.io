---
layout: post
title: '[TIL] 플러터(flutter) - 스낵 바(Snack bar)와 BuildContext'
subtitle: 
categories: til
tags: til flutter
comments: true
date: 2021-02-21 23:59:17 +0900
lastmod: 2021-02-21 23:59:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

플러터(Flutter) 스낵 바(Snack bar)와 BuildContext (강좌 18)<br />

# 플러터(Flutter) 스낵 바(Snack bar)와 BuildContext

## Scaffold.of(context) method

```
"현재 주어진 context에서 위로 올라가면서 가장 가까운 Scaffold를 찾아서 반환하라."
Something.of(context)
Theme.of(context)
```

### 에러 화면 코딩

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

// 단축어 stl
// MyApp
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Snack Bar',
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
        title: Text('Snack Bar'),
        centerTitle: true,
      ),
      body: Center(
        child: FlatButton(
          child: Text(
            'Show me',
            style: TextStyle(color: Colors.white),
          ),
          color: Colors.red,
          onPressed: () {
            Scaffold.of(context).showSnackBar(SnackBar(
              content: Text('Hello'),
            )); // 함숭이므로 끝에 ;
          },
        ),
      ),
    );
  }
}

```

```
The context used was: MyPage

════════════════════════════════════════════════════════════════════════════════

════════ Exception caught by gesture ═══════════════════════════════════════════
Scaffold.of() called with a context that does not contain a Scaffold.
════════════════════════════════════════════════════════════════════════════════
```

### 정상 화면 코딩

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

// 단축어 stl
// MyApp
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Snack Bar',
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
          title: Text('Snack Bar'),
          centerTitle: true,
        ),
        body: Builder(
          builder: (BuildContext ctx) {
            return Center(
              child: FlatButton(
                child: Text(
                  'Show me',
                  style: TextStyle(color: Colors.white),
                ),
                color: Colors.red,
                onPressed: () {
                  Scaffold.of(ctx).showSnackBar(SnackBar(
                    content: Text('Hello'),
                  )); // 함숭이므로 끝에 ;
                },
              ),
            );
          },
        ));
  }
}
```


### 구현 화면
<img width="368" alt="Screen Shot 2021-02-21 at 23 54 22" src="https://user-images.githubusercontent.com/59175609/108628894-99de3c80-74a0-11eb-8cc2-9228d317196c.png">



## 참고
- 유튜브 강의 : [코딩셰프](https://www.youtube.com/channel/UC_2ge45JCuJH1z6VYt4iCgQ)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>