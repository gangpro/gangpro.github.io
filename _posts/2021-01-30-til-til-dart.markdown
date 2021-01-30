---
layout: post
title: '[TIL] Dart 언어 기초'
subtitle: 
categories: til
tags: til dart
comments: true
date: 2021-01-30 12:12:17 +0900
lastmod: 2021-01-30 23:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

Dart 언어 기초<br />


## Dart 언어 기초 자료

- 공식 사이트 기초 문서 : [https://dart.dev/samples#functions](https://dart.dev/samples#functions)
- 공식 온라인 에디터 : [https://dartpad.dev/](https://dartpad.dev/)?

## Dart 언어 기초 - 변수와 함수

```dart
// 다트언어 사용시 main은 필수 함수
/*
void main() {
  print('Hello, World!');
}
*/

// 변수 표현
var name = 'Voyager I';
var year = 1977;
var antennaDiameter = 3.7;
var flybyObjects = ['Jupiter', 'Saturn', 'Uranus', 'Neptune'];
var image = {
  'tags': ['saturn'],
  'url': '//path/to/saturn.jpg'
};

// 변수 표현2
String name2 = 'Voyager I';
int year2 = 1977;
double antennaDiameter2 = 3.7;
List flybyObjects2 = ['Jupiter', 'Saturn', 'Uranus', 'Neptune'];
Map image2 = {
  'tags': ['saturn'],
  'url': '//path/to/saturn.jpg'
};

// 함수
int timesTwo(int x) {
  return x * 2;
}

// 화살표 함수
int timesThree(int x) => x * 3;

// 화살표 함수를 일반 함수로 변환
int timesFour(int x) {
  return timesTwo(timesTwo(x));
}

// 함수에 파라미터 사용
int runTwice(int x, Function f) {
  x = f(x);
  x = f(x);
  
  return x;
}

// main
main() {
  int numb = 16;
  
  print('16 is $numb');
  print('2 times two is ${timesTwo(2)}');
  print('3 times three is ${timesThree(3)}');
  print('4 times four is ${timesFour(4)}');
  print('2 x 2 x 2 is ${runTwice(2, timesTwo)}');
}
```

## Dart 언어 기초 - if 문, for 문

```dart
// 짝수면 true 아니면 false
bool isEven(int x) {
  // An if-else statement.
  if (x % 2 == 0) {
    return true;
  } else {
    return false;
  }
}

// 짝수만 가져오기
List<int> getEvenNumbers(Iterable<int> numbers) {
  var evenNumbers = <int>[];
  // A for-in loop.
  for (var i in numbers) {
    // A single line if statement.
    if (isEven(i)) evenNumbers.add(i);
  }
  return evenNumbers;
}

main() {
  //= var numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
  var numbers = List.generate(10, (i) => i);
  print(getEvenNumbers(numbers));
}
```

## Dart 언어 기초 - String

```dart
main() {
  print('a single quoted string');
  print("a double quoted string");
  
  // Strings can be combined with the + operator.
  print("cat" + "dog");
  
  // Triple quotes define a multi-line string.
  print('''triple quoted strings
are for multiple lines''');
  
  // Dart supports string interpolation.
  var pi = 3.14;
  print('pi is $pi');  // 변수만 선언할 땐 $'{변수}' 보다는 '$변수'로 쓰는게 좋다(퍼포먼스 때문에
  //print('pi is ${pi}');)
  //print('pi is ' + pi.toString());
  print('tau is ${2 * pi}');
}
```

## Dart 언어 기초 - Class

```dart
// class
class Car {
  int    seats;
  String color;
  
  /*
  // (1) constructor : [] 옵션 값
  Car(int sts, [String clr]) {  
    this.seats = sts;
    this.color = clr;
  }
  */
  
  /*
  // (2) constructor : [] 옵션 값에 default 값을 줄 수 있다.
  Car(int sts, [String clr = 'black']) {  
               // [] = option  // [String clr = 'default value'] 
    this.seats = sts;
    this.color = clr;
  }
  */
  
  /*
  // (3) constructor : {} 옵션 값, 파라미터 순서 상관 없이 받을 수 있다.
  Car({int sts, String clr = 'black'}) {  
    this.seats = sts;
    this.color = clr;
  }
  */
  
  // (4) constructor : this 안쓰고도 표현 가능
  Car({this.seats, this.color = 'black'});
  
  // function
  printVars() {
    print('seat: $seats, color: $color');
  }
  
}

main() {
  /*
  // (1) newCar = object || instance
  Car newCar = new Car(4);        // seat: 4, color: null
  //= Car newCar = Car(4, 'red'); // seat: 4, color: red
  */
  
  /*
  // (2) object || instance
  Car newCar = new Car(4);        // seat: 4, color: black
  //= Car newCar = Car(4, 'red'); // seat: 4, color: red
  */
  
  /*
  // (3) object || instance
  Car newCar = new Car(clr: 'red', sts: 6); // seat: 6, color: red
  */
  
  // (4) object || instance
  Car newCar = new Car(color: 'red', seats: 6); // seat: 6, color: red
  
  // print
  //print('seat: ${newCar.seats}, color: ${newCar.color}');
  newCar.printVars();
  
}
```

## 참고
- 유튜브 강의 : [https://www.youtube.com/watch?v=nRsxWt3BWzM](https://youtu.be/nRsxWt3BWzM)

<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀드립니다.<br/>