---
layout: post
title: '[TIL] 다트(Dart) 클래스와 위젯의 정체'
subtitle: 
categories: til
tags: til dart
comments: true
date: 2021-02-21 01:00:17 +0900
lastmod: 2021-02-21 01:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

다트(Dart) 클래스와 위젯의 정체 강좌 12~13<br />


# 다트(Dart) 클래스와 위젯의 정체

## Class and Widget 1

- 프로그래밍 상에서의 클래스란?
    - 객체가 가져야 하는 속성과 기능을 정의한 내용을 담고 있는 **설계도** 역할
- 프로그래밍 상에서의 객체란?
    - 클래스가 정의된 후 메모리상에 할당되었을 때 이를 **객체**락고 함
- 프로그래밍 상에서의 인스턴스란?
    - 클래스를 기반으로 생성 됨
    - 클래스의 속성과 기능을 똑같이 까지고 있고 프로그래밍 상에서 사용되는 대상

```dart
// class
// 클래스명은 대문자로 시작
class Person {
  // 객체에 대한 정보를 정의
  String name;
  int    age;
  String gender;
}

void main() {
  // 클래스 타입은 Person, 인스턴스 변수명은 p1 = new Person() 생성자
  // p1 인스턴스 생성
  Person p1 = new Person(); 
  p1.age = 30;
  
  // 인스턴스 출력
  print(p1.age);  //= 30
}
```

## Class and Widget 2

- 생성자와 관련된 함수의 구조와 기능
- 생성자의 구조와 역할
- 클래스와 위젯의 관계

```dart
// class
// 클래스명은 대문자로 시작
class Person {
  // 객체에 대한 정보를 정의
  String name;
  int    age;
  String gender;
}

// constructor(argument)
// 생성자는 소문자로 시작
int addNumber(int num1, int num2) {
  return num1 + num2;
}

void main() {
  // 클래스 타입은 Person, 인스턴스 변수명은 p1 = new Person() 생성자
  // p1 인스턴스 생성
  Person p1 = new Person(); 
  p1.age = 30;
  
  // 인스턴스 출력
  print(p1.age);  //= 30
  
  addNumber(3, 4);
  print(addNumber(3,4));
}
```

```dart
// class
// 클래스명은 대문자로 시작
class Person {
  // 객체에 대한 정보를 정의
  String name;    // 멤버변수
  int    age;     // 멤버변수
  String gender;  // 멤버변수
  
  // constructor(argument)
  // 생성자
  Person(String name, int age, String gender) {
    this.name = name;     // this.{} 멤버변수
    this.age = age;       // this.{} 멤버변수
    this.gender = gender; // this.{} 멤버변수
  }
}

void main() {
  // 클래스 타입은 Person, 인스턴스 변수명은 p1 = new Person() 생성자
  // p1 인스턴스 생성
  Person p1 = new Person('Tom', 30, 'male');
  Person p2 = new Person('Jane', 27, 'female');
  
  // 인스턴스 출력
  print(p1.age);  //= 30
  print(p2.age);  //= 27
  
}
```

```dart
// class
// 클래스명은 대문자로 시작
class Person {
  // 객체에 대한 정보를 정의
  String name;    // 멤버변수
  int    age;     // 멤버변수
  String gender;  // 멤버변수
  
  // constructor(argument)
  // 생성자
  Person({String name, int age, String gender}) { // {} named argument
    this.name = name;     // this.{} 멤버변수
    this.age = age;       // this.{} 멤버변수
    this.gender = gender; // this.{} 멤버변수
  }
}

void main() {
  // 클래스 타입은 Person, 인스턴스 변수명은 p1 = new Person() 생성자
  // p1 인스턴스 생성
  Person p1 = new Person(age: 39);
  Person p2 = new Person(gender: 'male');
  
  // 인스턴스 출력
  print(p1.age);  //= 30
  print(p2.gender);  //= male
  
}
```

## 참고
- 유튜브 강의 : [코딩셰프](https://www.youtube.com/channel/UC_2ge45JCuJH1z6VYt4iCgQ)
- 개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>