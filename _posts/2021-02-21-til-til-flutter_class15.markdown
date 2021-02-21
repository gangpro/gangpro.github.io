---
layout: post
title: '[TIL] 플러터(flutter) - Drawer 메뉴 만들기'
subtitle: 
categories: til
tags: til flutter
comments: true
date: 2021-02-21 01:55:17 +0900
lastmod: 2021-02-21 01:55:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

플러터(Flutter) Drawer 메뉴 만들기 강좌 15~16<br />

# 플러터(Flutter) Drawer 메뉴 만들기

## Drawer menu

```dart
Drawer
- ListView
-- UserAccountDrawerHeader
   - currentAccountPicture
   - accountName
   - accountEmail
   - onDetailsPressed
   - decoration
   - otherAccountPictures
-- ListTile
   - Icon
   - Text
   - onTop
```

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
        actions: <Widget>[
          IconButton(
            icon: Icon(Icons.shopping_cart), // 카트 아이콘 생성
            onPressed: () {
              // 아이콘 버튼 실행
              print('Shopping cart button is clicked');
            },
          ),
          IconButton(
            icon: Icon(Icons.search), // 검색 아이콘 생성
            onPressed: () {
              // 아이콘 버튼 실행
              print('Search button is clicked');
            },
          ),
        ],
      ),
      drawer: Drawer(
        child: ListView(
          padding: EdgeInsets.zero,
          children: <Widget>[
            // 프로젝트에 assets 폴더 생성 후 이미지 2개 넣기
            // pubspec.yaml 파일에 assets 주석에 이미지 추가하기
            UserAccountsDrawerHeader(
              currentAccountPicture: CircleAvatar(
                // 현재 계정 이미지 set
                backgroundImage: AssetImage('assets/profile.png'),
                backgroundColor: Colors.white,
              ),
              otherAccountsPictures: <Widget>[
                // 다른 계정 이미지[] set
                CircleAvatar(
                  backgroundColor: Colors.white,
                  backgroundImage: AssetImage('assets/profile2.png'),
                ),
                // CircleAvatar(
                //   backgroundColor: Colors.white,
                //   backgroundImage: AssetImage('assets/profile2.png'),
                // )
              ],
              accountName: Text('GANGPRO'),
              accountEmail: Text('gangpro@email.com'),
              onDetailsPressed: () {
                print('arrow is clicked');
              },
              decoration: BoxDecoration(
                  color: Colors.red[200],
                  borderRadius: BorderRadius.only(
                      bottomLeft: Radius.circular(40.0),
                      bottomRight: Radius.circular(40.0))),
            ),
            ListTile(
              leading: Icon(
                Icons.home,
                color: Colors.grey[850],
              ),
              title: Text('Home'),
              onTap: () {
                print('Home is clicked');
              },
              trailing: Icon(Icons.add),
            ),
            ListTile(
              leading: Icon(
                Icons.settings,
                color: Colors.grey[850],
              ),
              title: Text('Setting'),
              onTap: () {
                print('Setting is clicked');
              },
              trailing: Icon(Icons.add),
            ),
            ListTile(
              leading: Icon(
                Icons.question_answer,
                color: Colors.grey[850],
              ),
              title: Text('Q&A'),
              onTap: () {
                print('Q&A is clicked');
              },
              trailing: Icon(Icons.add),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 구현 화면
<img width="357" alt="Screen Shot 2021-02-21 at 1 55 06" src="https://user-images.githubusercontent.com/59175609/108602872-eb76c080-73e7-11eb-9a5c-24bd8bc17389.png">



## 참고
- 유튜브 강의 : [코딩셰프](https://www.youtube.com/channel/UC_2ge45JCuJH1z6VYt4iCgQ)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>